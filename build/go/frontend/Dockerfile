FROM golang AS build
LABEL maintainer="Target Brands, Inc. TTS-CFC-OpenSource@target.com"

# Copy source files and set the working directory
COPY ./src/go/ /go/src/github.com/target/strelka/src/go/
WORKDIR /go/src/github.com/target/strelka/src/go/

# Initialize, get, and build go modules and main package
RUN go mod init && \
    go mod tidy && \
    cd /go/src/github.com/target/strelka/src/go/cmd/strelka-frontend/ && \
    CGO_ENABLED=0 go build -o /tmp/strelka-frontend .

# Move build, create default logging directory, and initialize non-root user
FROM alpine
COPY --from=build /tmp/strelka-frontend /usr/local/bin/strelka-frontend
RUN mkdir /var/log/strelka/ && \
    chgrp -R 0 /var/log/strelka/ && \
    chmod -R g=u /var/log/strelka/
USER 1001
