VERSION=0.0.1

all:		redis

redis:		Dockerfile
		docker build --rm --tag vedarth/redis:4.0.10 --label VERSION=$(VERSION) .

clean:
		docker rmi vedarth/redis:4.0.10