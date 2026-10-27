FROM alpine:3.22
 
# Compilador, ferramentas de build e dependências do raylib
RUN apk add --no-cache \
    ca-certificates \
    wget \
    tar \
    xz \
    build-base \
    clang \
    clang-extra-tools \
    git \
    git-lfs \
    bash \
    && sed -i 's#^root:\(.*\):/bin/ash$#root:\1:/bin/bash#' /etc/passwd

RUN apk add --no-cache jq \
    && ARCH="$(uname -m)" \
    && URL="$(wget -qO- https://ziglang.org/download/index.json | jq -r --arg k "${ARCH}-linux" 'del(.master) | to_entries | sort_by(.key | split(".") | map(tonumber)) | last | .value[$k].tarball')" \
    && wget -q "$URL" -O /tmp/zig.tar.xz \
    && mkdir -p /usr/local/zig \
    && tar -xf /tmp/zig.tar.xz -C /usr/local/zig --strip-components=1 \
    && rm /tmp/zig.tar.xz \
    && apk del jq

ENV PATH="/usr/local/zig:${PATH}"
 
WORKDIR /app
 
# Compile sua aplicação com:
# gcc main.c -o game -lraylib -lGL -lm -lpthread -ldl -lrt -lX11
 
