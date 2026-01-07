# AWS Account Governance and Security Foundations
Implement a security architecture that includes centralized logging with CloudTrail, compliance monitoring with AWS Config, security posture management with Security Hub, and cost management with AWS Budgets. 

## AWS Services Used
1. AWS IAM Identity Center & IAM
2. AWS Config
3. AWS Security Hub
4. AWS CloudTrail
5. AWS Budgets

## Implementation
### 1. Identity Center and IAM Security -- Configure AWS IAM Identity Center, enable MFA, and implement least privilege access with permission sets
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

### Step 1.5: Create Permission Sets
1. In the IAM Identity Center console, navigate to "Permission sets" in the left sidebar
2. Click "Create permission set"
3. Select "Predefined permission set"
4. Choose "AdministratorAccess" for your admin user
5. Click "Next"
6. Provide a name for the permission set (e.g., "AdministratorAccess")
7. Click "Next", review the information, and click "Create"
8. Repeat the process to create additional permission sets for different job functions:

### Step 1.6: Assign Users to AWS Accounts
1. In the IAM Identity Center console, navigate to "AWS accounts" in the left sidebar
2. Select your AWS account
3. Click "Assign users or groups"
4. Select "Users" and choose your administrative user
5. Click "Next"
6. Select the "AdministratorAccess" permission set
7. Click "Next", review the information, and click "Submit"

### Step 1.7: Set up IAM Access Analyzer
1. Navigate to IAM Service
2. Click "Access analyzer" in the left navigation pane
3. Click "Create analyzer"
4. Set "Analyzer name" to "AccountAnalyzer"
5. For "Zone of trust", select "Current account"
6. Click "Create analyzer"

### 2. Logging and Monitoring -- Set up CloudTrail and CloudWatch for comprehensive logging and monitoring   
### Step 2.1: Configure CloudTrail
1. Navigate to CloudTrail service
2. Click "Create trail"
3. Set "Trail name" to "AccountTrail"
4. Under "Storage location", choose "Create new S3 bucket"
5. Set a unique bucket name (e.g., "accounttrail-logs-[account-id]")
6. Under "Additional settings":
   - Enable "Log file validation"
   - Enable "Enable for all accounts in my organization" if using AWS Organizations
   - Enable SSE-KMS encryption and choose "Create a new KMS key"
   - Set key alias to "cloudtrail-key"
7. Under "Additional configuration":
   - Select "Management events"
   - Select "All" for Read and Write events
   - Enable "Insights events" for enhanced monitoring
8. Click "Next" and then "Create trail"

### Step 2.2: Set up CloudWatch Alarms for CloudTrail
1. Navigate to CloudWatch service
2. Click "Alarms" in the left navigation pane
3. Click "Create alarm"
4. Click "Select metric"
5. In the search box, type "CloudTrail" and press Enter
6. You should see a list of CloudTrail metrics. Select any of the available metrics such as:
   - `CallCount` for any CloudTrail API operation (e.g., StartLogging, StopLogging, UpdateTrail)
   - Choose one that represents a security-sensitive operation
7. Select the metric and click "Select metric"
8. Configure the alarm conditions:
   - Statistic: Sum
   - Period: 5 minutes
   - Threshold type: Static
   - Condition: Greater/Equal than 1
9. Click "Next"
10. Configure notifications:
    - Alarm state trigger: In alarm
    - Select "Create new topic"
    - Topic name: "SecurityAlerts"
    - Email endpoint: Enter your email address
11. Click "Create topic"
12. Click "Next"
13. Set alarm name: "CloudTrail-API-Activity-Alarm"
14. Add description: "Alert on CloudTrail API activity"
15. Click "Next" and "Create alarm"
16. Confirm the SNS subscription in your email

### 3. AWS Config and Compliance -- Deploy AWS Config and configure compliance rules
### Step 3.1: Enable AWS Config
1. Navigate to AWS Config service
2. Click "Get started" or "Settings" (if already configured)
3. Under "Settings":
   - Select "Record all resources supported in this region"
   - Keep "Include global resources" checked
4. Under "Delivery method":
   - Choose "Create a new S3 bucket"
   - Set a unique bucket name (e.g., "config-bucket-[account-id]")
5. Enable "Enable Amazon SNS topic" and create a new topic named "ConfigAlerts"
6. Click "Next"
7. Skip adding rules for now (click "Next" without selecting any rules)
8. Click "Confirm" to enable AWS Config

### Step 3.2: Deploy Config Rules Using CloudFormation

Now that you have AWS Config set up, you'll use CloudFormation to deploy a set of security rules:

1. Navigate to the CloudFormation service
2. Click "Create stack" > "With new resources (standard)"
3. Under "Specify template":
   - Select "Upload a template file"
   - Click "Choose file" and select the `account-governance.yaml` template file 
4. Click "Next"
5. Enter a stack name (e.g., "config-security-rules")
6. Click "Next", then "Next" again on the Configure stack options page
7. Review the settings and click "Create stack"

This CloudFormation template deploys five essential AWS Config rules:
- IAM Password Policy check
- Root account MFA check
- IAM User MFA check
- CloudTrail enabled check
- S3 bucket public write protection check

### 4. Security Hub Implementation -- Enable Security Hub and configure security standards for centralized security management
### Step 4.1: Enable Security Hub
1. Navigate to AWS Security Hub
2. Click "Go to Security Hub"
3. On the welcome page, click "Enable Security Hub"
4. Select the following security standards:
   - AWS Foundational Security Best Practices
   - NIST 800-53
5. Click "Enable Security Hub"

### Step 4.2: Configure Security Hub Settings
1. In Security Hub, click "Settings" in the left navigation pane
2. Under "Configuration":
   - Enable "Automatically enable new controls when added to standards"
   - Enable "Enable Security Hub in new accounts automatically"
3. Under "Integrations", enable the following:
   - AWS IAM Access Analyzer
   - AWS Config
4. Click "Save"

### Step 4.3: Review Security Hub Dashboard
1. In Security Hub, click "Summary" in the left navigation pane
2. Review your current security posture
3. Note any critical or high severity findings for immediate remediation
4. Click on "Insights" to review predefined security insights

### 5. Cost Controls -- Implement AWS Budgets 
### Step 5.1: Set up AWS Budgets
1. Navigate to AWS Budgets (via AWS Cost Management)
2. Click "Create budget"
3. Select "Use a template" and choose "Monthly cost budget"
4. Set a realistic budget amount based on your expected usage
5. Configure alerts at 50%, 80%, and 100% of your budget
6. Add email recipients for the alerts
7. Click "Create budget"


























