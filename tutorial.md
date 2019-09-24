# Tutorial: citrix-on-gcp based on the project at: https://github.com/GoogleCloudPlatform/citrix-on-gcp #

You can list the active account name with this command:

```bash
gcloud auth list
```
Output:

Credentialed accounts:
 - <myaccount>@<mydomain>.com (active)
Example output:

Credentialed accounts:
 - google1623327_student@qwiklabs.net
You can list the project ID with this command:
```bash
gcloud config list project
```
Output:

[core]
project = <project_ID>

If you have valid settings for your project you can proced with launching the setup scripts in the next section.

## Clone the Repository and Deploy ##

You should have the full repository already in your home directory.  
Examine your local files using the following command:
```bash
ls
```
You should see a liskt of .ps1 files as well as this tutorial.md file.

You can install the Powershell tools in this shell by runnig this command:
```bash
sudo apt-get install -y powershell
```

After it completes you will be able to switch into the PowerShell environment using this command:
```bash
pwsh
```

Run deploy.ps1.
```bash
.\deploy.ps1
```

If you want to set the "UseMinimalResources" parameter to True, you can get a  "light weight" deployment.
```bash
.\deploy.ps1 -UseMinimalResources $True
```