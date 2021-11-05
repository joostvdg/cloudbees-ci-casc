# CasC for OC

## Helm and Git-Sync

### Credential for LDAP

```sh
LDAP_PASSWORD=
```

```sh
kubectl create secret generic ldap-manager-pass --from-literal=pass=${LDAP_PASSWORD} --namespace cbci
```

```yaml
  ContainerEnv:
    - name: LDAP_MANAGER_PASSWORD
      valueFrom:
        secretKeyRef:
          name: ldap-manager-pass
          key: pass
```

```yaml
  securityRealm:
    ldap:
      configurations:
        ...
        managerPasswordSecret: ${LDAP_MANAGER_PASSWORD}
        ...
```