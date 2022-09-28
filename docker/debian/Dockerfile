FROM debian:bullseye
FROM python:3.10-buster

SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

RUN apt-get update && \
    apt-get install -y software-properties-common \
                       apt-transport-https \
                       wget \
                       ca-certificates \
                       gnupg2 \
                       unzip \
                       zip \
                       ghostscript \
                       python3-dev \
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

RUN jupyter nbextension enable --py widgetsnbextension --sys-prefix && python -m ipykernel install --user --name="autoeq"

WORKDIR /root
RUN wget -O- https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor | tee /usr/share/keyrings/google-chrome.gpg
RUN echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" | tee /etc/apt/sources.list.d/google-chrome.list
RUN apt-get update && \
    apt-get install -y google-chrome-stable

RUN mkdir /root/chromedriver
RUN CHROME_VERSION=$(google-chrome --product-version | grep -o "[^\.]*\.[^\.]*\.[^\.]*") && \
    CHROMEDRIVER_VERSION=$(curl -s "https://chromedriver.storage.googleapis.com/LATEST_RELEASE_$CHROME_VERSION") && \
    wget -q --continue -P /root/chromedriver "http://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip" && \
    unzip /root/chromedriver/chromedriver* -d /usr/local/bin/

WORKDIR /root/AutoEq
