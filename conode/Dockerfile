FROM golang:1.10 as builder
ARG BUILDFLAG="-s -w -X main.gitTag=unknown github.com/dedis/onet.gitTag=unknown"
RUN apt-get update && apt-get install -y clang
RUN go get github.com/dedis/cothority
RUN cd /go/src/github.com/dedis/cothority/conode && go get -u ./...
RUN cd /go/src/github.com/dedis/cothority/conode && go install -ldflags="$BUILDFLAG" .

FROM debian:stretch-slim
WORKDIR /root/
COPY --from=builder /go/bin/conode .
COPY run_nodes.sh .
RUN mkdir /conode_data
RUN mkdir -p .local/share .config
RUN ln -s /conode_data .local/share/conode
RUN ln -s /conode_data .config/conode

EXPOSE 7770 7771

CMD "./run_nodes.sh -d /conode_data -v 2"
