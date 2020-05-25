# Pubfile

## Uploading

```shell
curl -A "ScPrime-Agent" -u "":<apipassword> "localhost:4280/pubaccess/pubfile/image.png" --data-binary @image.png
```

```python
from .pubaccess import Pubaccess
publink = Pubaccess.upload_file("./src.jpg")
print("Upload successful, publink: " + publink)
```

```javascript
import { upload } from "pubaccess-js";
const onUploadProgress = (progress, { loaded, total }) => {
  console.info(`Progress ${Math.round(progress * 100)}%`);
};

try {
  const { publink } = await upload(portalUrl, file, { onUploadProgress });
} catch (error) {
  // handle error
}
```
```go
package main

import (
	"fmt"
	skynet "github.com/scpcorp/go-pubaccess"
)
                        
func main() {
	publink, err := pubaccess.UploadFile("./src.jpg", pubaccess.DefaultUploadOptions)
	if err != nil {
		fmt.Printf("Unable to upload: %v", err.Error())
		return
	}
	fmt.Printf("Upload successful, publink: %v\n", publink)
}
```
> The above command returns JSON structured like this

```json
{
"publink": "CABAB_1Dt0FJsxqsu_J4TodNCbCGvtFf1Uys_3EgzOlTcg",
"merkleroot": "QAf9Q7dBSbMarLvyeE6HTQmwhr7RX9VMrP9xIMzpU3I",
"bitfield": 2048
}
```

This endpoint uploads a file to the network using a stream. If the upload call
fails or quits before the file is fully uploaded, the file can be repaired by a
subsequent call to the upload stream endpoint using the `repair` flag.

### HTTP Request
`POST /pubaccess/pubfile/<scprimepath>`

### URL Parameters

Parameter | Description
--------- | -----------
siapath | Location where the file will reside in the renter on the network. The path must be non-empty, may not include any path traversal strings ("./", "../"), and may not begin with a forward-slash character. If the 'root' flag is not set, the path will be prefixed with 'var/pubaccess/', placing the pubfile into the ScPrime system's default pubaccess folder.

### Query String Parameters

<aside class="notice"> All query string parameters are optional </aside>

Parameter | Type | Description
--------- | -----| -----------
basechunkredundancy | uint8 | The amount of redundancy to use when uploading the base chunk. The base chunk is the first chunk of the file, and is always uploaded using 1-of-N redundancy.
convertpath | string | The scprimepath of an existing scprimefile that should be converted to a publink. A new pubfile will be created. Both the new pubfile and the existing scprimefile are required to be maintained on the network in order for the publink to remain active. This field is mutually exclusive with uploading streaming.
filename | string | The name of the file. This name will be encoded into the pubfile metadata, and will be a part of the publink. If the name changes, the publink will change as well.
dryrun | bool | If dryrun is set to true, the request will return the Publink of the file without uploading the actual file to the ScPrime network.
force | bool | If there is already a file that exists at the provided scprimepath, setting this flag will cause the new file to overwrite/delete the existing file. If this flag is not set, an error will be returned preventing the user from destroying existing data.
mode | uint32 | The file mode / permissions of the file. Users who download this file will be presented a file with this mode. If no mode is set, the default of 0644 will be used.
root | bool | Whether or not to treat the scprimepath as being relative to the root directory. If this field is not set, the scprimepath will be interpreted as relative to 'var/pubaccess'.
pubkeyname **UNSTABLE - subject to change in v1.4.9** | string | The name of the pubkey that will be used to encrypt this pubfile. Only the name or the ID of the skykey should be specified.
pubkeyid **UNSTABLE - subject to change in v1.4.9** | string | The ID of the pubkey that will be used to encrypt this pubfile. Only the name or the ID of the pubkey should be specified.

<aside class="warning">the parameters 'pubkeyid' and 'pubkeyname' can not be supplied at the same time</aside>

### HTTP Request Headers

<aside class="notice"> All request headers are optional </aside>

Name | Type | Description
---- | -----| -----------
Content-Disposition | string | If the filename is set in the Content-Disposition field, that filename will be used as the filename of the object being uploaded. Note that this header is only taken into consideration when using a multipart form upload. For more details on setting Content-Disposition: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition
Pubaccess-Disable-Force | bool | This request header allows overruling the behaviour of the `force` parameter that can be passed in through the query string parameters. This header is useful for Publi Portals operators that would like to have some control over the requests that are being passed to spd. To avoid having to parse query string parameters and overrule them that way, this header can be set to disable the force flag and disallow overwriting the file at the given siapath.


### Response Fields

Field | Type | Description
----- | ---- | -----------
publink | string | This is the publink that can be used with the `/pubaccess/publink` GET endpoint to retrieve the file that has been uploaded.
merkleroot | hash | This is the hash that is encoded into the publink.
bitfield | int | This is the bitfield that gets encoded into the publink. The bitfield contains a version, an offset and a length in a heavily compressed and optimized format.
