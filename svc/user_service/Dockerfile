FROM golang:1.17 AS builder
ADD ./main.go main.go
ADD ./service service
ADD ../../go.mod go.mod
ADD ../../go.s go.sum
# Fetch dependencies
RUN git config --global http.sslverify false
RUN go get -d -v

# Build image as a truly static Go binary
#RUN export GO111MODULE="on"
RUN CGO_ENABLED=0 GOOS=linux go build -o /user_service -a -tags netgo -ldflags '-s -w' .

FROM scratch
MAINTAINER Gigi Sayfan <the.gigi@gmail.com>
COPY --from=builder /user_service /app/user_service
EXPOSE 7070
ENTRYPOINT ["/app/user_service"]
