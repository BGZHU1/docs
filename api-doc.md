# FINDMINE API

A reference guide to the FINDMINE API

## Item Identification
```http
POST /api/v1/items
```

This request identifies the product that the recommendation will be based upon. The request ensure that the "sample" product of the recommendation has been previously ingested into the FINDMINE system and has been processed. To do so, the endpoint will accept information about the product from the user, process it to conform the provided information to the FINDMINE standards, and finally return a `Localtion` header pointing to the api endpoint where this item can be examined and manipulated. This endpoint will always return a `Location` header with the `item_id` integer, assuming the request was successful.

> notes to pay attention to.

##### Sample Request
```http
POST /api/v1/items HTTP/1.1
Content-Type: application/json

{
	"url": "http://www.sample.com/{route/to/product/page}",
  	"html": "{HTML}",
  	"images": [
    	{
      		"img_url": "http://www.sample.com/product-image1.jpg",
      		"state": 1
    	},
        {
      		"img_url": "http://www.sample.com/product-image2.jpg",
      		"state": 1
    	}
  	]
}
```

##### Sample Response
```http
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://www.findmine.com/api/v1/items/<item_id:int>
```

##### JSON Body Parameters
> All passed parameters will be processed by the FINDMINE system and those parameters that are not specified by explicit numerical representation will be ingested using machine learning and mapping technologies to transform them into explicit IDs

- `url` [ *str* ] **required** - url of the product page that contains the identifiable item.
- `html` [ *str* ] - raw and rendered version of the html code of the product page.
- `words` - TODO
- `images` [ *list[dict]* ] - list of objects containing image information of the sample product.
- `sport` [ *int* | *str* ] - either an exact number representing the sport_id on the FINDMINE system or a string definition of the particular sport of the product.
- `category` [ *int* | *str* ] - either an exact number representing the category_id of the product or a string definition of the category for the product. For the string definition, this may include bread crumbs, category name, etc.
- `title` [*str*] - the title of the sample product.
- `description` [*str*] - description of the product. This can be any combination of words. The more the better. Our system will process out information from this and will help recognize the item better overall.
- `price` [*int* | *float*] - a numerical representation of the price for the product.
- `pid` [*str*] - (Product ID "pid") can be any string that represents a tracking id of the item at question. The purpose of this is to help you track one's products using your own tracking enumeration. FINDMINE will provide a unique id for a processed id in the form of `item_id` besides this value.
- `brand` [*int* | *str*] - either a number representing the explicit `brand_id` as provided by the FINDMINE system or a string definition of the brand name.
- `season` [*int* | *str*] - either the explicit `season_id` as recorded by the FINDMINE system or a string definition of the season name.
- `details` [*dict*] - extra storage information for any additional parameters that do not have direct access via the strucutres described above.

> `pid` does not have to be unique, however, if it is not when searching for those PID's the search result will provide multiple results.

##### Return Codes
- **200** - Item was found on the system.
- **201** - Item was not found but was scraped and added to the system with the help of the provided JSON encoded body parameters.
- **400** - Something is wrong with the request. Either required headers or parameters are missing. Malformed Request.
- **403** - Credentials were not provided with the request and the user is unauthorized to perform the operation.
- **500** - Internal server error. Something went wrong and you should email support@findmine.com

## Generating Recommended Sets
```http
GET /api/v1/match/sets/<item_id:int>
```

blah blah blah some explanations

> notes to pay attention to.

##### Sample Request
```http
GET https://www.findmine.com/api/v1/match/sets/<item_id:int> HTTP/1.1
Referer: http://www.sample.com/{route/to/canonical/product/page}
Accept: */*; application/json
```

##### Sample Response
```http
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://www.findmine.com/api/v1/items/<item_id:int>
```

##### Query Parameters
- `limit` - blah blah blah
- `fields` - blah blah blah
- `store_ids` - blah blah blah
- `published` - blah blah blah
- `branded` - blah blah blah
- `teamed` - blah blah blah
- `sported` - blah blah blah

##### Return Codes
- **200** - Item wa
- **201** - Item was n
- **400** - Something is w
- **403** - Credentials w
- **500** - Internal serv
