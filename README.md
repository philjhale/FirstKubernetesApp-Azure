# First Kubernetes app running on Azure

Basic steps on how you get a hello world app running in Azure Kubernetes Service using the Azure CLI on a Mac. Most of the steps come from [AKS CLI tutorial](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough).

## Steps 

Clone this repository.

Install [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)

```brew install azure-cli```

If you get the following error:

```
Error: An unexpected error occurred during the `brew link` step
The formula built, but is not symlinked into /usr/local
Permission denied @ dir_s_mkdir - /usr/local/Frameworks
Error: Permission denied @ dir_s_mkdir - /usr/local/Frameworks
```

Follow the steps listed [here](https://github.com/Azure/azure-cli/issues/5503).

```
1. Uninstall Azure CLI (could have an error, it is ok)
brew uninstall azure-cli

2. Update just brew
brew update

3. Upgrade brew
brew upgrade

4. Clean up brew
brew cleanup

5. Reinstall just the Azure CLI
brew install azure-cli

6. Check the CLI
az help
```

If running `az help` returns the error `command not found`, run

```
brew update && brew install python3 && brew upgrade python3
brew link --overwrite python3
```

Log in to Azure.

```az login```

Create a resource group.

```az group create --name test-k8s-group --location eastus```

Create a k8s cluster (takes a while).

```az aks create --resource-group test-k8s-group --name test-k8s-cluster --node-count 1 --enable-addons monitoring --generate-ssh-keys```

Install kubectl if you haven't already.

Download credentials and config kubectl to use them.

```az aks get-credentials --resource-group test-k8s-group --name test-k8s-cluster```

Check you can see the AKS nodes.

```kubectl get nodes```

Create the app.

```kubectl apply -f manifest.yaml```

Wait for the service to the service to get an external IP address.

```kubectl get services --watch```

Load in the browser. http://yourexternalip:4000/api/random
