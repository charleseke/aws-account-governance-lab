# AWS Account Governance and Security Foundations
Implement a security architecture that includes centralized logging with CloudTrail, compliance monitoring with AWS Config, security posture management with Security Hub, and cost management with AWS Budgets. 

## AWS Services Used
- AWS IAM Identity Center (formerly AWS SSO)
- AWS Identity and Access Management (IAM)
- AWS Config
- AWS Security Hub
- AWS CloudTrail
- AWS Budgets

## Implementation
1. **Identity Center and IAM Security** -- Configure AWS IAM Identity Center, enable MFA, and implement least privilege access with permission sets

### Step 1.1: Enable MFA for Root User
1. Sign in to the AWS Management Console as the root user
2. In the navigation bar, click on your account name, then "Security credentials"
3. In the "Multi-factor authentication (MFA)" section, click "Assign MFA device"
4. Choose your preferred MFA device type (Virtual, Security Key, or Hardware)
5. Follow the on-screen instructions to complete the setup
6. Verify that MFA status shows "Assigned" for your root user

### Step 1.2: Enable AWS IAM Identity Center
1. Navigate to the AWS IAM Identity Center service in the AWS Management Console
2. Click "Enable" to start the setup process
3. Choose "Default IAM Identity Center directory" as the identity source
4. Click "Next" to complete the setup

### Step 1.3: Create an Administrative User in Identity Center
1. In the IAM Identity Center console, navigate to "Users" in the left sidebar
2. Click "Add user"
3. Fill out the user details:
   - Username (e.g., "admin")
   - Email address
   - First name and Last name
   - Confirm email
4. Click "Next"
5. On the "Add user to groups" page, click "Next" (we'll create groups in the next step)
6. Review the information and click "Add user"

### Step 1.4: Set Up MFA for Identity Center User
1. Stay in the IAM Identity Center console and navigate to "Users"
2. Select the administrative user you just created
3. Under the "MFA devices" tab, click "Register MFA device"
4. Choose your preferred MFA type (Authenticator app, Security key, or Built-in authenticator)
5. Follow the prompts to register the MFA device
6. Click "Register MFA device" to complete the process
