# HCC Blueprint Designer - User Guide

## Overview

The HCC Blueprint Designer is a powerful visual tool within the IBM Consulting Hybrid Cloud Console that enables developers and platform engineers to design, configure, and deploy cloud infrastructure blueprints. This guide walks you through the complete process of creating and deploying infrastructure using the Blueprint Designer.

## Accessing the Blueprint Designer

The Blueprint Designer can be accessed from the HCC Console at:
- **URL**: https://console.hcc.eu-de-sbx.cloudaccelerator.ibm.com/create/templates/default/cloud-bp-designer
- **Navigation**: HCC Console → Create → Blueprint Designer

## Getting Started

### Step 1: Launch the Blueprint Designer

Navigate to the Blueprint Designer from the HCC Console main menu.

![Blueprint Designer Home](images/blueprint-designer/Screenshot%202025-12-16%20at%203.19.05%20PM.png)

The Blueprint Designer provides an intuitive interface for creating infrastructure blueprints with drag-and-drop functionality.

---

### Step 2: Understanding the Designer Interface

The Blueprint Designer interface consists of several key areas:
- **Component Palette**: Left sidebar with available infrastructure components
- **Canvas**: Central design area for building your blueprint
- **Properties Panel**: Right sidebar for configuring component properties
- **Action Bar**: Top toolbar for save, deploy, and validation actions

![Designer Interface Overview](images/blueprint-designer/Screenshot%202025-12-16%20at%203.25.55%20PM.png)

---

### Step 3: Select Infrastructure Components

Browse through the available components in the palette. Components are organized by category:
- Compute resources
- Networking components
- Storage options
- Security services
- Database services

![Component Selection](images/blueprint-designer/Screenshot%202025-12-16%20at%203.26.36%20PM.png)

---

### Step 4: Add Components to Canvas

Drag components from the palette onto the canvas to begin building your infrastructure blueprint.

![Adding Components](images/blueprint-designer/Screenshot%202025-12-16%20at%203.26.52%20PM.png)

Continue adding all required components for your infrastructure design.

![Multiple Components Added](images/blueprint-designer/Screenshot%202025-12-16%20at%203.26.57%20PM.png)

---

### Step 5: Configure Component Properties

Select a component on the canvas to view and edit its properties in the right panel.

![Component Properties](images/blueprint-designer/Screenshot%202025-12-16%20at%203.27.16%20PM.png)

Configure essential properties such as:
- Resource naming
- Size and capacity
- Region and availability zone
- Security settings
- Tags and metadata

![Property Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%203.27.36%20PM.png)

---

### Step 6: Establish Component Connections

Create connections between components to define their relationships and dependencies.

![Component Connections](images/blueprint-designer/Screenshot%202025-12-16%20at%203.28.17%20PM.png)

The designer automatically validates connections to ensure compatibility.

![Connection Validation](images/blueprint-designer/Screenshot%202025-12-16%20at%203.28.45%20PM.png)

---

### Step 7: Configure Network Settings

Set up networking configurations including:
- VPC configuration
- Subnet definitions
- Security group rules
- Load balancer settings

![Network Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%203.29.07%20PM.png)

![Advanced Network Settings](images/blueprint-designer/Screenshot%202025-12-16%20at%203.29.30%20PM.png)

---

### Step 8: Define Security Policies

Configure security policies and access controls for your infrastructure.

![Security Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%203.30.18%20PM.png)

![Security Policy Details](images/blueprint-designer/Screenshot%202025-12-16%20at%203.30.49%20PM.png)

---

### Step 9: Add Storage Configuration

Configure storage requirements for your infrastructure:
- Storage type and size
- Backup policies
- Encryption settings
- Performance tiers

![Storage Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%203.31.21%20PM.png)

![Storage Details](images/blueprint-designer/Screenshot%202025-12-16%20at%203.31.58%20PM.png)

---

### Step 10: Configure Database Services

If your blueprint includes database components, configure database settings:
- Database engine and version
- Instance size
- High availability options
- Backup and recovery settings

![Database Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%203.32.53%20PM.png)

![Database Properties](images/blueprint-designer/Screenshot%202025-12-16%20at%203.33.25%20PM.png)

---

### Step 11: Set Deployment Parameters

Configure deployment-specific parameters:
- Environment selection (dev, staging, production)
- Resource naming conventions
- Cost allocation tags
- Compliance requirements

![Deployment Parameters](images/blueprint-designer/Screenshot%202025-12-16%20at%203.33.52%20PM.png)

![Parameter Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%203.34.10%20PM.png)

---

### Step 12: Review Blueprint Architecture

Review the complete blueprint architecture before validation.

![Architecture Review](images/blueprint-designer/Screenshot%202025-12-16%20at%203.34.29%20PM.png)

![Complete Blueprint View](images/blueprint-designer/Screenshot%202025-12-16%20at%203.34.46%20PM.png)

---

### Step 13: Validate Blueprint Configuration

Run the validation process to ensure all components are correctly configured:
- Syntax validation
- Dependency checking
- Resource quota verification
- Security compliance check

![Validation Process](images/blueprint-designer/Screenshot%202025-12-16%20at%203.35.22%20PM.png)

Review validation results and address any errors or warnings.

![Validation Results](images/blueprint-designer/Screenshot%202025-12-16%20at%203.36.08%20PM.png)

![Validation Details](images/blueprint-designer/Screenshot%202025-12-16%20at%203.36.36%20PM.png)

---

### Step 14: Cost Estimation

View the estimated costs for your infrastructure blueprint.

![Cost Estimation](images/blueprint-designer/Screenshot%202025-12-16%20at%203.37.54%20PM.png)

![Detailed Cost Breakdown](images/blueprint-designer/Screenshot%202025-12-16%20at%203.38.27%20PM.png)

---

### Step 15: Save Blueprint

Save your blueprint for future use or modifications.

![Save Blueprint](images/blueprint-designer/Screenshot%202025-12-16%20at%203.39.04%20PM.png)

Provide a meaningful name and description:

![Blueprint Naming](images/blueprint-designer/Screenshot%202025-12-16%20at%203.39.19%20PM.png)

![Save Confirmation](images/blueprint-designer/Screenshot%202025-12-16%20at%203.39.43%20PM.png)

---

### Step 16: Review Deployment Configuration

Review the final deployment configuration before initiating deployment.

![Deployment Review](images/blueprint-designer/Screenshot%202025-12-16%20at%203.40.15%20PM.png)

![Configuration Summary](images/blueprint-designer/Screenshot%202025-12-16%20at%203.40.48%20PM.png)

---

### Step 17: Initiate Deployment

Start the deployment process by clicking the Deploy button.

![Initiate Deployment](images/blueprint-designer/Screenshot%202025-12-16%20at%203.41.23%20PM.png)

![Deployment Confirmation](images/blueprint-designer/Screenshot%202025-12-16%20at%203.41.46%20PM.png)

---

### Step 18: Monitor Deployment Progress

The system will create Tekton pipelines to orchestrate the deployment.

![Pipeline Creation](images/blueprint-designer/Screenshot%202025-12-16%20at%203.42.12%20PM.png)

![Deployment Pipeline](images/blueprint-designer/Screenshot%202025-12-16%20at%203.42.38%20PM.png)

Monitor the deployment progress in real-time:

![Deployment Progress](images/blueprint-designer/Screenshot%202025-12-16%20at%203.42.56%20PM.png)

---

### Step 19: View Deployment Status

Track the deployment status across different stages:

![Deployment Stages](images/blueprint-designer/Screenshot%202025-12-16%20at%203.46.50%20PM.png)

![Stage Progress](images/blueprint-designer/Screenshot%202025-12-16%20at%203.47.19%20PM.png)

---

### Step 20: Review Resource Provisioning

Monitor individual resource provisioning status:

![Resource Provisioning](images/blueprint-designer/Screenshot%202025-12-16%20at%203.48.07%20PM.png)

![Provisioning Details](images/blueprint-designer/Screenshot%202025-12-16%20at%203.48.52%20PM.png)

---

### Step 21: Check Deployment Logs

Access detailed deployment logs for troubleshooting or auditing:

![Deployment Logs](images/blueprint-designer/Screenshot%202025-12-16%20at%203.49.10%20PM.png)

![Log Details](images/blueprint-designer/Screenshot%202025-12-16%20at%203.49.36%20PM.png)

---

### Step 22: Verify Deployed Resources

Once deployment completes, verify all resources are properly provisioned:

![Resource Verification](images/blueprint-designer/Screenshot%202025-12-16%20at%203.52.22%20PM.png)

![Resource Status](images/blueprint-designer/Screenshot%202025-12-16%20at%203.52.45%20PM.png)

![Deployment Summary](images/blueprint-designer/Screenshot%202025-12-16%20at%203.53.10%20PM.png)

---

### Step 23: Access Deployed Infrastructure

View and access your deployed infrastructure:

![Infrastructure Access](images/blueprint-designer/Screenshot%202025-12-16%20at%204.19.43%20PM.png)

![Resource Dashboard](images/blueprint-designer/Screenshot%202025-12-16%20at%204.20.05%20PM.png)

---

### Step 24: Post-Deployment Configuration

Perform any post-deployment configuration tasks:

![Post-Deployment Config](images/blueprint-designer/Screenshot%202025-12-16%20at%204.21.28%20PM.png)

![Configuration Tasks](images/blueprint-designer/Screenshot%202025-12-16%20at%204.21.54%20PM.png)

---

### Step 25: Blueprint Management

Manage your deployed blueprints:

![Blueprint Management](images/blueprint-designer/Screenshot%202025-12-16%20at%204.32.27%20PM.png)

![Blueprint Dashboard](images/blueprint-designer/Screenshot%202025-12-16%20at%204.32.47%20PM.png)

---

### Step 26: Modify Existing Blueprints

Edit existing blueprints to add or modify components:

![Blueprint Editing](images/blueprint-designer/Screenshot%202025-12-16%20at%204.33.03%20PM.png)

![Blueprint Updates](images/blueprint-designer/Screenshot%202025-12-16%20at%204.33.23%20PM.png)

---

### Step 27: Version Control

Track blueprint versions and changes:

![Version Control](images/blueprint-designer/Screenshot%202025-12-16%20at%204.33.44%20PM.png)

![Version History](images/blueprint-designer/Screenshot%202025-12-16%20at%204.33.59%20PM.png)

---

### Step 28: Collaboration Features

Share and collaborate on blueprints with team members:

![Collaboration](images/blueprint-designer/Screenshot%202025-12-16%20at%204.35.25%20PM.png)

![Team Sharing](images/blueprint-designer/Screenshot%202025-12-16%20at%204.35.46%20PM.png)

---

### Step 29: Advanced Configuration

Access advanced configuration options for complex scenarios:

![Advanced Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%204.37.09%20PM.png)

![Advanced Settings](images/blueprint-designer/Screenshot%202025-12-16%20at%204.37.34%20PM.png)

---

### Step 30: Monitoring and Alerts

Set up monitoring and alerting for deployed infrastructure:

![Monitoring Setup](images/blueprint-designer/Screenshot%202025-12-16%20at%204.39.01%20PM.png)

![Alert Configuration](images/blueprint-designer/Screenshot%202025-12-16%20at%204.40.09%20PM.png)

---

### Step 31: Infrastructure Health Dashboard

View the health and status of your deployed infrastructure:

![Health Dashboard](images/blueprint-designer/Screenshot%202025-12-16%20at%204.40.22%20PM.png)

![Health Metrics](images/blueprint-designer/Screenshot%202025-12-16%20at%204.40.27%20PM.png)

![Detailed Metrics](images/blueprint-designer/Screenshot%202025-12-16%20at%204.41.03%20PM.png)

---

### Step 32: Performance Analytics

Access performance analytics and optimization recommendations:

![Performance Analytics](images/blueprint-designer/Screenshot%202025-12-16%20at%204.41.54%20PM.png)

![Optimization Recommendations](images/blueprint-designer/Screenshot%202025-12-16%20at%204.42.26%20PM.png)

---

## Key Features

### Visual Design Interface
- Drag-and-drop component placement
- Real-time validation
- Auto-layout optimization
- Component relationship visualization

### Component Library
- Pre-configured infrastructure components
- Custom component creation
- Component versioning
- Reusable component templates

### Validation Engine
- Pre-deployment validation
- Security compliance checking
- Cost estimation
- Resource quota verification
- Dependency resolution

### Integration Points
- **Tekton Pipelines**: Automated deployment orchestration
- **IBM Concert**: Complex workflow execution
- **ArgoCD**: GitOps synchronization
- **HashiCorp Vault**: Secure secret management

### Deployment Capabilities
- Multi-cloud support
- Environment-specific configurations
- Rollback capabilities
- Blue-green deployments
- Canary releases

## Best Practices

### Blueprint Design
1. **Start Simple**: Begin with core components and add complexity incrementally
2. **Use Templates**: Leverage existing templates as starting points
3. **Name Consistently**: Follow organizational naming conventions
4. **Tag Appropriately**: Use tags for cost tracking and resource organization
5. **Document Changes**: Add descriptions and comments for clarity

### Security Considerations
1. **Principle of Least Privilege**: Grant minimum required permissions
2. **Encrypt Sensitive Data**: Enable encryption for data at rest and in transit
3. **Network Segmentation**: Use VPCs and security groups appropriately
4. **Regular Audits**: Review and update security configurations regularly
5. **Secret Management**: Store all secrets in HashiCorp Vault

### Cost Optimization
1. **Right-Size Resources**: Choose appropriate instance sizes
2. **Use Auto-Scaling**: Implement auto-scaling where applicable
3. **Review Estimates**: Check cost estimates before deployment
4. **Monitor Usage**: Set up cost alerts and monitoring
5. **Clean Up Unused Resources**: Regularly remove unused infrastructure

### Deployment Strategy
1. **Test in Development**: Always test blueprints in dev environment first
2. **Version Control**: Maintain blueprint versions for rollback capability
3. **Gradual Rollout**: Use staged deployments for production changes
4. **Monitor Deployments**: Watch deployment logs and metrics
5. **Validation First**: Always validate before deploying