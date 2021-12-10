FROM golang:1.17 AS builder
ENV GO111MODULE=on

WORKDIR /app

COPY . .
# Fetch dependencies
RUN git config --global http.sslverify false

RUN echo $(pwd)

RUN go get -d -v



# Build image as a truly static Go binary
RUN CGO_ENABLED=0 GOOS=linux go build -o /social_graph_service -a -tags netgo -ldflags '-s -w' .

FROM scratch
MAINTAINER Gigi Sayfan <the.gigi@gmail.com>
COPY --from=builder /social_graph_service /app/social_graph_service
EXPOSE 9090
ENTRYPOINT ["/app/social_graph_service"]
