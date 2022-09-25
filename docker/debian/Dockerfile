FROM debian:bullseye
FROM python:3.10-buster

SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

RUN apt-get update && \
    apt-get install -y python3-dev \
                       python3-pip \
                       python3-venv \
                       libsndfile1
WORKDIR /root/AutoEq

COPY AutoEq/requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt
RUN rm /tmp/requirements.txt

COPY AutoEq/measurements/dev-requirements.txt /tmp/dev-requirements.txt
RUN pip install -r /tmp/dev-requirements.txt
RUN rm /tmp/dev-requirements.txt
