# Spire

This is a part of the Spire setup that installs Spire to the first cluster in a multi-cluster scenarios.

This example assumes [interdomain](../../interdomain/) scenario.
If your cluster setup differs from these scenarios you may need to adjust spire configs (rename trust domains, change URLS, etc.).

## Run

Check that we have config for the cluster:
```bash
[[ ! -z $KUBECONFIG1 ]]
```

Apply spire deployments:
```bash
kubectl --kubeconfig=$KUBECONFIG1 apply -k https://github.com/bszirtes/deployments-k8s/examples/spire/cluster1?ref=ce845a16f06e25a97be64723da300c7f92a730f0
```

Wait for PODs status ready:
```bash
kubectl --kubeconfig=$KUBECONFIG1 wait -n spire --timeout=3m --for=condition=ready pod -l app=spire-server
```
```bash
kubectl --kubeconfig=$KUBECONFIG1 wait -n spire --timeout=1m --for=condition=ready pod -l app=spire-agent
```

Apply the ClusterSPIFFEID CR for the cluster:
```bash
kubectl --kubeconfig=$KUBECONFIG1 apply -f https://raw.githubusercontent.com/networkservicemesh/deployments-k8s/0e8c3ce7819f0640d955dc1136a64ecff2ae8c56/examples/spire/cluster1/clusterspiffeid-template.yaml
```

```bash
kubectl --kubeconfig=$KUBECONFIG1 apply -f https://raw.githubusercontent.com/networkservicemesh/deployments-k8s/0e8c3ce7819f0640d955dc1136a64ecff2ae8c56/examples/spire/base/clusterspiffeid-webhook-template.yaml
```

## Cleanup

Delete ns:
```bash
kubectl --kubeconfig=$KUBECONFIG1 delete crd clusterspiffeids.spire.spiffe.io
kubectl --kubeconfig=$KUBECONFIG1 delete crd clusterfederatedtrustdomains.spire.spiffe.io
kubectl --kubeconfig=$KUBECONFIG1 delete validatingwebhookconfiguration.admissionregistration.k8s.io/spire-controller-manager-webhook
kubectl --kubeconfig=$KUBECONFIG1 delete ns spire
```
