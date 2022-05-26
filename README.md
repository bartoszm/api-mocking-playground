# Mock solutions

A quick tutorial on running docker versions of HTTP mock servers

## API sprout

### Run as docker

```
docker run -it --rm -v ${pwd}:/tmp -p 8000:8000 danielgtaylor/apisprout "/tmp/petstore-bundled.yaml"
```

To enable request validation `--validate-request`.
This tool cannot resolve cross-file `$ref` thus bundled version of the API is to be used.

No data preferences

- `GET http://127.0.0.1:8000/pets`
- `GET http://127.0.0.1:8000/cats/123`

Indicate example to use

- `GET http://127.0.0.1:8000/pets --header 'Prefer: example=allCats'` as a header

#### Notes

- cannot follow `$ref` in examples
- does support `example` at schema level
- does support `examples` at path level

## Fakeit

### Run as docker

```
docker run -it --rm -v ${pwd}:/tmp -p 8000:8080 realfengjia/fakeit:0.9.2 --use-example --spec "/tmp/petstore.yaml"
```

#### Notes

- latest image (0.10.0) fails with exception using petstore OAS.
- cannot follow `$ref` in examples
- does support `example` at schema level (with `--use-example` enabled)
- does not support `examples` at path level

## Prism

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
