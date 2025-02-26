# Overview - Prompt Master

Prompt Master consists of the following components:

- Azure OpenAI Instance
- Power App
- 2 x Power Automate flows.
- 1 x Custom Connector
- 4 x SharePoint lists for data storage

For more details on what each of these does please check out the [Architecture](Architecture.md) documentation.

## The App

The Prompt Master app allows you to provide a game for employees to play around Microsoft 365 Copilot prompting, specifially designed to be used at events/promptathons etc.

The app is a canvas app which can be opened directly in the browser, designed to be used full screen and displayed on a laptop/desktop for people to interact with.

It has been designed for tablets/desktops so it can easily be used on a touchscreen (it is not mobile responsive).

## Creating Challenges

Before using Prompt Master, you will need to create the challenges for people to complete.

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

To delete challenges, simply delete the challenge list item from the Challenges list. 

**Note - do not delete a challenge list item whilst a 'game' is in progress. If you delete challenges, please reset the data (see 'Reset data' below).

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-challenges-list-screenshot.png?raw=true" alt="Prompt Master Challenges List Screenshot"><br/>

## Play set up

To set up Prompt Master ready for play, simply launch the Power App in a browser through the PowerApps portal or the URL to the app itself, ensuring you are signed in with an account that has access to the app and it's resources.

When you launch the app for the first time you will be prompted for the API key for the Azure OpenAI instance you created during the deployment, this will open happen once for the user account.

The app will open at the start screen, ready for people to begin playing. For best results open the browser in full screen.

If the user account you are using to run Prompt Master as does not have a premium license, you can use a 30 day trial, or as mentioned in the deployment guide, you can use the developer plan and the user account who owns the plan. 

**Prompt master is designed to be 'played' under a single logged in user account. Players do not need to 'login'.**

## How to play

The below steps cover how to play Prompt Master although it has been designed to be intuitive so reading these steps may not be required.

1. Player enters a unique 'Player name' e.g. 'ThePromptWizard', the player can generate a name using the 'Generate name' button. 
2. Player enters their Email and Name (this data is captured in the 'Players' SharePoint list for you to export post event if you wish).
3. Click Start.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-start-screenshot.png?raw=true" alt="Prompt Master Start Screenshot"><br/>

4. From the main screen, the player can view the leaderboard by clicking on the 'Leaderboard' tab or select a challenge to complete by clicking on the desired challenge card. Each challenge card displays the 'app' that the challenge relates to in the upper right corner of the card.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-challenges-screenshot.png?raw=true" alt="Prompt Master Challenges Screenshot"><br/>

5. After clicking on a challenge, the player can read the instructions and tips for completing the challenge. From here the player can also launch Microsoft 365 Copilot directly using the button (to allow them to write/test their prompt).
6. Once the player is satisfied with their prompt, they should paste it into the textbox and click 'Submit'.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-challenge-screenshot.png?raw=true" alt="Prompt Master Challenge Screenshot"><br/>

7. The player receives a rating for their prompt (0-100) along with suggestions on how the prompt could be improved.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-rating-screenshot.png?raw=true" alt="Prompt Master Rating Screenshot"><br/>

8. The player clicks the button to view the leaderboard.
9. When viewing the leaderboard, it can be filtered by challenge and there is the ability to view the players current position in the leaderboard by clicking the 'Show me' button.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-leaderboard-screenshot.png?raw=true" alt="Prompt Master Leaderboard Screenshot"><br/>

10. Player navigates back to the main screen with the view of all challenges (completed challenge cards are greyed out and marked as completed). 
11. Player (or whomever is running the event) clicks the 'Sign out' button ready for the next player.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-newplayer-screenshot.png?raw=true" alt="Prompt Master New Player Button Screenshot"><br/>

Note - The leaderboard can be viewed at any time from the 'Start' screen without needing to register as a player. 

## Leaderboard

The top 3 players in the leaderboard are displayed in Gold, Silver and Bronze.

💡 **You could look to hand out 'swag' or rewards to the top players.**

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-leaderboard-screenshot.png?raw=true" alt="Prompt Master Leaderboard Screenshot"><br/>

## Administration

### Resetting data

Prompt Master data can be cleared ready for another event through the 'Settings' screen in the app. 

Clicking the 'Reset Data' button will remove all items from the 'Players' and 'Player Challenges' lists. Challenges will remain.

The 'Add/Remove Challenges' button will open the 'Challenges' list in the browser allowing you to easily manage challenges.

The 'Update Configuration Settings' button will open the 'Configuration' list in the browser allowing you to update the configuration settings for Prompt Master.

You may also clear the SharePoint lists manually if you wish - be sure to clear both the 'Players' and 'Player Challenges' lists.

### Azure OpenAI - Rating Prompts

Azure OpenAI is used to rate prompts when a user submits one as part of completing a challenge.

Specifically the 'Chat Completions' endpoint/functionality is used for rating a prompt.

This endpoint is called by the 'Rate Prompt' flow and a 'system prompt' is sent along with the prompt itself. The system prompt details how OpenAI should rate the prompt and how the scoring system works.

This system prompt can be amended if you wish to control the scoring by editing the **'Rate Prompt'** flow and updating the prompt text for the system prompt/message.

Expand the 'Rate Prompt' action in the 'Rate Prompt' and update the content in the 'messages content - 1' textbox.

<img src="https://github.com/pnp/prompt-master/blob/main/Documentation/Images/promptmaster-rating-systemprompt-screenshot.png?raw=true" alt="Prompt Master Rate Prompt Action Screenshot"><br/>

### Exporting data

If desired, you can export the 'game' data post use. This could be useful in terms of sharing some of the prompts that were created during your event day.

To export data, simply navigate to the 'Players' SharePoint list and use the 'Export' option in the menu to export to your tool of choice e.g. Excel.






