# Deploy Your ASP.NET Core Application to Azure

So you've built an ASP.NET Core application, added Okta for authentication, and are ready for the Internet to see it. If you're on Windows using Visual Studio, you could right-click and deploy it to Azure easily, but there are several reasons [that is a bad idea](https://damianbrady.com.au/2018/02/01/friends-dont-let-friends-right-click-publish/). Not to mention the fact that ASP.NET Core is all about the cross-platform. For instance, I write ASP.NET Core on my Linux laptop using VS Code, so right-click to publish isn't even an option. 

Deploying to Azure can be done in several ways. In this tutorial, I will show you the easiest, and most direct way to get your newly created application out into the World.

## Get the ASP.NET Core Starter App

If you don't already have an ASP.NET Core app, you can get a simple starter from the Okta [GitHub repo](https://github.com/oktadeveloper/okta-aspnetcore-mvc-example).

Start by cloning the repo to your local machine:

```bash
git clone https://github.com/oktadeveloper/okta-aspnetcore-mvc-example.git
```

next, you'll want to disconnect the GitHub repo, so you can check it in for yourself and start making your own changes. To do that, change into the `okta-aspnetcore-mvc-example` directory and delete the `.git` folder.

```bash
cd okta-aspnetcore-mvc-example
rm -rf .git
```
Then initialize a Git repository in that same folder and commit the code.

```bash
git init
git add -A
git commit -m "Initial Commit"
```

## Add Your Okta Application
If you don't already have one, make sure to [sign up for Okta Developer account](https://developer.okta.com/signup/). Once you have an account, log in to your dashboard and get your organization URL from the top right-hand corner of the dashboard page.

[Dashboard with OrgURL Highlighted]

Then, you'll need to create an Okta application. So clik on the **Applications** menu item and click the **Add Application** button.

[Add Application Button]

From the new application wizard page, choose the **Web** button and click next. Name your application whatever you want, but make sure to update the **Base URIs**, and **Login redirect URIs** settings changing the port to the local port that your application will be running on. If you're in Visual Studio on Windows, it will likely be 60611. If you're in VS Code, it will most likely be port 5000 by default. You'll also need to select the Implicit check box under **Allowed Grant Types**. Then click **Done**.

Once this is done, you will be taken to the application's **General Settings** tab. If you scroll down to the bottom, you'll see a section with your Client ID and Client secret. Copy those into your `app.settings` file of the ASP.NET Core app.

You'll also need to create an API token, so that the sample app can make calls to the API to get the profile page. To get that, hover over the **API** menu item and choose the **Tokens** menu item. Then click on the **Create Token** button at the top and give your new token a name. Click on **Create Token** and copy the API token into your `appsettings.json` file. Once you've got this successfully copied into your file, cliek the **OK, got it** button. 

>Keep in mind, that you can't see this token again, so make sure you've got it copied over before you close the window. If you happen to lose it, you can just delete the old one and create a new one.

Your final `appsettings.json` file will look like the one below, with your values copied into the placeholders.

```json
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "Okta": {
    "Issuer": "https://{yourOktaDomain}.com/oauth2/default",
    "ClientId": "{yourClientID}",
    "ClientSecret": "{yourClientSecret}",
    "APIToken": "{yourAPIToken}",
    "OrgUrl": "https://{yourOktaDomain}.com"
  }
}
```

Finally, commit your changes and push them to your GitHub repository. Obviously, this is not the *ideal* way to store your application's secrets and keys. In future blog posts, I'll show you a more secure way to store them, but for now I'll concentrate on getting the code deployed to Azure.

```bash
git commit -am "Added client creds"
```

## Create Your GitHub Repository

Now you can create your own GitHub repoository and check the code in there. Log into your GitHub account and create a new repository. I will call mine "OktaAspNetCoreExample", but you can call yours whatever you want. Once it's created, follow the directions to add the remote "origin" and to push your local repo to the remote.

```bash
git remote add origin {yourGitHubRepoUrl}
git push -u origin master
```

## Deploy To Azure Web App



