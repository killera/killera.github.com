### docker data volumes

* volume can be shared among containers.
* data volumes persist even if the container is deleted.


#### Add a data volume.(-v)  

* docker run -d -P --name web -v /webapp training/webapp python app.py


### Locate a volume

* docker inspect web

```
...
"Mounts": [
    {
        "Name": "fac362...80535",
        "Source": "/var/lib/docker/volumes/fac362...80535/_data",
        "Destination": "/webapp",
        "Driver": "local",
        "Mode": "",
        "RW": true,
        "Propagation": ""
    }
]
...
```


#### Mount a host directory as a data volume

* docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py


#### tell docker engine to remove data volume when the container is removed.

* docker run --rm -v /foo -v awesome:/bar busybox top

* docker volume rm $(docker volume ls -qf dangling=true)

