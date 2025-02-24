# Deployment guide

Please note - this deployment guide assumes a proficient level of knowledge in deploying Power Apps solutions and navigating/using the Azure Portal.

There are two sections to this deployment guide, one for **Prompt Master** and one for **Prompt Master Pro**, be sure to follow the correct section for the solution are you are deploying.

# Prompt Master

## Prerequisites

- 1 x Premium license for Power Apps **OR** use the [Developer plan](https://learn.microsoft.com/en-us/power-platform/developer/plan). The user account you run Prompt Master will require this license or the developer plan/environment. If you use a developer plan, you will need to run Prompt Master as the user account who is assigned the developer plan.
- Power Apps environment with a Dataverse database deployed (only required due to the solution using Environment Variables). You may use the default environment. If you do not have capacity to create a Dataverse database, you may need to use the default environment. If you are using the developer plan, you can use the developer environment.
- SharePoint site which will contain the lists - we recommend creating a new one for Prompt Master.
- Full Control access to the above site. 
- The [latest release](https://github.com/pnp/prompt-master/releases/latest) of Prompt Master.
- Azure subscription.
- Access to create an Azure OpenAI instance and model deployment (you may use an existing one if you wish). Azure OpenAI is used to rate a prompt when it is submitted to complete a challenge.

## Step 1: Create SharePoint Lists

  1. Navigate to the SharePoint site.
  2. Create a list named 'Challenges'.
  3. Create the following columns:

  | Column Name    | Type | Values |
| -------- | ------- | ------- | 
| Name  | Multiple lines of text    |
| Instructions | Multiple lines of text     |
| Image | Image     |  |
| Tips    | Multiple lines of text  |
| App    | Choice  | Copilot, Word, Excel, PowerPoint, Outlook, Teams, Loop, Designer, Forms   | Default value = Copilot

4. Create a list named 'Players'.
5. Create the following columns:

| Column Name    | Type | Values |
| -------- | ------- | ------- | 
| Name  | Single line of text    |
| User  | Person or Group    |
| Player Name  | Single line of text    |
| Email | Single line of text     |
| Score | Number     |

6. Create a list named 'PlayerChallenges' (rename list after creation to read 'Player Challenges').
7. Create the following columns:

| Column Name    | Type | Values |
| -------- | ------- | ------- | 
| Player ID  | Number |
| Challenge ID  | Number    |
| Prompt  | Multiple lines of text    |
| Rating | Number     |

8. Create a list named 'Configuration'.
9. Create the following columns:

| Column Name    | Type |
| -------- | ------- | 
| Value  | Single line of text 

10. Create a list item in the above list with the following details:

Title: AdminGroupId

Value: ID of an M365 group that contains admins for the Prompt Master app. This controls the visibility of the settings option in the app which provides the ability to reset/clear all data.

## Step 2: Create Azure OpenAI Instance/Deployment

1. Navigate to the Azure Portal.
2. Create a new Azure OpenAI instance in your desired resource group.
3. Once created, navigate to OpenAI Studio.
4. Create a new deployment through **Deployments > Deploy model > Deploy base model**, choose **gpt-4o** as the model. **Keep the name of the deployment as 'gpt-4o'**. You may use an existing OAI instance/deployment but bear in mind the rating results may differ depending on the model you use - Prompt Master has been tested on gpt-4o.
5. Copy the Key and Endpoint URI as you will need these later.

## Step 3: Deploy Prompt Master OAI Custom Connector Solution

1. Navigate to **Power Apps** and ensure you are in the correct environment you wish to deploy Prompt Master to.
2. Click on the **Solutions** tab.
3. Click on **Import**.
4. Upload the **PromptMasterOAIConnector_1_0_0_0.zip** solution file and click **Next** (version number may differ).
5. On the **Environment Variables** pane, enter the endpoint for your Open AI instance (**note you don't need the protocol or anything after /openai** for example **'promptmaster-contoso-oai.openai.azure.com'**).
7. Click **Import**.
8. A message should be displayed to say the solution has been imported successfully.

## Step 4: Deploy Prompt Master Solution

1. Navigate to **Power Apps** and ensure you are in the correct environment you wish to deploy Prompt Master to.
2. Click on the **Solutions** tab.
3. Click on **Import**.
4. Upload the **PromptMaster_1_0_0_0.zip** solution file and click **Next** (version number may differ).
5. Click **Sign in** next to the SharePoint and Office 365 Groups Connection Of and wait for the green tick.
6. Click **Create** next to the custom connector connection (Prompt Master Azure OAI Connection). Enter any name you wish for the 'Connection name' and enter the API key for your OpenAI deployment that you created earlier and click **Create**.
7. Click **Next**.
8. On the **Environment Variables** pane, select the SharePoint site you created (you may need to paste the URL into the dropdown if it cannot be found), the relevant lists and enter the URL to your Prompt Master site. (Ignore any warnings about access).
9. Click **Import**.
10. A message should be displayed to say the solution has been imported successfully.

## Step 5: Share the app/flows

1. Locate the app under **Apps**.
2. Share the app with the user accounts that will be running Prompt Master (you may want to add any administrators as co-owners so they can modify the app if you wish to). **We recommend running the app on a day as a single user.**

You may also wish to share the flows (**'Rate Prompt'**, **'Create Player Name'**) with admins who may need to view the run history or edit them. You can find them under **Flows** in the **Power Apps** portal or in the **Power Automate** portal.

If you are using a developer environment, you will need to run Prompt Master as the user who deployed it. It is possible to share with another user but this process is more involved and will require the use of Security Roles, this guide assumes you have the knowledge to do this and does not cover it.

## Step 6: Configure SharePoint list permissions

Before using Prompt Master, it is neccessary to configure the permisions on the SharePoint site. 

Add any user accounts that you will login as to use Prompt Master to the site, giving them 'Edit' rights.

Bear in mind that as the solution uses a custom connector and this is invoked via the app (through Power Automate), these accounts would need to have a premium license. As mentioned above, you will likely use only a single account to run the app.

**Note - These accounts will have access to all lists in the site to add/edit/delete items. As Prompt Master is designed to be used for excitment events/promptathons and not company wide use, this typically wouldn't be an issue. Ensure that you run the app as a user account whom you are happy to have this level of access.**

## Step 7: Reconnect the flows

At the time of writing, due to a Power Apps bug, the app must be edited and the flows removed and re-added. Follow the steps below to do this:

1. Locate the app in **Power Apps** and edit it.
2. Click the elipsis on the left menu.
3. Locate the 'Create Player Name' and 'Rate Prompt' flows and click the elipsis.
4. Click 'Remove from app' for each one.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-app-removeflows.png?raw=true" alt="Prompt Master App Remove Flow Screenshot"><br/>

5. Click 'Add flow' and click the 'Create Player Name' and 'Rate Prompt' flows to add them back into the app.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-app-addflows.png?raw=true" alt="Prompt Master App Add Flow Screenshot"><br/>

6. Save and publish the app and leave edit mode.

## Step 8: Run the app

Locate the app in the Power Apps portal and run it.

When you launch the app for the first time you **may** be prompted for the API key for the Azure OpenAI instance that you copied earlier (**Note - for each user account that runs the app they will prompted for this API key - this will only happen once**). 

If the user account you are using to run Prompt Master as does not have a premium license, you can use a 30 day trial, or as mentioned in the deployment guide, you can use the developer plan and the user account who owns the plan.

Ensure that the app opens at the start screen and is ready for play.

## Step 9 (Optional): Import sample challenges

Sample challenges can be found in the [Challenges.csv](/Challenges.csv) file.

Simply create the corresponding list items in the 'Challenges' list as per the csv, the Image column references the appropriate image in the [ChallengeImages](/Documentation/Images/ChallengeImages/) folder.

### Deployment is now complete - enjoy using Prompt Master!

# Prompt Master Pro

## Prerequisites

- Service account or user account to deploy Prompt Master Pro and to run the flows. Notifications to Teams will be sent from this account so consider the name.
- 1 x Premium license for Power Apps (for the above account) - the 'Rate Prompt' flow uses premium connectors so will run under this account.
- Power Apps environment with a Dataverse database deployed (only required due to the solution using Environment Variables). You may use the default environment. If you do not have capacity to create a Dataverse database, you may need to use the default environment.
- SharePoint site which will contain the lists - we recommend creating a new one for Prompt Master.
- Full Control access to the above site. 
- The [latest release](https://github.com/pnp/prompt-master/releases/latest) of Prompt Master Pro.
- Administrator account with access to the Microsoft Teams Admin Center (for deployment of the Prompt Master Pro app to Teams).
- Azure subscription.
- Access to create an Azure OpenAI instance and model deployment (you may use an existing one if you wish).

## Step 1: Create SharePoint Lists

  1. Login with the chosen service account/user account.
  2. Navigate to the SharePoint site.
  3. Create a list named 'Challenges'.
  4. Create the following columns:

  | Column Name    | Type | Values |
| -------- | ------- | ------- | 
| Name  | Multiple lines of text    |
| Instructions | Multiple lines of text     |
| Image | Image     |  |
| Tips    | Multiple lines of text  |
| App    | Choice  | Copilot, Word, Excel, PowerPoint, Outlook, Teams, Loop, Designer, Forms   | Default value = Copilot
| Start Date | Date and time (Date only) |
| End Date | Date and time (Date only) | 
| Notify | Yes/No (Default 'No') |

5. Create a list named 'Players'.
6. Create the following columns:

| Column Name    | Type | Values |
| -------- | ------- | ------- | 
| User  | Person or Group    |
| Score | Number     |
| Tutorial Complete | Yes/No (Default 'No') |

7. Create a list named 'PlayerChallenges' (rename list after creation to read 'Player Challenges').
8. Create the following columns:

| Column Name    | Type | Values |
| -------- | ------- | ------- | 
| Player ID  | Number |
| Challenge ID  | Number    |
| Prompt  | Multiple lines of text    |
| Rating | Number     |
| Suggestions| Multiple lines of text |
| Status | Choice | In Progress, Completed (Default - In Progress) |
| Date Completed | Date and time (include time) |

9. Create a list named 'ChallengeSubmissions' (rename list after creation to read 'Challenge Submissions').
10. Create the following columns:

| Column Name    | Type | Values |
| -------- | ------- | ------- | 
| Player ID  | Number |
| Challenge ID  | Number    |
| Player Challenge ID  | Number    |

11. Create a list named 'Configuration'.
12. Create the following columns:

| Column Name    | Type |
| -------- | ------- | 
| Value  | Single line of text 

13. Create a list item in the above list with the following details:

Title: AppId

Value: ID of the Prompt Master Pro Power App (we will get this value later).

## Step 2: Create Azure OpenAI Instance/Deployment

1. Navigate to the Azure Portal.
2. Create a new Azure OpenAI instance in your desired resource group.
3. Once created, navigate to OpenAI Studio.
4. Create a new deployment through **Deployments > Deploy model > Deploy base model**, choose **gpt-4o** as the model. **Keep the name of the deployment as 'gpt-4o'**. You may use an existing OAI instance/deployment but bear in mind the rating results may differ depending on the model you use - Prompt Master Pro has been tested on gpt-4o.
5. Copy the Key and Endpoint URI as you will need these later.

## Step 3: Deploy Prompt Master Pro OAI Custom Connector Solution

**Note - This step uses the same OAI Custom Connector as the non pro version of Prompt Master.**

1. Navigate to **Power Apps** and ensure you are in the correct environment you wish to deploy Prompt Master Pro to.
2. Click on the **Solutions** tab.
3. Click on **Import**.
4. Upload the **PromptMasterOAIConnector_1_0_0_0.zip** solution file and click **Next** (version number may differ).
5. On the **Environment Variables** pane, enter the endpoint for your Open AI instance (**note you don't need the protocol or anything after /openai** for example **'promptmaster-contoso-oai.openai.azure.com'**).
7. Click **Import**.
8. A message should be displayed to say the solution has been imported successfully.

## Step 4: Deploy Prompt Master Pro Solution

1. Navigate to **Power Apps** and ensure you are in the correct environment you wish to deploy Prompt Master Pro to.
2. Click on the **Solutions** tab.
3. Click on **Import**.
4. Upload the **PromptMasterPro_1_0_0_0.zip** solution file and click **Next** (version number may differ).
5. Click **Sign in** next to the SharePoint and Office 365 Groups Connection Of and wait for the green tick.
6. Click **Create** next to the custom connector connection (Prompt Master Pro Azure OAI Connection). Enter any name you wish for the 'Connection name' and enter the API key for your OpenAI deployment that you created earlier and click **Create**.
7. Click **Next**.
8. On the **Environment Variables** pane, select the SharePoint site you created (you may need to paste the URL into the dropdown if it cannot be found), the relevant lists and enter the URL to your Prompt Master Pro site. (Ignore any warnings about access).
9. Click **Import**.
10. A message should be displayed to say the solution has been imported successfully.

## Step 5: Share the app/flows

1. Locate the app under **Apps**.
2. Share the app with the user accounts that will use Prompt Master Pro (you may want to add any administrators as co-owners so they can modify the app if you wish to).

You may also wish to share the flows (**'Rate Prompt'**, **'New Challenge Notification'**) with admins who may need to view the run history or edit them. You can find them under **Flows** in the **Power Apps** portal or in the **Power Automate** portal.

## Step 6: Configure SharePoint list permissions

Before using Prompt Master Pro, it is neccessary to configure the permisions on the SharePoint site/lists. This step assumes a proficient level of knowledge in configuring SharePoint permissions.

If this is a production deployment, we recommend adding all users who will use Prompt Master Pro to the 'Visitors' group of the SharePoint site and breaking permission inheritance on each list. For administrators of Prompt Master Pro, you may wish to add these as members/owners of the entire SharePoint site.

Users will need **edit** rights to the following lists: Players, Player Challenges, Challenge Submissions. 

Users will need **read** rights to the following lists: Challenges, Configuration.

## Step 7: Add Prompt Master Pro app to Microsoft Teams

## Step 8 (Optional): Import sample challenges

Sample challenges can be found in the [Challenges.csv](/Challenges.csv) file.

Simply create the corresponding list items in the 'Challenges' list as per the csv, the Image column references the appropriate image in the [ChallengeImages](/Documentation/Images/ChallengeImages/) folder.






