# Azure Kubernetes Service (AKS) Deployment with Terraform

This Terraform configuration deploys an **Azure Kubernetes Service (AKS)** cluster in Microsoft Azure. It includes the creation of an Azure Resource Group, Virtual Network (VNET), Subnet, and AKS Cluster, with network and security configurations.

## Prerequisites

Before using this Terraform configuration, make sure you have:

- [Terraform](https://www.terraform.io/downloads.html) installed (version 1.0 or higher recommended).
- An **Azure** account and access to the Azure Subscription where the resources will be created.
- Azure CLI installed, if using `az login` for authentication.
- Note: Your Azure account needs permissions to create the resources listed below

---

## Resources Created

This Terraform configuration will create the following resources in Azure:

- **Resource Group**: A container to hold all Azure resources.
- **Virtual Network**: A custom VNet with a subnet specifically for the AKS cluster.
- **AKS Cluster**: A managed Kubernetes cluster with a node pool.
- **Role Assignment**: The necessary Azure role to grant AKS access to the subnet.

---

## Setup

### Clone the Repository

Clone this repository to your local machine:

```bash
git clone https://github.com/yourusername/aks-terraform-deployment.git
cd aks-terraform-deployment
```

### Configure Azure Authentication

You need to authenticate Terraform with your Azure account.

- authenticate via Azure CLI:

  ```bash
  az login
  az account set --subscription "your-subscription-id"
  ```

### Update Variables

Modify the variables defined in `terraform.tfvars` or set them manually in the `main.tf` file:

- `subscription_id` – Azure subscription ID.
- `admin_username` – Username for the AKS admin user - could be your SSH key name (must meet Azure's naming conventions).
- `admin_ssh_key` – The SSH public key for accessing AKS nodes.
- `location` – The Azure region for deploying resources.
- `vnet_resource_group_name` – Name of the resource group for the virtual network.
- `aks_vnet_name` – Name of the virtual network for AKS.
- `kube_version_prefix` – Kubernetes version prefix for AKS.
- `nodepool_vm_size` – Size of the virtual machine for the AKS node pool.
- `nodepool_nodes_count` – Number of nodes in the AKS node pool.
- `network_service_cidr` – Service CIDR for AKS.
- `network_dns_service_ip` – DNS service IP for AKS.

### Initialize Terraform

Initialize Terraform to download necessary providers and modules:

```bash
terraform init
```

### Plan the Deployment

Preview the changes Terraform will make:

```bash
terraform plan
```

### Apply the Changes

Deploy the infrastructure:

```bash
terraform apply
```

You will be prompted to confirm the action. Type `yes` to proceed.

### Access the AKS Cluster

Once the cluster is deployed, you can configure `kubectl` to interact with it:

```bash
az aks get-credentials --resource-group <resource-group-name> --name <aks-cluster-name>
```

This command will configure your local `kubectl` to interact with the newly created AKS cluster.

---

## Clean Up

To delete all resources created by this configuration, run:

```bash
terraform destroy
```

You will be prompted to confirm the destruction of resources. Type `yes` to proceed.

---

## Notes

- Ensure that the `admin_username` follows Azure's naming conventions: it must begin with a letter and contain only letters, numbers, underscores, or hyphens.
- The AKS node pool is configured with a default VM size and node count, but these can be customized via the `terraform.tfvars` file or directly in the `main.tf` file.
