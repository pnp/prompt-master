# Architecture

The below documentation details the architecture of the Prompt Master solution. Prompt Master is provided in the form of 2 x Power Apps solution files (**unmanaged**), this allows easy updates when new versions are released.

## Connectors

The following connectors are used in the Prompt Master solution:

- SharePoint
- Office 365 Groups
- Custom Connector (PromptMasterAzureOAI) - Used to connect to the Azure OpenAI instance.

The above connectors are also used in the Power Automate flows.

## Power App

The Prompt Master Power App is a canvas app using mostly **modern** controls where possible.

The app uses the above connectors (and flows listed below) to interact with the SharePoint site/lists, generate player names and rate prompts for challenges. 

The app can be customized if you wish, though it's worth noting that customizations will be lost if you attempt to update the solution in the future when new releases are available.

The Power App is not responsive and has been designed for tablet layout (to provide the best possible experience for playing full screen at an event or excitement day).

# Flows

Detailed information about the flows used in the solution can be found below:

## Create Player Name

Triggers when the user clicks the 'Generate name' button in the app.

This flow uses the PromptMasterAzureOAI custom connector to generate a fun player name for the player to use.

It uses a predefined system prompt along with a hardcoded user prompt.

If you wish to change the response in terms of the names it generates, simply edit the **'Create Player Name'** flow and update the system and user message in the 'Chat Completions' action.

## Rate Prompt

Triggers when the clicks the 'Submit' button in the app after entering their prompt.

This flow uses the same custom connector as above to rate/score their prompt, providing a numeric score between 0 and 100. 

It uses a predefined system prompt which includes the 'Instructions' and 'Tips' for the challenge and the users' entered prompt.

This system prompt can be amended if you wish to control the scoring by editing the **'Rate Prompt'** flow and updating the prompt text for the system prompt/message.

Expand the 'Rate Prompt' action in the 'Rate Prompt' and update the content in the 'messages content - 1' textbox.

## Data Sources

As detailed in the [Overview](Overview.md) documentation, there are 4 SharePoint lists used in prompt master. Please see the details of each list below and what each column is used for:

### Challenges list

  | Column Name    | Type | Used for |
| -------- | ------- | ------- | 
| Name  | Multiple lines of text    | Name of the challenge.
| Instructions | Multiple lines of text     | Instructions to complete the challenge.
| Image | image     | Image to display for the challenge.
| Tips    | Multiple lines of text  | Tips to help a user complete the challenge.
| App    | Choice  |  App that the challenge relates to e.g. Copilot, PowerPoint etc.

### Players list

| Column Name    | Type | Used for |
| -------- | ------- | ------- | 
| Name  | Single line of text    | Full name of the player.
| User  | Person or Group    | User account of the user that is running the app.
| Player Name  | Single line of text    | Player name for the player.
| Email | Single line of text     | Email of the player.
| Score | Number  | Players' total score.

### Player Challenges list

| Column Name    | Type | Used for |
| -------- | ------- | ------- | 
| Player ID  | Number | ID of the player from the players list.
| Challenge ID  | Number    | ID of the challenge from the challenges list.
| Prompt  | Multiple lines of text    | Prompt entered by the player for the challenge.
| Rating | Number     | Rating score given to the player for the challenge.

### Configuration list

| Column Name    | Type | Used for |
| -------- | ------- | ------- | 
| Value  | Single line of text | The configuration value.




