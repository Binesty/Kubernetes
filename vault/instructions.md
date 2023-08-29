## Install Helm chart

create namespace

````bash
kubectl create namespace vault
````

Vault state is inactive because it is unsealed 

## Initialize vault production

````bash
kubectl exec -ti vault-0 -n vault -- vault operator init
````

output
````bash
Unseal Key 1: [key-1]
Unseal Key 2: [key-2]
Unseal Key 3: [key-3]
Unseal Key 4: [key-4]
Unseal Key 5: [key-5]

Initial Root Token: [token]
````

## Unseal vault

use keys to sealed vault

Key-1
````bash
kubectl exec -ti [vault-pod] -n vault -- vault operator unseal [key-1]
````

Key-2
````bash
kubectl exec -ti [vault-pod] -n vault -- vault operator unseal [key-2]
````

Key-3
````bash
kubectl exec -ti [vault-pod] -n vault -- vault operator unseal [key-3]
````

Sample
````bash
kubectl exec -ti vault-0 -n vault -- vault operator init 

Unseal Key 1: ix2OgoILVTeGZx+OXqQ4ze2iI82S6WpzkWsTp5ejJQ9l
Unseal Key 2: b72ARUuzpyQUL63RUv3OSeGRDlG2ZvR2AwG2cvYe/8Pk
Unseal Key 3: 3CYMyETcA+nBMT0JaKX254lidNn2o6WrtwYDV6iB6Xtg
Unseal Key 4: XznJKYs+u2pG6c7BXe0Oj1wF3Dv6VbTr2KwFedl8Z5U+
Unseal Key 5: XC5S9eb75wr2a94e75Wnf0u/1hFNsqHBVAfX5vtQcN8X

Initial Root Token: hvs.6wgRqlgOj9aQUqVeDQTyT6Nv

kubectl exec -ti vault-0 -n vault -- vault operator unseal ix2OgoILVTeGZx+OXqQ4ze2iI82S6WpzkWsTp5ejJQ9l  
kubectl exec -ti vault-0 -n vault -- vault operator unseal b72ARUuzpyQUL63RUv3OSeGRDlG2ZvR2AwG2cvYe/8Pk  
kubectl exec -ti vault-0 -n vault -- vault operator unseal 3CYMyETcA+nBMT0JaKX254lidNn2o6WrtwYDV6iB6Xtg
````

## Create service and ingress to access vault

````yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:    
    kubernetes.io/ingress.class: nginx    
    nginx.ingress.kubernetes.io/rewrite-target: /
  name: vault-ingress
  namespace: vault
spec:
  rules:
  - host: vault.binesty.net
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: [vault-service]
            port:
              number: 8200
````

## Access webapp development

Access: http://vault.binesty.net/ui  
Initial Root Token: hvs.WhCerZdt30LAcCvf27CJXSCK



## Polices

create a police to get secret
````bash
path "secret/*" {
  capabilities = ["create", "read", "update", "patch", "delete", "list"]
}

path "secret/*" {
  capabilities = ["create", "read", "update", "patch", "delete", "list"]
}

````

# Auto unseal with azure key vault

configuration on deploy vault to connect with azure to generate keys on the azure key vault on initialize vault
vault hashicorp retrieve keys from azure key vault to auto unseal

key name create on azure: vault-hashicorp-kubernetes-unseal-key

Parameters on helm setup vault-hashicorp HA
|  |  |
|--|--|
| tenant-id | Key Vault’s Directory ID |
| client-id | Service Principal’s Application ID |
| client-secret | Service Principal’s generated secret (the one that is not retrievable) |
| vault-name | Name of Azure Key Vault instance  |
| key_name | Name of generated key on Azure Key Vault |
| subscription | ID of the Azure Subscription |
