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



##### Sample Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://www.findmine.com/api/v1/items/{item_id:int}
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
