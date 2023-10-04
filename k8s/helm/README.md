# Helm

[Helm](https://helm.sh) is a tool that streamlines installing and managing Kubernetes applications and resources. Think of it like apt/yum/homebrew for Kubernetes.

## Install Helm

From Homebrew (macOS):

```bash
brew install helm
```
(docs)[https://helm.sh/docs/intro/install/#from-homebrew-macos]

From Chocolatey (Windows):

```bash
choco install kubernetes-helm
```
(docs)[https://helm.sh/docs/intro/install/#from-chocolatey-windows]

From Scoop (Windows):
```
scoop install helm
```
(docs)[https://helm.sh/docs/intro/install/#from-scoop-windows]

For other (supported) OSes see the documentation
(https://helm.sh/docs/intro/install/#from-apt-debianubuntu)[https://helm.sh/docs/intro/install/#from-apt-debianubuntu]

## Test

Verify that helm is working:

```bash
helm version
```