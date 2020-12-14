# TeamsBotSSO

Bot Framework v4 Teams bot single sign on (SSO) authentication sample.

This bot has been created using [Bot Framework](https://dev.botframework.com), it shows how to add the [Single Sign On (SSO)](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-sso?view=azure-bot-service-4.0) authentication for Microsoft Teams.

The focus of this sample is how to use the Bot Framework with SSO support for oauth in your bot. The sample uses the bot authentication capabilities in [Azure Bot Service](https://docs.botframework.com), providing SSO feature to make it easier to develop a bot that authenticates users to [Azure AD v2](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-identity-providers?view=azure-bot-service-4.0&tabs=adv1%2Cga2#azure-active-directory-identity-provider) identity provider.

> IMPORTANT: The manifest file in this app adds resource url of bot to `webApplicationInfo` and "token.botframework.com" to the list of `validDomains`. These must be included in any bot that uses the Bot Framework SSO with OAuth flow.

## Prerequisites

- Microsoft Teams is installed and you have an account (not a guest account)
- [.NET Core SDK](https://dotnet.microsoft.com/download) version 3.1

  ```bash
  # determine dotnet version
  dotnet --version
  ```
- [ngrok](https://ngrok.com/) or equivalent tunnelling solution

## To try this sample

> Note these instructions are for running the sample on your local machine, the tunnelling solution is required because
> the Teams service needs to call into the bot.

1) Clone the repository

    ```bash
    git clone https://github.com/Microsoft/botbuilder-samples.git
    ```

2) If you are using Visual Studio
- Launch Visual Studio
- File -> Open -> Project/Solution
- Navigate to `TeamsBotSSO` folder
- Select `TeamsBotSSO.csproj` file
- Press `F5` to run the project

3) Run ngrok - point to port 3978

    ```bash
    ngrok http -host-header=rewrite 3978
    ```
### Configure SSO Authentication and add an OAuth Connection Setting to Bot  

4) Create [Bot Framework registration resource](https://docs.microsoft.com/en-us/microsoftteams/platform/bots/how-to/authentication/add-authentication?tabs=dotnet%2Cdotnet-sample#create-the-bot-channels-registration) in Azure
    - Use the current `https` URL you were given by running ngrok. Append with the path `/api/messages` used by this sample
    - Ensure that you've [enabled the Teams Channel](https://docs.microsoft.com/en-us/azure/bot-service/channel-connect-teams?view=azure-bot-service-4.0)
    - __*If you don't have an Azure account*__ you can use this [Bot Framework registration](https://docs.microsoft.com/en-us/microsoftteams/platform/bots/how-to/create-a-bot-for-teams#register-your-web-service-with-the-bot-framework)

5) To create identity provider to configure SSO Authentication, [register your app through the Azure Active Directory](https://docs.microsoft.com/en-us/microsoftteams/platform/tabs/how-to/authentication/auth-aad-sso#registering-your-app-through-the-azure-active-directory-portal-in-depth).

> NOTE: Steps in registering app through Azure AD to confirgure SSO for bots is similar to Tabs. Refer: [Develop a single sign-on Microsoft Teams bot](https://docs.microsoft.com/en-us/microsoftteams/platform/bots/how-to/authentication/auth-aad-sso-bots#develop-a-single-sign-on-microsoft-teams-bot)

6) Update the `appsettings.json` configuration for the bot to use the Microsoft App Id, App Password and Connection Name from the Bot Framework registration. (Note the App Password is referred to as the "client secret" in the azure portal and you can always create a new client secret anytime.)

7) __*This step is specific to Teams.*__
    - **Edit** the `manifest.json` contained in the  `teamsAppManifest` folder to replace your Microsoft App Id (that was created when you registered your bot earlier) *everywhere* you see the place holder string `<<YOUR-MICROSOFT-APP-ID>>` (depending on the scenario the Microsoft App Id may occur multiple times in the `manifest.json`)
    - **Zip** up the contents of the `teamsAppManifest` folder to create a `manifest.zip`
    - **Upload** the `manifest.zip` to Teams (in the Apps view click "Upload a custom app")

8) Run your bot, either from Visual Studio with `F5` or using `dotnet run` in the appropriate folder.

## Interacting with the bot in Teams

> Note this `manifest.json` specified that the bot will be installed in a "personal" scope only. Please refer to Teams documentation for more details.

You can interact with this bot by sending it a message. The bot sends a message with an OAuthCard that contains the tokenExchangeResource property. It tells Teams to obtain an authentication token for the bot application. Thus, you receives messages at all the active endpoints. If it is the first time the you are using this bot application, you will be prompted with consent experience for enabling additional permissions.
 

## Deploy the bot to Azure

To learn more about deploying a bot to Azure, see [Deploy your bot to Azure](https://aka.ms/azuredeployment) for a complete list of deployment instructions.

## Further reading

- [Bot Framework Documentation](https://docs.botframework.com)
- [Bot Basics](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [Azure Portal](https://portal.azure.com)
- [Develop a single sign-on Microsoft Teams bot](https://docs.microsoft.com/en-us/microsoftteams/platform/bots/how-to/authentication/auth-aad-sso-bots#develop-a-single-sign-on-microsoft-teams-bot)
- [Add Single sign on to your bot via Azure Bot Service](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-authentication-sso?view=azure-bot-service4.0&tabs=csharp)
- [Activity processing](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [Azure Bot Service Introduction](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Azure Bot Service Documentation](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [.NET Core CLI tools](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Language Understanding using LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [Channels and Bot Connector Service](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)
