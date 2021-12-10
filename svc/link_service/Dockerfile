FROM golang:1.17 AS builder
ADD ./main.go main.go
ADD ./service service
# Fetch dependencies
RUN git config --global http.sslverify false
RUN go get -d -v

# Build image as a truly static Go binar
#RUN export GO111MODULE="on"
RUN CGO_ENABLED=0 GOOS=linux go build -o /link_service -a -tags netgo -ldflags '-s -w' .

FROM scratch
MAINTAINER Gigi Sayfan <the.gigi@gmail.com>
COPY --from=builder /link_service /app/link_service
EXPOSE 7070
ENTRYPOINT ["/app/link_service"]
