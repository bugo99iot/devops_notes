FROM python:3.7

COPY requirements.txt /

RUN mkdir /app
WORKDIR /app

COPY src /app

WORKDIR /app

# create a dir where ml model weights will live
RUN mkdir /data/
RUN mkdir /data/ml_models

RUN pip --no-cache-dir install --upgrade pip && \
    pip --no-cache-dir install -r /requirements.txt

# The enviroment variable prevents .pyc files from being saved
ENV PYTHONDONTWRITEBYTECODE 1

# The enviroment variable ensures that the python output is set straight
# to the terminal with out buffering it first
ENV PYTHONUNBUFFERED 1

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# run command each time a container starts, used here to run migrations
ENTRYPOINT ["/entrypoint.sh"]
