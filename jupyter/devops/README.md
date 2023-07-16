version: '3'
services:
  jupyter-notebook:
    image: kubespacedev/jupyter-devops:latest
    ports:
      - 8888:8888
    volumes:
      - jupyter-notebook-data:/home/kube
volumes:
  jupyter-notebook-data: