# VP hashicorp-vault

## Updating the chart

1. Edit Chart.yaml with the new version
2. In the hashicorp-vault folder, run: `helm dependency update .`
3. Run `./update-helm-dependency.sh`
4. Check that the images in ./values.yaml are the same version as [upstream](https://github.com/hashicorp/vault-helm/blob/main/values.openshift.yaml)
5. Git add the new chart in `./charts/vault-<version>.tgz`

## Patches

### Issue 9136

**IMPORTANT**: Due to the fact that 'null' values do not work in helm charts
([GH#9136](https://github.com/helm/helm/issues/9136)), we need to patch the
chart to skip setting the host.

### Issue 674

In order to be able to use vault ssl we need to patch the helm chart to fix
upstream issue 674.

Make sure to run "./update-helm-dependency.sh" after you updated the subchart
(by calling helm dependency update .)

We can drop this local patch when any one the two conditions is true:

- [1] is fixed in helm and we can require the version that for installs
- [PR#779](https://github.com/hashicorp/vault-helm/pull/779) is merged in vault-helm *and* our minimum supported OCP version
  is OCP 4.11 (route subdomain is broken in OCP < 4.11 due to missing [commit](https://github.com/openshift/router/commit/6f730c7cae966f0ed8def50c81d1bf10fe9eb77b)




oc rsh vault-0
vault token lookup 

```
sh-4.4$ vault token lookup
Key                 Value
---                 -----
accessor            tdJmDU59YX7mgYUXBpkmBj6Y
creation_time       1681452312
creation_ttl        0s
display_name        root
entity_id           n/a
expire_time         <nil>
explicit_max_ttl    0s
id                  hvs.IIwloZkFZQ8Ufahon20olp4d.    ==> Token
meta                <nil>
num_uses            0
orphan              true
path                auth/token/root
policies            [root]
ttl                 0s
type                service
```
