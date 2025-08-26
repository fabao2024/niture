# OCI Web Server Infrastructure with Terraform

This Terraform project provisions a basic web server infrastructure on Oracle Cloud Infrastructure (OCI). It creates a complete networking setup with a virtual cloud network, subnet, security groups, and a compute instance running Oracle Linux.

## Architecture Overview

The infrastructure includes:

- **Virtual Cloud Network (VCN)** - A private network in the cloud with CIDR block `10.1.0.0/16`
- **Public Subnet** - A subnet with CIDR block `10.1.20.0/24` for hosting the web server
- **Internet Gateway** - Provides internet access to resources in the public subnet
- **Route Table** - Routes traffic between the subnet and internet gateway
- **Security List** - Acts as a virtual firewall with rules for SSH (port 22) and HTTP (port 80) access
- **Compute Instance** - A VM.Standard.E2.1.Micro instance running Oracle Linux 7.9

## Prerequisites

Before you begin, ensure you have:

1. **Oracle Cloud Infrastructure Account** - Active OCI account with appropriate permissions
2. **Terraform** - Version 0.12 or later installed on your local machine
3. **OCI CLI** (Optional) - For easier credential management
4. **SSH Key Pair** - For secure access to the compute instance

## Required OCI Permissions

Your OCI user must have permissions to:
- Manage VCNs and networking components
- Manage compute instances
- Read identity resources (availability domains)

## Setup Instructions

### 1. Clone the Repository

```bash
git clone <repository-url>
cd <project-directory>
```

### 2. Configure OCI Credentials

You'll need to provide the following OCI credentials:

- **Tenancy OCID** - Your OCI tenancy identifier
- **User OCID** - Your OCI user identifier  
- **Fingerprint** - Fingerprint of your API signing key
- **Private Key** - Path to your private API signing key file
- **Region** - OCI region where resources will be created
- **Compartment OCID** - Target compartment for resources

### 3. Generate SSH Key Pair

If you don't have an SSH key pair, generate one:

```bash
ssh-keygen -t rsa -b 2048 -f ~/.ssh/oci_key
```

### 4. Create terraform.tfvars File

Create a `terraform.tfvars` file in the project root with your specific values:

```hcl
# OCI Provider Configuration
tenancy_ocid     = "ocid1.tenancy.oc1..your-tenancy-ocid"
user_ocid        = "ocid1.user.oc1..your-user-ocid"
fingerprint      = "your-key-fingerprint"
private_key      = "~/.ssh/oci_api_key.pem"
region           = "us-phoenix-1"
compartment_ocid = "ocid1.compartment.oc1..your-compartment-ocid"

# SSH Configuration
ssh_public_key   = "ssh-rsa AAAAB3NzaC1yc2E... your-public-key"
```

## Supported Regions

This configuration supports the following OCI regions:

- `us-phoenix-1` (US West - Phoenix)
- `us-ashburn-1` (US East - Ashburn)  
- `sa-saopaulo-1` (South America - São Paulo)

## Deployment

### 1. Initialize Terraform

```bash
terraform init
```

### 2. Plan the Deployment

```bash
terraform plan
```

### 3. Apply the Configuration

```bash
terraform apply
```

Type `yes` when prompted to confirm the deployment.

### 4. Access Your Web Server

After deployment completes, you can SSH to your instance:

```bash
ssh -i ~/.ssh/oci_key opc@<public-ip-address>
```

The public IP address will be displayed in the Terraform output.

## Resource Details

### Networking
- **VCN CIDR**: 10.1.0.0/16
- **Subnet CIDR**: 10.1.20.0/24
- **DNS Labels**: tcbvcn, tcbsubnet

### Security
- **SSH Access**: Port 22 from anywhere (0.0.0.0/0)
- **HTTP Access**: Port 80 from anywhere (0.0.0.0/0)
- **Outbound**: All traffic allowed

### Compute
- **Shape**: VM.Standard.E2.1.Micro (Always Free eligible)
- **OS**: Oracle Linux 7.9
- **Storage**: Default boot volume

## Customization

### Changing Instance Shape

Modify the `shape` parameter in the `oci_core_instance` resource:

```hcl
shape = "VM.Standard2.1"  # Example: Different shape
```

### Adding More Security Rules

Add additional ingress rules to the security list:

```hcl
ingress_security_rules {
  protocol = "6"
  source   = "0.0.0.0/0"
  
  tcp_options {
    max = "443"
    min = "443"
  }
}
```

### Using Different Regions

Add your desired region to the `ad_region_mapping` and `images` variables, then update your `terraform.tfvars` file.

## Cleanup

To destroy all created resources:

```bash
terraform destroy
```

Type `yes` when prompted to confirm the destruction.

## Troubleshooting

### Common Issues

1. **Authentication Errors**
   - Verify your OCI credentials in `terraform.tfvars`
   - Ensure your API key is properly configured

2. **Permission Denied**
   - Check that your OCI user has the required permissions
   - Verify the compartment OCID is correct

3. **Resource Limits**
   - Ensure you haven't exceeded your tenancy limits
   - Check availability domain capacity

4. **SSH Connection Issues**
   - Verify the SSH public key is correctly formatted
   - Check security list rules allow SSH access
   - Ensure you're using the correct private key

## Security Considerations

- The current configuration allows SSH and HTTP access from anywhere (0.0.0.0/0)
- For production use, restrict source IP ranges to your specific networks
- Consider using a bastion host for SSH access
- Implement proper key rotation and management practices

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the Mozilla Public License v2.0 - see the license header in the source files for details.

## Support

For issues related to:
- **Terraform**: Check the [Terraform documentation](https://www.terraform.io/docs/)
- **OCI Provider**: See the [OCI Terraform Provider documentation](https://registry.terraform.io/providers/hashicorp/oci/latest/docs)
- **Oracle Cloud**: Visit the [OCI documentation](https://docs.oracle.com/en-us/iaas/)