FROM golang:alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY main.go ./
RUN CGO_ENABLED=0 GOOS=linux go build -o main main.go

FROM scratch
WORKDIR /app
COPY --from=builder /app/main ./main
COPY public ./public
EXPOSE 3000
CMD ["./main"]
