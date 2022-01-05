# Simple GZIP Proxy

Simple http server configured with GZIP for you to put in front of your endpoints

## Quick start

Create this docker-compose.yml

```yaml

version: '3.7'

services:
  http-proxy:
    image: tiagostutz/simple-gzip-proxy
    ports:
      - "4480:80"
    environment:  
      - PROXY_URI=http://whoami:80
  
  whoami:
    image: containous/whoami
    ports:
      - "4400:80"

```

Run

```bash
docker-compose up
```

Then execute the following curl commands:

Fetch the whoami endpoint directly (no gzip)

```bash
curl 'http://localhost:4400/' -I -H 'Accept: application/xhtml+xml,application/xml;q=0.9,image/avif,*/*' --compressed
```

Fetch the whoami endpoint with gzip proxy

```bash
curl 'http://localhost:4480/' -I -H 'Accept: application/xhtml+xml,application/xml;q=0.9,image/avif,*/*' --compressed
```

Check the response headers. In the second one there is the gzip header `Content-Encoding: gzip`

You can also check this with your browser by opening the URL directly and inspecting the response headers.

### Credits

`gzip_types` from https://github.com/h5bp/server-configs-nginx/blob/main/h5bp/web_performance/compression.conf#L38