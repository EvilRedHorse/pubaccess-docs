# Skylink

## Download File

```shell
# entire file
curl -A "ScPrime-Agent" "localhost:4280/pubaccess/publink/CABAB_1Dt0FJsxqsu_J4TodNCbCGvtFf1Uys_3EgzOlTcg"

# directory
curl -A "ScPrime-Agent" "localhost:4280/pubaccess/publink/CABAB_1Dt0FJsxqsu_J4TodNCbCGvtFf1Uys_3EgzOlTcg/folder"

# sub file
curl -A "ScPrime-Agent" "localhost:4280/pubaccess/publink/CABAB_1Dt0FJsxqsu_J4TodNCbCGvtFf1Uys_3EgzOlTcg/folder/file.txt"
```  

This endpoint downloads a publink using http streaming. This call blocks until
the data is received. There is a 30s default timeout applied to downloading a
publink. If the data can not be found within this 30s time constraint, a 404
will be returned. This timeout is configurable through the query string
parameters.

### HTTP Request
`GET /pubaccess/publink/<publink>`

### URL Parameters

Parameter | Description
--------- | -----------
publink | The publink that should be downloaded. The publink can contain an optional path. This path can specify a directory or a particular file. If specified, only that file or directory will be returned.

### Query String Parameters

Parameter | Type | Description
--------- | -----| -----------
attachment | bool | If 'attachment' is set to true, the Content-Disposition http header will be set to 'attachment' instead of 'inline'. This will cause web browsers to download the file as though it is an attachment instead of rendering it.
format | string | If 'format' is set, the publink can point to a directory and it will return the data inside that directory. Format will decide the format in which it is returned. Currently we only support 'concat', which will return the concatenated data of all subfiles in that directory.
timeout | int | If 'timeout' is set, the download will fail if the Pubfile can not be retrieved  before it expires. Note that this timeout does not cover the actual download time, but rather covers the TTFB. Timeout is specified in seconds, a timeout  value of 0 will be ignored. If no timeout is given, the default will be used, which is a 30 second timeout. The maximum allowed timeout is 900s (15 minutes).

### HTTP Response Headers

Name | Type | Description
---- | -----| -----------
Pubaccess-File-Metadata | PubfileMetadata | The header field "Pubaccess-FileMetadata" will be set such that it has an encoded json object which matches the modules.PubfileMetadata struct. If a path was supplied, this metadata will be relative to the given path.

### HTTP HEAD Request

```shell
curl -I -A "ScPrime-Agent" "localhost:4280/pubaccess/publink/CABAB_1Dt0FJsxqsu_J4TodNCbCGvtFf1Uys_3EgzOlTcg"
```

> The above command returns JSON structured like this

```json
{
"mode":     640,      // os.FileMode
"filename": "folder", // string
"subfiles": [         // []PubfileSubfileMetadata | null
  {
  "mode":         640,                // os.FileMode
  "filename":     "folder/file1.txt", // string
  "contenttype":  "text/plain",       // string
  "offset":       0,                  // uint64
  "len":          6                   // uint64
  }
]
}
```

This curl command performs a HEAD request that fetches the headers for
the given publink. These headers are identical to the ones that would be
returned if the request had been a GET request.
