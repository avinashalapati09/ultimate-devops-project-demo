FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app/

# Use Go build cache for dependencies
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build



COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy the source code
COPY . .

RUN go build -o product-catalog .

##########################################

FROM alpine AS final

WORKDIR /usr/src/app/
COPY ./products/ ./products/
COPY --from=builder /usr/src/app/product-catalog .

ENV PRODUCT_CATALOG_PORT 8088
EXPOSE ${PRODUCT_CATALOG_PORT}
ENTRYPOINT [ "./product-catalog" ]

