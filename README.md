---
  geometry: margin=3cm
---

# payobills-argo-config

Argo deployment files for payobills
Depends on [kube-homelab](https://github.com/mrsauravsahu/kube-homelab)'s Argo CD

Set up an easy alias for kubectl
```bash
$ alias k=kubectl
```

## GraphQL composition

Currently, the GraphQL composition done using [graphql-hive](https://github.com/mrsauravsahu/graphql-hive) doesn't work.

```bash
$ npx @graphql-hive/cli dev \
    --service bills --schema 'http://localhost:9002/graphql?sdl' \
    --url http://payobills-subgraph-bills.payobills.svc.cluster.local/graphql \
    --service payments --schema 'http://localhost:9003/graphql?sdl' \
    --url http://payobills-subgraph-payments.payobills.svc.cluster.local/graphql \
    --write=graphql-federation/supergraph.graphql
```

Using [rover](https://github.com/merapar/rover) to generate GraphQL schema

```bash
$ npx @apollo/rover supergraph compose \
    --config ./graphql-federation/roverconfig.yml \
    -o ./graphql-federation/supergraph.rover.gql
```

## Argo Deployments

Apply changes using kubectl
```bash
$ k apply -n argo-cd -f ./payobills-argo-config.yaml
```

Get admin secret
```bash
$ k get secret -n argo-cd argocd-initial-admin-secret -o json \
  | jq -r  '.data.password' \
  | base64 --decode \
  | pbcopy
```

Port forward Argo
```bash
$ k -n argo-cd port-forward svc/argo-cd-argocd-server 9001:80
```

