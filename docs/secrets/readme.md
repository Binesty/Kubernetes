# Secrets with vault

The environment secrets are synchronized with the hashicorp vault  

The main vault access secret must be created in the replicator namespace so that it can be replicated to the namespaces that will use the vault.  
This secret contain access token to access vault.  

**This should be the only secret created manually, the others will be synchronized in the cluster**


```yaml
apiVersion: v1
kind: Secret
metadata:
  name: vault-token
  namespace: kubernetes-replicator
  annotations:
    replicator.v1.mittwald.de/replicate-to-matching: > 
        use-vault=true
data:
  token: [base64-token-vault]
```