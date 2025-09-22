FROM golang:1.22.7 AS builder
WORKDIR /usr/src/app/
# Use Go build cache for dependencies
#RUN --mount=type=cache,target=/go/pkg/mod \
#   --mount=type=cache,target=/root/.cache/go-build \
#  mkdir -p /root/.cache/go-build
# Copy
COPY go.mod go.sum ./
# Download
RUN go mod download
# Copy the rest of the source code
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o product-catalog .

####################################

FROM alpine:3.19  AS release
WORKDIR /usr/src/app/
COPY ./products/ ./products/
COPY --from=builder /usr/src/app/product-catalog .
ENV PRODUCT_CATALOG_PORT=8088
ENTRYPOINT [ "./product-catalog" ]

