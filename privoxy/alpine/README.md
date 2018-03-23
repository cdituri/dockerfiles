## cdituri/privoxy

`cdituri/privoxy`


### building


```
docker build -t cdituri/privoxy
```

See [Dockefile](./Dockerfile) for available build ARGs

### usage

```
docker run -d -p 127.0.0.1:8118:8118 cdituri/privoxy
```

Note: privoxy cannot proxy https thus we bind to 127.0.0.1:8118
