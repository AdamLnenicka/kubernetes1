# Stage 1: Build the Go application
FROM golang:1.15 as builder

# Install dependencies
RUN go get github.com/codegangsta/negroni
RUN go get github.com/gorilla/mux
RUN go get github.com/xyproto/simpleredis/v2

# Copy the Go application source code
COPY main.go .

# Build the Go application
RUN go build main.go

# Stage 2: Create the final Docker image
FROM ubuntu:18.04

# Copy the built binary from the previous stage to the final image
COPY --from=builder /go/main /app/guestbook

# Copy static files for the web application
COPY public/index.html /app/public/index.html
COPY public/script.js /app/public/script.js
COPY public/style.css /app/public/style.css
COPY public/jquery.min.js /app/public/jquery.min.js

# Set the working directory
WORKDIR /app

# Define the default command to run the application
CMD ["./guestbook"]

# Expose port 3000 for the application
EXPOSE 3000
