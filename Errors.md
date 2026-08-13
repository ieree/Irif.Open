# Irif API — справочник ошибок: заявки, отклики, чаты

> v1.0 · 2026-08-13 · собрано по фактическим файлам RequestErrors / RequestChatLinkErrors / ChatErrors + FileErrors (по использованию).
> Формат ответа ошибки — единый (см. основной док): логика по `code`, `message` можно показывать пользователю как есть.

## Соглашения по HTTP-кодам

| Тип Error | HTTP | Поведение фронта |
|---|---|---|
| Validation | 400 | показать message; `Validation.<Field>` — подсветить поле |
| Forbidden | 403 | скрыть/задизейблить действие, показать message |
| NotFound | 404 | «не найдено» / уход со страницы |
| Conflict | 409 | показать message; для Conflict-повторов — предложить обновить данные |
| RateLimit | 429 | **всегда** есть заголовок `Retry-After: <сек>` — таймер строить по нему, message только для отображения |
| Failure | 500 | generic-ошибка, retry |

---

## 1. Управление заявками (`Request.*`)

### Лимиты — показывать пользователю обязательно

| Code | HTTP | Когда | Сообщение |
|---|---|---|---|
| `Request.DraftLimitReached` | 400 | создание черновика сверх лимита | «Максимум 10 черновиков. Удалите ненужные.» |
| `Request.ModerationLimitReached` | 400 | submit при 5 заявках на модерации | «Максимум 5 заявок на модерации одновременно.» |
| `Request.ActiveLimitReached` | 400 | публикация сверх тарифного лимита активных | «Достигнут лимит активных заявок по вашему тарифу.» ⚠️ upsell-точка: предложить тариф |
| `Request.TagsLimitExceeded` | 400 | слишком много тегов | «Превышен лимит тегов.» |
| `Request.AttachmentsLimitExceeded` | 400 | слишком много вложений заявки | «Превышен лимит вложений.» |

⚠️ В C#-поле лимит активных называется `ActiveRequestsLimitReached`, но **код в ответе — `Request.ActiveLimitReached`**. Фронту матчить по коду из ответа.

### Валидация публикации (форма создания/submit)

| Code | HTTP | Когда |
|---|---|---|
| `Request.DraftEmpty` | 400 | черновик без единого поля |
| `Request.TitleRequired` | 400 | нет названия |
| `Request.DescriptionRequired` | 400 | нет описания |
| `Request.CategoryRequired` | 400 | нет категории |
| `Request.RegionRequired` | 400 | нет региона |
| `Request.TagsRequired` | 400 | нет тегов |
| `Request.TagNotFound` | 400 | переданы несуществующие теги |
| `Request.NoCategoryTags` | 404 | у категории нет тегов |
| `Request.FileMetadataMissing` | 400 | файл без метаданных |
| `Request.MainImageInAttachments` | 400 | главное фото продублировано во вложениях |

### Статусные переходы (кнопки действий)

| Code | HTTP | Когда |
|---|---|---|
| `Request.CannotSubmitForModeration` | 400 | submit не из Draft/Rejected |
| `Request.CannotPause` | 400 | pause не из Active |
| `Request.CannotResume` | 400 | resume не из Paused |
| `Request.CannotComplete` | 400 | complete из недопустимого статуса (в т.ч. **InProgress** — сначала открепить исполнителей) |
| `Request.CannotCancel` | 400 | cancel из недопустимого статуса (в т.ч. InProgress) |
| `Request.CannotDelete` | 400 | delete из недопустимого статуса |
| `Request.CannotRenew` | 400 | renew не-истёкшей |
| `Request.NotEditable` | 400 | правка/отклик по заявке вне допустимых статусов |
| `Request.FilesNotEditable` | 400 | правка файлов в недопустимом статусе |

Примечание для InProgress: `Request.HasPinnedChats` в норме **недостижим** (статусный гард срабатывает раньше) — фронту обрабатывать `CannotComplete`/`CannotCancel` и подсказывать «открепите исполнителей».

### Общие / конкурентность

| Code | HTTP | Когда |
|---|---|---|
| `Request.NotFound` | 404 | нет заявки / удалена / не владелец (анти-IDOR) |
| `Request.Forbidden` | 403 | отклик на свою заявку и т.п. |
| `Request.Conflict` | 409 | параллельное изменение (второй pin/unpin в ту же секунду) → «Повторите действие», фронт повторяет запрос |
| `Request.NumberExists` | 409 | коллизия номера (в норме недостижимо) |
| `Request.InvalidStatus`, `Request.InvalidType`, `Request.ValidationFailed` | 400 | служебные, в норме недостижимы |

⚠️ В C#-поле конкурентность называется `ConcurrentUpdate`, **код в ответе — `Request.Conflict`**.

### Rate limit по заявкам

**Нет.** Пауза/возобновление, архивация — рейт-лимитов не существует, только статусные гарды выше. Единственные лимиты заявок — количественные (черновики/модерация/активные).

---

## 2. Отклики (`RequestChatLink.*`)

### Показывать пользователю на странице отклика/заявки

| Code | HTTP | Когда | Действие фронта |
|---|---|---|---|
| `RequestChatLink.ResponseCooldown` | 429 | <60 сек с прошлого отклика | таймер по `Retry-After`, кнопка «Откликнуться» дизейблится |
| `RequestChatLink.ResponseLimitReached` | 400 | >20 активных откликов | «Максимум 20 активных откликов. Отзовите ненужные.» + линк на список откликов |
| `RequestChatLink.AlreadyExists` | 409 | повторный отклик по живому линку | вести в существующий чат |
| `RequestChatLink.AlreadyRespondedArchived` | 409 | отклик по архивному чату | «Напишите сообщение в чат» — сообщение будит чат; вести в чат |
| `RequestChatLink.ReactivateLimitReached` | 409 | отклик после 3 циклов отзыва | терминал: повторный отклик по этой заявке недоступен навсегда, кнопку скрыть |
| `RequestChatLink.WithdrawLimitReached` | 400 | 4-я попытка отозвать | «Максимум 3 раза.» |
| `RequestChatLink.CannotWithdraw` | 400 | withdraw закреплённого | «Нельзя отозвать, пока вы закреплены» (текст обобщённый — фронт может уточнить по статусу pinned) |

### Закрепление (страница заказчика)

| Code | HTTP | Когда | Действие фронта |
|---|---|---|---|
| `RequestChatLink.PinLimitReached` | 400 | тарифный лимит слотов | ⚠️ upsell-точка: предложить тариф. Единственный наружный код лимита pin (`Request.PinnedChatsLimitExceeded` — недостижимая домен-страховка) |
| `RequestChatLink.PinCooldown` | 429 | re-pin <5 мин после unpin | таймер по `Retry-After` |
| `RequestChatLink.CannotPin` | 400 | pin недопустимого статуса линка | |
| `RequestChatLink.CannotUnpin` | 400 | unpin незакреплённого | |
| `Request.Conflict` | 409 | гонка двух pin | повторить действие |

### Служебные

| Code | HTTP | Когда |
|---|---|---|
| `RequestChatLink.NotFound` | 404 | линк не найден; также withdraw «чужого» линка (анти-IDOR) |
| `RequestChatLink.CannotArchive`, `.CannotReactivate` | 400 | внутренние гарды, наружу в норме не выходят |
| `RequestChatLink.Invalid*Id` (3 шт.) | 400 | доменная валидация, в норме недостижимы |

---

## 3. Чат (`Chat.*`)

### Показывать пользователю

| Code | HTTP | Когда | Действие фронта |
|---|---|---|---|
| `Chat.MessageRateLimit` | 429 | >30 сообщений/мин (глобально по профилю, не per-chat) | таймер по `Retry-After`, поле ввода блокируется |
| `Chat.Blocked` | 400 | сообщение/правка в чат завершённой/отменённой заявки или отозванного отклика | скрыть поле ввода; ⚠️ приоритетнее `chatLinkStatus` |
| `Chat.EmptyMessage` | 400 | пустой текст без вложений | |
| `Chat.TooManyAttachments` | 400 | >10 вложений | |
| `Chat.EditTimeExpired` | 400 | правка спустя >24ч | скрывать кнопку Edit по времени на фронте |
| `Chat.NotMessageOwner` | 403 | правка чужого сообщения | кнопку Edit показывать только своим |
| `Chat.CannotEditSystemMessage` | 400 | правка системки | |
| `Validation.Text` | 400 | текст >4000 символов (DTO-валидатор) | ограничить textarea |

### Служебные / безопасность

| Code | HTTP | Когда |
|---|---|---|
| `Chat.NotParticipant` | 403 | не участник комнаты (GET/POST messages, read) — анти-IDOR |
| `Chat.NotFound` | 404 | комната не найдена |
| `Chat.MessageNotFound` | 404 | edit/read по чужому или несуществующему messageId |
| `Chat.InvalidCursor` | 400 | битый курсор пагинации |
| `Chat.LoadFailed` | 500 | внутренний сбой загрузки |
| `Chat.AlreadyParticipant`, `.CannotChatWithSelf`, `.Invalid*Id` | 400/409 | служебные, в норме недостижимы |

### Файлы в чате (`File.*`, при отправке вложений)

| Code | Когда |
|---|---|
| `File.NotFound` | id не существует (в т.ч. дубль id после дедупликации) |
| `File.Forbidden` | вложение чужого аплоада |
| `File.NotUploaded` | файл не в состоянии Ready |

`POST /api/chat/files/urls` с чужими mediaFileId ошибки **не** возвращает — отдаёт пустой `items` (осознанно, анти-enumeration).

---

## 4. Сводка всех 429 (Retry-After обязателен)

| Code | Окно |
|---|---|
| `Auth.SmsCooldown` | 60 сек между SMS |
| `Auth.SmsRateLimitExceeded` | лимит/час по номеру и IP |
| `RequestChatLink.ResponseCooldown` | 60 сек между откликами |
| `RequestChatLink.PinCooldown` | 5 мин после unpin до повторного pin |
| `Chat.MessageRateLimit` | 30 сообщений/мин по профилю |

Напоминание: для кросс-доменного стейджа фронт читает заголовок только при `WithExposedHeaders("Retry-After")` в CORS (пункт деплой-чеклиста).

---

## 5. ПЕРВООЧЕРЕДНЫЕ — минимальный набор для фронта (текущий спринт)

**Принцип, который экономит время: парсить все коды НЕ нужно.** `message` каждой ошибки человекочитаем — дефолтная обработка для любого не перечисленного ниже кода: показать `errors[0].message` тостом/под формой. Спец-логика нужна только там, где кроме текста требуется действие интерфейса. Таких кодов 16:

### Таймеры (429 → дизейбл кнопки + обратный отсчёт по `Retry-After`)

| # | Code | Где |
|---|---|---|
| 1 | `RequestChatLink.ResponseCooldown` | кнопка «Откликнуться» |
| 2 | `RequestChatLink.PinCooldown` | кнопка «Закрепить» |
| 3 | `Chat.MessageRateLimit` | поле ввода сообщения |

Обработчик один на все три: `status === 429` → таймер по заголовку. Отдельные коды различать не обязательно.

### Лимиты с целевым действием (400 → message + кнопка/линк)

| # | Code | Действие |
|---|---|---|
| 4 | `Request.ActiveLimitReached` | message + предложение тарифа (upsell) |
| 5 | `RequestChatLink.PinLimitReached` | message + предложение тарифа (upsell) |
| 6 | `Request.DraftLimitReached` | message + линк на список черновиков |
| 7 | `RequestChatLink.ResponseLimitReached` | message + линк на список откликов |
| 8 | `Request.ModerationLimitReached` | просто message (действия нет — ждать модерацию) |

### Навигация вместо ошибки (409 → редирект/смена UI)

| # | Code | Действие |
|---|---|---|
| 9 | `RequestChatLink.AlreadyExists` | вести в существующий чат |
| 10 | `RequestChatLink.AlreadyRespondedArchived` | вести в чат + подсказка «сообщение возобновит общение» |
| 11 | `RequestChatLink.ReactivateLimitReached` | скрыть кнопку отклика по этой заявке насовсем |
| 12 | `Request.Conflict` | **тихий** одиночный повтор запроса; юзеру не показывать, если повтор прошёл |

### Состояние экрана

| # | Code | Действие |
|---|---|---|
| 13 | `Chat.Blocked` | скрыть/задизейблить поле ввода чата |
| 14 | `Request.CannotComplete` / `Request.CannotCancel` | message + подсказка «открепите исполнителей» на InProgress-заявке |
| 15 | `Validation.<FieldName>` | подсветка соответствующего поля формы (общий механизм, уже требуется для auth-форм) |
| 16 | `Auth.TokenExpired` | refresh → retry (уже в интерцепторе — здесь для полноты) |

### Осознанно НЕ в спринте

Всё остальное из разделов 1–3 — через дефолтный показ `message`: статусные гарды (`CannotPause`, `CannotWithdraw`, `WithdrawLimitReached`...), валидация формы заявки поверх подсветки полей, служебные и «в норме недостижимые» коды. Их спец-обработка — по мере жалоб/дизайна, контракт не изменится.
