ARG ARCH=
FROM ${ARCH}node:16.14.0-buster AS build
WORKDIR /app
ENV DOCKER=YES NODE_ENV=production
ADD . .
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends build-essential && \
    npm install --production=false && \
    npm run build && \
    npm install -g --unsafe-perm --production
RUN git clone https://github.com/stz2012/recpt1 && cd recpt1/recpt1 && ./autogen.sh && ./configure && make && make install

FROM ${ARCH}node:16.14.0-buster-slim
WORKDIR /app
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        make \
        gcc \
        g++ \
        pkg-config \
        pcscd \
        libpcsclite-dev \
        libccid \
        libdvbv5-dev \
        pcsc-tools \
        dvb-tools \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/lib/node_modules/mirakurun /app
COPY --from=build /usr/local/bin/recpt1 /usr/local/bin/
CMD ["./docker/container-init.sh"]
EXPOSE 40772 9229
