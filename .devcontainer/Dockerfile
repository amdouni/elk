# Utiliser une image de base légère comme Alpine
FROM mcr.microsoft.com/vscode/devcontainers/base:alpine-3.16

# Installer Docker CLI, Docker Compose, et d'autres dépendances
RUN apk add --no-cache \
    docker-cli \
    docker-compose \
    nginx \
    nodejs \
    npm

# Installation de ElasticSearch, Logstash et Kibana
RUN apk add --no-cache openjdk11 && \
    mkdir -p /usr/share/elasticsearch && \
    mkdir -p /usr/share/logstash && \
    mkdir -p /usr/share/kibana

# Installation de ElasticSearch
ENV ES_VERSION=7.17.12
RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$ES_VERSION-linux-x86_64.tar.gz && \
    tar -xzf elasticsearch-$ES_VERSION-linux-x86_64.tar.gz -C /usr/share/elasticsearch --strip-components=1 && \
    rm elasticsearch-$ES_VERSION-linux-x86_64.tar.gz

# Installation de Logstash
ENV LOGSTASH_VERSION=7.17.12
RUN wget https://artifacts.elastic.co/downloads/logstash/logstash-$LOGSTASH_VERSION-linux-x86_64.tar.gz && \
    tar -xzf logstash-$LOGSTASH_VERSION-linux-x86_64.tar.gz -C /usr/share/logstash --strip-components=1 && \
    rm logstash-$LOGSTASH_VERSION-linux-x86_64.tar.gz

# Installation de Kibana
ENV KIBANA_VERSION=7.17.12
RUN wget https://artifacts.elastic.co/downloads/kibana/kibana-$KIBANA_VERSION-linux-x86_64.tar.gz && \
    tar -xzf kibana-$KIBANA_VERSION-linux-x86_64.tar.gz -C /usr/share/kibana --strip-components=1 && \
    rm kibana-$KIBANA_VERSION-linux-x86_64.tar.gz

# Configurer les chemins
ENV PATH="/usr/share/elasticsearch/bin:/usr/share/logstash/bin:/usr/share/kibana/bin:${PATH}"

# Expose Ports
EXPOSE 80 5601 9200 9300

# Démarrage de l'ELK Stack
CMD service nginx start && /usr/share/elasticsearch/bin/elasticsearch & /usr/share/logstash/bin/logstash -f /workspace/elk/logstash/logstash.conf & /usr/share/kibana/bin/kibana
