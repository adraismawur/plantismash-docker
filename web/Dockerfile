FROM python:2.7

RUN git clone https://github.com/plantismash/webserver.git -b dev webserver; \
    cd webserver; \
    pip install -r requirements.txt; \
    pip install uwsgi

COPY ./settings.cfg /webserver/settings.cfg

ENV WEBSMASH_CONFIG=/webserver/settings.cfg

WORKDIR /webserver

STOPSIGNAL SIGINT

CMD uwsgi --pythonpath /var/www --http :8950 --module websmash:app --touch-reload /tmp/reload_websmash

EXPOSE 8950
