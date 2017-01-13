# FINDMINE API

A reference guide to the FINDMINE API

## Item Identification

```http
POST /api/v1/items
```

blah blah blah some explanations

> notes to pay attention to.



##### Sample Request

```http
POST /api/v1/items HTTP/1.1
Content-Type: application/json

{
	"url": "http://www.sample.com/{route/to/canonical/product/page}",
  	"html": "<html>{raw html of the product page that is loaded}</html>",
  	"images": [
    	{
      		"id_tag": null,
      		"class_tag": null,
      		"xpath": "/html/body/div[1]/div[4]/div/div[2]/segment[1]/div[1]/div[1]/div[1]/div/div[1]/div/div/ul/li[1]/img",
      		"height": 54,
      		"width": 54,
      		"magnitude": 76.36753236814714,
      		"theta": 0.7853981633974483,
      		"img_url": "http://demandware.edgesuite.net/sits_pod20-adidas/dw/image/v2/aaqx_prd/on/demandware.static/-/Sites-adidas-products/en_US/dw16bf7c60/zoom/S97153_21_model.jpg?sw=60&sfrm=jpg",
      		"tag_idx": 1958,
      		"top": 137.0000061035156,
      		"state": 0
    	}
  	]
}
```



##### Sample Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://www.findmine.com/api/v1/items/{item_id:int}
```



##### JSON Body Parameters

- `url` blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah
- `html` - blah blah blah



##### Return Codes

- **200** - Item was found on the system.

- **201** - Item was not found but was scraped and added to the system with the help of the provided JSON encoded body parameters.

- **400** - Something is wrong with the request. Either required headers or parameters are missing. Malformed Request.

- **403** - Credentials were not provided with the request and the user is unauthorized to perform the operation.

- **500** - Internal server error. Something went wrong and you should email support@findmine.com

  ​

## Generating Recommended Sets

```http
GET /api/v1/match/sets/{item_id:int}
```

blah blah blah some explanations

> notes to pay attention to.



##### Sample Request

```http
GET https://www.findmine.com/api/v1/match/sets/{item_id:int} HTTP/1.1
Referer: http://www.sample.com/{route/to/canonical/product/page}
Accept: */*, application/json
```

what about if you call GET /match/sets/ and pass item_ids?

##### Sample Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://www.findmine.com/api/v1/items/{item_id:int}
```



##### Query Parameters

- `limit` - The limit parameter is an integer value that can be used to limit the number of items returned. If limit is not specified, then the default value is 100.

- `fields` - Fields indicates which of the fields associated with an item or set of items you would like returned.

  If you don't specify any fields, the following fields will be returned by default:

  > set_id, item_ids, store_ids, brand_ids, user_id, published, flagged, style_id, style_name, occasion_id, occasion_name, date_created, date_updated, set_items

- `store_ids` - a list of store id values that items in the set can belong to. This makes it possible to generate cross-store outfits. // Not live functionality. 

- `published` - the published parameter is an integer value that indicates whether the returned sets should be published sets, or live on the client site (1) or unpublished sets, prevented from appearing on the client site (0). If the published parameter is not specified, the default value is published (1).

- `branded` - if the branded parameter is passed, then it means that only sets with items of the same brand as the item passed in the request are returned. 

- `teamed` - if the teamed parameter is passed, then only sets with items of the same team as the item passed in the request are returned. (Default)

- `sported` - If the sported parameter is passed, then only sets with items of the same sport as the item passed in the request are returned. (Default)


** potentially we could have seasoned too.. its in the query, but not in match_sets.py

** and flagged?!

##### JSON Body Parameters

An array of JSON objects, each representing a set.

- `brand_ids` - The possible set of brand ids for the items in the returned set. If branded was passed as a parameter, then `brand_ids` will simply be the `brand_id` of the item. If branded was not specified, then the `brand_ids` will be all the brands associated with the store.
- `date_created` - The datetime that the set was created
- `date_updated` - The datetime that the set was last updated
- `flagged` - a `0` or `1` integer indicating whether the set was flagged for review. 
- `href` - blah blah blah
- `item_ids` - blah blah blah
- `occasion_id` - blah blah blah
- `occasion_name` - blah blah blah
- `published` - blah blah blah
- `set_id` - blah blah blah
- `set_items` - an array of JSON objects, each representing an item in the set
  - `brand_id` - blah blah blah
  - `brand_name` - blah 
  - `category_id`
  - `category_name`
  - `colors` - an array of JSON objects, each representing the colors in an item in the set
    - `color_id`
    - `href`
    - `image_id`
  - `date_created`
  - `date_updated`
  - `flagged`
  - `href`
  - `images` - an array of JSON objects, each representing the image of an item in the set
    - `href`
    - `image_id`
    - `img_url`
    - `item_id`
    - `state`
  - `item_id`
  - `item_url`
  - `pid`
  - `price`
  - `season_id`
  - `season_name`
  - `set_id`
  - `sport_id`
  - `sport_name`
  - `stock`
  - `store_id`
  - `store_name`
  - `team_id`
  - `team_name`
  - `title`
  - `user_id`

##### Return Codes

- **200** - Item 

  ​

