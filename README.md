# docker-python-data-science

## 基础镜像说明

基于gdal官方的`ubuntu-small-latest-amd64`镜像制作模型基础镜像，在此镜像的基础上安装其它py包，形成以下两个模型基础镜像:

* Dockerfile_ubuntu_small_amd64
* Dockerfile_ubuntu_small_amd64_gpu 附带了tensorflow、keras及cuda驱动

使用以上两个镜像可方便自定义Dockerfile，来构建自己的项目和产品。

## py 和 gcc 版本

Python 3.8.10 (default, Jun  2 2021, 10:49:15)
[GCC 9.4.0] on linux

## Dockerfile_ubuntu_small_amd64

包含以下组件:

```bash
# ubuntu 20
# Package                        Version
# ------------------------------ ---------
# certifi                        2021.10.8
# charset-normalizer             2.0.7
# GDAL                           3.3.0
# idna                           3.3
# numpy                          1.21.2
# opencv-contrib-python-headless 4.5.3.56
# pip                            20.0.2
# PyYAML                         5.4.1
# requests                       2.26.0
# setuptools                     45.2.0
# urllib3                        1.26.7
# wheel                          0.34.2
```


## Dockerfile_ubuntu_small_amd64_gpu

包含以下组件:

```bash
# ubuntu 20
# Package                        Version
# ------------------------------ ---------
# absl-py                        0.15.0
# astunparse                     1.6.3
# cachetools                     4.2.4
# certifi                        2021.10.8
# charset-normalizer             2.0.7
# clang                          5.0
# flatbuffers                    1.12
# gast                           0.4.0
# GDAL                           3.3.0
# google-auth                    2.3.0
# google-auth-oauthlib           0.4.6
# google-pasta                   0.2.0
# grpcio                         1.41.0
# h5py                           3.1.0
# idna                           3.3
# keras                          2.6.0
# Keras-Preprocessing            1.1.2
# Markdown                       3.3.4
# numpy                          1.19.5
# oauthlib                       3.1.1
# opencv-contrib-python-headless 4.5.3.56
# opt-einsum                     3.3.0
# pip                            20.0.2
# protobuf                       3.18.1
# pyasn1                         0.4.8
# pyasn1-modules                 0.2.8
# PyYAML                         5.4.1
# requests                       2.26.0
# requests-oauthlib              1.3.0
# rsa                            4.7.2
# setuptools                     45.2.0
# six                            1.15.0
# tensorboard                    2.7.0
# tensorboard-data-server        0.6.1
# tensorboard-plugin-wit         1.8.0
# tensorflow-estimator           2.6.0
# tensorflow-gpu                 2.6.0
# termcolor                      1.1.0
# typing-extensions              3.7.4.3
# urllib3                        1.26.7
# Werkzeug                       2.0.2
# wheel                          0.37.0
# wrapt                          1.12.1
```

## 自定义Dockerfile

```dockerfile
FROM alg_base_ubuntu_samll_amd64:1.0.0
# or
#FROM alg_base_ubuntu_samll_amd64_gpu:1.0.0


WORKDIR /app  

ADD ./config.json /app/
ADD ./*.py /app/
ADD ./*.yml /app/
ADD ./requirements.txt /app/

RUN apt-get update \
&& sh -c '/bin/echo -e "Y\n" | apt-get install libxml2 libxml2-dev libxslt1.1 libxslt1-dev' \
&& pip3 install  -i https://pypi.mirrors.ustc.edu.cn/simple/ --no-cache-dir -r requirements.txt \
&& apt-get clean &&  apt-get autoclean

CMD ["python3", "/app/your.py"]
```