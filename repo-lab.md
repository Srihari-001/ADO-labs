```markdown
# Configure Branch Policies in Azure Repos

## Introduction

When working in Azure Repos, your company's policy is to have two people review the code before merging into the main branch. You must apply this policy using branch policies in Azure Repos.

## Solution

### Using your own credentials, log in to the Azure DevOps platform.

> **Note**: Please do not use the Azure portal credentials provided in this lab. To complete this lab, 2 of your own email addresses are necessary. You can create free accounts at [login.microsoft.com](https://login.microsoft.com). Afterward, create an Azure DevOps account at [dev.azure.com](https://dev.azure.com) with the new email.

### Create a New Azure DevOps Organization and Project

1. Create a second account at [Azure DevOps](https://dev.azure.com). To create an account:
   - On the Sign in page, click **Create one!**.
   - Select **Get a new email address** and type in a new email address.
   - Click **Next**.
   - Type in a password.
   - Click **Next > Next**.
   - Solve the puzzle, then click **Done**.
   - Select your country/region from the dropdown list.
   - Click **Continue**.

2. In Azure DevOps, click on **Create new organization**.
   - Click **Continue > Continue**.
   - Enter a project name and make sure Visibility is set to **Private**.
   - Click on **+ Create project**.

### Pull Code and Remove Remote Origin

1. Log in to the provisioned Linux VM using the credentials provided:
   ```sh
   ssh cloud_user@<PUBLIC_IP_ADDRESS>
   ```
2. Pull code from the URL in the lab instructions:
   ```sh
   git clone https://github.com/linuxacademy/content-az400-lab-resources.git
   ```
3. List the contents of the directory:
   ```sh
   ls
   ```
4. Change into that repository:
   ```sh
   cd content-az400-lab-resources/
   ```
5. List the contents:
   ```sh
   ls
   ```
6. Change into `aspnetcorewebapp`:
   ```sh
   cd aspnetcorewebapp/
   ```
7. List the contents:
   ```sh
   ls
   ```
8. View the git remote origin:
   ```sh
   git remote -v
   ```
9. Remove origin:
   ```sh
   git remote remove origin
   ```

### Push Code to Azure Repos

1. In Azure DevOps, click on **Repos** in the left-hand navigation menu.
2. Under **Push an existing repository from command line**, copy the commands.
3. Return to the Linux VM and paste in the commands.
4. In Azure DevOps, in the upper right-hand corner, click the **User settings icon > Personal access tokens**.
5. Click on **+ New Token** and set the following values:
   - **Name**: linuxvm
   - **Code**: Select **Full** and **Status**
   - Click **Create**.
6. Copy the `linuxvm` token and save it in a safe location.
7. Click **Close**.
8. Return to the Linux VM and paste in the access token.
9. In Azure DevOps, click on **Azure DevOps** in the upper left corner.
10. Select your project.
11. Click on **Repos** in the left-hand navigation menu. Verify that the code was pushed to Azure Repos.

### Add a New Member and Branch Policy to the Project

1. Click on **Branches** in the left-hand navigation menu.
2. On the right side of the main branch, click the vertical ellipsis. Select **Branch policies** and set the following values:
   - **Require a minimum number of reviewers**: On
   - **Minimum number of reviewers**: 2
   - Select **Allow requestors to approve their own changes**
3. In the upper left-hand corner, click the **Azure DevOps** icon.
4. In the left-hand navigation menu, under **Organization Settings**, select **Users**.
5. Click on **Add Users** and set the following values:
   - **Users**: Enter the second email address created at the beginning of the lab
   - **Access level**: Basic
   - **Add to projects**: Select your project
   - Click **Add**.
6. In the upper left-hand corner, click the **Azure DevOps** icon.
7. Select your project.
8. In the bottom right corner, click on **Project settings**.
9. Look under **Permissions > Contributors** to verify that there are two members.
10. Return to Azure Repos and click on **Branches**.
11. On the right side of the main branch, click the vertical ellipsis.
12. Select **Branch policies** and click the plus sign next to **Automatically included reviewers**. Set these values:
    - **Reviewers**: Select both email addresses in the dropdown list
    - Select **Allow requestors to approve their own changes**
    - Click **Save**.

### Create Pull Request and Merge

1. In the Linux VM, create a branch:
   ```sh
   git checkout -b users/<YOUR_NAME>/cool-new-feature
   ```
2. Verify that we switched to the new branch:
   ```sh
   git branch
   ```
3. Change into the `Pages` directory:
   ```sh
   cd Pages/
   ```
4. Open the `Index.cshtml` file:
   ```sh
   vi Index.cshtml
   ```
5. Make a minor change in the `<h1>` heading. Replace "Welcome" with "Welcome to Azure DevOps!":
   ```html
   <h1 class="display-4">Welcome to Azure DevOps!</h1>
   ```
   Press `Esc` and type `:wq` to save and quit.
6. Commit the changes:
   ```sh
   git status
   git add .
   git commit -m "changed Index.cshtml"
   ```
7. Push the changes:
   ```sh
   git push origin users/<YOUR_NAME>/cool-new-feature
   ```
8. Paste in the personal access token.
9. Return to Azure Repos, and select **Repositories** in the left-hand navigation menu.
10. Select the project, then click on **Permissions**.
11. Under **Contributors**, change the following value:
    - **Bypass policies when completing pull requests**: Deny
12. In the left-hand navigation menu, click on **Repos > Branches**.
13. Select the `cool-new-feature` branch.
14. Click on **Create pull request**.
15. Click on **Create**.
16. Click on **Approve**.
17. In a new browser window, log in as the second user using the new email address you created.
    - On the Sign in page, type in the new email address.
    - Click **Next**.
    - Select your country/region from the dropdown list.
    - Click **Continue**.
    - Select your project.
18. In the left-hand navigation menu, click on **Repos > Pull requests**.
19. Select the pull request.
20. Click on **Approve > Complete**.
21. In the **Complete pull request** window, set the **Merge type** to **Merge (no fast forward)**.
22. Click **Complete merge**.
23. In the left-hand navigation menu, click on **Files**.
24. Select the main branch, then open `aspnetcorewebapp`.
25. Open `Pages > Index.cshtml` to view the changed code.

## Conclusion

Congratulations — you've completed this hands-on lab!