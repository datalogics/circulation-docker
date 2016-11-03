FROM dl/circ-base:1.0
ENV TZ=US/Eastern

MAINTAINER Datalogics <support@datalogics.com>

RUN apt-get update && apt-get install -y --no-install-recommends \
        cron \
        supervisor \
    && rm -rf /var/lib/apt/lists/*

# Switch to local timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime

# Create cron tasks & logfile
RUN touch /var/log/cron.log
COPY libsimple_crontab /etc/cron.d/circulation

WORKDIR bin

COPY supervisord.conf /etc/supervisor/conf.d
CMD ["-c", "/usr/bin/supervisord"]
