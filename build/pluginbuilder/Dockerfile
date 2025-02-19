## Copyright 2018 The Nakama Authors
##
## Licensed under the Apache License, Version 2.0 (the "License");
## you may not use this file except in compliance with the License.
## You may obtain a copy of the License at
##
## http://www.apache.org/licenses/LICENSE-2.0
##
## Unless required by applicable law or agreed to in writing, software
## distributed under the License is distributed on an "AS IS" BASIS,
## WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
## See the License for the specific language governing permissions and
## limitations under the License.

# docker build . --file ./Dockerfile --build-arg version=v2.1.1 -t heroiclabs/nakama-pluginbuilder:2.1.1
# docker build . --file ./Dockerfile --build-arg version="v2.1.1-$(git rev-parse --short HEAD)" -t heroiclabs/nakama-pluginbuilder:"2.1.1-$(git rev-parse --short HEAD)"

FROM --platform=$BUILDPLATFORM golang:1.21.5-bookworm as builder

MAINTAINER Heroic Labs <support@heroiclabs.com>

ARG TARGETARCH
ARG version

LABEL version=$version
LABEL variant=nakama-pluginbuilder
LABEL description="A support container to build Go code for Nakama server's runtime."

ENV GOOS linux
ENV GOARCH=$TARGETARCH
ENV CGO_ENABLED 1

RUN dpkg --add-architecture $TARGETARCH && \
    apt-get update && \
    apt-get -y upgrade && \
    apt-get install -y --no-install-recommends ca-certificates git && \
    if [ "${TARGETARCH}" = "amd64" ]; then \
    apt-get install -y --no-install-recommends g++-x86-64-linux-gnu libc6-dev-amd64-cross; \
    elif [ "${TARGETARCH}" = "arm64" ]; then \
    apt-get install -y --no-install-recommends gcc-aarch64-linux-gnu libc6-dev-arm64-cross; \
    fi && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /builder

ENTRYPOINT ["go"]
