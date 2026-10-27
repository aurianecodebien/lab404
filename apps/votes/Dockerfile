FROM --platform=$BUILDPLATFORM dhi.io/golang:1.26-dev@sha256:c1e60bfcf708775a036d8777a4a29b6c26f618bbf639a9a6161559c5adda20af AS build

ARG TARGETOS
ARG TARGETARCH

WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY *.go ./
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH \
    go build -trimpath -ldflags="-s -w" -o /out/votes .

FROM scratch

COPY --from=build /out/votes /votes
USER 65532:65532

EXPOSE 8081
ENTRYPOINT ["/votes"]
