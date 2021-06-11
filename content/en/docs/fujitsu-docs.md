---
title: "Fujitsu Knowledge Base Setup Guide"
date: 2021-06-07T15:50:20+08:00
author: Michael Tam
email: michael.tam@fujitsu.com
weight: -1
---



# Tutorial: Welcome 123

Author: Michael Tam
Email: michael.tam@fujitsu.com
Date: 2021-06-07T15:50:20+08:00

## Prerequisites

- An Azure account with an active subscription. If you don't have one, you can [create an account for free](https://azure.microsoft.com/free/).
- A GitHub account. If you don't have one, you can [create an account for free](https://github.com/join).
- Chocolatey Package Manager
- Git for Windows
- Notepad++ as text editor
- Typora as Markdown editor
- Hugo as static Site Generator
- A purchased domain name

## Install Chocolatey

1. Open PowerShell as Administrator

1. Run the following command in PowerShell

   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```

1. Wait a few seconds for the command to complete.

1. If you don't see any errors, you are ready to use Chocolatey.

   ```powershell
   choco version
   ```


## Install Tools

1. Open PowerShell as Administrator

1. Run the following command in PowerShell

   ```powershell
   choco install typora -confirm
   choco install notepadplusplus -confirm
   choco install hugo -confirm
   choco install git.install -confirm
   ```

1. Wait a few seconds for the command to complete.

1. Check Start Menu if the above program has been installed.

## Create a Hugo App Locally

Create a Hugo app using the Hugo Command Line Interface (CLI):

1. Open PowerShell as Administrator

1. Run the following command in PowerShell

   ```powershell
   mkdir C:\hugo
   cd C:\hugo
   ```

1. If you don't see any errors, you are ready to use Hugo.

   ```powershell
   hugo version
   ```

1. Run the Hugo CLI to create a new app.

   ```bash
   hugo new site static-app
   ```

1. Navigate to the newly created app.

   ```bash
   cd static-app
   ```

1. Create content for your site
   ```bash
   hugo new posts/blog-post-1.md
   hugo new posts/blog-post-2.md
   hugo new posts/blog-post-3.md
   ```
   
1. Change draft mode of post1 and post2 to `false`. Verify post3 have draft mode set to `true`. Save and close the file and Hugo will automatically detect the change of the newly added blog post. The content will not be rendered by Hugo when draft mode is set to `true`.
   You can use **Typora** to edit the markdown files.
   
   ```bash
   notepad content/posts/blog-post-1.md
   draft: false
   
   notepad content/posts/blog-post-2.md
   draft: false
   ```
   
1. Hugo Theme can be download at https://themes.gohugo.io/

1. Next, add a theme to the site by installing a theme as a git submodule and then specifying it in the Hugo config file.

   ```bash
   #Bash
   git init
   git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
   echo 'theme = "ananke"' >> config.toml
   
   #PowerShell
   git init
   git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
   Add-Content config.toml 'theme = "ananke"'
   ```

1. Launch the site on the local machine.

   ```bash
   hugo server
   ```

1. Open a web browser and point it to **http://localhost:1313** and you should see the **Hugo site with ananke theme** setup.

   ```bash
   http://localhost:1313
   ```

1. Press **CTRL+C** to stop Hugo server.

## Configure Git Username and Email

To ensure your commits in GitHub appears. It must have a username and email:

1. Verify Git has installed correctly, if you don't see any error, Git is ready to use.

   ```bash
   git --version
   ```
1. Set your username

   ```bash
   git config --global user.name "FIRST_NAME LAST_NAME"
   ```

1. Set your email address

   ```bash
   git config --global user.email "MY_NAME@example.com"
   ```

## Push your application to GitHub

You need a repository on GitHub to connect to Azure Static Web Apps. The following steps show you how to create a repository for your site.

1. Create a blank GitHub repo (don't create a README) from [https://github.com/new](https://github.com/new) named **hugo-static-app**.

1. Create a readme file for your project.

   ```powershell
   New-Item -name README.md -Value "Example Hugo Site"
   ```

1. You have already create an empty git repository on local machine using `git init` command. The following steps show you how to commit change locally.

   ```bash
   git add -A
   git commit -m "initial commit"
   ```

1. Add the GitHub repository as a remote to your local repo. Make sure to add your GitHub username in place of the `<YOUR_USER_NAME>` placeholder in the following command.

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/hugo-static-app
   ```

1. Create a new branch.

   ```bash
   git branch -M main
   ```

1. Push your local repo up to GitHub.

   ```bash
   git push origin main
   ```

## Deploy your web app to Azure

The following steps show you how to create a new static site app and deploy it to a production environment.

1. Navigate to the [Azure portal](https://portal.azure.com)
1. Select **Create a Resource**
1. Search for **Static Web Apps**
1. Select **Static Web Apps**
1. Select **Create**
1. On the _Basics_ tab, enter the following values.

    | Property | Value |
    | --- | --- |
    | _Subscription_ | Your Azure subscription name. |
    | _Resource group_ | **my-hugo-group**  |
    | _Name_ | **hugo-static-app** |
    | _Plan type_ | **Free** |
    | _Region for Azure Functions API and staging environments_ | Select a region closest to you. |
    | _Source_ | **GitHub** |

1. Select **Sign in with GitHub** and authenticate with GitHub.

1. Enter the following GitHub values.

    | Property | Value |
    | --- | --- |
    | _Organization_ | Select your desired GitHub organization. |
    | _Repository_ | Select **hugo-static-app**. |
    | _Branch_ | Select **main**. |

1. In the _Build Details_ section, select **Hugo** from the _Build Presets_ drop-down and keep the default values.

1. Select the **Review + Create** button to verify the details are all correct.
10. Select **Create** to start the creation of the App Service Static Web App and provision a GitHub Action for deployment.
11. Once the deployment completes click, **Go to resource**.
12. On the resource screen, click the _URL_ link to open your deployed application. You may need to wait a minute or two for the GitHub Action to complete.

## Configure Base URL

Configure the hostname for Hugo

1. Navigate to the [Azure portal](https://portal.azure.com)

1. Select **my-hugo-group** in Resources Group.

1. Select **hugo-static-app** in Static Web App.

4. Copy the URL.

   ```powershell
   https://<YOUR_SITE>.azurestaticapps.net
   ```

5. Edit **C:\hugo\static-app\config.toml** with Notepad. Change BaseURL to Azure URL.
   ```powershell
   baseURL = "https://<YOUR_SITE>.azurestaticapps.net"
   ```
6. Push your local repo up to GitHub.
   ```powershell
   git commit -m "Change BaseURL"
   git push
   ```

7. It will take a few minutes for Git pipeline to build the website and publish to Azure Static Web. Navigate the site with your browser.

   ```powershell
   https://<YOUR_SITE>.azurestaticapps.net
   ```

## Set up a custom domain with SSL certificate in Azure Static Web Apps

By default, Azure Static Web Apps provides an auto-generated domain name. This article shows you how to map a custom domain name to an Azure Static Web Apps application.

1. Open your static web app in the [Azure portal](https://portal.azure.com).

1. Select **Custom domains** in the menu.

1. Select the **Add** button.

1. In the _Domain name_ field, enter your subdomain. Make sure that you enter it without any protocols. For example, `docs.mydomain.com`.

1. Select the **Next** button to move to the _Validate + configure_ step.

1. Make sure **CNAME** is selected from the _Hostname record type_ dropdown list.

1. Copy the value in the _Value_ field to your clipboard by selecting the **copy** icon.

   In a separate browser tab or window, sign in to the website of your domain provider.

1. Find the page for managing DNS records. Every domain provider has its own DNS records interface, so consult the provider's documentation. Look for areas of the site labeled **Domain Name**, **DNS**, or **Name Server Management**.

1. Often, you can find the DNS records page by viewing your account information, and then looking for a link such as **My domains**. Go to that page and then look for a link that is named similar to **Zone file**, **DNS Records**, or **Advanced configuration**.

   Create a new **CNAME** record with the following values.

   | Setting             | Value                                     |
   | ------------------- | ----------------------------------------- |
   | Type                | CNAME                                     |
   | Host                | Your subdomain, such as `docs`            |
   | Value               | Paste the domain name from your clipboard |
   | TTL (if applicable) | Leave as default value                    |

1. Save the changes with your DNS provider.

1. It will take a few minutes for DNS to update the change. Navigate the site with your browser.

   ```powershell
   https://docs.mydomain.com
   ```
1. Configure BaseURL to your custom domain.
