FROM python:3.7-buster
# Importante python 3.7 por la version permitida en AWS Glue
# https://docs.aws.amazon.com/glue/latest/dg/release-notes.html

RUN groupadd --gid 1000 admin && useradd --uid 1000 --gid admin --shell /bin/bash --create-home admin
ENV POETRY_HOME=/usr/local
# Install node prereqs, nodejs and yarn
# Ref: https://deb.nodesource.com/setup_14.x
# Ref: https://yarnpkg.com/en/docs/install
RUN \
  echo "deb https://deb.nodesource.com/node_16.x buster main" > /etc/apt/sources.list.d/nodesource.list && \
  wget -qO- https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
  echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \
  wget -qO- https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
  apt-get update && \
  apt-get install -yqq nodejs yarn && \
  pip install -U pip && pip install pipenv && \
  npm i -g npm@^8 && \
  curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/install-poetry.py | python - && \
  rm -rf /var/lib/apt/lists/*

USER admin
WORKDIR /home/admin/

## Add the path where all pip packages (Jupyter, Jupyter Labs, etc.) are hosted.

ENV PATH="/home/admin/.local/bin:${PATH}"

RUN pip install jupyter && \
    pip install jupyterlab && \
    pip install sparkmagic && \
    jupyter nbextension enable --py widgetsnbextension && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager && \
    pip show sparkmagic

RUN cd ./.local/lib/python3.7/site-packages && \
    jupyter-kernelspec install sparkmagic/kernels/sparkkernel --user && \
    jupyter-kernelspec install sparkmagic/kernels/pysparkkernel --user && \
    cd sparkmagic && \
    wget -O config.json https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json

EXPOSE 8888
EXPOSE 8998

## Build ##
# docker build -t rafafrdz/sandbox-jupyter-spark .

## Execute a shell and then jupyter lab ##
# docker run -it --rm -p 8888:8888 -p 8998:8998 rafafrdz/sandbox-jupyter-spark /bin/bash
# jupyter lab --ip 0.0.0.0 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''

## Execute jupyter lab ##
# docker run -it --rm -p 8888:8888 -p 8998:8998 rafafrdz/sandbox-jupyter-spark jupyter lab --ip 0.0.0.0 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''