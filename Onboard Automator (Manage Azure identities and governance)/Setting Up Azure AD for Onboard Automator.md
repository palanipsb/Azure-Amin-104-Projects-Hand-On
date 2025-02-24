# Step 1: Azure AD Setup

## Objective: Set up a new Azure AD instance (if not already present) using the Azure portal.

## Steps to Set Up Azure AD:

## Log in to the Azure Portal:

1. Go to the Azure Portal.

2. Sign in with your Azure account credentials.

## Navigate to Azure Active Directory:

1. In the left-hand menu, click on "Azure Active Directory" (you can also search for it in the search bar at the top).

2. If you already have an Azure AD instance, it will show up here. If not, you’ll need to create one.

## Create a New Azure AD Instance:

1. If you don’t have an Azure AD instance, follow these steps:

2. Click on "Create a resource" in the left-hand menu or on the homepage.

3. Search for "Azure Active Directory" in the marketplace.

4. Click on "Create".

5. Fill in the required details:

6. Organization name: Enter a name for your Azure AD tenant (e.g., OnboardAutomatorAD).

7. Initial domain name: This will be the default domain name for your Azure AD (e.g., onboardautomator.onmicrosoft.com).

8. Country or region: Select your country or region.

9. Click "Create" to provision the new Azure AD instance.
   ![New User Creation](<New User.png>)

# Step 2: Logic App Workflow Design

Objective: Design a Logic App workflow triggered by an event (like an email to a specific mailbox) indicating a new employee hire.

## Steps to Design the Logic App Workflow:

### Create a Logic App:

1. Go to the Azure Portal.

2. Click on "Create a resource" in the left-hand menu or on the homepage.

3. Search for "Logic App" and click "Create".

4. Fill in the required details:

   - Subscription: Select your Azure subscription.

   - Resource Group: Select an existing resource group or create a new one (e.g., OnboardAutomator-RG).

   - Logic App Name: Enter a name for your Logic App (e.g., OnboardAutomator-Workflow).

   - Region: Select the region closest to you.

   - Click "Review + Create", then "Create" to provision the Logic App.
     ![alt text](<new login app.png>)

### Open the Logic App Designer:

1. Once the Logic App is created, go to the resource.

2. In the Logic App blade, click on "Add Workflow".

### Choose a Trigger:

1. In the Logic App Designer, you’ll see a list of common triggers. Search for "When a new email arrives (V2)" under the "Connectors" section.

2. Select this trigger to start your workflow.

### Connect to an Email Account:

1. When prompted, sign in to the email account you want to use for the trigger (e.g., onboarding@yourcompany.com).

2. Once connected, configure the trigger:

3. Folder: Choose the folder to monitor (e.g., Inbox).

4. Importance: Set to Normal (or customize as needed).

5. Include Attachments: Set to No (unless you need to process attachments).

6. Subject Filter: Add a filter to trigger only when the email subject contains specific keywords (e.g., New Hire).
   ![alt text](<email trigger.png>)

### Add Actions to the Workflow:

1. After the trigger, you’ll add actions to automate the onboarding process. Here’s an example workflow:

#### Action 1: Parse Email Content:

1. Search for and add the "Javascript" action.

2. Use the "workflowContext.trigger.outputs.body.Body" output from the trigger as the input for this action.

3. This will extract details like the employee’s name, email, department, etc., from the email body.
   ![alt text](<extract email body.png>)

#### Action 2: Create User in Azure AD:

1. Search for and add the "Create user (V2)" action under the "Azure AD" connector.

2. Use the parsed email content to fill in the user details (e.g., Display Name, User Principal Name, Password).
   ![alt text](<New User.png>)

#### Action 3: Add Action for Role Assignment:

1. After the "Create user (V2)" action, add an action to assign a role to the newly created user.

2. Search for and add the "Add member to group (V2)" action under the "Azure AD" connector.

3. Configure the action:

   - Group ID: Select the group to which the user should be added (e.g., Employees).

   - User ID: Use the "User Principal Name" output from the "Create user (V2)" action.
     ![alt text](<Add user to group.png>)

#### Action 4: Add Action for Resource Provisioning (VM Creation):

1. After the role assignment, add an action to provision a VM for the new employee.

2. Search for and add the "Create or Update a Virtual Machine" action under the "Azure Resource Manager" connector.

3. Configure the action:

   - Subscription: Select your Azure subscription.

   - Resource Group: Select the resource group where the VM will be created (e.g., OnboardAutomator-VMs).

   - Location: Select the region for the VM.

   - Virtual Machine Name: Enter a name for the VM (e.g., VM-{UserPrincipalName}).

   - Image: Select the OS image for the VM (e.g., Windows Server 2022 Datacenter).

   - Size: Select the VM size (e.g., Standard_B1s).

   - Admin Username: Enter the admin username for the VM.

   - Admin Password: Enter the admin password for the VM.

   - Network Interface: Configure the network interface for the VM.

#### Action 3: Send Welcome Email:

1. Search for and add the "Send an email (V2)" action under the "Office 365 Outlook" connector.

2. Configure the email with a welcome message and include details like the new user’s credentials.

3. Save and Test the Workflow:

   - Click "Save" to save your Logic App workflow.

   - Test the workflow by sending an email to the specified mailbox with the subject New Hire.

   - Check the Logic App run history to ensure the workflow triggers and executes successfully.
     ![alt text](<send an email.png>)
