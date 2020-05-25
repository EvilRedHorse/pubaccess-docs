# Blacklist

## Get Blacklist

```shell
curl -A "ScPrime-Agent" "localhost:4280/pubaccess/blacklist"
```

> The above command returns JSON structured like this

```json
{
  "blacklist": {
    "QAf9Q7dBSbMarLvyeE6HTQmwhr7RX9VMrP9xIMzpU3I"
    "QAf9Q7dBSbMarLvyeE6HTQmwhr7RX9VMrP9xIMzpU3I"
    "QAf9Q7dBSbMarLvyeE6HTQmwhr7RX9VMrP9xIMzpU3I"
  }
}
```

This endpoint returns the list of merkleroots that are blacklisted.

### HTTP Request

`GET /pubaccess/blacklist`

### Response Fields

Field | Type | Description
----- | ---- | -----------
blacklist | object | The blacklist is a list of merkle roots, which are hashes, that are blacklisted.

## Edit Blacklist

```shell
// add publink
curl -A "ScPrime-Agent" --user "":<apipassword> --data '{"add" : ["GAC38Gan6YHVpLl-bfefa7aY85fn4C0EEOt5KJ6SPmEy4g","GAC38Gan6YHVpLl-bfefa7aY85fn4C0EEOt5KJ6SPmEy4g","GAC38Gan6YHVpLl-bfefa7aY85fn4C0EEOt5KJ6SPmEy4g"]}' "localhost:4280/pubaccess/blacklist"

// remove publink
curl -A "ScPrime-Agent" --user "":<apipassword> --data '{"remove" : ["GAC38Gan6YHVpLl-bfefa7aY85fn4C0EEOt5KJ6SPmEy4g","GAC38Gan6YHVpLl-bfefa7aY85fn4C0EEOt5KJ6SPmEy4g","GAC38Gan6YHVpLl-bfefa7aY85fn4C0EEOt5KJ6SPmEy4g"]}' "localhost:4280/pubaccess/blacklist"
```

This endpoint allows to update the list of publinks that should be blacklisted
from Public Portals. It can be used to both add and remove publinks from the blacklist.

### HTTP Request

`POST /pubaccess/blacklist`

### Request Body Fields

<aside class="warning">At least one of the following fields needs to be non empty</aside>

Field | Type | Description
----- | ---- | -----------
add | array of strings | add is an array of publinks that should be added to the blacklist.
remove | array of strings | remove is an array of publinks that should be removed from the blacklist.


### Response Fields

standard success or error response. See [standard
responses](#standard-responses).
