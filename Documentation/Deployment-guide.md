# Deployment guide

Please note - this deployment guide assumes a proficient level of knowledge in deploying Power Apps solutions and navigating/using the Azure Portal.

## Prerequisites

- 1 x Premium license for Power Apps **OR** use the [Developer plan](https://learn.microsoft.com/en-us/power-platform/developer/plan). The user account you run Prompt Master will require this license or the developer plan. If you use a developer plan, you will need to run Prompt Master as the user account who is assigned the developer plan.
- Power Apps environment with a Dataverse database deployed (only required due to the solution using Environment Variables). You may use the default environment. If you do not have capacity to create a Dataverse database, you may need to use the default environment. If you are using the developer plan, you can use the developer environment.
- SharePoint site which will contain the lists - we recommend creating a new one for Prompt Master.
- Full Control access to the above site. 
- The [latest release](https://github.com/pnp/prompt-master/releases/latest) of Prompt Master.
- Access to create an Azure OpenAI instance and model deployment (you may use an existing one if you wish). Azure OpenAI is used to rate a prompt when it is submitted to complete a challenge.

## Step 1: Create SharePoint Lists

  1. Navigate to the SharePoint site.
  2. Create a list named 'Challenges'.
  3. Create the following columns:

  | Column Name    | Type | Values |
| -------- | ------- | ------- | 
| Name  | Multiple lines of text    |
| Instructions | Multiple lines of text     |
| Image | image     |  |
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
4. Upload the **PromptMasterOAIConnector** solution zip file and click **Next**.
5. On the **Environment Variables** pane, enter the endpoint for your Open AI instance (**note you don't need the protocol or anything after /openai** for example **'promptmaster-contoso-oai.openai.azure.com'**).
7. Click **Import**.
8. A message should be displayed to say the solution has been imported successfully.

## Step 4: Deploy Prompt Master Solution

1. Navigate to **Power Apps** and ensure you are in the correct environment you wish to deploy Prompt Master to.
2. Click on the **Solutions** tab.
3. Click on **Import**.
4. Upload the **PromptMaster** solution zip file and click **Next**.
5. Click **Sign in** next to each of the connectors and wait for the green tick.
6. On the **Environment Variables** pane, select the SharePoint site you created, the relevant lists and enter the URL to your Prompt Master site.
7. Click **Import**.
8. A message should be displayed to say the solution has been imported successfully.

## Step 5: Share the app/flows

1. Locate the app under **Apps**.
2. Share the app with the user accounts that will be running Prompt Master (you may want to add any administrators as co-owners so they can modify the app if you wish to). **We recommend running the app on a day as a single user.**
3. Test the app by 'Playing' it.

You may also wish to share the flows (**'Rate Prompt'**, **'Create Player Name'**) with admins who may need to view the run history or edit them. You can find them under **Flows** in the **Power Apps** portal or in the **Power Automate** portal.

## Step 6: Configure SharePoint list permissions

Before using Prompt Master, it is neccessary to configure the permisions on the SharePoint site. 

Add any user accounts that you will login as to use Prompt Master to the site, giving them 'Edit' rights.

Bear in mind that as the solution uses a custom connector and this is invoked via the app (through Power Automate), these accounts would need to have a premium license. As mentioned above, you will likely use only a single account to run the app.

**Note - These accounts will have access to all lists in the site to add/edit/delete items. As Prompt Master is designed to be used for excitment events/promptathons and not company wide use, this typically wouldn't be an issue. Ensure that you run the app as a user account whom you are happy to have this level of access.**

## Step 7: Run the app

Locate the app in the Power Apps portal and run it.

When you launch the app for the first time you will be prompted for the API key for the Azure OpenAI instance that you copied earlier (**Note - for each user account that runs the app they will prompted for this API key - this will only happen once**). 

If the user account you are using to run Prompt Master as does not have a premium license, you can use a 30 day trial, or as mentioned in the deployment guide, you can use the developer plan and the user account who owns the plan. 

Ensure that the app opens at the start screen and is ready for play.

### Deployment is now complete - enjoy using Prompt Master!