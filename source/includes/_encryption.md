# Encryption

## Get Pubkey

```shell
curl -A "ScPrime-Agent"  -u "":<apipassword> --data "name=key_to_the_castle" "localhost:4280/pubaccess/pubkey"
curl -A "ScPrime-Agent"  -u "":<apipassword> --data "id=gi5z8cf5NWbcvPBaBn0DFQ==" "localhost:4280/pubaccess/pubkey"
```

> The above command returns JSON structured like this

```json
{
  "pubkey": "BAAAAAAAAABrZXkxAAAAAAAAAAQgAAAAAAAAADiObVg49-0juJ8udAx4qMW-TEHgDxfjA0fjJSNBuJ4a"
}
```

**UNSTABLE - subject to change in v1.4.9**

This endpoint returns the base-64 encoded pubkey stored under that name, or with
that ID.

### HTTP Request
`GET /pubaccess/pubkey/<name|id>`

### URL Parameters

Parameter | Description
--------- | -----------
name | name of the pubkey being queried
id | base-64 encoded ID of the pubkey being queried

### Response Fields

Field | Type | Description
----- | ---- | -----------
pubkey | string | base-64 encoded pubkey


## Get Pubkey ID

```shell
curl -A "ScPrime-Agent"  -u "":<apipassword> --data "name=key_to_the_castle" "localhost:4280/pubaccess/pubkeyid"
```

> The above command returns JSON structured like this

```json
{
  "pubkeyid": "gi5z8cf5NWbcvPBaBn0DFQ=="
}
```

**UNSTABLE - subject to change in v1.4.9**

This endpoint returns the base-64 encoded ID of the pubkey stored under that
name.

### HTTP Request
`GET /pubaccess/pubkeyid/<name>`

### URL Parameters

Parameter | Description
--------- | -----------
name | name of the pubkey being queried

### Response Fields

Field | Type | Description
----- | ---- | -----------
pubkeyid | string | base-64 encoded pubkey ID

## Create Skykey

```shell
curl -A "ScPrime-Agent"  -u "":<apipassword> --data "name=key_to_the_castle" "localhost:4280/pubaccess/createpubkey"
```

> The above command returns JSON structured like this

```json
{
  "pubkey": "BAAAAAAAAABrZXkxAAAAAAAAAAQgAAAAAAAAADiObVg49-0juJ8udAx4qMW-TEHgDxfjA0fjJSNBuJ4a"
}
```

**UNSTABLE - subject to change in v1.4.9**

This endpoint creates a pubkey stored under the given name.

### HTTP Request
`POST /pubaccess/createpubkey/<name>`

### URL Parameters

Parameter | Description
--------- | -----------
name | desired name of the pubkey

### Response Fields

Field | Type | Description
----- | ---- | -----------
pubkey | string | base-64 encoded pubkey

## Add Pubkey

```shell
curl -A "ScPrime-Agent"  -u "":<apipassword> --data "pubkey=BAAAAAAAAABrZXkxAAAAAAAAAAQgAAAAAAAAADiObVg49-0juJ8udAx4qMW-TEHgDxfjA0fjJSNBuJ4a" "localhost:4280/pubaccess/addpubkey"
```

**UNSTABLE - subject to change in v1.4.9**

This endpoint stores the given pubkey with the renter's skykey manager.

### HTTP Request

`POST /pubaccess/addpubkey/<pubkey>`

### URL Parameters

Parameter | Description
--------- | -----------
pubkey | base-64 encoded pubkey

### Response

standard success or error response. See [standard
responses](#standard-responses).
