FROM python:2.7
MAINTAINER Datalogics <support@datalogics.com>

ARG version=1.0

# Development libraries we'll need on top of python:2.7
RUN apt-get update && apt-get install -y --no-install-recommends \
        less nano \
        python-nose \
        git \
        libjpeg-dev \
    && rm -rf /var/lib/apt/lists/*

# Pull down the LS circulation manager and core submodule.
WORKDIR /var/www
RUN git clone https://github.com/datalogics/circulation.git
WORKDIR circulation
RUN git checkout master #tags/$version
RUN /bin/bash -c 'git config submodule.core.url https://github.com/datalogics/server_core && git submodule update --init --recursive'
COPY post-receive.sample .git/hooks

# Set up the virtual environment and install python libraries
RUN virtualenv env && \
    echo "export SIMPLIFIED_CONFIGURATION_FILE=\"/var/www/circulation/cm.config\"" >> env/bin/activate
RUN /bin/bash -c 'source env/bin/activate && pip install -r requirements.txt && python -m textblob.download_corpora'
RUN mv /root/nltk_data /usr/lib/

VOLUME /var/www/circulation
ENTRYPOINT ["/bin/bash"]
