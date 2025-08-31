# Dockerfile (Alpine)
FROM alpine:latest

# Tools + glibc compat for Ookla
RUN apk add --no-cache curl bash go parallel libc6-compat

# Install speedtest (may still be finicky on Alpine, but libc6-compat helps)
RUN ARCH="$(uname -m)" \
 && curl -L "https://install.speedtest.net/app/cli/ookla-speedtest-1.2.0-linux-${ARCH}.tgz" -o /tmp/speedtest.tgz \
 && tar -C /usr/local/bin -xzf /tmp/speedtest.tgz speedtest \
 && rm -f /tmp/speedtest.tgz

# Install grpcurl via go (puts binary in /root/go/bin)
RUN GOPATH=/root/go go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest \
 && mv /root/go/bin/grpcurl /usr/local/bin/grpcurl

# Fetch the client script into a known PATH dir
RUN curl -L https://raw.githubusercontent.com/Tysonpower/starlinkstatus/main/starlinkstatus_client.sh \
    -o /usr/local/bin/starlinkstatus_client.sh \
 && chmod +x /usr/local/bin/starlinkstatus_client.sh

ENV APIKEY=""
ENV INTERVAL="900"

# Accept license at start and run the script
ENTRYPOINT ["/bin/sh","-c","/usr/local/bin/speedtest --accept-license --accept-gdpr >/dev/null 2>&1 || true; /usr/local/bin/starlinkstatus_client.sh -k \"$APIKEY\" -s -d -i \"$INTERVAL\""]
