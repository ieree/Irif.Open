- [IRif Api](#irif-api)
	- [Category](#category)
		- [Get Categories](#get-categories)
		- [Get Category](#get-category)	
		- [Create Category](#create-category)
		- [Update Category](#update-category)
		- [Delete Category](#delete-category)
	- [SubCategory](#subcategory)
		- [Get SubCategories](#get-subcategories)
		- [Get SubCategoriy](#get-subcategory)	
		- [Create SubCategories](#create-subcategory)
		- [Update SubCategories](#update-subcategory)
		- [Delete SubCategories](#delete-subcategory)
    - [Products](#products)
        - [Get Products by Category](#get-products-by-category)
            - [Variant 1](#variant-1)
            - [Variant 2](#variant-2)
        - [Get Products by User](#get-products-by-user)
        - [Get Product Variations](#get-product-variations)
        - [Get Product Variation](#get-product-variation)
        - [Get Product Options](#get-product-options)
    - [Additional](#additional)
        - [Category Filter Example](#category-filter-example)

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
      "id": 1,
      "categoryName": "Электроника",
      "categoryHandle": "electronica"
   },
   {
      "id": 2,
      "categoryName": "Дом и сад",
      "categoryHandle": "dom-i-sad"
   }
}
```

### Get Category

```js
GET {{host}}/v1/categories/{categoryId}
```

#### Rsponse
```js
200 Ok
```
```json
{
   "id": 1,
   "categoryName": "Электроника",
   "categoryHandle": "electronica",
}
```

#
### Create Category
```js
POST {{host}}/v1/categories
```

#### Request
```json
{
   "categoryName": "Электроника",
   "categoryHandle": "electronica"
}
```

#### Rsponse
```js
200 Ok
```
```json
{
   "id": 1,
   "categoryName": "Электроника",
   "categoryHandle": "electronica"
}
```

##
### Update Category
```js
PATCH {{host}}/v1/categories/{categoryId}
```

#### Request
```json
{
   "categoryName": "Электроника и техника",
   "categoryHandle": "electronica-i-tehnika"
}
```

#### Rsponse
```js
200 Ok
```
```json
{
   "id": 1,
   "categoryName": "Электроника и техника",
   "categoryHandle": "electronica-i-tehnika"
}
```

##
### Delete Category
```js
DELETE {{host}}/v1/categories/{categoryId}
```

#### Rsponse
```js
204 Ok
```

#
#
## SubCategory

### Get SubCategories
```js
GET {{host}}/v1/subCategories
```
#### Rsponse
```js
200 Ok
```
```json
{
   {
      "id": 1,
      "subCategoryName": "Смартфоны и телефоны",
      "subCategoryHandle": "smartfoni-i-telefoni"
   },
   {
      "id": 2,
      "subCategoryName": "Хозяйственные товары",
      "subCategoryHandle": "hozyaystvennye-tovary"
    }
}
```

##
### Get SubCategory
```js
GET {{host}}/v1/subCategories/{subCategoryId}
```
#### Rsponse
```js
200 Ok
```
```json
{
   "id": 3,
   "subCategoryName": "Смартфоны и телефоны",
   "subCategoryHandle": "smartfoni-i-telefoni",
   "path": [
     {
       "name": "Электроника",
       "id": 1
     },
     {
       "name": "Смартфоны и телефоны",
       "id": 3
     }
   ]
}
```

##
### Create SubCategory
```js
POST {{host}}/v1/subCategories
```

#### Request
```json
{
   "categoryName": "Кухонные принадлежности",
   "categoryHandle": "kuhonnye-prinadlezhnosti"
}
```

#### Rsponse
```js
200 Ok
```
```json
{
   "id": 3,
   "categoryName": "Кухонные принадлежности",
   "categoryHandle": "kuhonnye-prinadlezhnosti"
}
```

##
### Update SubCategory
```js
PATCH {{host}}/v1/subCategories/{subCategoryId}
```

#### Request
```json
{
   "categoryName": "Посуда и кухонные принадлежности",
   "categoryHandle": "posuda-i-kuhonnye-prinadlezhnosti"
}
```

#### Rsponse
```js
200 Ok
```
```json
{
   "id": 3,
   "categoryName": "Посуда и кухонные принадлежности",
   "categoryHandle": "posuda-i-kuhonnye-prinadlezhnosti"
}
```

## 
### Delete SubCategory
```js
DELETE {{host}}/v1/subCategories/{subCategoryId}
```

#### Rsponse
```js
204 Ok
```

#
#
## Products

### Get Products by Category
```js
GET {{host}}/v1/categories/{categoryId}/products
```
#### Rsponse
```js
200 Ok
```

## Variant 1
```json
{
  "products": [
    {
      "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
      "productName": "IPhone 14pro",
      "productDescription": "IPhone 14pro",
      "previewImageId": 562641783,
      "productCategoryId": 3,
      "vendor":
      {
        "vendorId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
        "vendorType": "company"
      },
      "productVariants": [
        {
          "id": "bb4ad0e4-1a58-4d7d-a0cc-05bc534e2dae",
          "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c"
          "sku": "72865489",
          "quantityInStock": 10,
          "previewImageId": 562641783,
          "price": "100000",
          "priceBeforeDiscount": "150000",
          "currency": "RUB",
          "height": 170,
          "heightUnit": "mm",
          "width: "60",
          "widthUnit": "mm",
          "grams": 1250,
          "weight": 1.25,
          "weightUit": "kg",
          "options": [
            {
              "optionName": "Color",
              "value": "#000000",
              "parameter": "Black"
            },
            {
              "optionName": "memory",
              "value": "64",
              "parameter": "GB"
            }
          ]
        },
        {
          "id": "br4ad0g6-1n58-4f7t-30re-05bc456e2dae",
          "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c"
          "sku": "32862482",
          "quantityInStock": 3,
          "previewImageId": 264851213,
          "price": "1200",
          "priceBeforeDiscount": "2400",
          "currency": "RUB",
          "options": [
            {
              "optionName": "color",
              "value": "#FF0000",
              "parameter": "Red"
            },
            {
              "optionName": "memory",
              "value": "128",
              "parameter": "GB"
            }
          ]
        }
      ],
        "averageRating": "4.7",
    }
  ]
}
```

##
## Variant 2
```json
{
  "products": [
    {
      "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
      "productName": "IPhone 14pro",
      "productDescription": "IPhone 14pro",
      "previewImageId": 562641783,
      "price": "100000",
      "priceBeforeDiscount": "150000",
      "currency": "RUB",
      "vendor":
      {
        "vendorId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
        "vendorType": "company"
      },
      "productVariants": [
        {
          "id": "bb4ad0e4-1a58-4d7d-a0cc-05bc534e2dae",
          "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c"
          "sku": "72865489",
          "quantityInStock": 10,
          "previewImageId": 562641783,
          "price": "100000",
          "priceBeforeDiscount": "150000",
          "currency": "RUB",
          "height": 170,
          "heightUnit": "mm",
          "width: "60",
          "widthUnit": "mm",
          "grams": 1250,
          "weight": 1.25,
          "weightUit": "kg",
          "option1": "Red",
          "option2": "64 GB",
          "option3": null,
          "characteristics": [
            {
              "name": "Процессор",
              "value": "Tiger T606 (8 ядер), 1,6 ГГц",
            },
            {
              "name": "Диагональ экрана, дюймы:",
              "value": "6.56",
            }
          ]
        },
        {
          "id": "br4ad0g6-1n58-4f7t-30re-05bc456e2dae",
          "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c"
          "sku": "32862482",
          "quantityInStock": 3,
          "previewImageId": 264851213,
          "price": "1200",
          "priceBeforeDiscount": "2400",
          "currency": "RUB",
          "option1": "Black",
          "option2": "128 GB",
          "option3": null,
          "characteristics": [
            {
              "name": "Процессор",
              "value": "Tiger T606 (8 ядер), 1,6 ГГц",
            },
            {
              "name": "Диагональ экрана, дюймы:",
              "value": "6.56",
            }
          ]
        }
      ],
      "options":[
        {
          "id": "594680422",
          "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
          "name": "Color",
          "values": [
            "Red",
            "Black"
          ]
        },
        {
          "id": "3946804235",
          "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
          "name": "Memory",
          "values": [
            "64",
            "128"
          ]
        }
      ]
        "averageRating": "4.7",
    }
  ]
}
```

#
### Get Products by User
```js
GET {{host}}/v1/users/{userId}/products
```
#### Rsponse
```js
200 Ok
```
```json
{
  {
  "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
  "productName": "IPhone 14pro",
  "productDescription": "IPhone 14pro",
  "previewImage": "url",
  "price": "100000",
  "priceBeforeDiscount": "2400",
  "currency": "RUB",
  "productCategoryId": "1",
  "productVariants": [
    {
      "id": "bb4ad0e4-1a58-4d7d-a0cc-05bc534e2dae",
      "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
      "sku": "72865489",
      "quantityInStock": 10,
      "productImage": "url1",
      "price": "100000",
      "priceBeforeDiscount": "2400",
      "currency": "RUB",
      "created_at": "2024-01-02T08:59:11-05:00",
      "updated_at": "2024-01-02T08:59:11-05:00",
    },
    {
      "id": "br4ad0g6-1n58-4f7t-30re-05bc456e2dae",
      "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
      "sku": "32862482",
      "quantityInStock": 3,
      "productImage": "url2",
      "price": "1200",
      "price": "100000",
      "priceBeforeDiscount": "2400",
      "currency": "RUB",
      "created_at": "2024-01-02T08:59:11-05:00",
      "updated_at": "2024-01-02T08:59:11-05:00",
    }
  ]
 },
 {
   "productId": "239df979-9a6f-42ef-a4af-edbcc419182c",
   "productName": "Galaxy A24",
   "productDescription": "Galaxy A24 - это универсальный и мощный смартфон, который предлагает 
    множество функций и возможностей для вашего комфорта и развлечения...",
   "previewImage": "url",
   "productCategoryId": "1",
   "productVariants": [
     {
      "id": "bb4ad0e4-1a58-4d7d-a0cc-05bc534e2dae",
      "productId": "239df979-9a6f-42ef-a4af-edbcc419182c",
      "sku": "72865489",
      "quantityInStock": 10,
      "productImage": "url1",
      "price": "100000",
      "priceBeforeDiscount": "2400",
      "currency": "RUB",
      "created_at": "2024-01-02T08:59:11-05:00",
      "updated_at": "2024-01-02T08:59:11-05:00",
     },
     {
      "id": "br4ad0g6-1n58-4f7t-30re-05bc456e2dae",
      "productId": "239df979-9a6f-42ef-a4af-edbcc419182c",
      "sku": "32862482",
      "quantityInStock": 3,
      "productImage": "url2",
      "price": "1200",
      "price": "100000",
      "priceBeforeDiscount": "2400",
      "currency": "RUB",
      "created_at": "2024-01-02T08:59:11-05:00",
      "updated_at": "2024-01-02T08:59:11-05:00",
     }
   ]
 }
}
```
#
### Get Product Variations
```js
GET {{host}}/v1/products/{productId}/variations
```
#### Rsponse
```js
200 Ok
```
```json
{
  "productVariants": [
    {
      "id": "bb4ad0e4-1a58-4d7d-a0cc-05bc534e2dae",
      "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
      "sku": "72865489",
      "quantityInStock": 10,
      "productImage": "url1",
      "price": "100000",
      "priceBeforeDiscount": "2400",
      "currency": "RUB",
      "created_at": "2024-01-02T08:59:11-05:00",
      "updated_at": "2024-01-02T08:59:11-05:00",
    },
    {
      "id": "br4ad0g6-1n58-4f7t-30re-05bc456e2dae",
      "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
      "sku": "32862482",
      "quantityInStock": 3,
      "productImage": "url2",
      "price": "1200",
      "price": "100000",
      "priceBeforeDiscount": "2400",
      "currency": "RUB",
      "created_at": "2024-01-02T08:59:11-05:00",
      "updated_at": "2024-01-02T08:59:11-05:00",
    }
  ]
}
```

#
### Get Product Variation
```js
GET {{host}}/v1/variations/{variationId}
```
#### Rsponse
```js
200 Ok
```
```json
{
  "id": "bb4ad0e4-1a58-4d7d-a0cc-05bc534e2dae",
  "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
  "sku": "72865489",
  "quantityInStock": 10,
  "productImage": "url1",
  "price": "100000",
  "priceBeforeDiscount": "2400",
  "currency": "RUB",
  "created_at": "2024-01-02T08:59:11-05:00",
  "updated_at": "2024-01-02T08:59:11-05:00",
}
```

#
### Get Product Options
```js
GET {{host}}/v1/options/{variationId}
```
#### Rsponse
```js
200 Ok
```
```json
{
  "id": "3946804235",
  "productId": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
  "name": "Memory",
  "values": [
    "64",
    "128"
  ]
}
```

##
##
##

## Users

### Get User
```js
GET {{host}}/v1/users/{userId}
```
#### Rsponse
```js
200 Ok
```

```json
{
	"id": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
	"firstName": "Иван",
	"secondName": "Кошелков",
	"middleName": "Васильевич",
	"email": "ivanva@gmail.com",
	"phoneNumber": "9252947341",
	"createDate": "9252947341",
	"updateDate": "9252947341",
	"companiesIds":
	[
		"134",
		"135"
	],
	"addressesIds": 
	[
		"243",
		"244"
	]
}
```

```json
{
	"id": "bd9df979-9a6f-42ef-a4af-edbcc419182c",
	"firstName": "Иван",
	"secondName": "Кошелков",
	"middleName": "Васильевич",
	"email": "ivanva@gmail.com",
	"phoneNumber": "9252947341",
	"createDate": "9252947341",
	"updateDate": "9252947341",
	"companiesId":
	[
		"425",
		"456",
		"457"
	],
	"addresses": [
	   {
	      "id": "123",
	      "address1": "Академика Брилева, 17, корп. 2, кв 158",
	      "address2": "Академика Брилева, 18, корп. 3, кв 25",
	      "city": "Москва",
	      "country": "Россия",
	      "region": "Москва",
	      "countryCode": "RU",
	      "countryName": "Russia",
	      "postalCode": "1154896"
	   },
	   {
	      "124",
	      "address1": "Янгеля, 18, корп. 1, кв. 57",
	      "address2": "null",
	      "city": "Одинцово",
	      "Region": "Московская область",
	      "country": "Россия",
	      "countryCode": "RU",
	      "countryName": "Russia",
	      "postalCode": "1154896"
	   }
	]
}
```


##
##
## Additional
### Category Filter Example
```json
[
  {
    "filterName": "Производитель",
    "filterType": "checkbox",
    "filterSettings": ["Apple", "Xiaomi", "Samsung", "Realme", "Honor", "Huawei", "OPPO", "Vivo", "Sony", "Nokia"]
  },
  {
    "filterName": "Встроенная память",
    "filterType": "checkbox",
    "filterSettings": 
      {
        "dbQuery": "SELECT DISTINCT("internalMemory") FROM smartfones"
      }
  },

]
```
