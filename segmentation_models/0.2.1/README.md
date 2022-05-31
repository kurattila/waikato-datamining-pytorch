# Segmentation Models

Uses [Segmentation Models](https://github.com/qubvel/segmentation_models.pytorch) ([documentation](https://smp.readthedocs.io/en/v0.2.1/)). 

Uses PyTorch 1.9.0, CUDA 11.1 and Segmentation Models 0.2.1.

Though Segmentation Models is installed via a wheel file, you can find its source code \
inside the container in:

```commandline
/opt/segmentation_models
```

Additional code is located in:

```commandline
/opt/segmentation_models_ext
```

## Version

Segmentation Models github repo hash:

```
a288d337821716ad67125127b5dd96a1cd833391
```

and timestamp:

```
October 26, 2021
```

## Docker

### Quick start

* Log into registry using *public* credentials:

  ```commandline
  docker login -u public -p public public.aml-repo.cms.waikato.ac.nz:443 
  ```

* Pull and run image (adjust volume mappings `-v`):

  ```commandline
  docker run --runtime=nvidia --shm-size 8G --net=host \
    -v /local/dir:/container/dir \
    -it public.aml-repo.cms.waikato.ac.nz:443/pytorch/segmentation_models:0.2.1
  ```

  **NB:** For docker versions 19.03 (`docker version`) and newer, use `--gpus=all` instead of `--runtime=nvidia`.

* If need be, remove all containers and images from your system:

  ```commandline
  docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q) && docker system prune -a
  ```

### Docker hub

The image is also available from [Docker hub](https://hub.docker.com/u/waikatodatamining):

```
waikatodatamining/segmentation_models:0.2.1
```

### Build local image

* Build the image from Docker file (from within /path_to/segmentation_models/0.2.1)

  ```commandline
  docker build -t segmentation_models .
  ```
  
* Run the container

  ```commandline
  docker run --runtime=nvidia --shm-size 8G --net=host -v /local/dir:/container/dir -it segmentation_models
  ```
  `/local/dir:/container/dir` maps a local disk directory into a directory inside the container

## Pre-built images

* Build

  ```commandline
  docker build -t segmentation_models:0.2.1 .
  ```
  
* Tag

  ```commandline
  docker tag \
    segmentation_models:0.2.1 \
    public-push.aml-repo.cms.waikato.ac.nz:443/pytorch/segmentation_models:0.2.1
  ```
  
* Push

  ```commandline
  docker push public-push.aml-repo.cms.waikato.ac.nz:443/pytorch/segmentation_models:0.2.1
  ```
  If error "no basic auth credentials" occurs, then run (enter username/password when prompted):
  
  ```commandline
  docker login public-push.aml-repo.cms.waikato.ac.nz:443
  ```
  
* Pull

  If image is available in aml-repo and you just want to use it, you can pull using following command and then [run](#run).

  ```commandline
  docker pull public.aml-repo.cms.waikato.ac.nz:443/pytorch/segmentation_models:0.2.1
  ```
  If error "no basic auth credentials" occurs, then run (enter username/password when prompted):
  
  ```commandline
  docker login public.aml-repo.cms.waikato.ac.nz:443
  ```
  Then tag by running:
  
  ```commandline
  docker tag \
    public.aml-repo.cms.waikato.ac.nz:443/pytorch/segmentation_models:0.2.1 \
    pytorch/segmentation_models:0.2.1
  ```
  
* <a name="run">Run</a>

  ```commandline
  docker run --runtime=nvidia --shm-size 8G --net=host \
    -v /local/dir:/container/dir -it pytorch/segmentation_models:0.2.1
  ```
  `/local/dir:/container/dir` maps a local disk directory into a directory inside the container


## Permissions

When running the docker container as regular use, you will want to set the correct
user and group on the files generated by the container (aka the user:group launching
the container):

```commandline
docker run -u $(id -u):$(id -g) -e USER=$USER ...
```

## Caching

Segmentation models will download pretrained models and cache them locally. To avoid having
to download them constantly, you can the cache directory to the host machine:

* when running the container as `root`

  ```commandline
  -v /some/where/cache:/root/.cache \
  ```

* when running the container as current user

  ```commandline
  -v /some/where/cache:/.cache \
  ```


## Scripts

The following additional scripts are available:

* `sm_predict` - for generating batch predictions on images (calls `/opt/segmentation_models_ext/predict.py`)
* `...` - 


## Examples

### Car segmentation

A simple [car segmentation example](https://github.com/qubvel/segmentation_models.pytorch/blob/master/examples/cars%20segmentation%20(camvid).ipynb) 
is included in the image:

```
cd /opt/segmentation_models_ext
python3 cars_segmentation.py
```

**NB:** This will clone the repository with the [data](https://github.com/alexgkendall/SegNet-Tutorial/tree/master/CamVid)
in the `/opt/segmentation_models_ext` directory. If you want to avoid re-cloning it, then copy 
the `cars_segmentation.py` script into a directory that is mapped to a directory on the host, e.g.:

```bash
docker run --runtime=nvidia -u $(id -u):$(id -g) -e USER=$USER --shm-size 8G --net=host \
    -v `pwd`:/workspace \
    -v `pwd`/cache:/.cache \
    -it waikatodatamining/segmentation_models:0.2.1

cp /opt/segmentation_models_ext/cars_segmentation.py
cd /workspace/
python3 cars_segmentation.py
```