FROM python:3.8-buster as builder

WORKDIR /opt/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt


FROM python:3.8-slim-buster as runner

COPY --from=builder /usr/local/lib/python3.8/site-packages /usr/local/lib/python3.8/site-packages
COPY --from=builder /usr/local/bin/uwsgi /usr/local/bin/uwsgi

RUN apt update \
  && apt install -y libxml2 \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /opt/app

COPY uwsgi.ini ./

WORKDIR /opt/app/src
COPY src ./

CMD ["uwsgi", "/opt/app/uwsgi.ini"]
