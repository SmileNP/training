FROM python:3.7.5-alpine

RUN mkdir /app
WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

LABEL maintainer="!!Smile B <smile.bestybay@nabepero.co.jp>" \
      version="1.0"

CMD flask run --host=0.0.0.0 --port=5000
