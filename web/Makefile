account = $(shell aws sts get-caller-identity --query "Account" --output text)
image = $(account).dkr.ecr.us-east-2.amazonaws.com/webapp:$(version)
stage = dev
include stages/$(stage)

login:
	aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin $(account).dkr.ecr.us-east-2.amazonaws.com

build: login
	@docker build -t $(image) .

push:
	@docker push $(image)

deploy:
	@cat kubernetes/namespace.yaml | sed "s|NAMESPACE|$(namespace)|g" | kubectl apply -f -
	@cat kubernetes/deployment.yaml | sed "s|IMAGEVERSION|$(version)|g;s|NAMESPACE|$(namespace)|g;s|ACCOUNT|$(account)|g" | kubectl apply -f -
