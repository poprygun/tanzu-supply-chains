# Experiment with Cartographer Supply Chains

# Cartographer [follow along](https://tanzucommunityedition.io/posts/build-modern-software-supply-chains-with-cartographer/)

```bash
tanzu package available list -o yaml | yq  '.[].display-name'
tanzu package installed list
```

## Deploy workload

```bash
yq developer/workload.yaml

kubectl apply workload -f developer/workload.yaml
kubectl get workload client-profile -o yaml | yq 
kubectl get all,httpproxies
```

## Study Cartographer

```bash
kubectl api-resources --api-group carto.run
kubectl get clustersourcetemplate,clusterimagegemplate,clustertemplate
```

## Source - Flux

```bash
kubectl get clustersourcetemplate source -o yaml | yq 'del(.metadata)'
kubectl get gitrepository client-profile -o yaml | yq 'del(.metadata)'
```

## Build - kpack

```bash
kubectl get clusterimagetemplate image -o yaml | yq 'del(.metadata)'
kubectl get cnbimage client-profile -o yaml | yq 'del(.metadata)'
```

## Customizations

Install knative serving

```bash
tanzu package available list knative-serving.community.tanzu.vmware.com
tanzu package install knative-serving \
--package-name knative-serving.community.tanzu.vmware.com \
--version 1.0.0 -f knative-values.yaml
```

Create a new supply chain

```bash
kubectl apply -f app-operator/app-deploy-template-kn.yaml app-operator/supply-chain-n.yaml

kubectl apply -f developer/workload-kn.yaml
kubectl tree workload client-profile-kn
kubectl get all --selector serving.knative.dev/service=client-profile
```

## Resources

[Tanzu Commands](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.0/tap/GUID-cli-plugins-apps-command-reference-tanzu_apps_workload_create.html)

[TAP GUI](http://tap-gui.ashumilov.tapdemo.vmware.com/)

```bash
tanzu apps workload create -f app-developer/workload.yaml
```

kapp logs -f -a tanzu-java-web-app -n my-apps