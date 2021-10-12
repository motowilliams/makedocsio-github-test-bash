FROM alpine:latest

ENV PYTHONUNBUFFERED=1
ENV TZ=US/Mountain
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apk update && \
    apk add --update \
    bash \
    ca-certificates \
    curl \
    docker-cli \
    dos2unix \
    git \
    gnupg \
    make \
    py3-pip \
    python3 \
    tar \
    zip

# https://www.mkdocs.org/ https://mkdocs-macros-plugin.readthedocs.io/ https://squidfunk.github.io/mkdocs-material/
RUN pip3 install --no-cache --upgrade pip setuptools
RUN pip3 install mkdocs mkdocs-macros-plugin mkdocs-material
