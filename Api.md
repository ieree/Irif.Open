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
        - [Get Product Variations](#get-product-variations)
        - [Get Product Variation](#get-product-variation)
        - [Get Product Options](#get-product-options)

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
### Get Product
```js
GET {{host}}/api/products/{productHandle}
```
### Parameters
sku: 123143432 / default null

#### Rsponse
```js
200 Ok
```
```json
{
  "productName": "Xiaomi mi8",
  "productHandle": "test",
  "productDescription": "1",
  "price": 123124,
  "regularPrice": 123123,
  "discount": 60,
  "reviewsRating": 5,
  "reviewsCount": 24,
  "isSecondHand": false,
  "isDiscounted": false,
  "isFavourite": true,
  "isComparable": true,
  "inventoryQuantity": 2323,
  "countInCart": 10,
  "productVendor": {
    "vendorId": "ed71911d-0604-4fa0-84c3-cbe5299269a4",
    "vendorType": "company"
  },
  "characteristics": [
    {
        "name": "memory",
        "value": "64 Гб"
    }
  ],
  "features": [
    {
        "name": "memory",
        "value": "64 Гб"
    }
  ],
  "options": [
    {
    "name": "color"
    "productVariantId": "ed71911d-0604-4fa0-84c3-cbe5299269a4",
    "sku": "23847387",
    "value": "red"
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
