# Bitnami/Kubeseal Secret Management

## Install Controller

```
$ helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
"sealed-secrets" has been added to your repositories
```

```
$ helm install sealed-secrets-controller -n kube-system sealed-secrets/sealed-secrets
NAME: sealed-secrets-controller
LAST DEPLOYED: Mon Apr 17 11:26:08 2023
NAMESPACE: kube-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

You should now be able to create sealed secrets.

1. Install the client-side tool (kubeseal) as explained in the docs below:

    https://github.com/bitnami-labs/sealed-secrets#installation-from-source
2. Create a sealed secret file running the command below:

    kubectl create secret generic secret-name --dry-run=client --from-literal=foo=bar -o [json|yaml] | \
    kubeseal \
      --controller-name=sealed-secrets-controller \
      --controller-namespace=kube-system \
      --format yaml > mysealedsecret.[json|yaml]

The file mysealedsecret.[json|yaml] is a commitable file.

If you would rather not need access to the cluster to generate the sealed secret you can run:

    kubeseal \
      --controller-name=sealed-secrets-controller \
      --controller-namespace=kube-system \
      --fetch-cert > mycert.pem

to retrieve the public cert used for encryption and store it locally. You can then run 'kubeseal --cert mycert.pem' instead to use the local cert e.g.

    kubectl create secret generic secret-name --dry-run=client --from-literal=foo=bar -o [json|yaml] | \
    kubeseal \
      --controller-name=sealed-secrets-controller \
      --controller-namespace=kube-system \
      --format [json|yaml] --cert mycert.pem > mysealedsecret.[json|yaml]

3. Apply the sealed secret

    kubectl create -f mysealedsecret.[json|yaml]

Running 'kubectl get secret secret-name -o [json|yaml]' will show the decrypted secret that was generated from the sealed secret.

Both the SealedSecret and generated Secret must have the same name and namespace.
```

### Controller Logs
The controller on first start will create a new certificate if there is not one already. This certificate is used for secret encryption. The certificate shows up in clear text in the controller logs.
```
$ kubectl logs --tail -1 -n kube-system -l app.kubernetes.io/name=sealed-secrets 
controller version: 0.20.5
2023/04/17 19:13:51 Starting sealed-secrets controller version: 0.20.5
2023/04/17 19:13:51 Searching for existing private keys
2023/04/17 19:13:51 New key written to kube-system/sealed-secrets-keytqkbr
2023/04/17 19:13:51 Certificate is 
-----BEGIN CERTIFICATE-----
<< REDACTED >>
-----END CERTIFICATE-----

2023/04/17 19:13:51 HTTP server serving on :8080
```

## Install Kubeseal CLI

```
$ wget https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.20.5/kubeseal-0.20.5-linux-amd64.tar.gz
<< REDACTED >>
$ tar -xvzf kubeseal-0.20.5-linux-amd64.tar.gz
LICENSE
README.md
kubeseal
$ sudo install -m 755 kubeseal /usr/local/bin/kubeseal
```

## Create a Sealed Secret

## Setup Custom Controller Certificate Rotation

