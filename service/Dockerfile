######### Go Builder

FROM golang:1.20 as service-builder

# Working directory
WORKDIR /app

ENV CGO_ENABLED=0

# Copy everything at /app
COPY . /app

COPY fluent-bit.conf /app

# Build the go app
RUN go build -o vivo-service .


######## Fluent Bit
FROM ghcr.io/fluent/fluent-bit/unstable:master as production

### Set working directory
WORKDIR /app

### Copy built binary application from 'builder' image
COPY --from=service-builder /app/vivo-service .
COPY --from=service-builder /app/fluent-bit.conf .
#COPY --from=flb-download /fluent-bit/bin/fluent-bit /app

ENV PATH="$PATH:/fluent-bit/bin/:/app/"

# Expose ports
# ============

# Fluent Bit Forward Input
EXPOSE 24224

# Fluent Bit HTTP Input
EXPOSE 9880

### Run the binary application
CMD ["/app/vivo-service"]
