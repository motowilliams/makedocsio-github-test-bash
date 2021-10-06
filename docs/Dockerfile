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

RUN FILE=theme.list && \
    echo 'mkdocs'      >> /$FILE && \
    echo 'readthedocs' >> /$FILE && \
    echo 'material'    >> /$FILE

# //TODO setup this repo to be a pip package
RUN cd /tmp && \
    git clone https://gitlab.com/motowilliams/mkdocs-macros-file-include.git && \
    cd mkdocs-macros-file-include && \
    python3 setup.py install && \
    cd / && \
    rm -rf /tmp/

# these used to be added as files - heredocs would be useful but we don't know
#  if the users CI / local setup will support it
RUN echo "Creating wrapper scripts"                                         && \
    SCRIPT=/usr/local/bin/build                                             && \
    echo '#!/usr/bin/env bash'                                   >> $SCRIPT && \
    echo                                                         >> $SCRIPT && \
    echo 'echo "Setting directory to $DOCS_SRC_PATH"'            >> $SCRIPT && \
    echo 'cd $DOCS_SRC_PATH'                                     >> $SCRIPT && \
    echo                                                         >> $SCRIPT && \
    echo 'echo "Building documentation site"'                    >> $SCRIPT && \
    echo 'mkdocs build'                                          >> $SCRIPT && \
    chmod 755 $SCRIPT                                                       && \
    SCRIPT=/usr/local/bin/serve                                             && \
    echo '#!/usr/bin/env bash'                                   >> $SCRIPT && \
    echo                                                         >> $SCRIPT && \
    echo 'echo "Setting directory to $DOCS_SRC_PATH"'            >> $SCRIPT && \
    echo 'cd $DOCS_SRC_PATH'                                     >> $SCRIPT && \
    echo                                                         >> $SCRIPT && \
    echo 'echo "Serving documentation site on port $LOCAL_PORT"' >> $SCRIPT && \
    echo 'mkdocs serve -v --dev-addr=0.0.0.0:$LOCAL_PORT'        >> $SCRIPT && \
    chmod 755 $SCRIPT
