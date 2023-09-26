
Gitops multirepo using appset with matrix generator for microservices applications


- bootstrap-repo: repository containing the appset
- config-repo: repository containing configurations of each
- repos: cd app repositories 


# Steps to deploy


1. Create a Repo credential for the private repo

```
argocd admin repo generate-spec https://github.com/saiello/test-gitops.git  --username <GT_USER> \
--password <GT_TOKEN> | kubectl -n argocd apply -f -
```

or 

```
kubectl -n argocd  apply -f secret.yml
```

2. Create the appset

```
kubectl -n argocd apply -f bootstrap-repo/appset.yml
```