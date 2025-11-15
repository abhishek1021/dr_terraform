# DR Terraform Infrastructure Project

A comprehensive disaster recovery (DR) infrastructure project built with Terraform, featuring modular architecture, multi-environment support, and enterprise-grade CI/CD pipelines.

## 🎯 **Project Purpose**

This project provides a complete disaster recovery infrastructure solution for Apache Solr search clusters with the following objectives:

- **Disaster Recovery**: Rapid infrastructure deployment for business continuity
- **Multi-Environment**: Consistent infrastructure across DR, Stage, and Production
- **High Availability**: Auto-scaling Solr clusters with load balancing
- **Security**: Enterprise-grade security controls and access management
- **Automation**: Full CI/CD pipeline with GitHub Actions
- **Modularity**: Reusable Terraform modules for scalable infrastructure

## 🏗️ **Architecture Overview**

```
┌─────────────────────────────────────────────────────────────────┐
│                     DR Terraform Project                        │
├─────────────────────────────────────────────────────────────────┤
│  GitHub Actions CI/CD                                          │
│  ├── Bootstrap (S3 Backend Setup)                              │
│  ├── VPC Module Pipeline                                       │
│  └── Solr Stack Module Pipeline                                │
├─────────────────────────────────────────────────────────────────┤
│  Infrastructure Modules                                        │
│  ├── VPC Module (Network Foundation)                           │
│  │   ├── VPC, Subnets, Route Tables                           │
│  │   ├── Internet Gateway, NAT Gateways                       │
│  │   └── Transit Gateway (Cross-VPC)                          │
│  └── Solr Stack Module (Application Layer)                     │
│      ├── Auto Scaling Groups + Launch Templates               │
│      ├── Application Load Balancer                            │
│      ├── EFS Shared Storage                                   │
│      ├── S3 Backup Storage                                    │
│      ├── Security Groups + IAM Roles                          │
│      └── Multi-AZ Deployment                                  │
├─────────────────────────────────────────────────────────────────┤
│  Environments                                                  │
│  ├── DR (us-east-1)      - Auto-deploy, 1 node               │
│  ├── Stage (us-east-1)   - Manual approval, 2 nodes          │
│  └── Prod (us-east-1)    - Manual approval, 5 nodes          │
└─────────────────────────────────────────────────────────────────┘
```

## 📁 **Project Structure**

```
dr_terraform/
├── 📄 README.md                          # This file - project overview
├── 📄 GITHUB_SETUP.md                    # GitHub CI/CD setup guide
├── 📄 main.tf                            # Root Terraform configuration
├── 📄 variables.tf                       # Root variables
├── 📄 outputs.tf                         # Root outputs
├── 📄 backend.tf                         # Remote state configuration
├── 📄 provider.tf                        # AWS provider configuration
├── 📄 versions.tf                        # Terraform version constraints
│
├── 🗂️ .github/workflows/                 # CI/CD Pipeline
│   ├── 📄 bootstrap.yml                  # Backend setup workflow
│   ├── 📄 vpc-module.yml                 # VPC infrastructure CI/CD
│   ├── 📄 solr-stack-module.yml          # Solr application CI/CD
│   └── 📄 README.md                      # Workflow documentation
│
├── 🗂️ bootstrap/                         # Backend Infrastructure Setup
│   ├── 📄 main.tf                        # S3 + DynamoDB for remote state
│   ├── 📄 variables.tf                   # Bootstrap variables
│   ├── 📄 outputs.tf                     # Bootstrap outputs
│   ├── 📄 setup.sh                       # Automated setup script
│   ├── 📄 README.md                      # Bootstrap guide
│   ├── 📄 terraform.tfvars.dr            # DR bootstrap config
│   ├── 📄 terraform.tfvars.stage         # Stage bootstrap config
│   └── 📄 terraform.tfvars.prod          # Prod bootstrap config
│
├── 🗂️ backend-configs/                   # Remote State Configuration
│   ├── 📄 dr.hcl                         # DR backend config
│   ├── 📄 stage.hcl                      # Stage backend config
│   └── 📄 prod.hcl                       # Prod backend config
│
├── 🗂️ environments/                      # Environment-Specific Settings
│   ├── 📄 dr.json                        # DR environment configuration
│   ├── 📄 stage.json                     # Stage environment configuration
│   └── 📄 prod.json                      # Prod environment configuration
│
├── 🗂️ modules/                           # Custom Terraform Modules
│   ├── 🗂️ network/                       # Network Infrastructure
│   │   ├── 🗂️ vpc/                       # VPC Module
│   │   │   ├── 📄 main.tf                # VPC, subnets, routing
│   │   │   ├── 📄 variables.tf           # VPC variables
│   │   │   └── 📄 outputs.tf             # VPC outputs
│   │   └── 🗂️ networking/                # Advanced Networking
│   │       ├── 📄 main.tf                # IGW, NAT, TGW
│   │       ├── 📄 variables.tf           # Networking variables
│   │       └── 📄 outputs.tf             # Networking outputs
│   └── 🗂️ solr_stack_dr/                 # Solr Application Stack
│       ├── 📄 main.tf                    # Complete Solr infrastructure
│       ├── 📄 variables.tf               # Solr variables
│       ├── 📄 outputs.tf                 # Solr outputs
│       ├── 📄 README.md                  # Solr module documentation
│       ├── 📄 versions.tf                # Version constraints
│       └── 🗂️ examples/                  # Usage examples
│
├── 🗂️ reusable_modules/                  # Shared Terraform Modules
│   ├── 🗂️ vpc/                           # Reusable VPC components
│   ├── 🗂️ security_group/                # Security group templates
│   ├── 🗂️ alb/                           # Application Load Balancer
│   ├── 🗂️ autoscaling/                   # Auto Scaling Groups
│   ├── 🗂️ IAM/                           # IAM roles and policies
│   ├── 🗂️ S3/                            # S3 bucket configurations
│   ├── 🗂️ efs/                           # EFS file systems
│   ├── 🗂️ ec2/                           # EC2 instance templates
│   ├── 🗂️ dynamodb/                      # DynamoDB tables
│   ├── 🗂️ lambda/                        # Lambda functions
│   ├── 🗂️ opensearch/                    # OpenSearch clusters
│   ├── 🗂️ route53/                       # DNS management
│   ├── 🗂️ sns/                           # SNS topics
│   ├── 🗂️ sqs/                           # SQS queues
│   └── 🗂️ [other services]/              # Additional AWS services
│
├── 🗂️ user_data/                         # Instance Bootstrap Scripts
│   ├── 📄 solr_dr.sh                     # DR environment setup
│   ├── 📄 solr_stage.sh                  # Stage environment setup
│   └── 📄 solr_prod.sh                   # Prod environment setup
│
├── 🗂️ documentation/                     # Project Documentation
│   ├── 📄 MULTI_ENVIRONMENT_SETUP.md     # Multi-env configuration
│   ├── 📄 REQUEST_FLOW_DOCUMENTATION.md  # Request flow analysis
│   └── 📄 vpc_infrastructure_dr_summary.md # VPC architecture
│
└── 🗂️ backup_resources/                  # Legacy/Backup Configurations
    ├── 🗂️ load-balancers/                # ALB backup configs
    ├── 🗂️ networking/                    # Network backup configs
    ├── 🗂️ s3/                            # S3 backup configs
    ├── 🗂️ subnets/                       # Subnet backup configs
    ├── 🗂️ transit-gateway/               # TGW backup configs
    └── 🗂️ vpc-endpoints/                 # VPC endpoint configs
```

## 🚀 **Key Features**

### **Infrastructure Components**
- ✅ **Multi-AZ VPC** with public/private subnets
- ✅ **Auto Scaling Solr Cluster** with ELB health checks
- ✅ **Application Load Balancer** for high availability
- ✅ **EFS Shared Storage** for Solr data persistence
- ✅ **S3 Backup Storage** with lifecycle policies
- ✅ **Security Groups** with least-privilege access
- ✅ **IAM Roles** with minimal required permissions
- ✅ **Transit Gateway** for cross-VPC connectivity

### **Operational Excellence**
- ✅ **Multi-Environment Support** (DR, Stage, Prod)
- ✅ **Environment-Specific Configuration** via JSON files
- ✅ **Automated CI/CD Pipeline** with GitHub Actions
- ✅ **Feature Flag Controls** for safe deployments
- ✅ **Manual Approval Gates** for production changes
- ✅ **Security Scanning** with Checkov and Trivy
- ✅ **Cost Estimation** with Infracost integration
- ✅ **State Management** with S3 backend and DynamoDB locking

### **Security & Compliance**
- ✅ **OIDC Authentication** (no long-lived credentials)
- ✅ **Environment Isolation** with separate AWS roles
- ✅ **Encryption at Rest** for all storage components
- ✅ **Network Segmentation** with security groups
- ✅ **Audit Logging** via GitHub Actions history
- ✅ **Destroy Protection** with feature flags

## 🛠️ **Technology Stack**

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Infrastructure** | Terraform 1.5.0+ | Infrastructure as Code |
| **CI/CD** | GitHub Actions | Automated deployment pipeline |
| **Cloud Provider** | AWS | Infrastructure hosting |
| **Authentication** | AWS OIDC | Secure GitHub-to-AWS access |
| **State Management** | S3 + DynamoDB | Remote state storage and locking |
| **Security Scanning** | Checkov, Trivy | Infrastructure security validation |
| **Cost Analysis** | Infracost | Cost estimation and optimization |
| **Configuration** | JSON | Environment-specific settings |

## 📋 **Prerequisites**

### **Required Tools**
- **Terraform** >= 1.5.0
- **AWS CLI** >= 2.0
- **Git** >= 2.0
- **SSH** client for instance access

### **Required Access**
- **AWS Account** with administrative permissions
- **GitHub Repository** with Actions enabled
- **Domain/DNS** management (if using custom domains)

### **Required Knowledge**
- Basic Terraform concepts and syntax
- AWS networking and security fundamentals
- GitHub Actions workflow basics
- Apache Solr administration (for application management)

## 🚀 **Quick Start Guide**

### **Step 1: Clone Repository**
```bash
git clone <repository-url>
cd dr_terraform
```

### **Step 2: Setup GitHub CI/CD**
Follow the comprehensive setup guide:
```bash
# Read the complete setup instructions
cat GITHUB_SETUP.md
```

**Key setup steps:**
1. **AWS OIDC Setup** - Create identity provider and IAM roles
2. **GitHub Secrets** - Configure AWS roles and SSH keys
3. **GitHub Variables** - Set feature flags for deployment control
4. **Environment Protection** - Configure approval workflows

### **Step 3: Bootstrap Backend**
```bash
# Run bootstrap workflow in GitHub Actions
# Actions → Bootstrap Terraform Backend → dr → create
```

### **Step 4: Deploy Infrastructure**
```bash
# Create feature branch
git checkout -b feature/initial-deployment

# Make any necessary configuration changes
# Edit environments/dr.json for DR-specific settings

# Create pull request → triggers validation
# Merge PR → auto-deploys to DR environment
```

### **Step 5: Verify Deployment**
```bash
# Check infrastructure in AWS Console
# - VPC and subnets created
# - Auto Scaling Group with instances
# - Application Load Balancer healthy
# - EFS file system mounted
# - S3 backup bucket created
```

## 🔧 **Configuration Management**

### **Environment-Specific Settings**
Each environment has its own JSON configuration file:

**`environments/dr.json`** - Disaster Recovery
```json
{
  "vpc_cidr": "10.200.48.0/20",
  "solr_instance_type": "m5.xlarge",
  "solr_cluster_size": 1,
  "solr_fallback_ami_id": "ami-0abcdef1234567890"
}
```

**`environments/stage.json`** - Staging
```json
{
  "vpc_cidr": "10.210.48.0/20", 
  "solr_instance_type": "m5.large",
  "solr_cluster_size": 2,
  "solr_fallback_ami_id": "ami-0abcdef1234567891"
}
```

**`environments/prod.json`** - Production
```json
{
  "vpc_cidr": "10.220.48.0/20",
  "solr_instance_type": "m5.2xlarge", 
  "solr_cluster_size": 5,
  "solr_fallback_ami_id": "ami-0abcdef1234567892"
}
```

### **Feature Flag Controls**
Control deployment behavior via GitHub Variables:

```bash
# Enable/disable module deployments
FEATURE_VPC_SETUP = true/false
FEATURE_SOLR_SETUP = true/false

# Enable/disable destroy operations (safety)
FEATURE_VPC_DESTROY = false (recommended)
FEATURE_SOLR_DESTROY = false (recommended)
```

## 🔄 **Deployment Workflows**

### **Automatic Deployment (Recommended)**
```bash
# 1. Create feature branch
git checkout -b feature/infrastructure-updates

# 2. Make changes to modules or configuration
# Edit modules/solr_stack_dr/main.tf or environments/dr.json

# 3. Create pull request
git add .
git commit -m "Update Solr cluster configuration"
git push origin feature/infrastructure-updates

# 4. Create PR in GitHub → triggers validation for all environments
# 5. Review PR comments with validation results
# 6. Merge PR → automatically deploys to DR environment
```

### **Manual Deployment**
```bash
# Deploy specific module to specific environment
# Actions → VPC Module CI/CD → stage → apply → Approve
# Actions → Solr Stack Module CI/CD → prod → apply → Approve
```

### **Emergency Procedures**
```bash
# 1. Enable destroy feature flag
FEATURE_SOLR_DESTROY = true

# 2. Run destroy workflow
# Actions → Solr Stack Module CI/CD → environment → destroy → Approve

# 3. Reset feature flag
FEATURE_SOLR_DESTROY = false
```

## 🛡️ **Security Considerations**

### **Access Control**
- **AWS OIDC**: No long-lived credentials stored in GitHub
- **Environment Isolation**: Separate IAM roles per environment
- **Branch Protection**: Production restricted to main branch
- **Manual Approvals**: Required for stage/prod deployments

### **Network Security**
- **Private Subnets**: Solr instances in private subnets only
- **Security Groups**: Least-privilege access rules
- **NACLs**: Additional network-level protection
- **VPC Flow Logs**: Network traffic monitoring

### **Data Protection**
- **Encryption at Rest**: All EBS volumes and EFS encrypted
- **Encryption in Transit**: TLS for all communications
- **Backup Encryption**: S3 backup buckets encrypted
- **Key Management**: AWS KMS for encryption keys

## 📊 **Monitoring & Observability**

### **Infrastructure Monitoring**
- **CloudWatch Metrics**: Auto Scaling Group health
- **ALB Health Checks**: Application availability
- **EFS Monitoring**: Storage performance metrics
- **Cost Monitoring**: AWS Cost Explorer integration

### **Application Monitoring**
- **Solr Admin UI**: Cluster status and performance
- **Log Aggregation**: CloudWatch Logs integration
- **Alerting**: SNS notifications for critical events

## 🔧 **Maintenance & Operations**

### **Regular Tasks**
- **AMI Updates**: Update `solr_fallback_ami_id` in environment files
- **Security Patches**: Apply via new AMI deployments
- **Capacity Planning**: Monitor and adjust cluster sizes
- **Backup Verification**: Test restore procedures regularly

### **Scaling Operations**
```bash
# Update cluster size in environment JSON
"solr_cluster_size": 3  # Increase from 1 to 3

# Deploy via PR or manual workflow
# Auto Scaling Group will launch additional instances
```

### **Disaster Recovery Testing**
```bash
# 1. Deploy to DR environment
# 2. Verify all services operational
# 3. Test data restoration from backups
# 4. Validate network connectivity
# 5. Document any issues and improvements
```

## 🆘 **Troubleshooting**

### **Common Issues**

**Deployment Failures:**
- Check GitHub Actions logs for specific errors
- Verify AWS permissions and OIDC configuration
- Ensure feature flags are properly set
- Validate environment JSON syntax

**Infrastructure Issues:**
- Check Auto Scaling Group health in AWS Console
- Verify security group rules allow required traffic
- Ensure EFS mount targets are healthy
- Check ALB target group health

**Access Issues:**
- Verify SSH key pairs exist in AWS
- Check security group SSH rules
- Ensure instances are in private subnets with NAT gateway access

### **Support Resources**
- **GitHub Issues**: Report bugs and feature requests
- **Documentation**: Comprehensive guides in `/documentation/`
- **AWS Support**: For AWS-specific infrastructure issues
- **Terraform Documentation**: For module development

## 🤝 **Contributing**

### **Development Workflow**
1. **Fork repository** and create feature branch
2. **Make changes** following project conventions
3. **Test locally** with `terraform plan`
4. **Create pull request** with detailed description
5. **Address review feedback** and merge

### **Code Standards**
- **Terraform**: Follow HashiCorp style guide
- **Documentation**: Update README files for changes
- **Security**: Run security scans before submission
- **Testing**: Validate with multiple environments

## 📄 **License**

This project is licensed under the MIT License - see the LICENSE file for details.

## 📞 **Support**

For questions, issues, or contributions:
- **GitHub Issues**: Technical problems and feature requests
- **Documentation**: Comprehensive guides in project directories
- **Team Contact**: [Your team contact information]

---

**Built with ❤️ for reliable disaster recovery infrastructure**
