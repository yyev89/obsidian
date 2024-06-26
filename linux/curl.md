get the site from a link to terminal:
```bash
curl https://example.com
```

`-I` show only header information
`-L` follow redirect
`-v` verbose output
`-k` skip SSL or bad self-signed certificate

get output to a file:
```bash
curl -o file.html https://example.com
# Get particular file from the path:
curl -O https://example.com/index.html
```

submit some data with POST request to API:
```bash
curl -d "name=Kevan&salary=200&age=83" https://dummy.com/api/v1/create
# Send in JSON format:
curl -d '{"name":"Kevan","salary:200","age":91}' -H "Content-Type: application/json" https://dummy.com/api/v1/create
```

use different method instead of GET:
```bash
curl -X DELETE https://dummy.com/api/v1/delete/2727
```

connect to specific host on URL:
```bash
curl --connect-to example.com:443:host-47.example.com:443 https://example.com
```