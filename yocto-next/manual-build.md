
```
docker run -it \
    --mount type=bind,src=.,dst=/work/bcedge \
    --mount type=volume,src=yocto-build-cache,target=/work/build-cache \
    yocker-build-env
```




