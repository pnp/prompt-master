# Overview - Prompt Master Pro

Prompt Master Pro consists of the following components:

- Azure OpenAI Instance
- Power App
- 2 x Power Automate flows.
- 1 x Custom Connector
- 5 x SharePoint lists for data storage

For more details on what each of these does please check out the [Architecture](Architecture.md) documentation.

## The App

The Prompt Master Pro app allows you to provide a game for employees to play around Microsoft 365 Copilot prompting.

The app is a canvas app which can be deployed to Microsoft Teams and pinned in the rail or it can be accessed directly in a browser. 

The app uses the 'tablet' design format which is designed to work on laptops/desktop but is not mobile responsive.

## Creating Challenges

Before using Prompt Master Pro, you will need to create the challenges for users to complete.

**Note - sample challenges are provided in the [Challenges.csv](/Challenges.csv) file allowing you to get started quickly.**

Challenges are stored in a SharePoint list named 'Challenges' which you created during deployment.

To create a challenge, simply create a new list item in the Challenges list. See the table below for details of expected column values.

| Column Name    | Value | 
| -------- | ------- |
| Name  | Name of the challenge e.g. 'Productivity Wizard'.
| Instructions | Instructions for the user on how to complete the challenge.
| Image | Image for the challenge (tip - you can use Microsoft Designer to create imagery for your challenges).
| Tips    | Optional tips to help users complete the challenge.  | 
| App    | The app that the challenge applies to e.g. Copilot, Word, PowerPoint etc.
| Start Date | Start date that the challenge will begin on (challenge 'unlocks' in the app for completion on/after this date).
| End Date | End date on which the challenge will end (marked as 'Ended' in the app). **Note - if the challenge is already in progress or completed for users, it will retain this status.**
| Notify | Whether to notify users that a new challenge has been added.

To delete challenges, simply delete the challenge list item from the Challenges list. 

**Note - do not delete a challenge list item once you have rolled out Prompt Master Pro. If you wish to no longer display a challenge to a user, simply update the 'End Date' column with an appropriate date.**

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-challenges-list-screenshot.png?raw=true" alt="Prompt Master Pro Challenges List Screenshot"><br/>

## New Challenge Notifications

Users that have 'played' Prompt Master Pro will be notified of new challenges when they are created **if the value of the notify column has been set to 'Yes'**. This notification is sent in the form of an adaptive card to the user in Microsoft Teams.

You may wish to create new challenges with this column value set to false and modify the challenge when you want to send a notification.

The 'Send New Challenge Notification' flow is responsible for sending the notification. 

The adaptive card includes a button to launch the challenge directly in the Prompt Master Pro app. 

**Note - only users in the 'Players' list are sent the notifications, users are added to this list when they launch the Prompt Master Pro app.**

## Play set up

Ensure Prompt Master Pro has been deployed to Microsoft Teams and is shared with all users who will use it (detailed in the deployment guide).

## How users 'play' Prompt Master Pro

The below steps cover how users play Prompt Master Pro although it has been designed to be intuitive so reading these steps may not be required.

1. User launches the Prompt Master Pro app in Microsoft Teams or via Power Apps.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-start-screenshot.png?raw=true" alt="Prompt Master Pro Loading Screenshot"><br/>

2. User player 'profile' is automatically created for the user and they will appear in the leaderboard (item is automatically created in the 'Players' list).
3. User completes the tutorial ('Tutorial Complete' column set for the user in the 'Players' list).

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-tutorial-screenshot.png?raw=true" alt="Prompt Master Pro Tutorial Screenshot"><br/>

4. User clicks on a challenge card for a challenge that they wish to complete.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-challenges-screenshot.png?raw=true" alt="Prompt Master Pro Challenges Screenshot"><br/>

5. User can read the instructions and tips for completing the challenge. From here the player can also launch Microsoft 365 Copilot directly using the button (to allow them to write/test their prompt - if the challenge relates to M365 Copilot).
6. User submits their prompt by entering it into the textbox and clicking 'Submit'.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-challenge-prompt-screenshot.png?raw=true" alt="Prompt Master Pro Challenge Screenshot"><br/>

7. User receives a message to state their prompt has been submitted for rating and will be sent to them through Microsoft Teams.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-submit-dialog-screenshot.png?raw=true" alt="Prompt Master Pro Challenge Screenshot"><br/>

8. Challenge updated in the app to reflect it is 'In Progress'.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-challenge-inprogress-screenshot.png?raw=true" alt="Prompt Master Pro In progress Screenshot"><br/>

8. User receives an adaptive card in Teams notifying them that the challenge has been completed along with the rating their prompt received and their position in the leaderboard. The card contains buttons enabling the user to directly launch Prompt Master Pro and continue playing. 

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-notification-screenshot.png?raw=true" alt="Prompt Master Pro Challenge Completed Notification Screenshot"><br/>

9. User can access Prompt Master Pro and view the leaderboard through the adaptive card.
10. User accesses the 'My Stats' screen of the app and continues playing by completing more challenges.

## Leaderboard

The top 3 players in the leaderboard are displayed in Gold, Silver and Bronze.

If the current user is outside of the top 3, they are highlighted in blue.

💡 **You could look to hand out 'swag' or rewards to the top players.**

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-leaderboard-screenshot.png?raw=true" alt="Prompt Master Pro Leaderboard Screenshot"><br/>

## My Stats

The 'My Stats' screen in Prompt Master Pro allows the user to view their progress and see various metrics such as the number of challenges they have completed and their recent activity.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-stats-screenshot.png?raw=true" alt="Prompt Master Pro My Stats Challenge Completed Notification Screenshot"><br/>

## Administration

Prompt Master Pro data can is managed directly through the SharePoint site itself.

If you wish to reset the solution, simply remove all items from the following lists: 'Players', 'Player Challenges' and 'Challenge Submissions'. 

**Note - Removing data whilst users are using/playing will cause errors.**

### Azure OpenAI - Rating Prompts

Azure OpenAI is used to rate prompts when a user submits one as part of completing a challenge.

Specifically the 'Chat Completions' endpoint/functionality is used for rating a prompt.

This endpoint is called by the 'Rate Prompt - Prompt Master Pro' flow and a 'system prompt' is sent along with the prompt itself. The system prompt details how OpenAI should rate the prompt and how the scoring system works.

This system prompt can be amended if you wish to control the scoring by editing the **'Rate Prompt - Prompt Master Pro'** flow and updating the prompt text for the system prompt/message.

Expand the 'Rate Prompt' action in the 'Rate Prompt' and update the content in the 'messages content - 1' textbox.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmasterpro-rating-systemprompt-screenshot.png?raw=true" alt="Prompt Master Pro Rate Prompt Action Screenshot"><br/>






