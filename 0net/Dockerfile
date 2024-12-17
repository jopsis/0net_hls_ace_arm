FROM arm64v8/ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Madrid
SHELL ["/bin/bash", "-c"]

# Instalar dependencias
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y tor git openssl pkg-config libffi-dev build-essential \
                       libtool python3-pip python3-venv wget unzip nano && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Configurar Tor
RUN echo "ControlPort 9051" >> /etc/tor/torrc && \
    echo "CookieAuthentication 1" >> /etc/tor/torrc

# Descargar y configurar ZeroNet
RUN wget https://github.com/zeronet-conservancy/zeronet-conservancy/archive/refs/tags/v0.7.10.zip && \
    unzip v0.7.10.zip && \
    rm v0.7.10.zip
WORKDIR /zeronet-conservancy-0.7.10

# Crear script de inicio
RUN echo '#!/usr/bin/env bash' > 0net-start-venv.sh && \
    echo 'tor &' >> 0net-start-venv.sh && \
    echo 'VENVPATH="venv/bin/activate"' >> 0net-start-venv.sh && \
    echo 'if [ ! -f "$VENVPATH" ]; then' >> 0net-start-venv.sh && \
    echo '    python3 -m venv venv' >> 0net-start-venv.sh && \
    echo 'fi' >> 0net-start-venv.sh && \
    echo 'source "$VENVPATH"' >> 0net-start-venv.sh && \
    echo 'python3 -m pip install -r requirements.txt' >> 0net-start-venv.sh && \
    echo 'python3 zeronet.py --ui_ip 0.0.0.0 --fileserver_port 26552 "$@"' >> 0net-start-venv.sh

RUN chmod +x 0net-start-venv.sh

# Exponer puertos
EXPOSE 43110 26552

# Configurar punto de entrada
ENTRYPOINT ["/bin/bash", "/zeronet-conservancy-0.7.10/0net-start-venv.sh"]
