# Operators

Cluster operators installed.

## One Password Operator

One password operator used to connect to 1password, this operator injects enviroment secrets from the,
from the 1password vault.

[Operator installation guide](https://github.com/1Password/onepassword-operator)

After installing the Helm chart, use the example `k8s/onepassword-operator.yaml` file. to be able to create secrets in deployments that are **NOT** in the `default` namespaces, add the list of namespaces comma seperated on line 22 under the `WATCH_NAMESPACE` env value.
