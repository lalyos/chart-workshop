
This helm chart is usefull for kubernetes workshops, where participants

Todo:
[ ] integrate automatic route53 entries

## Features

- preinstalled tools: kubectl, helm, jq, jid, micro, ...
- bash-autocompletion for: k, kubectl, helm
- nvim: with k8s yaml support
- reach the session via:
  - ingress: http://userx.mydomain.com
  - nodePort: http://n1.mydomain.com:31234 (in case the ingress dropsconnections)
  - ssh: with the help of https://github.com/lalyos/k8s-sshfront

## Prereq

I don't want to maintain a proper helmrepo with index.yaml just use git.
Fortunately there is a plugin for that:
```
helm plugin install https://github.com/aslafy-z/helm-git --version 0.11.1
```

## Install

Install the helm repo
```
helm repo add workshop git+https://github.com/lalyos/chart-workshop@
```

Create a release
```
helm upgrade -i workshop workshop/workshop
```

## Configuration

See possible `values.yaml`
```
dns: k8z.eu

# basic auth password for browser sessions, username is fixed: user
secret: s3cr3t

# number of user sessions to create
users: 10
```

either use commandline `--set`
```
helm upgrade -i workshop workshop/workshop \
  --set users=5 \
  --set dns=mydomain.com \
  --set secret=N0tTuS3kr3t
```

or put all into a `values.yaml` and
```
helm upgrade -i workshop workshop/workshop --values values.yaml
```

## DNS

If you own a domain name you need the following entries:
- A record with asterix pointing top the Ingress Ctrl
  - *.mydomain.com
- A record for nodes external ips (`k get no -owide`):
  - n1.mydomain.com
  - n2.mydomain.com
  - n3.mydomain.com

If you don't own a DNS zone, you can useone of:
- https://sslip.io/
- https://nip.io/

They will resolv dynamically any ip:
- 10.0.0.1.nip.io maps to 10.0.0.1
- 10.0.0.1.sslip.io maps to 10.0.0.1


## SSH access

In case you prefer to work from a terminal instead of the browser session.
Run the following command in the browser session:
```
$ ssh-pubkey <your-github-username>
```