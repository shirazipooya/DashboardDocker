FROM ubuntu:latest

SHELL [ "/bin/bash", "--login", "-c" ]

ENV MINICONDA_DIR $HOME/miniconda3

RUN apt-get update

RUN apt-get upgrade -y

RUN apt-get install -y --no-install-recommends \
    git \
    wget \
    curl \
    g++ \
    ca-certificates \
    build-essential \
    libpq-dev \
    python3-dev

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh

RUN bash ~/miniconda.sh -b -p $MINICONDA_DIR

RUN rm ~/miniconda.sh

ENV PATH=$MINICONDA_DIR/bin:$PATH

RUN echo ". $MINICONDA_DIR/etc/profile.d/conda.sh" >> ~/.profile

RUN conda init bash

RUN conda install -y pip

RUN conda update -y conda

COPY ./app/requirements.txt requirements.txt

RUN pip install -r requirements.txt

RUN rm requirements.txt

RUN mkdir /app

ADD ./app /app

WORKDIR /app

CMD gunicorn -b 0.0.0.0:5000 app:app