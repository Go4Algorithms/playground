# Copyright 2014 The Go Authors.  All rights reserved.
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file.

FROM golang:1.4

# enable faketime
RUN apt-get update && apt-get install -yq --no-install-recommends patch
ADD enable-fake-time.patch /usr/src/go/
RUN patch /usr/src/go/src/runtime/rt0_nacl_amd64p32.s /usr/src/go/enable-fake-time.patch

# build go nacl tool chain
RUN cd /usr/src/go/src && GOOS=nacl GOARCH=amd64p32 ./make.bash --no-clean
RUN cd /usr/local/bin && curl -s -O https://storage.googleapis.com/gobuilder/sel_ldr_x86_64 && chmod +x sel_ldr_x86_64

# add and compile sandbox daemon
ADD . /go/src/sandbox/
RUN go install sandbox

EXPOSE 8080
ENTRYPOINT ["/go/bin/sandbox"]