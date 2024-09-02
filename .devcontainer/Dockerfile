# Utiliser une image de base légère comme Ubuntu
FROM ubuntu:22.04

# Installer les dépendances de base
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    gnupg \
    apt-transport-https \
    software-properties-common \
    openjdk-11-jdk \
    nginx \
    && rm -rf /var/lib/apt/lists/*

# Installer Elasticsearch
ENV ES_VERSION=7.17.12
RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$ES_VERSION-amd64.deb && \
    dpkg -i elasticsearch-$ES_VERSION-amd64.deb && \
    rm elasticsearch-$ES_VERSION-amd64.deb

# Installer Logstash
ENV LOGSTASH_VERSION=7.17.12
RUN wget https://artifacts.elastic.co/downloads/logstash/logstash-$LOGSTASH_VERSION-amd64.deb && \
    dpkg -i logstash-$LOGSTASH_VERSION-amd64.deb && \
    rm logstash-$LOGSTASH_VERSION-amd64.deb

# Installer Kibana
ENV KIBANA_VERSION=7.17.12
RUN wget https://artifacts.elastic.co/downloads/kibana/kibana-$KIBANA_VERSION-amd64.deb && \
    dpkg -i kibana-$KIBANA_VERSION-amd64.deb && \
    rm kibana-$KIBANA_VERSION-amd64.deb

# Copier les fichiers de configuration dans les bons emplacements
COPY ../web/nginx.conf /etc/nginx/nginx.conf
COPY ../web/index.html /usr/share/nginx/html/index.html
COPY ../elk/logstash/logstash.conf /etc/logstash/conf.d/logstash.conf

# Exposer les ports
EXPOSE 80 9200 9300 5601 5000

# Démarrage automatique des services lors de l'exécution du conteneur
CMD service elasticsearch start && \
    service logstash start && \
    service kibana start && \
    service nginx start && \
    tail -f /dev/null
