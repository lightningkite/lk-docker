FROM python:3.8-slim

ARG PUID=${PUID:-1000}
ARG PGID=${PGID:-1000}
ENV APP_DIR="/code"
ENV HOME="/home/python"
ENV PYTHON_ENV="/venv"
ENV PATH="${HOME}/.poetry/bin:${APP_DIR}/.venv/bin:${APP_DIR}:${PATH}"
ENV PYTHONUNBUFFERED 1

# create user/group
RUN set -ex \
  && addgroup --gid $PGID python \
  && useradd -u $PUID -g $PGID python \
  && mkdir -p $PYTHON_ENV $HOME $APP_DIR

# create and activate our virtual env
RUN set -ex \
  && python3 -m venv $PYTHON_ENV \
  && echo source $PYTHON_ENV/bin/activate > $HOME/.bashrc \
  && chown -R python.python $PYTHON_ENV $HOME $APP_DIR

# install dev dependencys
RUN set -ex \
  && apt-get update && apt-get install -y --no-install-recommends \
  ssh-client \
  git \
  curl \
  build-essential \
  python3-dev \
  vim \
  graphviz \
  libgraphviz-dev \
  && rm -rf /var/lib/apt/lists/*

USER python
WORKDIR $APP_DIR

# install poetry
RUN set -ex \
  && curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python \
  && $PYTHON_ENV/bin/python $HOME/.poetry/bin/poetry config virtualenvs.create false 
