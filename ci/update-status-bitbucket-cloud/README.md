Create an env file with some variables

```
cat <<-EOF >> .env
BITBUCKET_API_TOKEN=glpat-****************
BITBUCKET_BASE_URL=https://gitlab.private.repo.com
OCP_BASE_URL=https://console-openshift-console.apps.cluster.com/k8s/ns
EOF
```

Apply all resources to Openshift

```
source .env && cat *.yml | envsubst '$BITBUCKET_API_TOKEN,$BITBUCKET_BASE_URL,$OCP_BASE_URL' | oc apply -f -
```



To test produce a webhook payload example that look like the example below:




Get the route address and perform a test curl

```
ROUTE=$(oc get route el-bitbucket-event-listener  -o jsonpath='http://{.spec.host}')
```

```
curl -v ${ROUTE} -H 'X-Event-Key: repo:push' --data '{
  "push": {
    "changes": [
      {
        "new": {
          "target": {
            "links": {
              "self": {
                "href": "https://api.bitbucket.org/2.0/repositories/saiello/sample-repo/commit/2677dd6ddea622d076932f2710ed721e0d10cabd/statuses/build/"
              }
            }
          }
        }
      }
    ]
  }
}'
```