# Mock solutions

A quick tutorial on running docker versions of various HTTP mock servers.
This page reports on a simple zero-code zero-configuration (or close to that) apporach.

## API sprout

Link: https://github.com/danielgtaylor/apisprout

### Run as docker

```
docker run -it --rm -v ${pwd}:/tmp -p 8000:8000 danielgtaylor/apisprout "/tmp/petstore-bundled.yaml"
```

To enable request validation `--validate-request`.
This tool cannot resolve cross-file `$ref` thus bundled version of the API is to be used.

### Example requests

No data preferences

- `GET http://127.0.0.1:8000/pets`
- `GET http://127.0.0.1:8000/cats/123`

Indicate example to use

- `GET http://127.0.0.1:8000/pets --header 'Prefer: example=allCats'` as a header

### Notes

- does not suppport external definitions
- cannot follow `$ref` in examples
- does support `example` at schema level
- does support `examples` at path level

## Fakeit

Link: https://github.com/JustinFeng/fakeit

### Run as docker

```
docker run -it --rm -v ${pwd}:/tmp -p 8000:8080 realfengjia/fakeit:0.9.2 --use-example --spec "/tmp/petstore.yaml"
```

### Example requests

No data preferences

- `GET http://127.0.0.1:8000/v1/pets`
- `GET http://127.0.0.1:8000/cats/123`

### Notes

- latest image (0.10.0) fails with exception using petstore OAS.
- cannot follow `$ref` in examples
- does support `example` at schema level (with `--use-example` enabled)
- does not support `examples` at path level

## Mock Server

Link: https://github.com/mock-server/mockserver

### Run as docker

```
docker run -it --rm -v ${pwd}:/tmp -p 8000:1080 mockserver/mockserver
```

Additional step to configure OAS is needed after instance is up and running, e.g. using curl

```bash
curl --location --request PUT 'http://localhost:8000/mockserver/openapi' \
--header 'Content-Type: application/json' \
--data-raw '{
        "specUrlOrPayload": "file:/tmp/petstore-bundled.yaml"
}'
```

### Example requests

No data preferences

- `GET http://127.0.0.1:8000/v1/pets`
- `GET http://127.0.0.1:8000/cats/123`

### Notes

- loading OAS as configuration parameter does not work in current version (5.13.2)
- loading OAS can be done on runtime using mock server REST API
- limited support for examples, uses single
- cannot follow `$ref` in examples
- base support for auto generated response samples

## Open API mocker

Link: https://github.com/jormaechea/open-api-mocker

### Run as docker

```
docker run -it --rm -v ${pwd}:/tmp -p "8000:5000" jormaechea/open-api-mocker -s "/tmp/petstore.yaml"
```

### Example requests

No data preferences

- `GET http://127.0.0.1:8000/v1/pets`
- `GET http://127.0.0.1:8000/cats/123`

Indicate example to use

- `GET http://127.0.0.1:8000/v1/pets --header 'Prefer: example=allCats'` as a header

### Notes

- cannot follow `$ref` in examples
- does support `example` at path level
- does support `examples` at path level
- takes into consideration servers definition when constructing paths

## Prism

Link: https://github.com/stoplightio/prism

### Run as docker

```
docker run -it --rm -v ${pwd}:/tmp -p 8000:4010 stoplight/prism mock -h 0.0.0.0 "/tmp/petstore.yaml"
```

### Example requests

No data preferences

- `GET http://127.0.0.1:8000/pets`
- `GET http://127.0.0.1:8000/cats/123`

Indicate example to use

- `GET http://127.0.0.1:8000/pets?example=allDogs` as a query parameter
- `GET http://127.0.0.1:8000/pets --header 'Prefer: example=allCats'` as a header
