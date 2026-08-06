# API Documentation

**Base URL:** `{host}`

---

## Table of Contents

- [Common types](#common-types)
  - [StatusBadge](#statusbadge)
  - [Actions](#actions)
  - [PaginationMeta](#paginationmeta)
  - [CursorMeta](#cursormeta)
- [Categories](#categories)
  - [GET /api/request-categories/tree](#get-apirequest-categoriestree)
  - [GET /api/request-categories/tree/tags](#get-apirequest-categoriestreetags)
  - [GET /api/request-categories/popular](#get-apirequest-categoriespopular)
- [Dashboard](#dashboard)
  - [GET /api/dashboard](#get-apidashboard)
- [Requests](#requests)
  - [GET /api/requests/my](#get-apirequestsmy)
  - [GET /api/requests/{requestId}/details](#get-apirequestsrequestiddetails)
  - [GET /api/requests/{requestId}/change-log](#get-apirequestsrequestidchange-log)
  - [GET /api/requests/{requestId}/views/stats](#get-apirequestsrequestidviewsstats)
  - [GET /api/requests/history](#get-apirequestshistory)
  - [GET /api/requests/history/filters](#get-apirequestshistoryfilters)
- [Responses](#responses)
  - [GET /api/responses](#get-apiresponses)
  - [GET /api/responses/{requestId}](#get-apiresponsesrequestid)
  - [GET /api/responses/chat-links/by-request/{requestId}](#get-apiresponseschat-linksby-requestrequestid)
- [Chats](#chats)
  - [GET /api/chat](#get-apichat)
  - [GET /api/chat/{chatRoomId}/messages](#get-apichatchatroomidmessages)

---

## Common types

Shapes reused across several endpoints. <!-- Общие типы -->

### StatusBadge

Every status returned by the API uses this shape. <!-- Единый вид статусов -->

| Field   | Type     | Description                              |
|---------|----------|------------------------------------------|
| `code`  | `string` | Machine-readable status — switch on this  |
| `label` | `string` | Ready-to-render label, localised (ru)     |
| `theme` | `string` | UI hint for colouring the badge           |

#### Request status codes

| `code`      | `label`   | `theme`   | Description        |
|-------------|-----------|-----------|--------------------|
| `active`    | Активна   | `live`    | Open for responses |
| `in_progress` | В работе | `live`   | An executor is working on it |
| `completed` | Завершена | `success` | Closed as completed |

<!-- дополнить: завершена, отменена, на модерации, черновик, приостановлена, истекла -->

#### Response status codes

| `code`    | `label`           | `theme`   | Description                        |
|-----------|-------------------|-----------|------------------------------------|
| `pending` | Отклик отправлен  | `waiting` | Response sent, awaiting a decision |

<!-- пока встречался только pending -->

#### Chat link status codes

| `code`    | `label`                                | `theme`   | Description                          |
|-----------|----------------------------------------|-----------|--------------------------------------|
| `pending` | Отклик отправлен                       | `waiting` | Response sent, awaiting a decision    |
| `pinned`  | Закреплён / Выбран приоритетным        | `success` | Executor pinned as the priority one   |

<!-- у pinned разные label в разных эндпоинтах: "Закреплён" в chat-links,
     "Выбран приоритетным" в /api/responses — рендерить по label из ответа -->

### Actions

Actions currently allowed on an entity, computed server-side from its status.
<!-- Кнопки на карточке приходят с бэка -->

| Field              | Type       | Nullable | Description                                          |
|--------------------|------------|----------|------------------------------------------------------|
| `primaryAction`    | `string`   | yes      | Main action, rendered as the accent button            |
| `secondaryActions` | `string[]` | no       | Extra actions, usually rendered in the overflow menu  |

#### Action codes

| Value      | Description                |
|------------|----------------------------|
| `pause`    | Pause the request           |
| `edit`     | Edit the request            |
| `complete` | Mark the request completed  |
| `cancel`   | Cancel the request          |

<!-- набор зависит от статуса — сейчас известен только вариант для active -->

### PaginationMeta

| Field         | Type      | Description                                  |
|---------------|-----------|----------------------------------------------|
| `totalCount`  | `number`  | Items matching the current filter             |
| `page`        | `number`  | Current page, 1-based                         |
| `pageSize`    | `number`  | Items per page. Default `20`                  |
| `totalPages`  | `number`  | Total number of pages                         |
| `hasNext`     | `boolean` | `true` if a next page exists                  |
| `hasPrevious` | `boolean` | `true` if a previous page exists              |

Not all endpoints use this shape. `/api/requests/history` returns the same data flattened into
the root object and names two fields differently — see the note there.
<!-- Пагинация не унифицирована -->

### CursorMeta

Cursor-based pagination. Used where the list is scrolled rather than paged.

| Field            | Type      | Nullable | Description                                              |
|------------------|-----------|----------|----------------------------------------------------------|
| `totalCount`     | `number`  | no       | Items matching the current filter                         |
| `pageSize`       | `number`  | no       | Items per page. Default `20`, max `50`                    |
| `nextCursor`     | `string`  | yes      | Pass as `cursor` to load the next page. `null` if last    |
| `previousCursor` | `string`  | yes      | Pass as `cursor` to load the previous page. `null` if first |
| `hasNext`        | `boolean` | no       | `true` if a next page exists                              |
| `hasPrevious`    | `boolean` | no       | `true` if a previous page exists                          |

---

## Categories

Request categories used to classify incoming requests. <!-- Категории заявок -->

### GET /api/request-categories/tree

**Request**

```http
GET {host}/api/request-categories/tree
```

No parameters.

**Response `200 OK`**

| Field       | Type            | Nullable | Description                                                        |
|-------------|-----------------|----------|--------------------------------------------------------------------|
| `id`        | `string (uuid)` | no       | Category identifier                                                 |
| `shortId`   | `number`        | no       | Short numeric identifier (human-readable / external code)           |
| `parentId`  | `string (uuid)` | yes      | Parent category id. `null` for root categories                      |
| `depth`     | `number`        | no       | Nesting level. `0` — root, `1` — child, etc.                        |
| `name`      | `string`        | no       | Display name                                                        |
| `slug`      | `string`        | no       | URL-safe transliterated name, unique                                |
| `isLeaf`    | `boolean`       | no       | `true` if the category has no children and can be selected          |
| `sortOrder` | `number`        | no       | Sort position within the same parent (ascending)                    |
| `tags`      | `Tag[]`         | yes      | Always `null` here — use `/tree/tags` to get tags                   |

**Example**

```json
[
  {
    "id": "38524686-b4e9-41ce-af59-3d525315aec0",
    "shortId": 1000,
    "parentId": null,
    "depth": 0,
    "name": "Строительство",
    "slug": "stroitelstvo",
    "isLeaf": false,
    "sortOrder": 0,
    "tags": null
  },
  {
    "id": "b9d44ba6-b1c8-47ac-b2cd-f179d7d17534",
    "shortId": 1001,
    "parentId": null,
    "depth": 0,
    "name": "Транспорт",
    "slug": "transport",
    "isLeaf": false,
    "sortOrder": 0,
    "tags": null
  },
  {
    "id": "2c2a79b7-074a-43b7-8a8e-7d3e97ba2dd1",
    "shortId": 1002,
    "parentId": null,
    "depth": 0,
    "name": "Клининг",
    "slug": "klining",
    "isLeaf": false,
    "sortOrder": 0,
    "tags": null
  },
  {
    "id": "2a3e0646-6d28-4221-8d7c-892186e6901b",
    "shortId": 1010,
    "parentId": "38524686-b4e9-41ce-af59-3d525315aec0",
    "depth": 1,
    "name": "Фундаменты",
    "slug": "fundamenty",
    "isLeaf": true,
    "sortOrder": 0,
    "tags": null
  },
  {
    "id": "1371891f-455c-48a7-8bfa-1f3b37af4fa3",
    "shortId": 1020,
    "parentId": "b9d44ba6-b1c8-47ac-b2cd-f179d7d17534",
    "depth": 1,
    "name": "Грузоперевозки",
    "slug": "gruzoperevozki",
    "isLeaf": true,
    "sortOrder": 0,
    "tags": null
  }
]
```

**Notes**

- The response is a flat array, not a nested tree — build the hierarchy via `parentId` / `depth`.
  <!-- Дерево плоское -->
- Only categories with `isLeaf: true` are selectable when creating a request.
  <!-- Выбирать можно только листья -->
- Items are ordered by `sortOrder` inside each parent; the flat array order is not guaranteed
  to match the visual tree order — sort on the client.

---

### GET /api/request-categories/tree/tags

**Request**

```http
GET {host}/api/request-categories/tree/tags
```

No parameters.

**Response `200 OK`**

Same shape as `/tree`, with `tags` populated.

| Field       | Type            | Nullable | Description                                                        |
|-------------|-----------------|----------|--------------------------------------------------------------------|
| `id`        | `string (uuid)` | no       | Category identifier                                                 |
| `shortId`   | `number`        | no       | Short numeric identifier (human-readable / external code)           |
| `parentId`  | `string (uuid)` | yes      | Parent category id. `null` for root categories                      |
| `depth`     | `number`        | no       | Nesting level. `0` — root, `1` — child, etc.                        |
| `name`      | `string`        | no       | Display name                                                        |
| `slug`      | `string`        | no       | URL-safe transliterated name, unique                                |
| `isLeaf`    | `boolean`       | no       | `true` if the category has no children and can be selected          |
| `sortOrder` | `number`        | no       | Sort position within the same parent (ascending)                    |
| `tags`      | `Tag[]`         | yes      | Tags attached to the category. `null` for non-leaf categories        |

**`Tag` object**

| Field  | Type            | Nullable | Description      |
|--------|-----------------|----------|------------------|
| `id`   | `string (uuid)` | no       | Tag identifier   |
| `name` | `string`        | no       | Tag display name |

**Example**

```json
[
  {
    "id": "38524686-b4e9-41ce-af59-3d525315aec0",
    "shortId": 1000,
    "parentId": null,
    "depth": 0,
    "name": "Строительство",
    "slug": "stroitelstvo",
    "isLeaf": false,
    "sortOrder": 0,
    "tags": null
  },
  {
    "id": "b9d44ba6-b1c8-47ac-b2cd-f179d7d17534",
    "shortId": 1001,
    "parentId": null,
    "depth": 0,
    "name": "Транспорт",
    "slug": "transport",
    "isLeaf": false,
    "sortOrder": 1,
    "tags": null
  },
  {
    "id": "2c2a79b7-074a-43b7-8a8e-7d3e97ba2dd1",
    "shortId": 1002,
    "parentId": null,
    "depth": 0,
    "name": "Клининг",
    "slug": "klining",
    "isLeaf": false,
    "sortOrder": 2,
    "tags": null
  },
  {
    "id": "2a3e0646-6d28-4221-8d7c-892186e6901b",
    "shortId": 1010,
    "parentId": "38524686-b4e9-41ce-af59-3d525315aec0",
    "depth": 1,
    "name": "Фундаменты",
    "slug": "fundamenty",
    "isLeaf": true,
    "sortOrder": 0,
    "tags": [
      { "id": "5358649a-c275-4205-aa25-3ebdc202b2a3", "name": "монолитный фундамент" },
      { "id": "861736a5-3519-4a2d-80bf-b8dd931cbfd0", "name": "свайный фундамент" },
      { "id": "efc0c036-ddf7-40b1-bd67-db7118409011", "name": "ленточный фундамент" }
    ]
  },
  {
    "id": "896dd893-c633-4937-9656-252e3ca0e7b8",
    "shortId": 1011,
    "parentId": "38524686-b4e9-41ce-af59-3d525315aec0",
    "depth": 1,
    "name": "Кровля",
    "slug": "krovlya",
    "isLeaf": true,
    "sortOrder": 1,
    "tags": [
      { "id": "4981690d-9b68-4cfa-be61-be8d8bf184f4", "name": "металлочерепица" },
      { "id": "962c8712-84c6-4e1e-9462-818313f6c928", "name": "мягкая кровля" },
      { "id": "87ca511a-51ff-403e-b2f0-d78e08bd1aff", "name": "андулин" },
      { "id": "67581fab-f0bb-48b4-9fe7-1e7b660c5a29", "name": "кровля" },
      { "id": "8633bc34-3581-478e-9290-bd71c8d12334", "name": "крыша" },
      { "id": "c6a8a2a3-06ff-4ee0-9789-76f8a940c12c", "name": "перекройка" }
    ]
  },
  {
    "id": "1371891f-455c-48a7-8bfa-1f3b37af4fa3",
    "shortId": 1020,
    "parentId": "b9d44ba6-b1c8-47ac-b2cd-f179d7d17534",
    "depth": 1,
    "name": "Грузоперевозки",
    "slug": "gruzoperevozki",
    "isLeaf": true,
    "sortOrder": 0,
    "tags": [
      { "id": "3441d8ac-9991-4290-a70e-2060c23652cb", "name": "газель" },
      { "id": "805c1e9c-9b34-48c7-94f2-be115f91d17d", "name": "фура" }
    ]
  },
  {
    "id": "42aaba04-2b01-44e2-98e3-1782dc18936e",
    "shortId": 1021,
    "parentId": "b9d44ba6-b1c8-47ac-b2cd-f179d7d17534",
    "depth": 1,
    "name": "Спецтехника",
    "slug": "spectehnika",
    "isLeaf": true,
    "sortOrder": 1,
    "tags": [
      { "id": "40f2eb2e-23cb-4718-a0e6-cfed7b1163cd", "name": "кран" },
      { "id": "a9afc10c-877d-47c6-9955-a4d988dd98a4", "name": "экскаватор" }
    ]
  },
  {
    "id": "2fae2da0-7d36-44ba-945e-5bdfe5634e0a",
    "shortId": 1030,
    "parentId": "2c2a79b7-074a-43b7-8a8e-7d3e97ba2dd1",
    "depth": 1,
    "name": "Уборка офисов",
    "slug": "uborka-ofisov",
    "isLeaf": true,
    "sortOrder": 0,
    "tags": [
      { "id": "9b888468-381d-4a99-8bdd-a9d7cd2f696b", "name": "ежедневная уборка" },
      { "id": "fab71ea1-7cee-48f4-b27c-e4725e626c5d", "name": "генеральная уборка" }
    ]
  }
]
```

**Notes**

- The response is a flat array, not a nested tree — build the hierarchy via `parentId` / `depth`.
  <!-- Дерево плоское -->
- Tags are present only on leaf categories (`isLeaf: true`); root categories return `tags: null`.
  <!-- Теги только у листьев -->
- Tag names are not guaranteed unique across categories — match by `id`.

### GET /api/request-categories/popular

Most requested categories, for the sidebar on the category tree page.
<!-- Сайд-блок «Популярные категории» -->

**Request**

```http
GET {host}/api/request-categories/popular
```

| Parameter | Type     | Required | Description                              |
|-----------|----------|----------|------------------------------------------|
| `limit`   | `number` | no       | How many categories to return             |

<!-- параметр предположен, в примере вернулись все непустые — подтвердить -->

**Response `200 OK`**

Flat array, ordered by `requestsCount` descending.

| Field           | Type            | Description                                        |
|-----------------|-----------------|----------------------------------------------------|
| `id`            | `string (uuid)` | Category identifier                                 |
| `shortId`       | `number`        | Short numeric identifier                            |
| `name`          | `string`        | Display name                                        |
| `slug`          | `string`        | URL-safe transliterated name, unique                |
| `requestsCount` | `number`        | Active requests in this category                    |

**Example**

```json
[
  {
    "id": "2a3e0646-6d28-4221-8d7c-892186e6901b",
    "shortId": 1010,
    "name": "Фундаменты",
    "slug": "fundamenty",
    "requestsCount": 2
  },
  {
    "id": "1371891f-455c-48a7-8bfa-1f3b37af4fa3",
    "shortId": 1020,
    "name": "Грузоперевозки",
    "slug": "gruzoperevozki",
    "requestsCount": 1
  }
]
```

**Notes**

- Only leaf categories appear here — the sample returns the same ids that carry `isLeaf: true`
  in the tree. <!-- Только листья -->
- Categories with no requests are omitted, so an empty array is a valid response.
- No `parentId` or `depth`: the item is a standalone link, not a tree node. Look the id up in
  [/tree](#get-apirequest-categoriestree) if the breadcrumb is needed.
- `requestsCount` is assumed to count active requests only — worth confirming whether closed
  ones are included. <!-- уточнить, что именно считается -->



---

## Dashboard

Aggregated data for the authenticated profile's home screen. <!-- Главная страница ЛК -->

### GET /api/dashboard

**Request**

```http
GET {host}/api/dashboard
```

No parameters. Data is resolved from the authenticated profile.

**Response `200 OK`**

| Field            | Type                | Nullable | Description                                                     |
|------------------|---------------------|----------|-----------------------------------------------------------------|
| `profileName`    | `string`            | no       | Display name of the current profile                              |
| `isExecutor`     | `boolean`           | no       | `true` if the profile can respond to requests                    |
| `date`           | `string (ISO 8601)` | no       | Server time the snapshot was generated (UTC)                     |
| `summary`        | `Summary`           | no       | Counters for the top badges                                      |
| `recentRequests` | `RecentRequest[]`   | no       | Latest requests created by the profile. Empty array if none      |
| `recentResponses`| `RecentResponse[]`  | no       | Latest responses submitted by the profile. Empty array if none   |
| `customerStats`  | `CustomerStats`     | no       | Stats as a customer. All-zero object if the profile never created requests |
| `executorStats`  | `ExecutorStats`     | yes      | Stats as an executor. `null` if the profile never responded      |
| `events`         | `Event[]`           | no       | Activity feed, newest first                                      |

**`Summary` object**

| Field              | Type     | Description                                        |
|--------------------|----------|----------------------------------------------------|
| `activeRequests`   | `number` | Requests currently in `Active` status               |
| `incomingResponses`| `number` | Responses received on the profile's own requests    |
| `unreadMessages`   | `number` | Unread chat messages across all chats               |
| `favouritesCount`  | `number` | Items saved to favourites                           |
| `myResponses`      | `number` | Responses submitted by the profile                  |

**`RecentRequest` object**

| Field                | Type            | Nullable | Description                                          |
|----------------------|-----------------|----------|------------------------------------------------------|
| `requestId`          | `string (uuid)` | no       | Request identifier                                    |
| `requestNumber`      | `string`        | no       | Human-readable number, format `yyyyMMdd-NNNNNN`       |
| `title`              | `string`        | no       | Request title                                         |
| `status`             | `StatusBadge`   | no       | Request status, see [Request status](#request-status-codes) |
| `regionName`         | `string`        | no       | Region the request belongs to                         |
| `totalResponses`     | `number`        | no       | Number of responses received                          |
| `pinnedChatsCount`   | `number`        | no       | Number of chats pinned on this request                |
| `previewMediaFileId` | `string (uuid)` | yes      | Cover image id. `null` if the request has no media    |

**`RecentResponse` object**

| Field           | Type                | Nullable | Description                                                  |
|-----------------|---------------------|----------|--------------------------------------------------------------|
| `chatRoomId`    | `string (uuid)`     | no       | Chat room opened for this response — used as the item key     |
| `status`        | `StatusBadge`       | no       | Response status, see [Response status](#response-status-codes)      |
| `requestTitle`  | `string`            | no       | Title of the request responded to                             |
| `ownerName`     | `string`            | no       | Name of the customer who owns the request                     |
| `regionName`    | `string`            | no       | Region the request belongs to                                 |
| `lastMessageAt` | `string (ISO 8601)` | no       | Timestamp of the last message in the chat (UTC)               |
| `unreadCount`   | `number`            | no       | Unread messages in this chat                                  |

**`StatusBadge` object** — see [Common types → StatusBadge](#statusbadge).
Codes: [Request status](#request-status-codes), [Response status](#response-status-codes).

**`CustomerStats` object**

| Field            | Type     | Description                                  |
|------------------|----------|----------------------------------------------|
| `totalCreated`   | `number` | Total requests created                        |
| `inProgress`     | `number` | Requests currently in progress                |
| `totalResponses` | `number` | Total responses received across all requests  |
| `completed`      | `number` | Requests completed                            |
| `cancelled`      | `number` | Requests cancelled                            |

**`ExecutorStats` object**

| Field               | Type     | Description                                              |
|---------------------|----------|----------------------------------------------------------|
| `totalResponses`    | `number` | Total responses submitted, including withdrawn ones       |
| `activeChats`       | `number` | Chats currently active                                    |
| `archived`          | `number` | Chats moved to archive                                    |
| `pinned`            | `number` | Chats where the executor was pinned as priority           |
| `conversionPercent` | `number` | Share of responses that converted to a deal, in percent   |

**`Event` object**

| Field             | Type                | Nullable | Description                                              |
|-------------------|---------------------|----------|----------------------------------------------------------|
| `type`            | `string`            | no       | Event type, see [Event types](#event-types)               |
| `message`         | `string`            | no       | Ready-to-render text, already localised (ru)              |
| `relatedEntityId` | `string (uuid)`     | no       | Id of the entity the event refers to (request id)         |
| `entityNumber`    | `string`            | no       | Human-readable number of that entity                      |
| `actorName`       | `string`            | yes      | Who triggered the event. `null` for system events         |
| `createdAt`       | `string (ISO 8601)` | no       | Event timestamp (UTC)                                     |

#### Event types

| Value                | Description                                        | `actorName` |
|----------------------|----------------------------------------------------|-------------|
| `new_response`       | Someone responded to the profile's request         | executor    |
| `response_withdrawn` | An executor withdrew their response                | executor    |
| `request_completed`  | The request was marked completed                   | `null`      |
| `my_response`        | The profile responded to someone else's request    | `null`      |
| `chat_pinned`        | The profile was pinned as the priority executor    | `null`      |
| `chat_unpinned`      | The profile was unpinned                           | `null`      |

<!-- new_response / response_withdrawn / request_completed — со стороны заказчика,
     my_response / chat_pinned / chat_unpinned — со стороны исполнителя -->

**Example — customer side** (`recentResponses` empty, `executorStats: null`)

```json
{
  "profileName": "ООО \"ОРВЕЛЛии\"",
  "isExecutor": true,
  "date": "2026-07-31T06:32:15.0866027Z",
  "summary": {
    "activeRequests": 3,
    "incomingResponses": 4,
    "unreadMessages": 10,
    "favouritesCount": 0,
    "myResponses": 0
  },
  "recentRequests": [
    {
      "requestId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
      "requestNumber": "20260730-001029",
      "title": "заявка 5",
      "status": {
        "code": "active",
        "label": "Активна",
        "theme": "live"
      },
      "regionName": "Саратов",
      "totalResponses": 1,
      "pinnedChatsCount": 0,
      "previewMediaFileId": null
    },
    {
      "requestId": "9599dfe0-746a-4451-b15c-0c13ac923796",
      "requestNumber": "20260715-001026",
      "title": "заявка 2",
      "status": {
        "code": "active",
        "label": "Активна",
        "theme": "live"
      },
      "regionName": "Саратов",
      "totalResponses": 1,
      "pinnedChatsCount": 0,
      "previewMediaFileId": null
    }
  ],
  "recentResponses": [],
  "customerStats": {
    "totalCreated": 6,
    "inProgress": 0,
    "totalResponses": 6,
    "completed": 3,
    "cancelled": 0
  },
  "executorStats": null,
  "events": [
    {
      "type": "new_response",
      "message": "Новый отклик на #20260730-001029 — ООО \"ОТКЛИК\"",
      "relatedEntityId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
      "entityNumber": "20260730-001029",
      "actorName": "ООО \"ОТКЛИК\"",
      "createdAt": "2026-07-30T16:01:42.422124Z"
    },
    {
      "type": "request_completed",
      "message": "Заявка #20260730-001028 завершена",
      "relatedEntityId": "d7e2bbc2-2c9a-482c-b2b7-448d7fe5f636",
      "entityNumber": "20260730-001028",
      "actorName": null,
      "createdAt": "2026-07-30T15:53:59.744251Z"
    },
    {
      "type": "response_withdrawn",
      "message": "Отклик отозван по #20260714-001025 — ООО \"ОТКЛИК\"",
      "relatedEntityId": "5dd292dd-a6d6-47b9-9985-63ff4e67f5f4",
      "entityNumber": "20260714-001025",
      "actorName": "ООО \"ОТКЛИК\"",
      "createdAt": "2026-07-14T19:39:52.881882Z"
    }
  ]
}
```

**Example — executor side** (`recentRequests` empty, `executorStats` filled)

```json
{
  "profileName": "ООО \"ОТКЛИК\"",
  "isExecutor": true,
  "date": "2026-07-31T05:36:21.4220776Z",
  "summary": {
    "activeRequests": 0,
    "incomingResponses": 0,
    "unreadMessages": 13,
    "favouritesCount": 0,
    "myResponses": 4
  },
  "recentRequests": [],
  "recentResponses": [
    {
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "status": {
        "code": "pending",
        "label": "Отклик отправлен",
        "theme": "waiting"
      },
      "requestTitle": "заявка 5",
      "ownerName": "ООО \"ОРВЕЛЛии\"",
      "regionName": "Саратов",
      "lastMessageAt": "2026-07-30T16:01:42.456656Z",
      "unreadCount": 0
    },
    {
      "chatRoomId": "d76f24bb-b5d7-4f82-82ca-ff32a14f517c",
      "status": {
        "code": "pending",
        "label": "Отклик отправлен",
        "theme": "waiting"
      },
      "requestTitle": "заявка 4",
      "ownerName": "ООО \"ОРВЕЛЛии\"",
      "regionName": "Саратов",
      "lastMessageAt": "2026-07-30T15:53:59.762804Z",
      "unreadCount": 1
    }
  ],
  "customerStats": {
    "totalCreated": 0,
    "inProgress": 0,
    "totalResponses": 0,
    "completed": 0,
    "cancelled": 0
  },
  "executorStats": {
    "totalResponses": 6,
    "activeChats": 4,
    "archived": 1,
    "pinned": 0,
    "conversionPercent": 0
  },
  "events": [
    {
      "type": "my_response",
      "message": "Вы откликнулись на заявку #20260730-001029",
      "relatedEntityId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
      "entityNumber": "20260730-001029",
      "actorName": null,
      "createdAt": "2026-07-30T16:01:42.422124Z"
    },
    {
      "type": "chat_pinned",
      "message": "Вы выбраны приоритетным по #20260714-001024",
      "relatedEntityId": "0267cc94-9196-438f-9c6e-bf730b3c7c23",
      "entityNumber": "20260714-001024",
      "actorName": null,
      "createdAt": "2026-07-14T12:13:58.206122Z"
    },
    {
      "type": "chat_unpinned",
      "message": "Вы откреплены по #20260714-001024",
      "relatedEntityId": "0267cc94-9196-438f-9c6e-bf730b3c7c23",
      "entityNumber": "20260714-001024",
      "actorName": null,
      "createdAt": "2026-07-14T12:14:55.807774Z"
    }
  ]
}
```

**Notes**

- The endpoint returns one and the same schema for both roles; which blocks are filled depends
  on what the profile actually did. A profile can be customer and executor at the same time.
  <!-- Схема одна, различается только заполнение блоков -->
- `events` is sorted by `createdAt` descending. <!-- Лента событий: новые сверху -->
- `message` is already formatted for display and embeds `entityNumber` and `actorName`;
  use the raw fields if the client renders its own text.
- For executor-side events `actorName` is `null` — the actor is the profile itself.
- `relatedEntityId` always points to the request, including in `chat_pinned` / `chat_unpinned`.
- `previewMediaFileId` is only an id — build the file URL on the client.
- `recentResponses` items are keyed by `chatRoomId`, not by a response id.
- `executorStats.totalResponses` counts all responses ever sent, including withdrawn ones,
  so it can exceed `summary.myResponses`. <!-- 6 против 4 в примере -->
- Examples above are truncated; the real responses returned 10 events each.

---

## Requests

Managing requests created by the current profile. <!-- Управление заявками -->

### GET /api/requests/my

**Request**

```http
GET {host}/api/requests/my
```

| Parameter  | Type     | Required | Description                                                  |
|------------|----------|----------|--------------------------------------------------------------|
| `tab`      | `string` | no       | Tab filter, one of the [tab keys](#tab-keys). Default `all`   |
| `page`     | `number` | no       | Page number, 1-based. Default `1`                             |
| `pageSize` | `number` | no       | Items per page. Default `20`                                  |

<!-- параметры выведены из meta/tabCount — подтвердить реальные имена -->

**Response `200 OK`**

| Field      | Type                | Description                                            |
|------------|---------------------|--------------------------------------------------------|
| `items`    | `RequestListItem[]` | Requests on the current page                            |
| `stats`    | `Stats`             | Aggregated counters over all the profile's requests     |
| `tabCount` | `TabCount`          | Badge counters for the tab bar                          |
| `meta`     | `PaginationMeta`    | Pagination, see [Common types](#paginationmeta)         |

**`RequestListItem` object**

| Field                    | Type                | Nullable | Description                                                   |
|--------------------------|---------------------|----------|---------------------------------------------------------------|
| `requestId`              | `string (uuid)`     | no       | Request identifier                                             |
| `requestNumber`          | `string`            | no       | Human-readable number, format `yyyyMMdd-NNNNNN`                |
| `title`                  | `string`            | no       | Request title                                                  |
| `description`            | `string`            | no       | Request description                                            |
| `previewMediaFileId`     | `string (uuid)`     | yes      | Cover image id. `null` if the request has no media             |
| `status`                 | `StatusBadge`       | no       | Request status, see [Request status](#request-status-codes)    |
| `pinnedChatsCount`       | `number`            | no       | Chats pinned as priority on this request                       |
| `totalChats`             | `number`            | no       | Total chats opened on this request                             |
| `newChats`               | `number`            | no       | Chats not yet opened by the customer                           |
| `viewsCount`             | `number`            | no       | How many times the request was viewed                          |
| `favouritesCount`        | `number`            | no       | How many profiles added it to favourites                       |
| `expireAt`               | `string (ISO 8601)` | no       | When the request expires (UTC)                                 |
| `createdAt`              | `string (ISO 8601)` | no       | Creation timestamp (UTC)                                       |
| `completedAt`            | `string (ISO 8601)` | yes      | Completion timestamp. `null` while not completed               |
| `cancelledAt`            | `string (ISO 8601)` | yes      | Cancellation timestamp. `null` while not cancelled             |
| `moderationRejectReason` | `string`            | yes      | Reason the request was rejected by moderation. `null` if none  |
| `tags`                   | `string[]`          | no       | Tag names to render on the card — names only, no ids           |
| `extraTagCount`          | `number`            | no       | Tags not included in `tags`, render as `+N`                    |
| `actions`                | `Actions`           | no       | Allowed actions, see [Common types](#actions)                  |

**`Stats` object**

| Field        | Type     | Description                                        |
|--------------|----------|----------------------------------------------------|
| `total`      | `number` | All requests ever created by the profile            |
| `active`     | `number` | Requests open for responses                         |
| `inProgress` | `number` | Requests currently in progress                      |
| `expired`    | `number` | Requests past `expireAt`                            |

**`TabCount` object**

Counter per tab in the UI. <!-- Счётчики для табов -->

#### Tab keys

| Key          | Description                         |
|--------------|-------------------------------------|
| `all`        | Everything except drafts             |
| `active`     | Open for responses                   |
| `inProgress` | In progress                          |
| `paused`     | Paused by the customer               |
| `moderating` | Awaiting moderation                  |
| `drafts`     | Not published yet                    |
| `expired`    | Past `expireAt`                      |

**Example**

```json
{
  "items": [
    {
      "requestId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
      "requestNumber": "20260730-001029",
      "title": "заявка 5",
      "description": "тестовое описание. проверка",
      "previewMediaFileId": null,
      "status": {
        "code": "active",
        "label": "Активна",
        "theme": "live"
      },
      "pinnedChatsCount": 0,
      "totalChats": 1,
      "newChats": 1,
      "viewsCount": 0,
      "favouritesCount": 0,
      "expireAt": "2026-08-29T15:58:06.533707Z",
      "createdAt": "2026-07-30T15:57:55.349036Z",
      "completedAt": null,
      "cancelledAt": null,
      "moderationRejectReason": null,
      "tags": [
        "монолитный фундамент"
      ],
      "extraTagCount": 0,
      "actions": {
        "primaryAction": "pause",
        "secondaryActions": [
          "edit",
          "complete",
          "cancel"
        ]
      }
    },
    {
      "requestId": "5dd292dd-a6d6-47b9-9985-63ff4e67f5f4",
      "requestNumber": "20260714-001025",
      "title": "заявка 3",
      "description": "тестовое описание. проверка",
      "previewMediaFileId": null,
      "status": {
        "code": "active",
        "label": "Активна",
        "theme": "live"
      },
      "pinnedChatsCount": 0,
      "totalChats": 1,
      "newChats": 0,
      "viewsCount": 0,
      "favouritesCount": 0,
      "expireAt": "2026-08-13T19:35:44.792536Z",
      "createdAt": "2026-07-14T19:35:24.451112Z",
      "completedAt": null,
      "cancelledAt": null,
      "moderationRejectReason": null,
      "tags": [
        "монолитный фундамент"
      ],
      "extraTagCount": 0,
      "actions": {
        "primaryAction": "pause",
        "secondaryActions": [
          "edit",
          "complete",
          "cancel"
        ]
      }
    }
  ],
  "stats": {
    "total": 6,
    "active": 3,
    "inProgress": 0,
    "expired": 0
  },
  "tabCount": {
    "all": 3,
    "active": 3,
    "inProgress": 0,
    "paused": 0,
    "moderating": 0,
    "drafts": 1,
    "expired": 0
  },
  "meta": {
    "totalCount": 3,
    "page": 1,
    "pageSize": 20,
    "totalPages": 1,
    "hasNext": false,
    "hasPrevious": false
  }
}
```

**Notes**

- `tags` here are plain strings, unlike `/request-categories/tree/tags` where tags are objects
  with an `id`. <!-- на карточке только имена -->
- `tags` is already trimmed for the card; `extraTagCount` holds the remainder.
- `stats.total` counts every request ever created, while `tabCount.all` counts only what the
  current tab set shows — in the example `6` vs `3`. <!-- разница: завершённые и отменённые -->
- `drafts` are excluded from `all`.
- `actions` is computed server-side from the status — render buttons from it instead of
  deriving them on the client.
- The example above is truncated to 2 items of 3.

---

### GET /api/requests/{requestId}/details

Customer-side view of a single request. <!-- Детали заявки для владельца -->

**Request**

```http
GET {host}/api/requests/{requestId}/details
```

| Parameter   | In   | Type            | Required | Description        |
|-------------|------|-----------------|----------|--------------------|
| `requestId` | path | `string (uuid)` | yes      | Request identifier |

**Response `200 OK`**

| Field                    | Type                | Nullable | Description                                                   |
|--------------------------|---------------------|----------|---------------------------------------------------------------|
| `requestId`              | `string (uuid)`     | no       | Request identifier                                             |
| `requestNumber`          | `string`            | no       | Human-readable number, format `yyyyMMdd-NNNNNN`                |
| `title`                  | `string`            | no       | Request title                                                  |
| `description`            | `string`            | no       | Request description                                            |
| `status`                 | `StatusBadge`       | no       | Request status, see [Request status](#request-status-codes)    |
| `version`                | `number`            | no       | Revision counter, incremented on every edit                    |
| `categories`             | `string[]`          | no       | Category names the request belongs to — names only, no ids     |
| `regionId`               | `string (uuid)`     | no       | Region identifier                                              |
| `regionName`             | `string`            | no       | Region display name                                            |
| `moderationRejectReason` | `string`            | yes      | Reason the request was rejected by moderation. `null` if none  |
| `createdAt`              | `string (ISO 8601)` | no       | Creation timestamp (UTC)                                       |
| `expireAt`               | `string (ISO 8601)` | no       | When the request expires (UTC)                                 |
| `completedAt`            | `string (ISO 8601)` | yes      | Completion timestamp. `null` while not completed               |
| `cancelledAt`            | `string (ISO 8601)` | yes      | Cancellation timestamp. `null` while not cancelled             |
| `pinnedChatsCount`       | `number`            | no       | Chats currently pinned as priority                             |
| `maxPinnedChats`         | `number`            | no       | Pin limit for this request                                     |
| `totalChats`             | `number`            | no       | Total chats opened on this request                             |
| `newChats`               | `number`            | no       | Chats not yet opened by the customer                           |
| `archivedChats`          | `number`            | no       | Chats moved to archive                                         |
| `viewedCount`            | `number`            | no       | How many times the request was viewed                          |
| `favouritesCount`        | `number`            | no       | How many profiles added it to favourites                       |
| `previewMediaFileId`     | `string (uuid)`     | yes      | Cover image id. `null` if the request has no media             |
| `attachmentIds`          | `string (uuid)[]`   | no       | Attached media file ids. Empty array if none                   |
| `tags`                   | `RequestTag[]`      | no       | Tags attached to the request                                   |
| `actions`                | `Actions`           | no       | Allowed actions, see [Common types](#actions)                  |

**`RequestTag` object**

| Field     | Type            | Description      |
|-----------|-----------------|------------------|
| `tagId`   | `string (uuid)` | Tag identifier   |
| `tagName` | `string`        | Tag display name |

**Example**

```json
{
  "requestId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
  "requestNumber": "20260730-001029",
  "title": "заявка 5",
  "description": "тестовое описание. проверка",
  "status": {
    "code": "active",
    "label": "Активна",
    "theme": "live"
  },
  "version": 1,
  "categories": [
    "Фундаменты"
  ],
  "regionId": "6ad5d88c-3cc3-473e-a6ec-fd2042eccc3d",
  "regionName": "Саратов",
  "moderationRejectReason": null,
  "createdAt": "2026-07-30T15:57:55.349036Z",
  "expireAt": "2026-08-29T15:58:06.533707Z",
  "completedAt": null,
  "cancelledAt": null,
  "pinnedChatsCount": 0,
  "maxPinnedChats": 7,
  "totalChats": 1,
  "newChats": 1,
  "archivedChats": 0,
  "viewedCount": 0,
  "favouritesCount": 0,
  "previewMediaFileId": null,
  "attachmentIds": [],
  "tags": [
    {
      "tagId": "5358649a-c275-4205-aa25-3ebdc202b2a3",
      "tagName": "монолитный фундамент"
    }
  ],
  "actions": {
    "primaryAction": "pause",
    "secondaryActions": [
      "edit",
      "complete",
      "cancel"
    ]
  }
}
```

**Notes**

- Available to the request owner only. <!-- Витрина для заказчика -->
- Tag shape differs from other endpoints: here it is `tagId` / `tagName`, in
  `/request-categories/tree/tags` it is `id` / `name`, and in `/requests/my` tags are plain
  strings. <!-- три разных представления тега -->
- `categories` are names without ids — the client cannot link back to a category.
- The view counter is `viewedCount` here and `viewsCount` in `/requests/my`.
  <!-- расхождение в именовании -->
- `pinnedChatsCount` against `maxPinnedChats` gives the remaining pin slots.
- No `tags` trimming here — `extraTagCount` exists only in the list endpoint.

---

### GET /api/requests/{requestId}/change-log

Edit history of a request, shown on the details page. <!-- История изменений заявки -->

**Request**

```http
GET {host}/api/requests/{requestId}/change-log
```

| Parameter   | In   | Type            | Required | Description        |
|-------------|------|-----------------|----------|--------------------|
| `requestId` | path | `string (uuid)` | yes      | Request identifier |

**Response `200 OK`**

| Field    | Type              | Description                             |
|----------|-------------------|-----------------------------------------|
| `events` | `ChangeEvent[]`   | Change entries. Empty array if never edited |

**`ChangeEvent` object**

All payload fields are always present; only the one matching `type` is filled, the rest are `null`.
<!-- Плоский DTO: заполнено только поле, соответствующее type -->

| Field        | Type                | Nullable | Description                                             |
|--------------|---------------------|----------|---------------------------------------------------------|
| `type`       | `string`            | no       | What was changed, see [Change types](#change-types)      |
| `changedAt`  | `string (ISO 8601)` | no       | When the change was applied (UTC)                        |
| `oldText`    | `string`            | yes      | Previous value, for text-like changes                    |
| `newText`    | `string`            | yes      | New value, for text-like changes                         |
| `tags`       | `ChangedItem[]`     | yes      | Tag diff, filled when `type` is `tags`                   |
| `categories` | `ChangedItem[]`     | yes      | Category diff, filled when `type` is `categories`        |
| `file`       | `ChangedFile`       | yes      | File info, filled when `type` is file-related            |

#### Change types

| `type`        | Filled payload      | Description                          |
|---------------|---------------------|--------------------------------------|
| `title`       | `oldText`/`newText` | Title changed                         |
| `description` | —                   | Description changed, text is not logged |
| `region`      | `oldText`/`newText` | Region changed                        |
| `tags`        | `tags`              | Tags added and/or removed             |
| `categories`  | `categories`        | Categories added and/or removed       |
| `file`        | `file`              | Attachment added or removed           |

<!-- код для файлов уточнить: file / attachment / media -->

**`ChangedItem` object**

One entry per added or removed item — the diff, not the resulting set.
<!-- Диф, а не итоговый список -->

| Field     | Type      | Description                                   |
|-----------|-----------|-----------------------------------------------|
| `name`    | `string`  | Item display name                              |
| `isAdded` | `boolean` | `true` — added, `false` — removed              |

**`ChangedFile` object**

Maps to `ChangedFileDto(string FileName, long FileSize)`.

| Field      | Type     | Description                   |
|------------|----------|-------------------------------|
| `fileName` | `string` | File name with extension       |
| `fileSize` | `number` | File size in bytes (`long`)    |

**Example**

```json
{
  "events": [
    {
      "type": "title",
      "changedAt": "2026-07-31T08:19:41.058986Z",
      "oldText": "заявка 5",
      "newText": "заявка 1.8. заявка тесть тест. Изменение",
      "tags": null,
      "categories": null,
      "file": null
    },
    {
      "type": "region",
      "changedAt": "2026-07-31T08:19:41.058986Z",
      "oldText": "Саратов",
      "newText": "Владивосток",
      "tags": null,
      "categories": null,
      "file": null
    },
    {
      "type": "tags",
      "changedAt": "2026-07-31T08:19:41.058986Z",
      "oldText": null,
      "newText": null,
      "tags": [
        {
          "name": "монолитный фундамент",
          "isAdded": false
        },
        {
          "name": "фура",
          "isAdded": true
        }
      ],
      "categories": null,
      "file": null
    },
    {
      "type": "categories",
      "changedAt": "2026-07-31T08:19:41.058986Z",
      "oldText": null,
      "newText": null,
      "tags": null,
      "categories": [
        {
          "name": "Фундаменты",
          "isAdded": false
        },
        {
          "name": "Грузоперевозки",
          "isAdded": true
        }
      ],
      "file": null
    },
    {
      "type": "file",
      "changedAt": "2026-07-31T08:19:41.058986Z",
      "oldText": null,
      "newText": null,
      "tags": null,
      "categories": null,
      "file": {
        "fileName": "smeta.pdf",
        "fileSize": 284913
      }
    }
  ]
}
```

<!-- событие file собрано вручную по ChangedFileDto, реального ответа не было -->

**Notes**

- Events from a single edit share the same `changedAt` — group by it to render one revision block.
  <!-- Одна правка = несколько событий с одинаковым changedAt -->
- Sort order is not guaranteed by the API — sort by `changedAt` on the client.
- `type: "description"` always comes with `oldText` and `newText` as `null` — the description text
  is not logged, the event only states that it changed.
  <!-- Описание не логируется по тексту, только сам факт -->
- `ChangedItem` carries names only, no ids.
- The file event above is illustrative — built from the DTO, not from a real response.

---

### GET /api/requests/{requestId}/views/stats

Daily view counts for the histogram on the details page. <!-- Гистограмма просмотров -->

**Request**

```http
GET {host}/api/requests/{requestId}/views/stats?days=14
```

| Parameter   | In    | Type            | Required | Description                       |
|-------------|-------|-----------------|----------|-----------------------------------|
| `requestId` | path  | `string (uuid)` | yes      | Request identifier                 |
| `days`      | query | `number`        | no       | Size of the window in days          |

**Response `200 OK`**

Array of daily buckets, one per day, oldest first.

| Field   | Type                | Description                                  |
|---------|---------------------|----------------------------------------------|
| `date`  | `string (ISO 8601)` | Day the bucket covers, midnight UTC           |
| `count` | `number`            | Views on that day. `0` if there were none     |

**Example**

```json
[
  { "date": "2026-07-17T00:00:00Z", "count": 0 },
  { "date": "2026-07-18T00:00:00Z", "count": 0 },
  { "date": "2026-07-19T00:00:00Z", "count": 0 },
  { "date": "2026-07-20T00:00:00Z", "count": 0 },
  { "date": "2026-07-21T00:00:00Z", "count": 0 },
  { "date": "2026-07-22T00:00:00Z", "count": 0 },
  { "date": "2026-07-23T00:00:00Z", "count": 0 },
  { "date": "2026-07-24T00:00:00Z", "count": 0 },
  { "date": "2026-07-25T00:00:00Z", "count": 0 },
  { "date": "2026-07-26T00:00:00Z", "count": 0 },
  { "date": "2026-07-27T00:00:00Z", "count": 0 },
  { "date": "2026-07-28T00:00:00Z", "count": 0 },
  { "date": "2026-07-29T00:00:00Z", "count": 0 },
  { "date": "2026-07-30T00:00:00Z", "count": 0 }
]
```

**Notes**

- Days with no views are returned with `count: 0`, so the series has no gaps — the client can
  render it as is. <!-- Пустые дни уже заполнены нулями -->
- Buckets are keyed by UTC midnight; a client in another timezone will shift the bars.
- The window ends on the previous day: `days=14` on 2026-07-31 returned 07-17 … 07-30.
  <!-- текущий день в выборку не попадает -->
- The array length equals `days`; default and maximum values are not confirmed yet.

---

---

### GET /api/requests/history

Closed requests of the current profile. <!-- Страница истории заказчика -->

**Request**

```http
GET {host}/api/requests/history
```

| Parameter  | Type     | Required | Description                                                        |
|------------|----------|----------|--------------------------------------------------------------------|
| `year`     | `number` | no       | Filter by year, values from [filters](#get-apirequestshistoryfilters) |
| `status`   | `string` | no       | Filter by status code, values from [filters](#get-apirequestshistoryfilters) |
| `page`     | `number` | no       | Page number, 1-based. Default `1`                                   |
| `pageSize` | `number` | no       | Items per page. Default `20`                                        |

<!-- параметры выведены из ответа и из /history/filters — подтвердить имена -->

**Response `200 OK`**

Pagination is flattened into the root object here, it is not wrapped in `meta`.

| Field             | Type                   | Description                                   |
|-------------------|------------------------|-----------------------------------------------|
| `items`           | `HistoryRequestItem[]` | Closed requests on the current page             |
| `totalCount`      | `number`               | Items matching the filter                       |
| `page`            | `number`               | Current page, 1-based                           |
| `pageSize`        | `number`               | Items per page                                  |
| `totalPages`      | `number`               | Total number of pages                           |
| `hasNextPage`     | `boolean`              | `true` if a next page exists                    |
| `hasPreviousPage` | `boolean`              | `true` if a previous page exists                |

**`HistoryRequestItem` object**

| Field                    | Type                | Nullable | Description                                                 |
|--------------------------|---------------------|----------|-------------------------------------------------------------|
| `requestId`              | `string (uuid)`     | no       | Request identifier                                           |
| `requestNumber`          | `string`            | no       | Human-readable number, format `yyyyMMdd-NNNNNN`              |
| `title`                  | `string`            | no       | Request title                                                |
| `previewMediaFileId`     | `string (uuid)`     | yes      | Cover image id. `null` if the request has no media           |
| `status`                 | `StatusBadge`       | no       | Final status, see [Request status](#request-status-codes)    |
| `closedAt`               | `string (ISO 8601)` | no       | When the request was closed (UTC)                            |
| `regionName`             | `string`            | no       | Region the request belongs to                                |
| `totalChats`             | `number`            | no       | Chats opened on this request                                 |
| `viewsCount`             | `number`            | no       | How many times the request was viewed                        |
| `favouriteCount`         | `number`            | no       | How many profiles added it to favourites                     |
| `participants`           | `Participant[]`     | no       | Executors involved, trimmed for the card                     |
| `extraParticipantsCount` | `number`            | no       | Participants not included in `participants`, render as `+N`  |
| `actions`                | `Actions`           | no       | Allowed actions, see [Common types](#actions)                |

**`Participant` object**

| Field         | Type            | Description                |
|---------------|-----------------|----------------------------|
| `profileId`   | `string (uuid)` | Executor profile identifier |
| `displayName` | `string`        | Executor display name       |

**Example**

```json
{
  "items": [
    {
      "requestId": "d7e2bbc2-2c9a-482c-b2b7-448d7fe5f636",
      "requestNumber": "20260730-001028",
      "title": "заявка 4",
      "previewMediaFileId": null,
      "status": {
        "code": "completed",
        "label": "Завершена",
        "theme": "success"
      },
      "closedAt": "2026-07-30T15:53:59.711402Z",
      "regionName": "Саратов",
      "totalChats": 1,
      "viewsCount": 0,
      "favouriteCount": 0,
      "participants": [
        {
          "profileId": "a8dd4416-1296-42bc-bea9-72bd9d96c64c",
          "displayName": "ООО \"ОТКЛИК\""
        }
      ],
      "extraParticipantsCount": 0,
      "actions": {
        "primaryAction": null,
        "secondaryActions": []
      }
    },
    {
      "requestId": "0267cc94-9196-438f-9c6e-bf730b3c7c23",
      "requestNumber": "20260714-001024",
      "title": "заявка 1",
      "previewMediaFileId": null,
      "status": {
        "code": "completed",
        "label": "Завершена",
        "theme": "success"
      },
      "closedAt": "2026-07-14T15:38:07.68891Z",
      "regionName": "Саратов",
      "totalChats": 1,
      "viewsCount": 0,
      "favouriteCount": 0,
      "participants": [
        {
          "profileId": "a8dd4416-1296-42bc-bea9-72bd9d96c64c",
          "displayName": "ООО \"ОТКЛИК\""
        }
      ],
      "extraParticipantsCount": 0,
      "actions": {
        "primaryAction": null,
        "secondaryActions": []
      }
    }
  ],
  "totalCount": 3,
  "page": 1,
  "pageSize": 20,
  "totalPages": 1,
  "hasNextPage": false,
  "hasPreviousPage": false
}
```

**Notes**

- Closed requests carry no actions: `primaryAction` is `null` and `secondaryActions` is empty.
  <!-- Действий нет, но объект приходит -->
- Pagination differs from `/api/requests/my`: it is flat instead of nested in `meta`, and the
  flags are `hasNextPage` / `hasPreviousPage` instead of `hasNext` / `hasPrevious`.
  <!-- расхождение в пагинации -->
- The favourites counter is `favouriteCount` here and `favouritesCount` elsewhere.
  <!-- расхождение в именовании -->
- `closedAt` is the single closing timestamp; the details endpoint splits it into `completedAt`
  and `cancelledAt`.
- The example above is truncated to 2 items of 3.
---

### GET /api/requests/history/filters

Values available in the history filters, with counters. <!-- Фильтры страницы истории -->

**Request**

```http
GET {host}/api/requests/history/filters
```

No parameters.

**Response `200 OK`**

| Field      | Type             | Description                                    |
|------------|------------------|------------------------------------------------|
| `years`    | `YearFilter[]`   | Years that have closed requests, with counters  |
| `statuses` | `StatusFilter[]` | Final statuses present in the history           |

**`YearFilter` object**

| Field   | Type     | Description                     |
|---------|----------|---------------------------------|
| `year`  | `number` | Calendar year                    |
| `count` | `number` | Closed requests in that year     |

**`StatusFilter` object**

| Field   | Type     | Description                                                   |
|---------|----------|---------------------------------------------------------------|
| `code`  | `string` | Status code, matches [Request status](#request-status-codes)   |
| `label` | `string` | Filter caption, localised (ru)                                 |
| `count` | `number` | Closed requests with that status                               |

**Example**

```json
{
  "years": [
    {
      "year": 2026,
      "count": 3
    }
  ],
  "statuses": [
    {
      "code": "completed",
      "label": "Завершённые",
      "count": 3
    }
  ]
}
```

**Notes**

- Only values that actually occur are returned — an empty history yields empty arrays, and the
  client should not render a hardcoded filter list. <!-- Пустые значения не приходят -->
- `label` is the filter caption in plural and differs from the badge label of the same code:
  `Завершённые` here against `Завершена` in `StatusBadge`. <!-- разные подписи одного кода -->
- `StatusFilter` has no `theme` — it is not a [StatusBadge](#statusbadge).
- Counters ignore the currently applied filters; they always describe the whole history.
  <!-- уточнить: не пересчитываются при выборе года? -->

---

## Responses

Responses to a request. One response opens one chat, so a chat and a response are the same
entity here. A *linked* chat means the executor was pinned as the priority one.
<!-- Чат = отклик. Залинкован = исполнитель выбран приоритетным -->

### GET /api/responses

Responses submitted by the current profile. <!-- Страница управления откликами -->

**Request**

```http
GET {host}/api/responses?tab=all&pageSize=20&sortBy=Newest
```

| Parameter     | Type     | Required | Description                                                          |
|---------------|----------|----------|----------------------------------------------------------------------|
| `pageSize`    | `number` | no       | Items per page. Default `20`, allowed range `1…50`                    |
| `cursor`      | `string` | no       | Cursor from `meta.nextCursor` / `meta.previousCursor`. Max 500 chars  |
| `tab`         | `string` | no       | Tab filter, one of the [tab keys](#response-tab-keys)                 |
| `searchTerms` | `string` | no       | Free-text search. Max 100 chars                                       |
| `sortBy`      | `string` | no       | Sort order, see [Sort values](#sort-values). Default `Newest`         |

#### Sort values

| Value    | Description                |
|----------|----------------------------|
| `Newest` | Newest responses first      |

<!-- enum ResponseSortBy — дополнить остальными значениями -->

**Response `200 OK`**

| Field      | Type             | Description                                          |
|------------|------------------|------------------------------------------------------|
| `items`    | `ResponseItem[]` | Responses on the current page                         |
| `stats`    | `ResponseStats`  | Aggregated counters over all the profile's responses  |
| `tabCount` | `ResponseTabCount` | Badge counters for the tab bar                      |
| `meta`     | `CursorMeta`     | Pagination, see [Common types](#cursormeta)           |

**`ResponseItem` object**

| Field                         | Type                | Nullable | Description                                                      |
|-------------------------------|---------------------|----------|------------------------------------------------------------------|
| `requestId`                   | `string (uuid)`     | no       | Request the response belongs to                                   |
| `chatLinkId`                  | `string (uuid)`     | no       | Chat link identifier                                              |
| `chatRoomId`                  | `string (uuid)`     | no       | Chat room identifier — use it to open the chat                    |
| `title`                       | `string`            | no       | Request title, current value                                      |
| `previewMediaFileId`          | `string (uuid)`     | yes      | Cover image id. `null` if the request has no media                |
| `regionName`                  | `string`            | no       | Region of the request                                             |
| `categoryNames`               | `string[]`          | no       | Category names of the request — names only, no ids                |
| `hasChanges`                  | `boolean`           | no       | `true` if the request was edited after the response was sent      |
| `ownerProfileId`              | `string (uuid)`     | no       | Customer profile identifier                                       |
| `ownerDisplayName`            | `string`            | no       | Customer display name                                             |
| `chatLinkStatus`              | `StatusBadge`       | no       | Response status, see [Chat link status](#chat-link-status-codes)  |
| `newMessages`                 | `number`            | no       | Unread messages in this chat                                      |
| `lastMessageText`             | `string`            | yes      | Preview of the last message. `null` if there are no messages      |
| `lastMessageAt`               | `string (ISO 8601)` | yes      | Timestamp of the last message. `null` if there are no messages    |
| `lastMessageAttachmentType`   | `string`            | yes      | Kind of the last attachment, see [Attachment types](#attachment-types). `null` if none |
| `lastMessageAttachmentsCount` | `number`            | no       | Attachments in the last message. `0` if none                      |
| `respondedAt`                 | `string (ISO 8601)` | no       | When the response was sent (UTC)                                  |

**`ResponseStats` object**

| Field            | Type     | Description                                          |
|------------------|----------|------------------------------------------------------|
| `totalResponses` | `number` | All responses ever sent by the profile                |
| `activeChats`    | `number` | Chats currently active                                |
| `pinnedCount`    | `number` | Chats where the profile was pinned as priority        |
| `newMessages`    | `number` | Unread messages across all chats                      |

**`ResponseTabCount` object**

Counter per tab in the UI. <!-- Счётчики для табов -->

#### Response tab keys

| Key       | Description                                     |
|-----------|-------------------------------------------------|
| `all`     | Everything except archive and history            |
| `active`  | Chats in progress                                |
| `pinned`  | Chats where the profile was pinned as priority   |
| `new`     | Chats with unread messages                       |
| `archive` | Chats moved to archive                           |
| `history` | Responses on closed requests                     |

**Example**

```json
{
  "items": [
    {
      "requestId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
      "chatLinkId": "b39ec480-bb9f-4bb8-a3d6-563e999708dd",
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "title": "заявка 1.8. заявка тесть тест. Изменение",
      "previewMediaFileId": null,
      "regionName": "Владивосток",
      "categoryNames": [
        "Грузоперевозки"
      ],
      "hasChanges": true,
      "ownerProfileId": "5dda5bf1-a3a6-45cc-9aa3-1cf2e70bb6c8",
      "ownerDisplayName": "ООО \"ОРВЕЛЛии\"",
      "chatLinkStatus": {
        "code": "pinned",
        "label": "Выбран приоритетным",
        "theme": "success"
      },
      "newMessages": 2,
      "lastMessageText": "Еще раз122",
      "lastMessageAt": "2026-07-31T08:42:29.356054Z",
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "respondedAt": "2026-07-30T16:01:42.400132Z"
    },
    {
      "requestId": "9599dfe0-746a-4451-b15c-0c13ac923796",
      "chatLinkId": "787de610-8390-4c89-8929-b7809ea9f042",
      "chatRoomId": "32e012ff-fce1-4291-8633-8917509af6c9",
      "title": "заявка 2",
      "previewMediaFileId": null,
      "regionName": "Саратов",
      "categoryNames": [
        "Фундаменты"
      ],
      "hasChanges": false,
      "ownerProfileId": "5dda5bf1-a3a6-45cc-9aa3-1cf2e70bb6c8",
      "ownerDisplayName": "ООО \"ОРВЕЛЛии\"",
      "chatLinkStatus": {
        "code": "pending",
        "label": "Отклик отправлен",
        "theme": "waiting"
      },
      "newMessages": 6,
      "lastMessageText": "Еще раз1",
      "lastMessageAt": "2026-07-30T15:02:32.315392Z",
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "respondedAt": "2026-07-12T10:28:08.587Z"
    }
  ],
  "stats": {
    "totalResponses": 5,
    "activeChats": 1,
    "pinnedCount": 1,
    "newMessages": 15
  },
  "tabCount": {
    "all": 2,
    "active": 1,
    "pinned": 1,
    "new": 1,
    "archive": 0,
    "history": 3
  },
  "meta": {
    "totalCount": 2,
    "pageSize": 20,
    "nextCursor": null,
    "previousCursor": null,
    "hasNext": false,
    "hasPrevious": false
  }
}
```

**Notes**

- Pagination is cursor-based here, unlike the page-based `/api/requests/my`. Pass `meta.nextCursor`
  back as `cursor`; `page` is not supported. <!-- курсорная пагинация -->
- `hasChanges: true` means the request was edited after the response was sent — pair it with
  [change-log](#get-apirequestsrequestidchange-log) to show what changed.
  <!-- В примере заголовок уже изменённый -->
- `title`, `regionName` and `categoryNames` reflect the current state of the request, not the
  state at the moment of responding.
- The unread counter is `newMessages` here and `unreadCount` in `chat-links/by-request`.
  <!-- расхождение в именовании -->
- `stats.totalResponses` counts every response ever sent, so it exceeds `tabCount.all`, which
  covers only the current tab set — `5` against `2` in the example.
- Use `chatRoomId` to open the chat and `chatLinkId` for pin-related operations.

---

### GET /api/responses/{requestId}

Executor-side view of a request the profile responded to.
<!-- Детали заявки со стороны исполнителя -->

**Request**

```http
GET {host}/api/responses/{requestId}
```

| Parameter   | In   | Type            | Required | Description                                  |
|-------------|------|-----------------|----------|----------------------------------------------|
| `requestId` | path | `string (uuid)` | yes      | Request identifier, not a response identifier |

**Response `200 OK`**

| Field                | Type                | Nullable | Description                                                    |
|----------------------|---------------------|----------|----------------------------------------------------------------|
| `requestId`          | `string (uuid)`     | no       | Request identifier                                              |
| `requestNumber`      | `string`            | no       | Human-readable number, format `yyyyMMdd-NNNNNN`                 |
| `status`             | `StatusBadge`       | no       | Request status, see [Request status](#request-status-codes)     |
| `title`              | `string`            | no       | Request title                                                   |
| `description`        | `string`            | no       | Request description                                             |
| `regionName`         | `string`            | no       | Region of the request                                           |
| `categoryNames`      | `string[]`          | no       | Category names — names only, no ids                             |
| `createdAt`          | `string (ISO 8601)` | no       | Creation timestamp (UTC)                                        |
| `version`            | `number`            | no       | Revision counter, incremented on every edit                     |
| `hasChanges`         | `boolean`           | no       | `true` if the request was edited after the response was sent    |
| `tags`               | `RequestTag[]`      | no       | Tags attached to the request                                    |
| `previewMediaFileId` | `string (uuid)`     | yes      | Cover image id. `null` if the request has no media              |
| `attachmentIds`      | `string (uuid)[]`   | no       | Attached media file ids. Empty array if none                    |
| `executorStatus`     | `ExecutorStatus`    | no       | Status of the current profile's own response                    |
| `pinnedChatsCount`   | `number`            | no       | Chats pinned as priority on this request                        |
| `maxPinnedChats`     | `number`            | no       | Pin limit for this request                                      |
| `totalResponses`     | `number`            | no       | Responses on this request                                       |
| `owner`              | `Owner`             | no       | Customer who created the request                                |
| `actions`            | `ExecutorActions`   | no       | What the executor is allowed to do right now                    |

**`ExecutorStatus` object**

| Field   | Type     | Description                                                        |
|---------|----------|--------------------------------------------------------------------|
| `code`  | `string` | Status code, matches [Chat link status](#chat-link-status-codes)    |
| `label` | `string` | Ready-to-render label, localised (ru)                               |

**`Owner` object**

| Field               | Type            | Description                                |
|---------------------|-----------------|--------------------------------------------|
| `profileId`         | `string (uuid)` | Customer profile identifier                 |
| `displayName`       | `string`        | Customer display name                       |
| `totalRequests`     | `number`        | Requests the customer has created           |
| `completedRequests` | `number`        | Requests the customer has completed         |

**`ExecutorActions` object**

Boolean flags, not the shared [Actions](#actions) type used on the customer side.
<!-- Другая форма действий: флаги вместо primary/secondary -->

| Field               | Type      | Description                                        |
|---------------------|-----------|----------------------------------------------------|
| `canOpenChat`       | `boolean` | The chat with the customer can be opened            |
| `canWithdraw`       | `boolean` | The response can be withdrawn                       |
| `canOfferGuarantee` | `boolean` | A guarantee can be offered on this request          |

**Example**

```json
{
  "requestId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
  "requestNumber": "20260730-001029",
  "status": {
    "code": "in_progress",
    "label": "В работе",
    "theme": "live"
  },
  "title": "заявка 1.8. заявка тесть тест. Изменение",
  "description": "тестовое описание. проверка? bpvtytybt. проверка апдейта активной заявки через edit published",
  "regionName": "Владивосток",
  "categoryNames": [
    "Грузоперевозки"
  ],
  "createdAt": "2026-07-30T15:57:55.349036Z",
  "version": 2,
  "hasChanges": true,
  "tags": [
    {
      "tagId": "805c1e9c-9b34-48c7-94f2-be115f91d17d",
      "tagName": "фура"
    }
  ],
  "previewMediaFileId": null,
  "attachmentIds": [],
  "executorStatus": {
    "code": "pinned",
    "label": "Выбран приоритетным"
  },
  "pinnedChatsCount": 1,
  "maxPinnedChats": 7,
  "totalResponses": 1,
  "owner": {
    "profileId": "5dda5bf1-a3a6-45cc-9aa3-1cf2e70bb6c8",
    "displayName": "ООО \"ОРВЕЛЛии\"",
    "totalRequests": 7,
    "completedRequests": 3
  },
  "actions": {
    "canOpenChat": true,
    "canWithdraw": false,
    "canOfferGuarantee": true
  }
}
```

**Notes**

- The path segment is the **request** id, the same one used in `/api/requests/{requestId}/details`;
  the two endpoints are the customer and executor views of one request.
  <!-- Один id, две витрины -->
- `executorStatus` has no `theme`, so it is not a [StatusBadge](#statusbadge) even though the
  codes match. <!-- расхождение с общим типом -->
- `actions` here is a set of boolean flags, while the customer-side `actions` is
  `primaryAction` / `secondaryActions`. Same field name, different shape.
  <!-- расхождение с общим типом -->
- `canWithdraw: false` while pinned — a pinned executor cannot withdraw the response.
  <!-- проверить: правило или совпадение -->
- `version` and `hasChanges` pair with [change-log](#get-apirequestsrequestidchange-log) to show
  what the customer edited after the response was sent.
- `owner.totalRequests` / `completedRequests` are the customer's reputation numbers on the card.

---

### GET /api/responses/chat-links/by-request/{requestId}

Pinned slots and recent chats for a request, used on the details page.
<!-- Слоты добавленных чатов -->

**Request**

```http
GET {host}/api/responses/chat-links/by-request/{requestId}
```

| Parameter   | In   | Type            | Required | Description        |
|-------------|------|-----------------|----------|--------------------|
| `requestId` | path | `string (uuid)` | yes      | Request identifier |

**Response `200 OK`**

| Field            | Type         | Description                                                       |
|------------------|--------------|-------------------------------------------------------------------|
| `pinnedSlots`    | `ChatLink[]` | Chats pinned as priority. Empty array if none                      |
| `recentChats`    | `ChatLink[]` | Latest non-pinned chats. Empty array if none                       |
| `totalChats`     | `number`     | Total chats on the request, pinned included                        |
| `remainingCount` | `number`     | Chats not returned in either array, render as "and N more"         |

**`ChatLink` object**

Same shape for both arrays; the pin-specific fields are only filled for pinned entries.
<!-- ДТО одно и то же, у незакреплённых чатов поля закрепления пустые -->

| Field                         | Type                | Nullable | Description                                                    |
|-------------------------------|---------------------|----------|----------------------------------------------------------------|
| `chatLinkId`                  | `string (uuid)`     | no       | Chat link identifier — the id to use in pin/unpin calls         |
| `chatRoomId`                  | `string (uuid)`     | no       | Chat room identifier — the id to use when opening the chat      |
| `executorProfileId`           | `string (uuid)`     | no       | Executor profile identifier                                     |
| `displayName`                 | `string`            | no       | Executor display name                                           |
| `status`                      | `StatusBadge`       | no       | Chat status, see [Chat link status](#chat-link-status-codes)    |
| `pinnedAt`                    | `string (ISO 8601)` | yes      | When the chat was pinned. `null` for non-pinned chats           |
| `lastActivityAt`              | `string (ISO 8601)` | no       | Last activity in the chat (UTC)                                 |
| `lastMessageText`             | `string`            | yes      | Preview of the last message. `null` if there are no messages    |
| `lastMessageAt`               | `string (ISO 8601)` | yes      | Timestamp of the last message. `null` if there are no messages  |
| `lastMessageAttachmentType`   | `string`            | yes      | Kind of the last attachment, see [Attachment types](#attachment-types). `null` if none |
| `lastMessageAttachmentsCount` | `number`            | no       | Attachments in the last message. `0` if none                    |
| `lastMessageIsSystem`         | `boolean`           | no       | `true` if the last message was generated by the system          |
| `unreadCount`                 | `number`            | no       | Unread messages for the current profile                         |

**Example — a pinned slot**

```json
{
  "pinnedSlots": [
    {
      "chatLinkId": "b39ec480-bb9f-4bb8-a3d6-563e999708dd",
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "executorProfileId": "a8dd4416-1296-42bc-bea9-72bd9d96c64c",
      "displayName": "ООО \"ОТКЛИК\"",
      "status": {
        "code": "pinned",
        "label": "Закреплён",
        "theme": "success"
      },
      "pinnedAt": "2026-07-31T08:39:43.530828Z",
      "lastActivityAt": "2026-07-31T08:42:29.356054Z",
      "lastMessageText": "Еще раз122",
      "lastMessageAt": "2026-07-31T08:42:29.356054Z",
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "lastMessageIsSystem": false,
      "unreadCount": 2
    }
  ],
  "recentChats": [],
  "totalChats": 1,
  "remainingCount": 0
}
```

**Example — with recent chats**

```json
{
  "pinnedSlots": [
    {
      "chatLinkId": "b39ec480-bb9f-4bb8-a3d6-563e999708dd",
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "executorProfileId": "a8dd4416-1296-42bc-bea9-72bd9d96c64c",
      "displayName": "ООО \"ОТКЛИК\"",
      "status": {
        "code": "pinned",
        "label": "Закреплён",
        "theme": "success"
      },
      "pinnedAt": "2026-07-31T08:39:43.530828Z",
      "lastActivityAt": "2026-07-31T08:42:29.356054Z",
      "lastMessageText": "Еще раз122",
      "lastMessageAt": "2026-07-31T08:42:29.356054Z",
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "lastMessageIsSystem": false,
      "unreadCount": 2
    }
  ],
  "recentChats": [
    {
      "chatLinkId": "f1c3b90a-7a41-4d0e-9c2b-1d8e4a5b6c70",
      "chatRoomId": "32e012ff-fce1-4291-8633-8917509af6c9",
      "executorProfileId": "c7b21d54-9e08-4a3f-8b16-2f5c0e9d7a11",
      "displayName": "ООО \"СТРОЙБАЗА\"",
      "status": {
        "code": "new",
        "label": "Новый отклик",
        "theme": "info"
      },
      "pinnedAt": null,
      "lastActivityAt": "2026-07-31T07:05:12.114233Z",
      "lastMessageText": null,
      "lastMessageAt": null,
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "lastMessageIsSystem": false,
      "unreadCount": 0
    }
  ],
  "totalChats": 2,
  "remainingCount": 0
}
```

<!-- второй пример собран вручную: реального ответа с recentChats не было,
     статус незакреплённого чата (code/label/theme) нужно подтвердить -->

**Notes**

- Use `chatLinkId` for pin and unpin operations, and `chatRoomId` to open the chat.
  <!-- Два разных id, не перепутать -->
- `pinnedAt` is `null` for everything in `recentChats`.
- `remainingCount` counts chats not returned in this response, not free pin slots — the pin
  limit is `maxPinnedChats` from the details endpoint.
- A chat with no messages yet still exists as a response: `lastMessageText` and `lastMessageAt`
  are `null` while `lastActivityAt` is set.
- The second example is illustrative, assembled from the same DTO.

---

## Chats

Chat rooms of the current profile. One chat belongs to one response on one request.
<!-- Страница чатов -->

### GET /api/chat

**Request**

```http
GET {host}/api/chat?filter=all&limit=20
```

| Parameter   | Type                | Required | Description                                                       |
|-------------|---------------------|----------|-------------------------------------------------------------------|
| `filter`    | `string`            | no       | Role filter, see [Chat filters](#chat-filters). Default `all`      |
| `requestId` | `string (uuid)`     | no       | Return only chats of this request. All chats if omitted            |
| `limit`     | `number`            | no       | Items to return. Default `20`                                      |
| `before`    | `string (ISO 8601)` | no       | Keyset cursor — return chats older than this timestamp             |

#### Chat filters

Bound from the `ChatFilter` enum, matching is case-insensitive.

| Value        | Description                                             |
|--------------|---------------------------------------------------------|
| `all`        | Every chat of the profile                                |
| `asExecutor` | Chats where the profile responded to someone's request   |
| `asCustomer` | Chats on requests the profile created                    |

**Response `200 OK`**

| Field     | Type             | Description                                    |
|-----------|------------------|------------------------------------------------|
| `items`   | `ChatListItem[]` | Chats, newest activity first                    |
| `hasMore` | `boolean`        | `true` if more chats exist beyond those returned |

**`ChatListItem` object**

| Field                         | Type                | Nullable | Description                                                   |
|-------------------------------|---------------------|----------|---------------------------------------------------------------|
| `chatRoomId`                  | `string (uuid)`     | no       | Chat room identifier — use it to open the chat                 |
| `companionName`               | `string`            | no       | Display name of the other side                                 |
| `companionProfileId`          | `string (uuid)`     | no       | Profile identifier of the other side                           |
| `lastMessageText`             | `string`            | yes      | Preview of the last message. `null` if there are no messages   |
| `lastMessageAt`               | `string (ISO 8601)` | yes      | Timestamp of the last message. `null` if there are no messages |
| `lastMessageIsSystem`         | `boolean`           | no       | `true` if the last message was generated by the system         |
| `lastMessageAttachmentType`   | `string`            | yes      | Kind of the last attachment, see [Attachment types](#attachment-types). `null` if none |
| `lastMessageAttachmentsCount` | `number`            | no       | Attachments in the last message. `0` if none                   |
| `unreadCount`                 | `number`            | no       | Unread messages for the current profile                        |
| `isBlocked`                   | `boolean`           | no       | `true` if the chat is read-only, see notes                     |
| `requestInfo`                 | `ChatRequestInfo`   | no       | The request this chat belongs to                               |

**`ChatRequestInfo` object**

| Field            | Type            | Description                                                     |
|------------------|-----------------|-----------------------------------------------------------------|
| `requestId`      | `string (uuid)` | Request identifier                                               |
| `requestTitle`   | `string`        | Request title, current value                                     |
| `chatLinkStatus` | `string`        | Raw status, see [Chat link raw statuses](#chat-link-raw-statuses) |
| `chatLinkId`     | `string (uuid)` | Chat link identifier                                             |

#### Chat link raw statuses

Plain enum names, PascalCase — not a [StatusBadge](#statusbadge) and not the lower-case codes
used elsewhere. <!-- Сырой enum вместо объекта статуса -->

| Value       | Matches code | Description                          |
|-------------|--------------|--------------------------------------|
| `New`       | `pending`    | Response sent, awaiting a decision    |
| `Pinned`    | `pinned`     | Executor pinned as the priority one   |
| `Withdrawn` | —            | Response withdrawn by the executor    |
| `Archived`  | —            | Chat moved to archive                 |

<!-- соответствие New ↔ pending предположено, подтвердить -->

**Example**

```json
{
  "items": [
    {
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "companionName": "ООО \"ОРВЕЛЛии\"",
      "companionProfileId": "5dda5bf1-a3a6-45cc-9aa3-1cf2e70bb6c8",
      "lastMessageText": "Еще раз122",
      "lastMessageAt": "2026-07-31T08:42:29.356054Z",
      "lastMessageIsSystem": false,
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "unreadCount": 2,
      "isBlocked": false,
      "requestInfo": {
        "requestId": "67a8d8fb-66b4-4aa1-8c69-4308156f8a36",
        "requestTitle": "заявка 1.8. заявка тесть тест. Изменение",
        "chatLinkStatus": "Pinned",
        "chatLinkId": "b39ec480-bb9f-4bb8-a3d6-563e999708dd"
      }
    },
    {
      "chatRoomId": "d76f24bb-b5d7-4f82-82ca-ff32a14f517c",
      "companionName": "ООО \"ОРВЕЛЛии\"",
      "companionProfileId": "5dda5bf1-a3a6-45cc-9aa3-1cf2e70bb6c8",
      "lastMessageText": "Заявка завершена.",
      "lastMessageAt": "2026-07-30T15:53:59.762804Z",
      "lastMessageIsSystem": true,
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "unreadCount": 1,
      "isBlocked": true,
      "requestInfo": {
        "requestId": "d7e2bbc2-2c9a-482c-b2b7-448d7fe5f636",
        "requestTitle": "заявка 4",
        "chatLinkStatus": "New",
        "chatLinkId": "e205954a-09a0-4ef1-8917-96584a792ae3"
      }
    },
    {
      "chatRoomId": "3b85f687-bf3e-4a20-af49-3d8164e7d938",
      "companionName": "ООО \"ОРВЕЛЛии\"",
      "companionProfileId": "5dda5bf1-a3a6-45cc-9aa3-1cf2e70bb6c8",
      "lastMessageText": "Отклик отозван.",
      "lastMessageAt": "2026-07-14T19:39:52.90211Z",
      "lastMessageIsSystem": true,
      "lastMessageAttachmentType": null,
      "lastMessageAttachmentsCount": 0,
      "unreadCount": 0,
      "isBlocked": true,
      "requestInfo": {
        "requestId": "5dd292dd-a6d6-47b9-9985-63ff4e67f5f4",
        "requestTitle": "заявка 3",
        "chatLinkStatus": "Withdrawn",
        "chatLinkId": "a50fe2bb-2b96-4c22-ac69-e4227b106145"
      }
    }
  ],
  "hasMore": false
}
```

**Notes**

- `isBlocked: true` means the chat is read-only. In the sample it is set once the request is
  closed or the response is withdrawn — the last message is a system one such as
  «Заявка завершена.» or «Отклик отозван.» <!-- Чат доступен только на чтение -->
- Blocked chats still carry `unreadCount` — a closed chat can hold unread messages.
- `chatLinkStatus` is a bare enum string here, while `/api/responses` returns the same status as
  a `StatusBadge` object with a lower-case code. The client has to map the two.
  <!-- расхождение с общим типом -->
- `Withdrawn` and `Archived` have no counterpart among the documented lower-case codes yet.
- Pagination is keyset-based and has no envelope: when `hasMore` is `true`, repeat the call with
  `before` set to the `lastMessageAt` of the last item and the same `filter` / `limit`.
  <!-- Ни meta, ни курсора — подгрузка по before -->
- Chats with `lastMessageAt: null` cannot serve as a `before` cursor — the paging key is the
  last message timestamp. <!-- уточнить, как такие чаты попадают в порядок -->
- System messages are rendered from `lastMessageText` as is; use `lastMessageIsSystem` to style
  them differently.

---

### GET /api/chat/{chatRoomId}/messages

Messages of a single chat room. <!-- Сообщения по чату -->

**Request**

```http
GET {host}/api/chat/{chatRoomId}/messages
```

| Parameter    | In    | Type                | Required | Description                                        |
|--------------|-------|---------------------|----------|----------------------------------------------------|
| `chatRoomId` | path  | `string (uuid)`     | yes      | Chat room identifier, from `chatRoomId`             |
| `before`     | query | `string (ISO 8601)` | no       | Keyset cursor — return messages older than this     |
| `limit`      | query | `number`            | no       | Messages to return. Default `20`                    |

**Response `200 OK`**

| Field      | Type            | Description                                        |
|------------|-----------------|----------------------------------------------------|
| `messages` | `ChatMessage[]` | Messages, newest first                              |
| `hasMore`  | `boolean`       | `true` if older messages exist beyond those returned |

**`ChatMessage` object**

| Field             | Type                | Nullable | Description                                                  |
|-------------------|---------------------|----------|--------------------------------------------------------------|
| `messageId`       | `string (uuid)`     | no       | Message identifier                                            |
| `chatRoomId`      | `string (uuid)`     | no       | Chat room the message belongs to                              |
| `senderProfileId` | `string (uuid)`     | yes      | Author profile id. `null` for system messages                 |
| `senderName`      | `string`            | no       | Author display name. `Система` for system messages            |
| `text`            | `string`            | no       | Message body                                                  |
| `isSystem`        | `boolean`           | no       | `true` if generated by the platform, not by a user            |
| `isEdited`        | `boolean`           | no       | `true` if the message was edited                              |
| `isMine`          | `boolean`           | no       | `true` if sent by the current profile — use it for alignment  |
| `createdAt`       | `string (ISO 8601)` | no       | When the message was sent (UTC)                               |
| `editedAt`        | `string (ISO 8601)` | yes      | When it was last edited. `null` if never edited               |
| `attachments`     | `Attachment[]`      | no       | Attached files, ordered by `sortOrder`. Empty array if none   |

**`Attachment` object**

Maps to `ChatAttachmentDto`.

| Field         | Type            | Description                                                     |
|---------------|-----------------|-----------------------------------------------------------------|
| `id`          | `string (uuid)` | Attachment identifier, unique within the message                 |
| `mediaFileId` | `string (uuid)` | Media file identifier — use it to build the download URL          |
| `type`        | `string`        | Attachment kind, see [Attachment types](#attachment-types)        |
| `sortOrder`   | `number`        | Position in the message, ascending                                |
| `fileName`    | `string`        | Original file name with extension                                 |
| `contentType` | `string`        | MIME type, e.g. `image/jpeg`                                      |
| `fileSize`    | `number`        | File size in bytes (`long`)                                       |

#### Attachment types

Bound from the `AttachmentType` enum.

<!-- значения enum пока неизвестны — прислать список (image / document / video?) -->

**Example**

```json
{
  "messages": [
    {
      "messageId": "e0200bf3-07f7-48e6-b9fa-d745851a46d9",
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "senderProfileId": "a8dd4416-1296-42bc-bea9-72bd9d96c64c",
      "senderName": "ООО \"ОТКЛИК\"",
      "text": "Еще раз122",
      "isSystem": false,
      "isEdited": false,
      "isMine": true,
      "createdAt": "2026-07-31T08:42:29.356054Z",
      "editedAt": null,
      "attachments": []
    },
    {
      "messageId": "9f01f0ea-7620-40dc-b556-206e90df87db",
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "senderProfileId": "5dda5bf1-a3a6-45cc-9aa3-1cf2e70bb6c8",
      "senderName": "ООО \"ОРВЕЛЛии\"",
      "text": "Еще раз12",
      "isSystem": false,
      "isEdited": false,
      "isMine": false,
      "createdAt": "2026-07-31T08:41:35.831725Z",
      "editedAt": null,
      "attachments": []
    },
    {
      "messageId": "2638d3a7-1a06-4eab-851d-627bc23c86ca",
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "senderProfileId": null,
      "senderName": "Система",
      "text": "Исполнитель закреплён в приоритетный слот.",
      "isSystem": true,
      "isEdited": false,
      "isMine": false,
      "createdAt": "2026-07-31T08:39:43.602256Z",
      "editedAt": null,
      "attachments": []
    },
    {
      "messageId": "69977c1e-6d6d-418f-a4cc-6a40a8769535",
      "chatRoomId": "0b224478-2b4f-40c4-a5ac-4d611830684f",
      "senderProfileId": null,
      "senderName": "Система",
      "text": "Добро пожаловать! Переписка сохраняется и может быть приложением к договору. Рекомендуем услугу «Гарант-сделка».",
      "isSystem": true,
      "isEdited": false,
      "isMine": false,
      "createdAt": "2026-07-30T16:01:42.456656Z",
      "editedAt": null,
      "attachments": []
    }
  ],
  "hasMore": false
}
```

**Notes**

- Messages come newest first, the reverse of the reading order — the client renders them bottom-up.
  <!-- Порядок обратный: сверху самое свежее -->
- System messages have `senderProfileId: null` and `senderName: "Система"`; `isSystem` is the
  reliable check, the name is display text.
- The first message in every chat is the system greeting, sent when the response is created —
  its `createdAt` matches `respondedAt` from `/api/responses`.
- `isMine` is resolved server-side for the requesting profile; do not compare
  `senderProfileId` on the client.
- Pagination mirrors `/api/chat`: when `hasMore` is `true`, repeat the call with `before` set to
  the `createdAt` of the last returned message. There is no `limit` cap declared in the
  signature. <!-- Ключ подгрузки — createdAt последнего сообщения -->
- Unlike `/api/chat`, `limit` here has no upper bound in the signature, and `pageSize` naming is
  not used at all — three list endpoints, three different paging parameter sets.
