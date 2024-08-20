FROM python:2.7

RUN git clone https://github.com/plantismash/webserver.git; \
    cd webserver; \
    pip install -r requirements.txt; \
    pip install uwsgi

COPY ./settings.cfg /webserver/settings.cfg

ENV WEBSMASH_CONFIG=/webserver/settings.cfg

WORKDIR /webserver

STOPSIGNAL SIGINT

CMD uwsgi --pythonpath /var/www --http :8950 --module websmash:app --uid 33 --gid 33 --touch-reload /tmp/reload_websmash --daemonize /var/log/uwsgi.log

EXPOSE 8950
