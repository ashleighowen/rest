# Resources and Representations

##<a name="schema"></a>Schema

* This data model is describing retail stores and their aisles.

###<a name="schema-base"></a>Base

* [base.schema.json](https://github.concur.com/developer/api/blob/master/schema/base.schema.json)
* The base schema provides a starting point for all documents.
* It is the default schema for all documents.
* It is the schema used for: metadata, HATEOAS operations and errors (should the occur).

Name | Type | Format | Description
-----|------|--------|------------
`href`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|Hyperlink to this document, matches the URI the client code used.
`id`|`string`|-|For relational database systems to save the identifier separate from the URI template. When combined with the `template` key results in the `href` key value.
`template`|`string`|[URI Template](https://tools.ietf.org/html/rfc6570)|For relational database systems to save the template separate from the identifier.
`schema`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|**Required** Hyperlink to the schema describing this document.
`operations`|`array`|[`operation`](#schema-base-operation)|The hypermedia as the engine of application state (HATEOAS) information.
`errors`|`array`|[`error`](#schema-base-error)|The errors collection for when the response code is 4xx.

####<a name="schema-base-operation"></a>Operation

Name | Type | Format | Description
-----|------|--------|------------
`rel`|`string`|[RFC 5988 Relation Type](https://tools.ietf.org/html/rfc5988#section-4)|**Required** Relation type as defined by the server. There are registered relation types listed in [RFC 5988 6.2.2. Initial Registry Contents](https://tools.ietf.org/html/rfc5988#section-6.2.2) including pagination relation types of `next`, `prev`, `first` and `last`.
`href`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|**Required** Hyperlink to the resource. This key name is borrowed from the HTML `href` element and behaves similarly.
`method`|`string`|[RFC 7231 Methods](https://tools.ietf.org/html/rfc7231#section-4.3)|Default is GET -  (GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE) + RFC 5789 PATCH method.

####<a name="schema-base-error"></a>Error

Name | Type | Format | Description
-----|------|--------|------------
`errors`|`array`|[`error`](#schema-base-error)|**Required** An array of errors.

#####<a name="schema-base-error"></a>Error Schema
Name | Type | Format | Description
-----|------|--------|------------
`errorCode`|`string`|-|**Required** Machine readable code associated with the error. Examples: `dateTimeMissing`, `OutOfMem`, `invalidUser`. Contextual strings are recommended over numbers or UUIDs.
`errorMessage`|`string`|-|**Required** Message associated with the error.
`dataPath`|`string`|-|Relative data path.
`schemaPath`|`string`|-|Relative schema path.
`errors`|`array`|[`error`](#schema-base-error)|An array of errors. Note: this points to this schema as errors can nest.

###<a name="schema-list"></a>List

* [list.schema.json](https://github.concur.com/developer/api/blob/master/schema/list.schema.json)
* This schema is used for listing out related data.

Name | Type | Format | Description
-----|------|--------|------------
`rel`|`string`|[RFC 5988 Relation Type](https://tools.ietf.org/html/rfc5988#section-4)|**Required** Relation type as defined by the server.
`method`|`string`|[RFC 7231 Methods](https://tools.ietf.org/html/rfc7231#section-4.3)|The method available for all items in the list. Default is GET -  (GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE) + RFC 5789 PATCH method.
`itemschema`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|**Required** Hyperlink to the schema describing the items in the list.
`items`|`array`|[`items`](#schema-list-items)|**Required** The list of `href` items.

####<a name="schema-list-items"></a>Items

Name | Type | Format | Description
-----|------|--------|------------
`href`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|**Required** Hyperlink to the resource. This key name is borrowed from the HTML `href` element and behaves similarly.

###<a name="schema-store"></a>Store

* [store.v1.schema.json](https://github.concur.com/developer/api/blob/master/schema/store.v1.schema.json)
* Versioning in the schema name allows the service to revise schema with versioning.
	* Generally speaking, you can rather easily add new, non-required keys to existing schema without breaking clients as they should be coded in such a way as to ignore keys they don't understand. This type of change can be considered a breaking change.
	* This makes it relatively easy to introduce new schema features or breaking changes (such as new and required keys) without breaking existing clients.

Name | Type | Format | Description
-----|------|--------|------------
`schema`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|**Required** - Hyperlink to the schema describing this document.
`name`|`string`|-|**Required** - Store name. Examples: `Piggly Wiggly`, `Safeway` and `Albertsons`.
`phone`|`string`|-|Phone number of the store.

###<a name="schema-aisle"></a>Aisle

* [aisle.v1.schema.json](https://github.concur.com/developer/api/blob/master/schema/aisle.v1.schema.json)
* See note about versioning in the Store schema.

Name | Type | Format | Description
-----|------|--------|------------
`schema`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|**Required** - Hyperlink to the schema describing this document.
`store`|`string`|[RFC 3986 URI](https://www.ietf.org/rfc/rfc3986.txt)|**Required** - Hyperlink to the store where this aisle is located.
`name`|`string`|-|**Required** - Aisle name. Examples: `Baking` and `Dairy`.
`number`|`integer`|-|**Required** - Aisle number. Examples: `1` and `2`.

## Start - Find all of the operations currently available

* This is the only URI the client code must know about ahead of time.
* The llustration starts with zero data.
* The example shows two different services (`stores/v1` and `aisles/v1`).
	* This is for human readability -- they could be the same exact service.
* There isn't an `id` or `template` here because they aren't needed for the index.
* The server can make additional data available here, like links to schema(s).

### Request

`GET https://example.com/stores/v1`

### Response

```
200 OK
```

```json
{
	"href": "https://example.com/stores/v1",
	"schema": "https://example.com/schemas/base.schema.json",
  "alternates": [
    {
      "rel": "all-stores",
      "href": "https://example.com/stores/v1/all"
    }
  ],
	"operations": [
		{
			"rel": "create-store",
			"href": "https://example.com/stores/v1",
			"method": "POST"
		}
	]
}
```

## Create a store

* Client code is creating the data.
* The URI for where to POST comes from the service index: `create-store`.
* The schema is defined by the server and is used by the client code to specify the data being sent by the client.
	* `schema` key comes from `base.schema.json`.
	* `name` and `phone` comes from `store.v1.schema.json`

### Request

`POST https://example.com/stores/v1`

```json
{
	"schema": "https://example.com/schemas/store.v1.schema.json",
	"name": "Alpha",
	"phone": "(425) 555-1212"
}
```

### Response

* Note the additional metadata leveraging `base.schema.json`: `href`, `id` and `template`.
* The client is given the hyperllink for an `operation` on the store for adding an aisle.
	* This hyperlink is at the heart of Hypermedia as the Engine of Application State.
	* `operations` is also defined in `base.schema.json`.

```
201 Created
Location: https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b
```

```json
{
	"schema": "https://example.com/schemas/store.v1.schema.json",
	"name": "Alpha",
	"phone": "(425) 555-1212",
	"href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
	"id": "97b83a73-5620-465c-b8a0-1bf82392336b",
	"template": "https://example.com/stores/v1/{id}",
	"operations": [
		{
			"rel": "add-aisle",
			"href": "https://example.com/aisles/v1",
			"method": "POST"
		}
	]
}
```

## Create some aisles

* Relationships between documents are established.
* The URI to accomplish the task was provided by the response to creating a store in `"rel": "add-aisle"`.
* The response contains an operation link to replace the aisle data using a PUT.

### Request

`POST https://example.com/aisles/v1`

```json
{
	"schema": "https://example.com/schemas/aisle.v1.schema.json",
	"store": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
	"name": "Dairy",
	"number": "10"
}
```

### Response

```
201 Created
Location: https://example.com/aisles/v1/b29e4945-2ebf-4625-b44d-e3034ad99e4d
```

```json
{
	"schema": "https://example.com/schemas/aisle.v1.schema.json",
	"store": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
	"name": "Dairy",
	"number": "10",
	"href": "https://example.com/aisles/v1/b29e4945-2ebf-4625-b44d-e3034ad99e4d",
	"id": "b29e4945-2ebf-4625-b44d-e3034ad99e4d",
	"template": "https://example.com/aisles/v1/{id}",
	"operations": [
		{
			"rel": "update-aisle",
			"href": "https://example.com/aisles/v1/b29e4945-2ebf-4625-b44d-e3034ad99e4d",
			"method": "PUT"
		}
	]
}
```

### Request

`POST https://example.com/aisles/v1`

```json
{
	"schema": "https://example.com/schemas/aisle.v1.schema.json",
	"store": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
	"name": "Baking",
	"number": "11"
}
```

### Response

```
201 Created
Location: https://example.com/aisles/v1/a256aabd-e988-4b37-9f1f-99222ebdfe3d
```

```json
{
	"schema": "https://example.com/schemas/aisle.v1.schema.json",
	"store": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
	"name": "Baking",
	"number": "11",
	"href": "https://example.com/aisles/v1/a256aabd-e988-4b37-9f1f-99222ebdfe3d",
	"id": "a256aabd-e988-4b37-9f1f-99222ebdfe3d",
	"template": "https://example.com/aisles/v1/{id}",
	"operations": [
		{
			"rel": "update-aisle",
			"href": "https://example.com/aisles/v1/a256aabd-e988-4b37-9f1f-99222ebdfe3d",
			"method": "PUT"
		}
	]
}
```

## Get the store

* Using the value in the `Location` header or the data in the response document from creating a store perform a `GET` operation.
* The server has made additional operations available:
	* GET the store with a list of aisles.
	* GET the store with aisles data.
	* Not listed is the relation type for the store itself (because that's what we have here).

### Request

`GET https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b`

### Response

```
200 OK
```

```json
{
	"schema": "https://example.com/schemas/store.v1.schema.json",
	"name": "Alpha",
	"phone": "(425) 555-1212",
	"href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
	"id": "a",
	"template": "https://example.com/stores/v1/{id}",
  "alternates": [
    {
      "rel": "store-aisle-list",
      "href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aislelist"
    },
    {
      "rel": "store-aisle-data",
      "href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aisledata"
    }
  ],
	"operations": [
		{
			"rel": "add-aisle",
			"href": "https://example.com/aisles/v1",
			"method": "POST"
		}
	]
}
```

## Get a store and list of aisles

* This representation can be helpful if the related data document for each item in the array is really large, infrequently accessed or accessed individually.
* Also helps mobile clients potentially keep data usage levels lower.
* Note the change of operations:
	* GET the store.
	* GET the store with aisles data.
	* There is no store with list of aisles (because that is the representation we now have).

### Request

`GET https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aislelist`

### Response

```
200 OK
```

```json
{
	"schema": "https://example.com/schemas/store.v1.schema.json",
	"name": "Alpha",
	"phone": "(425) 555-1212",
	"href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aislelist",
	"id": "97b83a73-5620-465c-b8a0-1bf82392336b",
	"template": "https://example.com/stores/v1/{id}/aislelist",
	"lists": [
		{
			"schema": "https://example.com/schemas/aisle.v1.schema.json",
			"listItems": [
				"https://example.com/aisles/v1/b29e4945-2ebf-4625-b44d-e3034ad99e4d",
				"https://example.com/aisles/v1/a256aabd-e988-4b37-9f1f-99222ebdfe3d"
			]
		}
	],
  "alternates": [
    {
      "rel": "store",
      "href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b"
    },
    {
      "rel": "store-aisle-data",
      "href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aisledata"
    }
  ],
	"operations": [
    {
			"rel": "add-aisle",
			"href": "https://example.com/aisles/v1",
			"method": "POST"
		}
	]
}
```

## Get a store with aisles data

* Note the full aisle data is included inline with the stores, complete with operations.
	* This uses `aisle.v1.schema.json` (rather than base.schema.json as in the list of aisles endpoint) because you are getting the actual data rather than a link to the data.
* Note the change of operations:
	* GET the store.
	* GET the store with list of aisles.
	* There is no store with aisles data (because that is the representation we now have).

### Request

`GET https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aisledata`

```json
{
	"schema": "https://example.com/schemas/store.v1.schema.json",
	"name": "Alpha",
	"phone": "(425) 555-1212",
	"href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aisledata",
	"id": "97b83a73-5620-465c-b8a0-1bf82392336b",
	"template": "https://example.com/stores/v1/{id}/aisledata",
	"data": [
		{
			"schema": "https://example.com/schemas/aisles.v1.schema.json",
			"store": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
			"name": "Dairy",
			"number": "10",
			"href": "https://example.com/aisles/v1/b29e4945-2ebf-4625-b44d-e3034ad99e4d",
			"id": "b29e4945-2ebf-4625-b44d-e3034ad99e4d",
			"template": "https://example.com/aisles/v1/{id}",
			"operations": [
				{
					"rel": "update",
					"href": "https://example.com/aisles/v1/1",
					"method": "PUT"
				}
			]
		},
		{
			"schema": "https://example.com/schemas/aisles.v1.schema.json",
			"store": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b",
			"name": "Baking",
			"number": "11",
			"href": "https://example.com/aisles/v1/a256aabd-e988-4b37-9f1f-99222ebdfe3d",
			"id": "a256aabd-e988-4b37-9f1f-99222ebdfe3d",
			"template": "https://example.com/aisles/v1/{id}",
			"operations": [
				{
					"rel": "update",
					"href": "https://example.com/aisles/v1/2",
					"method": "PUT"
				}
			]
		}
	],
	"alternates": [
		{
			"rel": "store",
			"href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b"
		},
		{
			"rel": "store-aisle-list",
			"href": "https://example.com/stores/v1/97b83a73-5620-465c-b8a0-1bf82392336b/aislelist"
		}
	],
  "operations": [
    {
      "rel": "add-aisle",
      "href": "https://example.com/aisles/v1",
      "method": "POST"
    }
  ]
}
```
