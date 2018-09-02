## Assignment 2

## Installing Blue Ocean
- From an administrator account go to `Manage jenkins > Manage plugins`
- Install the blueocean aggregator plugin from the `Available` tab. This is available after jenkins 2.7 
- All the other plugins needed will be installed automatically as dependencies, and hence do not need to be installed seperatly
- Blue Ocean will be installed and can be used when jenkins is restarted
- To restart jenkins type `sudo systemctl restart jenkins` in the terminal
- In the menu select `Open Blue Ocean`
- Voila!, jenkins is now visually appealing

We will now create a new freestyle project and use it for the remaining actions

## Linking a private GitHub repository:
- Open your project and go to `Configuration`
- Under the `General` section select `Github Project` and provide the url of the repo
- Under `Source Code Management` select `Git`
- Provide the repository URL in the `Repository URL` section
- Hit the `Add` button next to `Credentials` and enter your credentials for your github account (needed since jenkins will try to access a private repository)
- Once they are added select them from the dropdown menu for `Credentials`
- Select the branches to build or leave them as `*/master` (default)
- This will link jenkins to your private repository

## Remote Build Trigger:

Note: According to the GitHub documentation this should have worked but it did not trigger a build like expected.

A webhook is a tool available with GitHub which allows GitHub to send a payload to a particular location on the internet based on certain triggers. This payload delivery can be used by Jenkins to trigger a build

### Adding A WebHook:
- Go to the repository and go to `Settings > Webhooks` and click `Add webhook`
- Fill the payload URL section with `http://localhost:8080/github-webhook/` (this will later be replaced with a corresponding public URL)
- Select which events you would like to trigger the payload delivery
- Once you're done click on `Add webhook`

### Getting a public URL for Jenkins:
- The tool `ngrok` helps us achieve this. It can be downloaded [here](https://ngrok.com/download)
- Once installed, enter `./ngrok http 8080` in your terminal. This will give you a public URL which has a secure link to the page on port 8080 of your localhost 
- You should see something like `Forwarding   https://ad33c62e.ngrok.io -> localhost:8080`. The `*.ngrok.io` URL is the corresponding pulic URL.
- Use this as the `payload URL` on the github page of your repository described above.

### Configuring build triggers:
- Open your project and go to `Configuration > Build Triggers`
- Select `GitHub hook trigger for GITScm polling` and save changes

Ideally this should be enough to use GITScm polling and trigger builds on Jenkins based on actions on github. 

## Post build action:
Here, we adding the `Archive` post build action. This will archive selected files.

- Select the project and go to `Configure > Post-build Actions`
- Click the `Add post-build action` button and select `Archive the artifacts`
- Select the files that you want to archive (eg: `*` will archive all the files)
- Click the `Advanced` button
- Select
	- `Use default excludes`
	- `Archive artifacts only if build is successful`
- This will archive all files in the linked github repo when the build is successful
