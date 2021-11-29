# Helm

[Helm](https://helm.sh) is a tool that streamlines installing and managing Kubernetes applications and resources. Think of it like apt/yum/homebrew for Kubernetes.

## Install Helm

From Homebrew (macOS):

```bash
brew install helm
```

From Chocolatey (Windows):

```bash
choco install kubernetes-helm
```

From Apt (Debian/Ubuntu):

```bash
curl https://helm.baltorepo.com/organization/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

Or using script:

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Test

Verify that helm is working:

```bash
helm version
```