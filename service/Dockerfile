######## Fluent Bit

FROM ghcr.io/fluent/fluent-bit/unstable:master as flb-download

######### Go Builder

FROM golang:1.20 as builder

# Working directory
WORKDIR /app

ENV CGO_ENABLED=0

# Copy everything at /app
COPY . /app

COPY fluent-bit.conf /app

# Build the go app
RUN go build -o vivo-service .


# Final image: I am using debian to simplify the dependencies needed by Fluent Bit
FROM debian:bullseye-slim

RUN apt-get update && \
    apt-get install -y --no-install-recommends libyaml-0-2  \
        libssl1.1 \
        libsasl2-2 \
        pkg-config \
        libpq5 \
        zlib1g \
        ca-certificates \
        libatomic1 \
        libgcrypt20 \
        libzstd1 \
        liblz4-1 \
        libgssapi-krb5-2 \
        libldap-2.4-2 \
        libgpg-error0 \
        libkrb5-3 \
        libk5crypto3 \
        libcom-err2 \
        libkrb5support0 \
        libgnutls30 \
        libkeyutils1 \
        libp11-kit0 \
        libidn2-0 \
        libunistring2 \
        libtasn1-6 \
        libnettle8 \
        libhogweed6 \
        libgmp10 \
        libffi7 \
        liblzma5

### Set working directory
WORKDIR /app

### Copy built binary application from 'builder' image
COPY --from=builder /app/vivo-service .
COPY --from=builder /app/fluent-bit.conf .
COPY --from=flb-download /fluent-bit/bin/fluent-bit .

ENV PATH="$PATH:/app/"

# Expose ports
# ============

# Fluent Bit Forward Input
EXPOSE 24224

# Fluent Bit HTTP Input
EXPOSE 9880

### Run the binary application
CMD ["/app/vivo-service"]
