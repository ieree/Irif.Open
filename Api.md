- [IRif Api](#irif-api)
	- [Category](#category)
		- [Get Categories](#get-categories)
        - [Get Category Tree](#get-category-tree)
	- [SubCategory](#subcategory)
		- [Get SubCategories](#get-subcategories)
		- [Get SubCategoriy](#get-subcategory)	
    - [Product Category](#product-category)
        - [Get Product Categories](#get-product-categories)
        - [Get Product Category](#get-product-category)
    - [Products](#products)
        - [Get Products by Category](#get-products-by-category)
        - [Get Product Detail](#get-product-detail)
    - [Reviews](#reviews)
      	- [Get Reviews](#get-reviews)
      	- [Add Reviews](#add-reviews)
      	- [Update Reviews](#update-reviews)
      	- [Add Reviews Opinion](#add-reviews-opinion)
    - [Questions](#questions)
	- [Get Questions](#get-questions)
    - [Company](#company)
      	- [Company About](#company-about)
      	- [Company Short Info](#company-short-info)
  - [Cart](#cart)
    - [Get Active Cart](#get-active-cart)
    - [Add To Cart](#add-to-cart)
    - [Change Cart Item Count](#change-cart-item-count)
    - [Delete Cart Item](#delete-cart-item)
    - [Delete Range Cart Items](#delete-range-cart-items)
    - [Cart Checkout](#cart-checkout)
    - [Cart Item Available](#cart-item-available)
    - [Get Current Cart Status](#get-current-cart-status)
    - [Generate Cart Report Link](#generate-cart-report-link)
    - [Get Saved Carts](#get-saved-carts)
    - [Delete Cart](#delete-cart)
    - [Delete Saved Carts](#delete-saved-carts)
    - [Save Cart](#save-cart)
    - [Restore Saved Cart](#restore-saved-cart)
    - [Checkout Saved carts](#checkout-saved-carts)
  - [Favourites](#favourite)
     - [Get Favourite products](#get-favourite-products)
     - [Add Favourite Products](#add-favourite-products)
     - [Delete Favourite Products](#delete-favourite-products)

# IRif Api
Api v1 

## Category

### Get Categories
```js
GET {{host}}/v1/categories
```
#### Rsponse
```js
200 Ok
```
```json
{
   {
      "id": "b7b45ac6-52ca-458e-9f03-8fb1e1c14144",
      "categoryName": "Электроника",
      "categoryHandle": "elektronika-b7b45ac6-52ca-458e-9f03-8fb1e1c14144"
   },
   {
      "id": "b7b45ac6-52ca-458e-9f03-8fb1e1c14144",
      "categoryName": "Дом и сад",
      "categoryHandle": "smartfoni-i-telefoni-941eae43-fff7-44db-8dd8-bf62561240a2"
   }
}
```
#
### Get Category Tree
```js
GET {{host}}/api/categories-tree
```

#### Rsponse
```js
200 Ok
```
```json
{
   "categories": [
    {
      "categoryId": "b7b45ac6-52ca-458e-9f03-8fb1e1c14144",
      "categoryName": "Электроника",
      "categoryHandle": "elektronika-b7b45ac6-52ca-458e-9f03-8fb1e1c14144",
      "subCategories": [
        {
          "subCategoryId": "941eae43-fff7-44db-8dd8-bf62561240a2",
          "subCategoryName": "Смартфоны и телефоны",
          "subCategoryHandle": "smartfoni-i-telefoni-941eae43-fff7-44db-8dd8-bf62561240a2",
          "productCategories": [
            {
              "productCategoryId": "96c49bfa-49b2-4704-8190-62a2a323e65c",
              "productCategoryName": "Смартфоны",
              "productCategoryHandle": "smartfoni-96c49bfa-49b2-4704-8190-62a2a323e65c"
            }
          ]
        }
      ]
    }
  ]
}
```

#
## SubCategory

### Get SubCategories
```js
GET {{host}}/api/sub-categories
```
#### Rsponse
```js
200 Ok
```
```json
{
   {
    "subCategoryId": "941eae43-fff7-44db-8dd8-bf62561240a2",
    "subCategoryName": "Смартфоны и телефоны",
    "subCategoryHandle": "smartfoni-i-telefoni-941eae43-fff7-44db-8dd8-bf62561240a2"
   }
}
```

##
### Get SubCategory
```js
GET {{host}}/api/categories/{categoryId}/sub-categories
```
#### Rsponse
```js
200 Ok
```
```json
{
   {
    "subCategoryId": "941eae43-fff7-44db-8dd8-bf62561240a2",
    "subCategoryName": "Смартфоны и телефоны",
    "subCategoryHandle": "smartfoni-i-telefoni-941eae43-fff7-44db-8dd8-bf62561240a2"
  }
}
```

## Product Category

### Get Product Categories
```js
GET {{host}}/api/product-categories
```
#### Rsponse
```js
200 Ok
```
```json
{
   {
    "productCategoryId": "96c49bfa-49b2-4704-8190-62a2a323e65c",
    "productCategoryName": "Смартфоны",
    "productCategoryHandle": "smartfoni-96c49bfa-49b2-4704-8190-62a2a323e65c"
  }
}
```

### Get Product Category
```js
GET {{host}}/api/sub-categories/{subCategoryId}/product-categories
```
#### Rsponse
```js
200 Ok
```
```json
{
   {
    "productCategoryId": "96c49bfa-49b2-4704-8190-62a2a323e65c",
    "productCategoryName": "Смартфоны",
    "productCategoryHandle": "smartfoni-96c49bfa-49b2-4704-8190-62a2a323e65c"
  }
}
```

#
#
## Products

### Get Products by Category
```js
GET {{host}}/api/category/{categoryHandle}/products
```

### Parameters
sortColumn: price, rating, reviews / default: productName;

sortOrder: desc, asc;

page: default 1;

pageSize: default: 15


#### Rsponse
```js
200 Ok
```
```json
{
  "products": [
    {
      "productName": "Xiaomi mi8",
      "productHandler": "example handler",
      "price": 123124,
      "regularPrice": 123123,
      "discount": 60,
      "reviewsRating": 4.8,
      "reviewsCount": 5,
      "isSecondHand": false,
      "isDiscounted": false,
      "isFavourite": true,
      "isComparable": false,
      "inventoryQuantity": 2323,
      "countInCart": 10,
      "vendorName": null,
      "features": [
        {
          "name": "Внутренняя память",
          "value": "64 Гб"
        }
      ]
    }
  ],
  "meta": {
    "pages": {
      "page": 1,
      "pageSize": 15,
      "totalCount": 1,
      "hasNextPage": false,
      "hasPreviousPage": false
    },
    "path": [
      {
        "position": "ProductCategory",
        "name": "Смартфоны",
        "handle": "smartfoni-96c49bfa-49b2-4704-8190-62a2a323e65c"
      },
      {
        "position": "SubCategory",
        "name": "Смартфоны и телефоны",
        "handle": "smartfoni-i-telefoni-941eae43-fff7-44db-8dd8-bf62561240a2"
      },
      {
        "position": "Category",
        "name": "Электроника",
        "handle": "elektronika-b7b45ac6-52ca-458e-9f03-8fb1e1c14144"
      }
    ]
  }
}
```
##
### Get Product Detail
```js
GET {{host}}/api/products/{productHandle}
```
### Parameters
sku: 123143432 / default null

#### Response
```js
200 Ok
```
```json
{
    "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
    "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
    "productDescription": "Смартфон Xiaomi 12 Sky Blue оснащен восьмиядерным процессором MediaTek Helio G88 частотой 2x2,2 ГГц + 6x1,8 ГГц. Объем оперативной памяти — 4 Гб, встроенной — 128 Гб. При необходимости хранилище можно расширить за счет карты microSD (до 1024 Гб). Девайс работает на базе ОС Android 13.\r\nСмартфон поддерживает установку двух nano-SIM, функционирует в сетях вплоть до 4G. Беспроводные технологии представлены модулями Wi-Fi 802.11a/b/g/n/ac для выхода в Интернет и Bluetooth 5.1 для обмена данными с другими совместимыми устройствами. Поддерживаются навигационные системы GPS, ГЛОНАСС, Galileo и BeiDou. Они помогут без труда сориентироваться в незнакомом месте, найти нужный адрес.\r\nДисплей диагональю 6,79-дюйма выполнен по технологии IPS и обладает разрешением 1080х2400 пикселей. \r\nОсновная камера обладает разрешением 50/8/2 Мп, снабжена системой оптической стабилизации и выпышкой. Фронтальная камера для селфи и видеосвязи — на 5 Мп.\r\nНа боковой грани расположен сканер отпечатков пальцев, который обеспечит быструю авторизацию владельца и защитит данные от посторонних. Кроме того, доступ возможен по распознаванию лица. В смартфон установлен аккумулятор емкостью 5000 мА*ч. Он поддерживает быструю зарядку (22,5 Вт), соответствующее зарядное устройство поставляется в комплекте. Тип интерфейса для подключения — USB Type-C.",
    "price": 15952,
    "regularPrice": 27000,
    "discount": 41,
    "reviewsRating": 3.9,
    "reviewsCount": 14,
    "questionCount": 4,
    "isSecondHand": false,
    "isDiscounted": true,
    "inventoryQuantity": 10,
    "productVendor": {
        "vendorId": "5133d578-a099-4230-b2ac-ed6e40b4dd35",
        "vendorType": "company",
        "shopContact": "Андрей В.",
        "contactPersonPosition": "Менеджер по продажам",
        "contactImageUrl": "Data/companies/5133d578-a099-4230-b2ac-ed6e40b4dd35/images/personal/8097645556.jpg",
        "isContactShown": true,
        "shopName": "MWInformTech",
        "isSideInfoShown": true,
        "isCompanySideSectionShown": true,
        "isCompanyAboutShown": true,
        "companyName": "OOO \"ИнформТехнологии\"",
        "shopRating": null,
        "startOnMarketDate": "2024-06-20T11:16:28.500179Z",
        "businessType": "Компания",
        "employeesNumber": 30,
        "foundationDate": "2024-06-20T11:16:28.500268Z"
    },
    "images": [
        {
            "imageName": "6706256093",
            "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6706256093.png"
        },
        {
            "imageName": "6706256094",
            "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6706256094.png"
        },
        {
            "imageName": "6951689772",
            "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png"
        },
        {
            "imageName": "6706256091",
            "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6706256091.png"
        },
        {
            "imageName": "6951689770",
            "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689770.png"
        }
    ],
    "documents": [
        {
            "documentName": "Презентация Xiaomi Смартфон Redmi 12",
            "documentPath": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/documents/187451568732.pdf"
        },
        {
            "documentName": "Техническая документация Xiaomi Смартфон Redmi 12",
            "documentPath": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/documents/187451568733.pdf"
        },
        {
            "documentName": "Инструкция по эксплуатации",
            "documentPath": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/documents/187451568731.pdf"
        }
    ],
    "characteristics": [
        {
            "name": "Цвет",
            "value": "Черный"
        },
        {
            "name": "Процессор",
            "value": "Qualcomm Snapdragon 439"
        },
        {
            "name": "Операционная система",
            "value": "Android"
        },
        {
            "name": "Оперативная память",
            "value": "8 Гб"
        },
        {
            "name": "Камера",
            "value": "Основная 108 МП"
        },
        {
            "name": "Диагональ",
            "value": "5.4"
        },
        {
            "name": "Встроенная память",
            "value": "128 Гб"
        },
        {
            "name": "Аккумулятор",
            "value": "5020 мА·ч"
        }
    ],
    "features": [
        {
            "name": "Камера",
            "value": "Основная 108 МП"
        },
        {
            "name": "Аккумулятор",
            "value": "5020 мА·ч"
        },
        {
            "name": "Операционная система",
            "value": "Android"
        },
        {
            "name": "Цвет",
            "value": "Черный"
        },
        {
            "name": "Оперативная память",
            "value": "8 Гб"
        },
        {
            "name": "Процессор",
            "value": "Qualcomm Snapdragon 439"
        }
    ],
    "options": [
        {
            "optionName": "Цвет",
            "values": [
                {
                    "productVariantId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
                    "sku": "58745216",
                    "value": "Зеленый"
                },
                {
                    "productVariantId": "c2d998f1-d31d-48bc-a50f-633e4ef4b6c1",
                    "sku": "58745217",
                    "value": "Черный"
                }
            ]
        }
    ],
    "meta": {
        "pages": null,
        "path": [
            {
                "position": "ProductCategory",
                "name": "Смартфоны",
                "handle": "smartfoni-781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a"
            },
            {
                "position": "SubCategory",
                "name": "Смартфоны и телефоны",
                "handle": "smartfoni-i-telefoni-69ebe83d-b7ad-4095-a323-950414539b8b"
            },
            {
                "position": "Category",
                "name": "Электроника",
                "handle": "electronika-e2ca89d0-b9d4-4ec7-998c-9d8a1429c8bc"
            }
        ]
    }
}
```

##
##
##
## Category Filter Example
```json
[
  {
    filterName: “Стоимость”,
    filterType:  “interval”,
    filterSettings: {minVal: 0, maxVal: 22000} 
  },
  {
    "filterName": "Производитель",
    "filterType": "checkbox",
    "filterSettings": ["Apple", "Xiaomi", "Samsung", "Realme", "Honor", "Huawei", "OPPO", "Vivo", "Sony", "Nokia"]
  }
]
```


##
# Reviews
### Get Reviews
```js
GET {{host}}/api/products/{productId}/reviews
```
### Parameters
sortColumn: rating, date
sortOrder: asc, dsc
cursor

#### Response
```js
200 Ok
```
```json
{
    {
    "reviews": [
        {
            "reviewId": "ae83153c-1b21-4a1a-9071-565598d5f446",
            "userId": "5b5cbcab-3e97-45b0-931e-83a97675ef55",
            "companyId": null,
            "reviewerType": "user",
            "reviewerName": "Александр А.",
            "reviewRating": 5,
            "experience": "Месяц назад",
            "advantages": "Отличный телефон, удобно держать в руке)",
            "disadvantages": "Пока не обнаружил недостатков",
            "comment": "Купил дочке в подарок на день рождения, очень довольна телефоном",
            "likes": 0,
            "dislikes": 0,
            "createDate": "2024-06-14T07:47:28.126141Z",
            "reviewImages": [
                {
                    "reviewImageName": "1000548151",
                    "reviewImagePath": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/reviews/ae83153c-1b21-4a1a-9071-565598d5f446/1000548151.png"
                },
                {
                    "reviewImageName": "1000548152",
                    "reviewImagePath": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/reviews/ae83153c-1b21-4a1a-9071-565598d5f446/1000548152.png"
                },
                {
                    "reviewImageName": "1000548154",
                    "reviewImagePath": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/reviews/ae83153c-1b21-4a1a-9071-565598d5f446/1000548154.png"
                },
                {
                    "reviewImageName": "1000548153",
                    "reviewImagePath": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/reviews/ae83153c-1b21-4a1a-9071-565598d5f446/1000548153.png"
                }
            ],
            "reviewAnswers": [
                {
                    "answerId": "a7e03aaa-6ee5-4fa6-ac4c-45958c2fceec",
                    "userId": null,
                    "companyId": "5133d578-a099-4230-b2ac-ed6e40b4dd35",
                    "responderType": "company",
                    "shopName": "MWInformTech",
                    "answer": "Добрый день! Спасибо за отзыв. Ждем вас за покупками :)",
                    "likes": 0,
                    "dislikes": 0,
                    "createDate": "2024-06-14T07:47:28.458256Z"
                }
            ]
        }
    ],
    "cursorPaging": {
        "nextCursor": 14,
        "cursorLimit": 14
    }
}
```

### Add Reviews
```js
POST {{host}}/api/reviews
```

#### Response
```js
200 Ok
```
```json
{
  "userId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "companyId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "ratingValue": 0,
  "experience": "string",
  "advantage": "string",
  "disadvantage": "string",
  "comment": "string"
}
```

### Update Reviews
```js
PUT {{host}}/api/reviews
```

#### Response
```js
200 Ok
```
```json
{
  "reviewId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "comment": "string"
}
```

### Add Reviews Opinion
```js
POST {{host}}/api/reviews/opinion
```
#### Response
```js
200 Ok
```
```json
{
  "reviewId": "0a59835c-1bcc-4ef3-adba-f369ec2af2ad",
  "likeCount": 1,
  "dislikeCount": 0
}
```

##
# Questions

### Get Questions

```js
GET {{host}}/api/products/{productId}/questions
```

#### Response
```js
200 Ok
```

```json
{
  "productQuestions": [
    {
      "productQuestionId": "4cb1d88c-30a7-4e9e-9aa1-b689175ab8f6",
      "userId": "161598ac-ae4f-4bce-bb9b-892fd5ac7c18",
      "companyId": null,
      "senderType": "user",
      "senderName": "Иван Т.",
      "questionText": "Здравствуйте!сколько сим карт поддерживает данная модель?",
      "createDate": "2024-06-20T11:16:31.34105Z",
      "questionAnswer": [
        {
          "questionAnswerId": "b4ebd06c-fffc-4594-a86f-d37ad5c3b76c",
          "answerFrom": "MWInformTech",
          "answerText": "Здравствуйте! Благодарим Вас за интерес к нашим товарам. 2 С уважением, магазин Смарт Девайс.",
          "createDate": "2024-06-20T11:16:31.450379Z"
        }
      ]
    }
  ],
  "cursorPaging": {
    "nextCursor": 4,
    "cursorLimit": 4
  }
}
```

##
# Company
### Company About
```js
GET {{host}}/api/companies/{companyId}/about
```

#### Response
```js
200 Ok
```

```json
{
    "companyId": "5133d578-a099-4230-b2ac-ed6e40b4dd35",
    "companyDescription": "Компания «Информационные технологии» начала свою деятельность на рынке информационных услуг в 2008 году. Наше первое направление – создание сайтов. Мы набирались опыта, повышали квалификацию, получали сертификаты и статусы, пока не пришли к тому, что просто создать хороший сайт – этого мало, нужно чтобы его мог найти не только Заказчик, но и другие пользователи Интернета. Так мы решили заняться продвижением сайтов.\r\n\r\nОднако стали появляться случаи, когда мы не могли продемонстрировать работу сайта по причине неисправности компьютеров заказчика, а нам хотелось сделать все красиво и качественно от начала и до конца. Так родилось еще одно направление - обслуживание ПК и оргтехники. Но и этого оказалось недостаточно. Большинству компаний нужен не только свой сайт, не только работающие компьютеры, но нужны и множество дополнительных систем\r\nи сервисов. Мы видели что в любой организации возникает множество разных вопросов, связанных с информационными технологиями: это ведение бухгалтерии, отправка электронной отчетности, построение удобного платежного календаря, составление бюджетов доходов и расходов, постановка задач сотрудникам и контроль их исполнения, монтаж компьютерной или телефонной сети, контроль рабочего времени сотрудников, доступ в помещения, видеонаблюдение, мониторинг движения автотранспорта и многое многое другое.\r\n\r\nТак появились такие направления как конфигурирование, обслуживание и внедрение систем 1С, система электронного документооборота Directum, система бюджетирования и управления финансами Prestima, видеонаблюдение, мониторинг автотранспорта и др. Наши специалисты активно сдавали экзамены, проходили курсы и тренинги, набирались знаний и опыта, чтобы предоставить наиболее подходящие клиенту решения и качественный сервис.\r\n\r\nСегодня наши специалисты работают по более чем 10 различным направлениям, чтобы суметь оказать Вам полный спектр IT-услуг. И это еще не предел. Для нас нет неразрешимых задач",
    "companyName": "OOO "ИнформТехнологии"",
    "businessFeatures": {
        "businessType": "Компания",
        "mainProducts": "Телекоммуникационные технлогии",
        "employeeCount": 30,
        "foundationDate": "2024-06-20T11:16:28.500268Z",
        "managementCertification": "ISO 9001",
        "ssgCertification": "0179328"
    },
    "galery": [
        {
            "section": "certificates",
            "images": [
                {
                    "imageName": "6564971002",
                    "imageUrl": "Data/companies/5133d578-a099-4230-b2ac-ed6e40b4dd35/images/certs/6564971002.png"
                }
            ]
        },
        {
            "section": "profile",
            "images": [
                {
                    "imageName": "652165161",
                    "imageUrl": "Data/companies/5133d578-a099-4230-b2ac-ed6e40b4dd35/images/ava/652165161.png"
                }
            ]
        },
        {
            "section": "about",
            "images": [
                {
                    "imageName": "156481233333",
                    "imageUrl": "Data/companies/5133d578-a099-4230-b2ac-ed6e40b4dd35/images/factory/156481233333.png"
                },
                {
                    "imageName": "156481233332",
                    "imageUrl": "Data/companies/5133d578-a099-4230-b2ac-ed6e40b4dd35/images/factory/156481233332.png"
                }
            ]
        }
    ]
}
```

##
### Company Short Info
```js
GET {{host}}/api/companies/{companyId}/short-info
```

#### Response
```js
200 Ok
```

```json
{
  "vendorId": "5133d578-a099-4230-b2ac-ed6e40b4dd35",
  "vendorType": "company",
  "shopContact": "Андрей В.",
  "contactPersonPosition": "Менеджер по продажам",
  "contactImageUrl": "Data/companies/5133d578-a099-4230-b2ac-ed6e40b4dd35/images/personal/8097645556.jpg",
  "isContactShown": true,
  "shopName": "MWInformTech",
  "isSideInfoShown": true,
  "isCompanySideSectionShown": true,
  "isCompanyAboutShown": true,
  "companyName": "OOO \"ИнформТехнологии\"",
  "shopRating": null,
  "startOnMarketDate": "2024-06-20T11:16:28.500179Z",
  "businessType": "Компания",
  "employeesNumber": 30,
  "foundationDate": "2024-06-20T11:16:28.500268Z"
}
```

#
# Cart

### Get Active Cart
```js
GET {{host}}/api/carts
```
#### Response
```js
200 Ok
```
```js
Params: searchTerms: {ProductName/Article}
```
```json
{
  "cartId": "61ac4c04-df80-494c-b744-e9f5b980ef29",
  "isSaved": false,
  "cartItems": [
    {
      "cartItemId": "4af8c547-2aaa-4871-8535-629ff962455a",
      "productVariantId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
      "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
      "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
      "sku": "58745216",
      "article": 24941315,
      "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png",
      "seller": "MWInformTech",
      "regularPrice": 27000,
      "price": 15952,
      "quantity": 2,
      "isFavourite": true
    },
    {
      "cartItemId": "66a5db41-834f-4ca4-a7cf-8651d53d0ed5",
      "productVariantId": "3912995a-31fa-480c-aefb-fa932fab6513",
      "productName": "Смартфон Xiaomi Redmi 12",
      "productHandle": "Смартфон-Redmi-Note-12-5f3778dd-9284-48dc-a8fe-52dc41577373",
      "sku": "58745219",
      "article": 24941315,
      "productImageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585055.png",
      "seller": "MWInformTech",
      "regularPrice": 17990,
      "price": 10261,
      "quantity": 3,
      "isFavourite": false
    },
    {
      "cartItemId": "741a6785-e01d-4726-b5e3-53cd47d9bff2",
      "productVariantId": "d1a2d81c-98b6-4bd9-b14f-0b43a2a2dd54",
      "productName": "Смартфон Xiaomi Redmi 12",
      "productHandle": "Смартфон-Redmi-Note-12-5f3778dd-9284-48dc-a8fe-52dc41577373",
      "sku": "58745218",
      "article": 24941315,
      "productImageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585049.png",
      "seller": "MWInformTech",
      "regularPrice": 17990,
      "price": 10261,
      "quantity": 1,
      "isFavourite": false
    },
    {
      "cartItemId": "bbca2c0c-d5fd-4a72-a072-4bde6be64c4a",
      "productVariantId": "c2d998f1-d31d-48bc-a50f-633e4ef4b6c1",
      "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
      "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
      "sku": "58745217",
      "article": 24941315,
      "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951723789.png",
      "seller": "MWInformTech",
      "regularPrice": 27000,
      "price": 15952,
      "quantity": 1,
      "isFavourite": false
    },
    {
      "cartItemId": "c035e48c-2a2c-49f5-b108-f2d1ef852c41",
      "productVariantId": "1c2cb05c-f950-4d40-a6b6-b6678e6d5b08",
      "productName": "Смартфон Xiaomi Redmi 12",
      "productHandle": "Смартфон-Redmi-Note-12-5f3778dd-9284-48dc-a8fe-52dc41577373",
      "sku": "58745220",
      "article": 24941315,
      "productImageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585059.png",
      "seller": "MWInformTech",
      "regularPrice": 17990,
      "price": 10261,
      "quantity": 1,
      "isFavourite": false
    }
  ]
}
```

### Add To Cart
```js
POST {{host}}/api/carts/cartItems
```
#### Request
```js
[
  {
    "productVriantId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
    "count": 2
  },
  {
    "productVriantId": "d1a2d81c-98b6-4bd9-b14f-0b43a2a2dd54",
    "count": 1
  }
]
```

#### Response
```js
200 Ok
```
```js
{
    "message": "Cart Items successfuly added"
}
```

### Change Cart Item Count
```js
PATCH {{host}}/api/carts/cartItem
```

#### Request
```js
{
  "cartItemId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
  "quantity": 2
}
```

#### Response
```js
200 Ok
```
```js
{
    "cartItemId": "1edca0a2-f2fc-44ee-8f00-f77b37ca47b2",
    "quantity": 2
}
```

### Delete Cart Item
```js
DELETE {{host}}/api/carts/cartItem/{cartItemId}
```

#### Response
```js
200 Ok
```
```js

{
    "cartItemId": "dc775944-1819-40a6-bd9b-823d5e4a829a",
    "message": "Successfuly removed"
}
```

### Delete Range Cart Items
```js
POST {{host}}/api/carts/removeCartItems
```
#### Response
```js
200 Ok
```
```js
[
    {
        "cartItemId": "fee5eb02-cde4-4227-a6da-434eb27022c2"
    }
]
```

### Cart Checkout
```js
GET {{host}}/api/carts/{cartId}/checkout
```

#### Response
```js
200 Ok
```

```js
{
    "proudctCountInCart": 4,
    "totalRegularPrice": 80970,
    "savings": 34235,
    "totalPrice": 46735
}
```


### Cart Item Available
```js
POST {{host}}/api/carts/productAvailable
```

#### Request
```js
{
  "cartItemId": "1edca0a2-f2fc-44ee-8f00-f77b37ca47b2",
  "quantity": 110
}
```
#### Response
```js
200 Ok
```

```js
{
    "cartItemId": "1edca0a2-f2fc-44ee-8f00-f77b37ca47b2",
    "canChange": false,
    "requestedQuantity": 110,
    "inventoryLevel": 10,
    "nextRemainingStock": 0,
    "message": "Insufficient stock, only 10 items available"
}
```

#### Response Messages Types
```js
1. Нет товара на складе (inventoryLevel = 0)
"message": "Item is unavailable"

2. Количество запрашиваемого товара выше имеющегося на складе (quantity > inventoryLevel)
"message": "Insufficient stock, only {inventoryLevel} items available"

3. Превышено значение остаточного товара (для Cart item count + 1) - случай не наступает
"message": "Next remaining stock will exhausted"
```

### Get Current Cart Status
```js
GET {{host}}/api/carts/{cartId}/currentStatus
```
#### Response
```js
200 Ok
```

```js
{
    "cartId": "cbd90aef-8adc-4f82-8b25-285e85e40b37",
    "cartItemsState": [
        {
            "cartItemId": "4cfb4f1d-0fac-4691-8bb7-0ac6b548d6b8",
            "updatedQuantity": 2,
            "setQuantity": 2,
            "isQuantityChanged": false,
            "message": "No changes"
        },
        {
            "cartItemId": "de466632-2224-4be6-b21c-066e48408cf3",
            "updatedQuantity": 3,
            "setQuantity": 3,
            "isQuantityChanged": false,
            "message": "No changes"
        }
    ]
}
```
#### Response messages
```js
"message": "No changes"
"message": "No item in the stock"
"message": "Exceeds stock. Changed to inventoryLevel"
```

### Generate Cart Report Link
```js
GET {{host}}/api/carts/{cartId}/generateExel
```
#### Response
```js
200 Ok
```
```js
{
    "cartId": "cbd90aef-8adc-4f82-8b25-285e85e40b37",
    "reportUrl": "Data/cart/cbd90aef-8adc-4f82-8b25-285e85e40b37.xlsx",
    "message": "Successfuly created"
}
```

### Get Saved Carts
```js
GET {{host}}/api/carts/savedCarts
```

#### Response
```js
200 Ok
```
```js
[
    {
        "cartId": "4892a224-05aa-4888-84c0-091aa8b95f07",
        "cartNumber": 25649875,
        "cartPrice": 15952,
        "shortCartItemImagesPreview": [
            {
                "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951723789.png"
            }
        ],
        "hiddenItemImagesCount": 0,
        "createDate": "2024-07-19T19:48:09.318Z",
        "cartItems": [
            {
                "cartItemId": "1edca0a2-f2fc-44ee-8f00-f77b37ca47b2",
                "productVariantId": "c2d998f1-d31d-48bc-a50f-633e4ef4b6c1",
                "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
                "sku": "58745217",
                "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
                "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951723789.png",
                "seller": "MWInformTech",
                "price": 15952,
                "quantity": 1
            }
        ]
    },
    {
        "cartId": "cbd90aef-8adc-4f82-8b25-285e85e40b37",
        "cartNumber": 25649875,
        "cartPrice": 46735,
        "shortCartItemImagesPreview": [
            {
                "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png"
            },
            {
                "imageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585049.png"
            },
            {
                "imageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585055.png"
            }
        ],
        "hiddenItemImagesCount": 1,
        "createDate": "2024-07-19T19:48:09.319106Z",
        "cartItems": [
            {
                "cartItemId": "4cfb4f1d-0fac-4691-8bb7-0ac6b548d6b8",
                "productVariantId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
                "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
                "sku": "58745216",
                "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
                "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png",
                "seller": "MWInformTech",
                "price": 15952,
                "quantity": 2
            },
            {
                "cartItemId": "dc775944-1819-40a6-bd9b-823d5e4a829a",
                "productVariantId": "d1a2d81c-98b6-4bd9-b14f-0b43a2a2dd54",
                "productHandle": "Смартфон-Redmi-Note-12-5f3778dd-9284-48dc-a8fe-52dc41577373",
                "sku": "58745218",
                "productName": "Смартфон Xiaomi Redmi 12",
                "productImageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585049.png",
                "seller": "MWInformTech",
                "price": 10261,
                "quantity": 1
            },
            {
                "cartItemId": "de466632-2224-4be6-b21c-066e48408cf3",
                "productVariantId": "3912995a-31fa-480c-aefb-fa932fab6513",
                "productHandle": "Смартфон-Redmi-Note-12-5f3778dd-9284-48dc-a8fe-52dc41577373",
                "sku": "58745219",
                "productName": "Смартфон Xiaomi Redmi 12",
                "productImageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585055.png",
                "seller": "MWInformTech",
                "price": 10261,
                "quantity": 3
            },
            {
                "cartItemId": "fee5eb02-cde4-4227-a6da-434eb27022c2",
                "productVariantId": "1c2cb05c-f950-4d40-a6b6-b6678e6d5b08",
                "productHandle": "Смартфон-Redmi-Note-12-5f3778dd-9284-48dc-a8fe-52dc41577373",
                "sku": "58745220",
                "productName": "Смартфон Xiaomi Redmi 12",
                "productImageUrl": "Data/products/5f3778dd-9284-48dc-a8fe-52dc41577373/images/main/6699585059.png",
                "seller": "MWInformTech",
                "price": 10261,
                "quantity": 1
            }
        ]
    }
]
```

### Delete Cart
```js
DELETE {{host}}/api/carts
```
#### Response
```js
200 Ok
```
```js
{
    "cartId": existingCart.CartId,
    "message": "Cart seccessfyly deleted"
}
```

### Delete Saved Carts
```js
POST {{host}}/api/carts/removeSavedCarts
```

#### Request
```js
[
  {
    "cartId": "d6e9d606-cd1f-4730-9fe4-a85df96ecdd0"
  },
  {
    "cartId": "1491c76d-5d84-478c-882a-98b7dd984f63"
  }
]
```

#### Response
```js
200 Ok
```
```js
[
  {
    "cartId": "d6e9d606-cd1f-4730-9fe4-a85df96ecdd0"
  },
  {
    "cartId": "1491c76d-5d84-478c-882a-98b7dd984f63"
  }
]
```

### Save Cart
```js
POST {{host}}/api/carts/{cartId}/saveCart
```
#### Response
```js
200 Ok
```
```js
{
    "cartId": "283cf2b6-9945-4986-b229-18213b6e5a75",
    "message": "Cart saved successfully"
}
```

### Restore Saved Cart
```js
POST {{host}}/api/carts/savedRestore
```
#### Request
```js
[
  {
    "cartId": "d6e9d606-cd1f-4730-9fe4-a85df96ecdd0"
  },
  {
    "cartId": "1491c76d-5d84-478c-882a-98b7dd984f63"
  }
]
```
#### Response
```js
200 Ok
```
```js
{
    "cartId": "283cf2b6-9945-4986-b229-18213b6e5a75"
}
```

### Checkout Saved Carts
```js
POST {{host}}/api/carts/savedCheckout
```

#### Request
```js
[
  {
    "cartId": "d6e9d606-cd1f-4730-9fe4-a85df96ecdd0"
  },
  {
    "cartId": "1491c76d-5d84-478c-882a-98b7dd984f63"
  }
]
```

#### Response
```js
200 Ok
```
```js
{
    "totalProductCount": 4,
    "totalPrice": 52426
}
```

# Favourites

### Get Favourite Products
```js
GET {{host}}/api/favourites
```
### Parameters
```js
productCategoryId
```

#### Response
```js
200 Ok
```
```js
{
    "favourites": [
        {
            "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
            "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
            "sku": "58745216",
            "price": 15952,
            "regularPrice": 27000,
            "isSecondHand": false,
            "isDiscounted": true,
            "isAvailable": true,
            "images": [
                {
                    "imageName": "6951689772",
                    "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png"
                },
                {
                    "imageName": "6706256093",
                    "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6706256093.png"
                },
                {
                    "imageName": "6951689770",
                    "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689770.png"
                },
                {
                    "imageName": "6706256091",
                    "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6706256091.png"
                },
                {
                    "imageName": "6706256094",
                    "imageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6706256094.png"
                }
            ]
        }
    ],
    "productCategories": [
        {
            "categoryName": "Смартфоны",
            "categoryId": "781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a"
        }
    ]
}
```

### Add Favourite Products
```js
POST {{host}}/api/favourites/favourite/{productVariantId}
```
#### Response
```js
200 Ok
```
```js
{
    "message": "Successfuly added"
}
```

### Delete Favourite Products
```js
DELETE {{host}}/api/favourites/favourite/{productVariantId}
```
#### Response
```js
200 Ok
```
```js
{
    "message": "Successfuly removed"
}
```
