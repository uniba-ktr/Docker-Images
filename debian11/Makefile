BUILDX=docker buildx build --platform linux/amd64,linux/arm64
VERSION=11
TAG=debian$(VERSION)
BASE=kathara/base:$(TAG)

.PHONY: base quagga frr sdn p4 all pushall all-multi create-builder base-multi quagga-multi frr-multi sdn-multi p4-multi delete-builder

all: base quagga frr sdn p4
all-multi: create-builder base-multi quagga-multi frr-multi sdn-multi p4-multi delete-builder

pushall:
	docker push kathara/base:$(TAG)
	#docker push kathara/quagga:$(TAG)
	docker push kathara/frr:$(TAG)
	docker push kathara/sdn:$(TAG)
	docker push kathara/p4:$(TAG)
	docker image tag kathara/base:$(TAG) kathara/base:latest
	docker push kathara/base:latest
	#docker image tag kathara/quagga:$(TAG) kathara/quagga:latest
	#docker push kathara/quagga:latest
	docker image tag kathara/frr:$(TAG) kathara/frr:latest
	docker push kathara/frr:latest
	docker image tag kathara/sdn:$(TAG) kathara/sdn:latest
	docker push kathara/sdn:latest
	docker image tag kathara/p4:$(TAG) kathara/p4:latest
	docker push kathara/p4:latest

base:
	docker build -t kathara/base:$(TAG) --build-arg=VERSION=$(VERSION) base

#quagga: base
#	docker build -t kathara/quagga:$(TAG) --build-arg=BASE=$(TAG) quagga

frr: base
	docker build -t kathara/frr:$(TAG) --build-arg=BASE=$(BASE) frr

sdn: base
	docker build -t kathara/sdn:$(TAG) --build-arg=BASE=$(BASE) sdn

p4: base
	docker build -t kathara/p4:$(TAG) --build-arg=BASE=$(BASE) p4

base-multi: create-builder
	$(BUILDX) -t kathara/base:$(TAG) -t kathara/base:latest --build-arg=VERSION=$(VERSION) --push base

#quagga-multi: create-builder base-multi
#	$(BUILDX) -t kathara/quagga:$(TAG) -t kathara/quagga:latest --build-arg=BASE=$(TAG) --push quagga

frr-multi: create-builder base-multi
	$(BUILDX) -t kathara/frr:$(TAG) -t kathara/frr:latest --build-arg=BASE=$(BASE) --push frr

sdn-multi: create-builder base-multi
	$(BUILDX) -t kathara/sdn:$(TAG) -t kathara/sdn:latest --build-arg=BASE=$(BASE) --push sdn

p4-multi: create-builder base-multi
	$(BUILDX) -t kathara/p4:$(TAG) -t kathara/p4:latest --build-arg=BASE=$(BASE) --push p4

create-builder:
	docker buildx create --name kat-builder --use
	docker buildx inspect --bootstrap

delete-builder:
	docker buildx rm kat-builder
