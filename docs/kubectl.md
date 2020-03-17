# Installation and Configuration the kubectl

## Installation

```
sudo snap install kubectl --classic
kubectl version
```

## Configuration

#### Auto-completion

* Install bash-completion

```
sudo apt-get install bash-completion
```

* Reload the bash

```
type _init_completion
```

* Activate auto-completion of kubectl

```
kubectl completion bash >/etc/bash_completion.d/kubectl
```

swith to root when having error like Permission issue

* Reload the bash

#### Change default namespace

In order to avoid repeat the namespace in all subsequent command kubectl, we can change the preference namespace to ours

```
kubectl config set-context --current --namespace=hands-on-lab

# verify it
kubectl config view --minify | grep namespace:
```