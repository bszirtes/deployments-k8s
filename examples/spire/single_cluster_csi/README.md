# Spire CSI

This setup deploys SPIRE along with [SPIFFE CSI driver](https://github.com/spiffe/spiffe-csi)

## Run

To apply spire deployments following the next command:
```bash
kubectl apply -k https://github.com/bszirtes/deployments-k8s/examples/spire/single_cluster_csi?ref=aaf1e1522fbe81126acda26e7d83cb3571fd3baa
```

Wait for PODs status ready:
```bash
kubectl wait -n spire --timeout=4m --for=condition=ready pod -l app=spire-server
```
```bash
kubectl wait -n spire --timeout=1m --for=condition=ready pod -l app=spire-agent
```

Apply the ClusterSPIFFEID CR for the cluster:
```bash
kubectl apply -f https://github.com/networkservicemesh/deployments-k8s/0e8c3ce7819f0640d955dc1136a64ecff2ae8c56/examples/spire/single_cluster/clusterspiffeid-template.yaml
```

```bash
kubectl apply -f https://github.com/networkservicemesh/deployments-k8s/0e8c3ce7819f0640d955dc1136a64ecff2ae8c56/examples/spire/base/clusterspiffeid-webhook-template.yaml
```

## Cleanup

Delete ns:
```bash
kubectl delete crd clusterspiffeids.spire.spiffe.io
kubectl delete crd clusterfederatedtrustdomains.spire.spiffe.io
kubectl delete validatingwebhookconfiguration.admissionregistration.k8s.io/spire-controller-manager-webhook
kubectl delete ns spire
```
