# Build Pipeline

## Setup

kubectl create namespace getting-started

kubectl -n getting-started apply -f ./docs/getting-started/rbac/admin-role.yaml -f ./docs/getting-started/rbac/clusterrolebinding.yaml

// kubectl -n getting-started apply -f ./docs/getting-started/rbac/webhook-role.yaml

kubectl -n getting-started apply -f https://raw.githubusercontent.com/tektoncd/catalog/master/task/git-clone/0.4/git-clone.yaml

kubectl -n getting-started apply -f https://raw.githubusercontent.com/tektoncd/catalog/master/task/buildpacks/0.3/buildpacks.yaml

kubectl -n getting-started apply -f ./docs/getting-started/static-ingress-rule.yaml

kubectl -n getting-started apply -f docs/getting-started/ingress-run.yaml

kubectl create secret docker-registry docker-user-pass \
    --docker-username=jspruancedome \
    --docker-password=Molokova37! \
    --docker-server=https://registry.hub.docker.com \
    --namespace default

kubectl -n getting-started apply -f ./docs/getting-started/sa.yaml

kubectl -n getting-started apply -f ./docs/resources.yaml

kubectl -n getting-started apply -f ./docs/getting-started/pipeline.yaml

kubectl -n getting-started apply -f ./docs/getting-started/triggers.yaml

// kubectl -n getting-started apply -f docs/getting-started/secret.yaml

// kubectl -n getting-started apply -f docs/getting-started/webhook-run.yaml



 In browser, should return a JSON payload.

## Test

git commit -a -m "build commit" --allow-empty && git push origin main

kubectl -n getting-started logs -l tekton.dev/pipeline=getting-started-pipeline --all-containers

## Tear down
kubectl delete namespace getting-started