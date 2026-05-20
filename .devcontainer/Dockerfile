ENV XRAY_VERSION=v26.3.27

COPY setup.sh /tmp/setup.sh
COPY config.json /etc/xray/g2ray.json

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       bash curl wget unzip ca-certificates openssl \
    && rm -rf /var/lib/apt/lists/* \
    && chmod +x /tmp/setup.sh \
    && /tmp/setup.sh \
    && rm /tmp/setup.sh

# Download Enhanced Geosite and GeoIP data files (Loyalsoldier)
RUN curl -sL https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat -o /usr/local/bin/geosite.dat \
    && curl -sL https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat -o /usr/local/bin/geoip.dat

# Fixed connection string script
RUN echo 'printf "\\n\\033[1;32m[g2ray]\\033[0m Connection string:\\nvless://2166797c-f912-4a24-a467-c5d0c216cc2d@\${CODESPACE_NAME}-443.app.github.dev:443?encryption=none&security=tls&type=xhttp&mode=packet-up&sni=\${CODESPACE_NAME}-443.app.github.dev&path=%2F#g2ray-optimized\\n"' >> /etc/bash.bashrc

EXPOSE 443

CMD ["/usr/local/bin/xray", "run", "-c", "/etc/xray/g2ray.json"]
