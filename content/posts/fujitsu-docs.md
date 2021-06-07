---
title: "Fujitsu Knowledge Base Setup Guide"
date: 2021-06-07T15:50:20+08:00
author: Michael Tam
email: michael.tam@fujitsu.com
---

## Prerequisites

- An Azure account with an active subscription. If you don't have one, you can [create an account for free](https://azure.microsoft.com/free/).
- A GitHub account. If you don't have one, you can [create an account for free](https://github.com/join).
- Chocolatey Package Manager
- Git for Windows
- Notepad++ as text editor
- Typora as Markdown editor
- Hugo as static Site Generator

## Install Chocolatey

1. Open PowerShell as Administrator

1. Run the following command in PowerShell

   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```

1. Wait a few seconds for the command to complete.

1. If you don't see any errors, you are ready to use Chocolatey! Type `choco`




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

## Create a Hugo App

Create a Hugo app using the Hugo Command Line Interface (CLI):

1. Open PowerShell as Administrator

1. Run the following command in PowerShell

   ```powershell
   mkdir C:\hugo
   cd C:\hugo
   ```

1. Run the Hugo CLI to create a new app.

   ```bash
   hugo new site static-app
   ```

1. Navigate to the newly created app.

   ```bash
   cd static-app
   ```

1. Initialize a Git repo.

   ```bash
    git init
   ```

1. Next, add a theme to the site by installing a theme as a git submodule and then specifying it in the Hugo config file.

   ```bash
   git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
   echo 'theme = "ananke"' >> config.toml
   ```

1. Commit the changes.

   ```bash
   git add -A
   git commit -m "initial commit"
   ```

## Push your application to GitHub

You need a repository on GitHub to connect to Azure Static Web Apps. The following steps show you how to create a repository for your site.

1. Create a blank GitHub repo (don't create a README) from [https://github.com/new](https://github.com/new) named **hugo-static-app**.

1. Add the GitHub repository as a remote to your local repo. Make sure to add your GitHub username in place of the `<YOUR_USER_NAME>` placeholder in the following command.

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/hugo-static-app
   ```

1. Push your local repo up to GitHub.

   ```bash
   git push --set-upstream origin main
   ```

## Deploy your web app

The following steps show you how to create a new static site app and deploy it to a production environment.

### Create the application

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

### Review and create

1. Select the **Review + Create** button to verify the details are all correct.

1. Select **Create** to start the creation of the App Service Static Web App and provision a GitHub Action for deployment.

1. Once the deployment completes click, **Go to resource**.

1. On the resource screen, click the _URL_ link to open your deployed application. You may need to wait a minute or two for the GitHub Action to complete.