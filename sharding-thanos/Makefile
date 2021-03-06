CLUSTER_NAME := thanos
K8S_CONTEXT := kind-$(CLUSTER_NAME)

.PHONY: cluster

cluster:
	kind get clusters | grep -q $(CLUSTER_NAME) || kind create cluster --config cluster.yaml --name $(CLUSTER_NAME)
	kubectl --context=$(K8S_CONTEXT) get ns ingress-nginx || kubectl --context=$(K8S_CONTEXT) apply -f https://raw.githubusercontent.com/CuddlyDemos/kind-resources/main/nginx-ingress-controller/deploy.yaml
	kubectl --context=$(K8S_CONTEXT) get customresourcedefinitions prometheuses.monitoring.coreos.com || kubectl --context=$(K8S_CONTEXT) apply -f https://raw.githubusercontent.com/CuddlyDemos/kind-resources/main/prometheus-operator/bundle.yaml

.PHONY: deploy
deploy:
	kubectl --context=$(K8S_CONTEXT) apply -f deploy/

.PHONY: demo-app
demo-app:
	kubectl --context=$(K8S_CONTEXT) apply -f demo-app/

.PHONY: all
all: cluster deploy demo-app

.PHONY: clean
clean:
	kind delete cluster --name $(CLUSTER_NAME)

.PHONY: reset
reset:
	kubectl --context=$(K8S_CONTEXT) delete -f deploy/
	kubectl --context=$(K8S_CONTEXT) delete -f demo-app/
