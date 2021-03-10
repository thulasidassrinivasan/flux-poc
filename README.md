# flux-poc
Experimenting with flux at AKS

## Install

```bash
# Find your AAD group object id
ADMIN_GROUP_ID="$(az ad group show -g YOUR-AAD-GROUP-NAME --query objectId -o tsv)"

# Provision new AKS cluster
./scripts/provision-aks.sh flux-poc 10.14.0.0 ${ADMIN_GROUP_ID}

# Get AKS credentials (if you use different PREFIX inside provision-aks.sh, replace iac- with your prefix)
az aks get-credentials -g iac-flux-poc-aks-rg -n aks-flux-poc

# Add flux repo 
helm repo add fluxcd https://charts.fluxcd.io

# deploy flux 
helm upgrade -i flux fluxcd/flux --set git.url=git@ssh.dev.azure.com:v3/AZURE-DEVOPS-ORG/PROJECT-NAME/GIT-REPO --set git.branch=master --set git.path=manifests  --namespace flux

# Get SSH key 
kubectl -n flux logs deployment/flux | grep identity.pub | cut -d '"' -f2

# Add SSH key to your `SSH public keys` https://dev.azure.com/AZURE-DEVOPS-ORG/_usersSettings/keys

# 
```
