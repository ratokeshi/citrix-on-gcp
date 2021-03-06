## Overview
This repository contains scripts and templates to simplify deployment of the resources described in Citrix's [Deploying Citrix Cloud XenApp and XenDesktop Service on the Google Cloud Platform](https://citrix.sharefile.com/d-sd6bf3fabc954f608) published December 2017.

## Before you begin
1. You'll need a PowerShell environment with the [Google Cloud SDK](https://cloud.google.com/sdk/) installed.
1. Check your local configuration in gcloud
   - get your current gcloud version `gcloud version`
   - update to the current version of the Google Cloud SDK
      - (if you are running as administrator) `gcloud components update`
      - if you need to elevate try `start-process  powershell -verb runas`
   - list all local credentialed accounts `gcloud auth list`
   - get your current configuration `gcloud config list`
   - logout of account (i.e. test@gmail.com) `gcloud auth revoke test@gmail.com`
   - list available regions `gcloud compute regions list`
   - set default region (i.e. us-central1) using  `gcloud config set compute/region us-central1`
   - set default zone (i.e. us-central1-a) using `gcloud config set compute/zone us-central1-a`


## Deploying Citrix
Clone the repository and run deploy.ps1.

``` shell
.\deploy.ps1
```

Optionally, you can set the "UseMinimalResources" parameter to True, if you
prefer a "light weight" deployment.

``` shell
.\deploy.ps1 -UseMinimalResources $True
```

This flag will reduce overall resource
consumption by removing some of the formality of the original reference
architecture, namely:
- Instances will be deployed with public IPs to avoid necessity of NAT instances
- Single instances will be deployed instead of highly-available pairs for the
  following instances categories:
  - Domain Controllers
  - Cloud Connectors

With "UseMinimalResources" enabled, the deployment will consume only 8 vCPUs
during initial deployment, eventually shutting down the management instance to
maintain steady state consumption of only 6 vCPUs.  The remaining instances can be stopped and
started at will to minimize resource consumption when the environment is not in
use.

This smaller deployment footprint can be useful especially when utilizing the [Google Cloud Platform Free Tier
](https://cloud.google.com/free/) to experiment with Citrix on GCP.

If not otherwise provided, you will be prompted for three parameter values to allow the script to interact with Citrix Cloud APIs:
- CTXSecureClientID: This is the client ID of a client configured at Citrix Cloud ([cloud.com](https://cloud.com/)) > Identity and Access Management > API Access.
- CTXSecureClientSecret: This is the secret you were provided when the client ID was created.
- CTXCustomerID: This is the Customer ID identified on Citrix Cloud ([cloud.com](https://cloud.com/)) > Identity and Access Management > API Access.

The script will use gcloud defaults to initialize parameters such as region and project.

The deployment takes about 30 minutes to complete.

## Retrieving passwords

Use get-domain-admin-password.ps1 to retrieve the password associated with the domain administrator (ctx-[SUFFIX]\Administrator where [SUFFIX] is the randomly-assigned suffix associated with you deployment).

``` shell
.\get-domain-admin-password.ps1
```

You can use the domain administrator credentials to Remote Desktop to the mgmt instance.

The ctx-[SUFFIX] domain will be populated with ten sample users.  The usernames and passwords can be retrived with get-domain-users.ps1.

``` shell
.\get-domain-users.ps1
```

These ten domain users are configured with access to Notepad as a sample application through your xendesktop.net Citrix Storefront.

## Resizing
Add VDA instances by running resize.ps1 and specifying a larger number for the "Workers" parameter.

``` shell
.\resize.ps1 -Workers 3
```

## Cleaning up
Remove billable GCP resources with cleanup.ps1.

``` shell
.\cleanup.ps1
```

## Contributing
See [CONTRIBUTING](CONTRIBUTING.md).

## License
Copyright 2018, Google, Inc.

Licensed under the Apache License, Version 2.0

If you want to perform theres tasks with the help of a Cloud Shell Tutorial then follow these steps:
To begin this guide open your Google Cloud Consoles as you normally would.
1.  If you have issues with the console try using incognito mode and go to https://console.cloud.google.com/
2.  If you are in the console session of the GCP Console, click the Cloud Shell icn on the top right that looks like this: ![alt text](https://walkthroughs.googleusercontent.com/tutorial/resources/cloud-shell-icon-v1.svg "Cloud Shell Icon on the top right of the GCP Console")
3.  Enter the following commands to clone this repository and launch the turotial.
```bash
git clone https://github.com/ratokeshi/citrix-on-gcp.git && cd citrix-on-gcp && cloudshell launch-tutorial tutorial.md
```
To begin the tutorial in your currently authenticated Google account, click on the button given below:
[![Open in Cloud Shell](http://gstatic.com/cloudssh/images/open-btn.png)](https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/ratokeshi/citrix-on-gcp&tutorial=tutorial.md)

See [LICENSE](LICENSE).
