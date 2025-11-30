# payobills-argo-config

Argo deployment for payobills.

Depends on [kube-homelab](https://github.com/mrsauravsahu/kube-homelab)'s Argo CD

```bash
alias k=kubectl
k apply -n argo-cd -f 'payobills-argo-config.yaml'
```

## GraphQL composition

Currently, the GraphQL composition done using [graphql-hive](https://github.com/mrsauravsahu/graphql-hive) doesn't work.

```bash
npx @graphql-hive/cli dev \
    --service bills --schema 'http://localhost:9002/graphql?sdl' \
    --url http://payobills-subgraph-bills.payobills.svc.cluster.local/graphql \
    --service payments --schema 'http://localhost:9003/graphql?sdl' \
    --url http://payobills-subgraph-payments.payobills.svc.cluster.local/graphql \
    --write=graphql-federation/supergraph.graphql
```

Using [rover](https://github.com/merapar/rover) to generate GraphQL schema

```bash
npx @apollo/rover supergraph compose \
    --config ./graphql-federation/roverconfig.yml \
    -o ./graphql-federation/supergraph.rover.gql
```
