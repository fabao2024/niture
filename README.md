readme_text = """# ⚙️ Infrastructure as Code + Pipeline  
Automated Deployment of Networking and Compute Resources on **Oracle Cloud Infrastructure (OCI)** using **GitHub + Terraform Cloud + Terraform IaC**  

## 📖 Project Overview  
This project provisions and deploys the infrastructure for the **Niture Website** entirely using **Infrastructure as Code (IaC)**.  
Every step from networking to compute to deployment is **fully automated** — no manual console clicks.  

Pipeline flow:  
**GitHub → Terraform Cloud → OCI**  

---

## 🎯 Objectives  
- Provision **OCI Networking + Compute** via **Terraform**.  
- Ensure **automation, reproducibility, and security**.  
- Automate provisioning and deployment through **GitHub + Terraform Cloud** pipeline.  

---

## 🏗️ Architecture  

**High-Level Components**  
- 🌐 Networking: VCN, Subnet, IGW, Route Table, Security Rules  
- 🖥️ Compute: VM instance (with SSH access + website deployment)  
- 🔄 CI/CD Pipeline: GitHub repo → Terraform Cloud → OCI  

![Architecture Diagram Placeholder](./docs/architecture-diagram.png)  
*(Insert high-level diagram here)*  

---

## 🔑 Setup & Requirements  

### Accounts & Tools  
- [GitHub](https://github.com) repository with Terraform configs  
- [Terraform Cloud](https://app.terraform.io) (VCS-connected to GitHub)  
- [Oracle Cloud Infrastructure](https://oracle.com/cloud/) account with API keys  

### Terraform Cloud Variables  
Add to **Terraform Cloud → Workspace → Variables**:  
- `tenancy_ocid`  
- `compartment_ocid`  
- `user_ocid`  
- `fingerprint`  
- `region`  
- `private_key` (sensitive, full PEM)  
- `ssh_public_key`  

### OCI IAM Prep  
- Create user → add to `Administrators` group  
- Generate API keypairs  
- Upload `ssh_public_key` via Terraform Cloud variables  

**Screenshot Placeholder**  
![Terraform Cloud Workspace Setup Placeholder](./docs/terraform-cloud-setup.png)  

---

## 📂 Repository Structure  

```bash
/niture-iac  
  ├─ main.tf  
  ├─ variables.tf  
  ├─ outputs.tf  
  ├─ network/  
  │   ├─ vcn.tf  
  │   ├─ subnet.tf  
  │   ├─ igw_route.tf  
  │   └─ security.tf  
  └─ compute/  
      └─ instance.tf
