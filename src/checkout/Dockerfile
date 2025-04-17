# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app/

RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,source=./src/checkout/go.sum,target=go.sum \
    --mount=type=bind,source=./src/checkout/go.mod,target=go.mod \
    go mod download

RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=cache,target=/root/.cache/go-build \
    --mount=type=bind,rw,source=./src/checkout,target=. \
    go build -ldflags "-s -w" -o /go/bin/checkout/ ./

FROM alpine

WORKDIR /usr/src/app/

COPY --from=builder /go/bin/checkout/ ./

# Set environment variables
ENV OTEL_SERVICE_NAME="checkout" \
    OTEL_COLLECTOR_NAME="otel-collector" \
    OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE="cumulative" \
    CHECKOUT_PORT="8080" \
    CART_ADDR="cart:8080" \
    CURRENCY_ADDR="currency:8080" \
    EMAIL_ADDR="http://email:8080" \
    PAYMENT_ADDR="payment:8080" \
    PRODUCT_CATALOG_ADDR="product-catalog:8080" \
    SHIPPING_ADDR="shipping:8080" \
    KAFKA_ADDR="kafka:9092" \
    FLAGD_HOST="flagd" \
    FLAGD_PORT="8013" \
    OTEL_EXPORTER_OTLP_ENDPOINT="http://$(OTEL_COLLECTOR_NAME):4317" \
    OTEL_RESOURCE_ATTRIBUTES="service.name=$(OTEL_SERVICE_NAME),service.namespace=opentelemetry-demo,service.version=2.0.0"

EXPOSE ${CHECKOUT_PORT}
ENTRYPOINT [ "./checkout" ]
