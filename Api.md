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
        - [Get Products by User](#get-products-by-user)
        - [Get Product Detail](#get-product-detail)
    - [Reviews](#reviews) 

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
    "reviewsRating": 3.9285714285714284,
    "reviewsCount": 14,
    "questionCount": null,
    "isSecondHand": false,
    "isDiscounted": true,
    "inventoryQuantity": 10,
    "productVendor": {
        "vendorId": "5133d578-a099-4230-b2ac-ed6e40b4dd35",
        "vendorType": "company",
        "shopContact": "Андрей В.",
        "contactPersonPosition": "Менеджер по продажам",
        "shopName": "MWInformTech",
        "companyName": "OOO \"ИнформТехнологии\"",
        "shopRating": null,
        "startOnMarketDate": "2024-06-14T07:47:26.585981Z",
        "businessType": "Компания",
        "employeesNumber": 30,
        "foundationDate": "2024-06-14T07:47:26.58608Z",
        "showSideCompanyInfo": true
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
    ]
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
