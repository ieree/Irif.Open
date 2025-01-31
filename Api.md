- [IRif Api](#irif-api)
  	- [Authentication]
  	  	- [Generate sms](#generate-sms)
  	  	- [Validate](#validate)
        - [Account](#account)
  	     - [Get Account Profiles](#get-account-profiles)
  	     - [Aggregate info](#aggregate-info)
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
        - [Product Variant Available](#product-variant-available)
    - [Reviews](#reviews)
      	- [Get Reviews](#get-reviews)
      	- [Update Reviews](#update-reviews)
      	- [Add Reviews Opinion](#add-reviews-opinion)
      	- [Add Reviews](#add-reviews)
      	- [Upload images](#upload-images)
      	- [Check eligibility](#check-eligibility)
      	- [Get list reviewers](#get-list-reviewers)
    - [Questions](#questions)
	- [Get Questions](#get-questions)
	- [Add question](#add-question)
   	- [Get question senders](#get-question-senders)
    - [User](#user)
        - [Get user profile](#get-user-profile)
    - [Company](#company)
      	- [Company About](#company-about)
      	- [Company Short Info](#company-short-info)
      	- [User companies grid](#user-companies-grid)
      	- [Get company profile](#get-company-profile)
      	- [Add company](#add-company)
      	- [Upadte company card](#update-company-card)
      	- [Delete company](#delete-company)
      	- [Add document](#add-document)
      	- [Delete document](#delete-document)
  - [Cart](#cart)
    - [Get Active Cart](#get-active-cart)
    - [Add To Cart](#add-to-cart)
    - [Change Cart Item Count](#change-cart-item-count)
    - [Delete Cart Item](#delete-cart-item)
    - [Delete Range Cart Items](#delete-range-cart-items)
    - [Select All Cart Items](#select-all-cart-items)
    - [Select Single Cart Item](#select-single-cart-item)
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
     - [Add Multiple Favourites](#add-multiple-favourites)
     - [Delete Favourite Products](#delete-favourite-products)
  -[Orders](#orders)
	- [Get Active Orders](#get-active-orders)
    	- [Get All Orders](#get-all-orders)
       	- [Get Order Details](#get-order-details)
  - [Notification](#notification)
     - [Delete company warning](#delete-compnay-warning)
  - [Seller](#seller)
  	- [Product List To Manage](#product-list-to-manage)
  	- [Linked Products In Manage List](#linked-products-in-manage-list)
  	- [Product details ready to merge](#product-details-ready-to-merge)
   	- [Product details ready merging to existing card](#product-details-ready-merging-to-existing-card)

# IRif Api
Api v1 

## Authentication
### Generate sms
```js
POST {{host}}/api/auth/generate
```
#### Request
```json
{
  "phoneNumber": "9284526577"
}
```

#### Rsponse
```js
200 Ok
```
```json
{
  "phoneNumber": "9284526577",
  "code": 1111,
  "message": "Success"
}
```

### Validate
```js
POST {{host}}/api/auth/validate
```
#### Request
```json
{
  "phoneNumber": "9284526577",
  "code": 1111
}
```
#### Rsponse
```js
200 Ok
```
```json
{
  "userId": "b7b45ac6-52ca-458e-9f03-8fb1e1c14144",
  "phoneNumber": "9284526577",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI3NWU0MTkwOS03ZWJjLTQ1NzctYjgwZS1hMTlmOTc2ZjcyYzIiLCJqdGkiOiI4YjMzMDAwZi1kMTQyLTQzNjItYjFlNS02NzNmY2U3NjFiZjkiLCJuYW1lIjoiOTI4NDUyNjU3NyIsImV4cCI6MTcyNDY1ODM4MCwiaXNzIjoiSXJpZkNvbXBhbnkiLCJhdWQiOiJJcmlmQ29tcGFueSJ9.z5c47LlGMY_JSEyCCf-		ZrIHbU6VHijNqyJRAUCsHbQE"
}
```

## Account
### Get Account Profiles
```js
GET {{host}}/api/acc/profiles
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Rsponse
```js
200 Ok
```
```json
[
    {
        "profileId": "75e41909-7ebc-4577-b80e-a19f976f72c2",
        "profileName": "Михаил К.",
        "type": "user",
        "isHasShop": false,
        "shopName": null
    },
    {
        "profileId": "03b8dbc5-b222-4cb9-8555-7fb55fb6040d",
        "profileName": "ИП \"Кашницкий Михаил Вадимович\"",
        "type": "company",
        "isHasShop": true,
        "shopName": "STelWell"
    }
]
```

### Aggregate info
```js
GET {{host}}/api/acc/aggregateInfo
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Rsponse
```js
200 Ok
```
```json
{
  "userItems": {
    "favourites": {
      "itemCount": 0,
      "itemIds": []
    },
    "cart": {
      "itemCount": 0,
      "itemIds": [
	    "f30c7bc3-c6ba-46a1-9587-137570eea47a"
            ]
    },
    "orders": {
      "itemCount": 0
    }
  }
}
```


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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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

### Product Variant Available
```js
POST {{host}}/api/products/checkAvailable
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
    "productVariantId": "1edca0a2-f2fc-44ee-8f00-f77b37ca47b2",
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

### Add Reviews
```js
POST {{host}}/api/reviews/add
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request

```json
{
  "profileId": "0a59835c-1bcc-4ef3-adba-f369ec2af2ad",
  "productId": "0a59835c-1bcc-4ef3-adba-f369ec2af2ad",
  "ratingValue": 5,
  "experience": "Меньше месяца",
  "advantage": "text",
  "disadvantage": "text",
  "Comment": "text",
  "IsAnonimous": false,
  "profileType": "user"
}
```
#### Response
```js
200 Ok
```
```json
{
  "reviewId": "0a59835c-1bcc-4ef3-adba-f369ec2af2ad",
}
```

### Upload images
```js
POST {{host}}/api/reviews/{reviewId}/upload
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```json
curl -X 'POST' \
  'https://localhost:5001/api/reviews/62f52745-d436-4579-9351-18ddc710d57f/upload' \
  -H 'accept: */*' \
  -H 'Content-Type: multipart/form-data' \
  -F 'request=@1234.png;type=image/png' \
  -F 'request=@1.png;type=image/png'
```



### Check eligibility
```js
POST {{host}}/api/reviews/eligibility
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```json
{
  "productId": "string"
}
```

#### Response
```json
{
  "isEligible": false
}
```

### Get list reviewers
```js
POST {{host}}/api/reviews/reviewers
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```json
{
  "productId": "string"
}
```

#### Response
```json
[
  {
    "reviewerName": "Александр А.",
    "reviewerType": "user"
  }
]
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

### Add question
```js
POST {{host}}/api/questions/add
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```json
{
  "productId": "string",
  "profileId": "string",
  "profileType": "user",
  "questionText": "string",
  "isAnonimous": true
}
```

#### Response
```json
 {
    "reviewId": "b4ebd06c-fffc-4594-a86f-d37ad5c3b76c",
  }
```

### Get question senders
```js
GET {{host}}/api/questions/select-sender
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Response
```json
 {
    "profileId": "b4ebd06c-fffc-4594-a86f-d37ad5c3b76c",
    "name": "ООО Омела",
    "type": "company",
  }
```
##
# User
### Get user profile
```js
GET {{host}}/api/user
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Response
```json
{
	    "userId": "b4ebd06c-fffc-4594-a86f-d37ad5c3b76c",
            "fullName": "Михаил Вадимович Кашницкий",
            "email": "michailktest@gmail.com",
            "phoneNumber": "9284526577",
            "whatsAppMsg": "9284526577",
            "telegramMsg": "9284526577",
            "address": null
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

### User companies grid
```js
GET {{host}}/api/companies/profileGrid
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Response
```js
200 Ok
```

```json
[
    {
        "companyId": "03b8dbc5-b222-4cb9-8555-7fb55fb6040d",
        "companyName": "ИП \"Кашницкий Михаил Вадимович\"",
        "inn": "3813431215",
        "kpp": "381004005",
        "ogrn": "1193450044015",
        "legalAdress": "Москва, Венницкая 15, пом 13/2",
        "accountBalance": 0
    }
]
```

### Get company profile
```js
GET {{host}}/api/companies/{companyId}
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Response
```js
200 Ok
```

```json
{
    "companyId": "03b8dbc5-b222-4cb9-8555-7fb55fb6040d",
    "companyName": "ИП \"Кашницкий Михаил Вадимович\"",
    "inn": "3813431215",
    "kpp": "381004005",
    "ogrn": "1193450044015",
    "bic": "456183681",
    "cleaningAccount": "15465431215487854156",
    "correspondentAccount": "15465421235487854147",
    "bank": null,
    "legalAdress": "Москва, Венницкая 15, пом 13/2",
    "postalAddress": "Москва, Венницкая 15, пом 13/2",
    "phoneNumber": "9252937347",
    "email": "telwell@gmail.com",
    "title": "Генеральный директор",
    "accountType": "Администратор",
    "companyCardLink": "test/link",
    "isSeller": true,
    "documents": [
	{
            "docName": "783264772",
            "docPath": "path/0238927.pdf"
	}
     ]
}
```

### Add company
```js
POST {{host}}/api/companies/addDetails
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Request
```json
{
  "companyName": "string",
  "inn": "string",
  "kpp": "string",
  "ogrn": "string",
  "legalAdress": "string",
  "postalAdress": "string"
  ""
}
```

#### Response
```js
200 Ok
```


### Upadte company card
```js
POST {{host}}/api/companies/updateProfile
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Request
```json
{
  "companyId": "string",
  "companyName": "string",
  "kpp": "string",
  "bic": "string",
  "cleaningAccount": "string",
  "correspondentAccount": "string",
  "bank": "string",
  "legalAdress": "string",
  "postalAdress": "string",
  "phoneNumber": "string",
  "email": "string",
  "position": "Генеральный директор"
}
```

#### Response
```js
200 Ok
```


### Delete company
```js
DELETE {{host}}/api/companies/{companyId}
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Request
```json
{
  "companyId": "string",
  "companyName": "string",
  "kpp": "string",
  "bic": "string",
  "cleaningAccount": "string",
  "correspondentAccount": "string",
  "bank": "string",
  "legalAdress": "string",
  "postalAdress": "string",
  "phoneNumber": "string",
  "email": "string",
  "position": "Генеральный директор"
}
```

#### Response
```js
200 Ok
```


### Add document
```js
POST {{host}}/api/companies/{companyId}/uploadFile
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

```js
multipart/form-data
```

#### Request
```json
{
	
}
```

#### Response
```js
200 Ok
```


### Delete document
```js
DELETE {{host}}/companies/document/{documentId}
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Response
```js
200 Ok
```


#
# Cart

### Get Active Cart
```js
GET {{host}}/api/carts
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
  "cartId": "cbd90aef-8adc-4f82-8b25-285e85e40b37",
  "isSaved": false,
  "cartItems": [
    {
      "cartItemId": "1edca0a2-f2fc-44ee-8f00-f77b37ca47b2",
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
      "inventoryLevel": 10,
      "isAvailable": true,
      "checked": true,
      "isFavourite": false
    }
  ],
  "cartLinks": {
    "summaryUrl": "Data\\cart\\Report.xlsx"
  }
}
```

### Add To Cart
```js
POST {{host}}/api/carts/cartItems
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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

### Select All Cart Items
```js
POST {{host}}/api/carts/cartItems/selectAll
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```js
{
  "action": "select"
}
```
```js
{
  "action": "unselect"
}
```
#### Response
```js
200 Ok
```

### Select Single CartItem
```js
POST {{host}}/api/carts/cartItems/selectSingle
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```js
{
  "cartItemId": "fee5eb02-cde4-4227-a6da-434eb27022c2",
  "action": "select"
}
```
```js
{
  "cartItemId": "fee5eb02-cde4-4227-a6da-434eb27022c2",
  "action": "unselect"
}
```
#### Response
```js
200 Ok
```

### Cart Checkout
```js
GET {{host}}/api/carts/{cartId}/checkout
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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

### Add Multiple Favourites
```js
POST {{host}}/api/favourites/addRange
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Request
```js
[
  {
    "productVariantId": "781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a"
  }
]
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
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
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


# Orders

### Add orders
```js
POST {{host}}/api/orders
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```js
{
  "profileId": "string",
  "profileType": "string",
  "orderItems": [
    {
      "productVariantId": "string",
      "quantity": 0,
      "price": 0
    }
  ]
}
```
#### Response
```js
200 Ok
```

### Get Active Orders
```js
GET {{host}}/api/current-orders
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
```js
params:
profileId=781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a,
profileType=user/company
```

#### Response
```js
200 Ok
```
```js
[
    {
        "orderId": "52d976fa-ef0c-49a6-a27d-0c183f81681a",
        "orderNumber": "123456",
        "orderingDate": "2024-10-24T07:09:36.925775Z",
        "orderPrice": 15952,
        "orderStatus": "preparing",
        "orderStatusDisplay": "Собирается",
        "orderPositions": [
            {
                "sellerName": "MWInformTech",
                "deliveryData": {
                    "deliveryStatus": "completed",
                    "deliveryStatusDisplay": "Доставлен",
                    "deliveryMethod": "Достака продавцом",
                    "deliveryAddress": "Москва, Аббасова12, 2, 123",
                    "deliveryDate": "2024-10-24T07:12:45.384358Z",
                    "deliveryDateDisplay": "Дата получения: 24 октября 2024"
                },
                "orderPositionDetails": [
                    {
                        "productVariantId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
                        "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
                        "sku": "58745216",
                        "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
                        "productCount": 1,
                        "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png"
                    }
                ]
            }
        ]
    }
]
```

### Get All Orders
```js
GET {{host}}/api/all-orders
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
```js
params:
profileId=781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a,
profileType=user/company
orderType=product/service
dateSort=2024
```

#### Response
```js
200 Ok
```
```js
{
    "orders": [
        {
            "orderId": "d536fbbc-d510-48a3-b54e-75e5d42effd3",
            "orderNumber": "123123123",
            "orderDate": "2024-10-24T07:09:36.925777Z",
            "orderStatus": "completed",
            "orderStatusDisplay": "Заказ доставлен",
            "orderPrice": 15952,
            "orderBilsLink": "/Data/document.pdf",
            "additionalProductCount": 0,
            "productImages": [
                {
                    "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png"
                }
            ],
            "orderedProducts": [
                {
                    "productVariantId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
                    "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
                    "sku": "58745216",
                    "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
                    "seller": "MWInformTech",
                    "orderLineQuantity": 1,
                    "orderLinePrice": 15952,
                    "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png"
                }
            ]
        }
    ],
    "sortingData": [
        {
            "sortingOrderType": "product",
            "sortingOrderTypeDisplay": "Заказы товаров",
            "years": [
                {
                    "year": "2024"
                }
            ]
        }
    ]
}
```

### Get Order Details
```js
GET {{host}}/api/orders/{orderId}/details
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Response
```js
200 Ok
```
```js
{
    "orderId": "52d976fa-ef0c-49a6-a27d-0c183f81681a",
    "orderNumber": "123123123",
    "customer": "Иван Терещенко, тел. +79284526573",
    "recipient": "Иван Терещенко, тел. +79284526573",
    "paymentMethod": "bank_card",
    "totalProductCount": 1,
    "deliveryPrice": 1000,
    "orderTotalPrice": 15952,
    "orderStatus": "preparing",
    "orderStatusDisplay": "Собирается",
    "orderDocs": "Data/docs.pdf",
    "orderLines": [
        {
            "deliveryStatus": "completed",
            "deliveryStatusDisplay": "Доставлен",
            "deliveryDateLineDisplay": "Дата получения: 24 октября 2024",
            "partialCount": 1,
            "deliveryData": {
                "deliveryMethod": "Достака продавцом",
                "deliveryAddress": "Москва, Аббасова12, 2, 123"
            },
            "groupedProducts": [
                {
                    "productVariantId": "aaafd9b4-6c68-45a7-9f97-46c0b16c8ae8",
                    "productHandle": "Смартфон-Redmi-Note-13-ea69c644-c2c1-414e-862a-fe8705e8781a",
                    "sku": "58745216",
                    "sellerName": "MWInformTech",
                    "productName": "Xiaomi Смартфон Redmi Note 13 Ростест (EAC)",
                    "productImageUrl": "Data/products/ea69c644-c2c1-414e-862a-fe8705e8781a/images/main/6951689772.png",
                    "productLineFullPrice": 15952
                }
            ]
        }
    ]
}
```


# Notification
### Delete company warning
```js
GET {{host}}/api/company/{companyId}/deleteWarning
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Response
```js
200 Ok
```
```js
{
  "hasProductsInSale": true,
  "balance": 0
}
```

# Seller
### Product List To Manage
```js
GET {{host}}/api/seller/{profileId}/products
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Response
```js
200 Ok
```
```js
{
    "products": [
        {
            "productId": "9ef26781-e5e6-4043-866c-32856b834b3e",
            "productName": "Xiaomi test phone",
            "article": "123434355-45",
            "barcode": "1231231",
            "brand": "Xiaomi",
            "brandHandle": "xiaomi",
            "categoryName": "Смартфоны",
            "categoryHandle": "smartfoni-781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a",
            "reviewCount": 0,
            "rating": null,
            "productStatus": "В черновиках",
            "createDate": "2025-01-13T09:03:12.127025Z",
            "updateDate": null,
            "haveLinked": false,
            "linkedCount": 0
        },
        {
            "productId": "a6e24a0d-ebe7-4815-9463-f766b1379699",
            "productName": "Xiaomi test2 phone",
            "article": "1233123-423",
            "barcode": "1231231",
            "brand": "Xiaomi",
            "brandHandle": "xiaomi",
            "categoryName": "Смартфоны",
            "categoryHandle": "smartfoni-781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a",
            "reviewCount": 0,
            "rating": null,
            "productStatus": "В черновиках",
            "createDate": "2025-01-13T10:20:47.522603Z",
            "updateDate": null,
            "haveLinked": false,
            "linkedCount": 0
        },
        {
            "productId": "db47b836-6eec-4dfa-a71b-eaa134fd5666",
            "productName": "Xiaomi test3 phone",
            "article": "123434355-45",
            "barcode": "4896522661",
            "brand": "Xiaomi",
            "brandHandle": "xiaomi",
            "categoryName": "Смартфоны",
            "categoryHandle": "smartfoni-781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a",
            "reviewCount": 0,
            "rating": null,
            "productStatus": "Ожидает действия",
            "createDate": "2024-12-24T16:03:15.246Z",
            "updateDate": null,
            "haveLinked": true,
            "linkedCount": 1
        }
    ],
    "tabsCount": {
        "all": 3,
        "draft": 2,
        "archived": 0,
        "moderating": 0,
        "removed": 0
    },
    "filters": [
        {
            "filterName": "category",
            "filterValues": [
                "smartfoni"
            ]
        },
        {
            "filterName": "brand",
            "filterValues": [
                "xiaomi"
            ]
        },
        {
            "filterName": "rating",
            "filterValues": [
                "1",
                "2",
                "3",
                "4",
                "5"
            ]
        },
        {
            "filterName": "status",
            "filterValues": [
                "active",
                "draft",
                "approved",
                "pendingApproval",
                "removed"
            ]
        }
    ]
}
```
```js
Params
GET {{host}}/api/seller/{profileId}/products?searchTerms=Смартфон&statusTabs=draft&sortColumn=name&sortOrder=desc&smartfoni=xiaomi
```
searchTerms - поиск по наименованию, баркоду, артикулу, 
statusTabs: draft, archive
sortColumn: name, category, article, barcode, brand, create, update
sortOrder: asc, desc

### Linked Products In Manage List
```js
GET {{host}}/seller/linked-products/{productId}
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Response
```js
200 Ok
```
```js
[
    {
        "productId": "4c227b08-8d77-4a33-90e2-b4735db2bda4",
        "productVariantId": "a776f5e8-8112-4119-96bf-432beeabf498",
        "productName": "Xiaomi test4 phone",
	"productImgPath": "/dsdsd/1.png"
        "article": "121121121",
        "barcode": "4896522661"
    }
]
```

### Product details ready to merge
```js
GET {{host}}/seller/products/merge-details
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```js
[
  "9ef26781-e5e6-4043-866c-32856b834b3e",
  "a6e24a0d-ebe7-4815-9463-f766b1379699",
  "9bd4a2a7-3a29-49fb-8bcc-aee875b0172f"
]
```
#### Response
```js
200 Ok
```
```js
[
    {
        "productId": "9bd4a2a7-3a29-49fb-8bcc-aee875b0172f",
        "productName": "Мягкая игрушка Кот-Батон, цвет серый, 60 см",
        "article": "",
        "productImagePath": "",
        "mergeStatus": "Не возможно объеденить",
        "isUniqueVariant": false,
        "categoryOption": {
            "categoryId": "88827e1e-bd91-4517-bfda-b7591b077199",
            "categoryName": "Для малышей",
            "isValidCategory": false
        },
        "variantCharacteristicsOptions": [
            {
                "optionName": "Цвет",
                "optionVerified": false,
                "optionValue": "Розовый"
            }
        ]
    },
    {
        "productId": "9ef26781-e5e6-4043-866c-32856b834b3e",
        "productName": "Xiaomi test phone",
        "article": "123434355-45",
        "productImagePath": "",
        "mergeStatus": "Готов к объеденению",
        "isUniqueVariant": true,
        "categoryOption": {
            "categoryId": "781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a",
            "categoryName": "Смартфоны",
            "isValidCategory": true
        },
        "variantCharacteristicsOptions": [
            {
                "optionName": "Цвет",
                "optionVerified": true,
                "optionValue": "Красный"
            },
            {
                "optionName": "Встроенная память",
                "optionVerified": true,
                "optionValue": "256 Гб"
            }
        ]
    },
    {
        "productId": "a6e24a0d-ebe7-4815-9463-f766b1379699",
        "productName": "Xiaomi test2 phone",
        "article": "1233123-423",
        "productImagePath": "",
        "mergeStatus": "Не возможно объеденить",
        "isUniqueVariant": false,
        "categoryOption": {
            "categoryId": "781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a",
            "categoryName": "Смартфоны",
            "isValidCategory": true
        },
        "variantCharacteristicsOptions": [
            {
                "optionName": "Цвет",
                "optionVerified": true,
                "optionValue": "Красный"
            },
            {
                "optionName": "Встроенная память",
                "optionVerified": true,
                "optionValue": "256 Гб"
            }
        ]
    }
]
```


### Product details ready merging to existing card
```js
GET {{host}}/seller/products/merge-details
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### Request
```js
[
  "9ef26781-e5e6-4043-866c-32856b834b3e",
  "a6e24a0d-ebe7-4815-9463-f766b1379699",
  "9bd4a2a7-3a29-49fb-8bcc-aee875b0172f"
]
```
#### Response
```js
200 Ok
```
```js
{
    "linkedProductsToCard": [
        {
            "productId": "db47b836-6eec-4dfa-a71b-eaa134fd5666",
            "categoryName": "Смартфоны",
            "mergeStatus": "Готов к объединению",
            "productVariants": [
                {
                    "productName": "Xiaomi test4 phone",
                    "article": "",
                    "productImagePath": "",
                    "variantCharacteristicsOptions": [
                        {
                            "optionId": "9a106785-47c6-4f06-9284-5d8bf3478a33",
                            "optionName": "Цвет",
                            "optionValue": "Белый"
                        },
                        {
                            "optionId": "9641e761-2741-4b6c-9be8-844ee3970bd1",
                            "optionName": "Встроенная память",
                            "optionValue": "128 Гб"
                        }
                    ]
                },
                {
                    "productName": "Xiaomi test3 phone",
                    "article": "123434355-45",
                    "productImagePath": "",
                    "variantCharacteristicsOptions": [
                        {
                            "optionId": "9a106785-47c6-4f06-9284-5d8bf3478a33",
                            "optionName": "Цвет",
                            "optionValue": "Белый"
                        },
                        {
                            "optionId": "9641e761-2741-4b6c-9be8-844ee3970bd1",
                            "optionName": "Встроенная память",
                            "optionValue": "256 Гб"
                        }
                    ]
                }
            ]
        }
    ],
    "productsToMerge": [
        {
            "productId": "9ef26781-e5e6-4043-866c-32856b834b3e",
            "productName": "Xiaomi test phone",
            "article": "123434355-45",
            "productImagePath": "",
            "mergeStatus": "Готов к объеденению",
            "isUniqueVariant": true,
            "categoryOption": {
                "categoryId": "781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a",
                "categoryName": "Смартфоны",
                "isValidCategory": true
            },
            "variantCharacteristicsOptions": [
                {
                    "optionName": "Цвет",
                    "optionVerified": true,
                    "optionValue": "Красный"
                },
                {
                    "optionName": "Встроенная память",
                    "optionVerified": true,
                    "optionValue": "256 Гб"
                }
            ]
        },
        {
            "productId": "a6e24a0d-ebe7-4815-9463-f766b1379699",
            "productName": "Xiaomi test2 phone",
            "article": "1233123-423",
            "productImagePath": "",
            "mergeStatus": "Нельзя объеденить",
            "isUniqueVariant": false,
            "categoryOption": {
                "categoryId": "781001bc-3a72-4e5b-8d2a-ee22e0ea7b0a",
                "categoryName": "Смартфоны",
                "isValidCategory": true
            },
            "variantCharacteristicsOptions": [
                {
                    "optionName": "Цвет",
                    "optionVerified": true,
                    "optionValue": "Красный"
                },
                {
                    "optionName": "Встроенная память",
                    "optionVerified": true,
                    "optionValue": "256 Гб"
                }
            ]
        }
    ]
}
```
