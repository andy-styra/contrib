MODULE := pam_authz
NEED_SYMLINK := $(shell if ! stat -q .go/src/pam-authz 2>&1 > /dev/null ; then echo "yes" ; fi)

module: test
	GOPATH=${PWD}/.go go build -buildmode=c-shared -o ${MODULE}.so

test: *.go .go/src
	GOPATH=${PWD}/.go go test -cover

.go/src:
	-mkdir -p ${PWD}/.go/src
ifeq ($(NEED_SYMLINK),yes)
	ln -s ${PWD} ${PWD}/.go/src/pam-authz
endif
	GOPATH=${PWD}/.go go get golang.org/x/crypto/ssh
	GOPATH=${PWD}/.go go get golang.org/x/crypto/ssh/agent
	GOPATH=${PWD}/.go go get github.com/stretchr/testify/require
	GOPATH=${PWD}/.go go get github.com/golang/glog
	GOPATH=${PWD}/.go go get github.com/pkg/errors

clean:
	go clean
	-rm -f ${MODULE}.so ${MODULE}.h
	-rm -rf .go/

.PHONY: test module download_deps clean
