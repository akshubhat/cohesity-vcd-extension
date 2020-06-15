Cohesity Extension for VMware vCloud Director
=================

## Overview

*Cohesity Extension for VMware vCloud Director* enables cloud providers to offer data protection as a service in multi-tenant environments.

This extension integrates natively with vCloud Director HTML UI and makes self-service data protection  available to tenants. It delivers self-service in a secure manner through role-based access control. Having integrated data protection services streamlines operations as well as provides a rich tenant experience.

## Table of contents :scroll:

 - [Prerequisites](#pre-req)
 - [Installation](#install)
 - [Features](#features)
 - [VCD Tutorial](#demo-video)
 - [How can you contribute](#contribute)
 - [Suggestions and Feedback](#suggest)
 

## <a name="pre-req"></a> Prerequisites :clipboard:

**Software Required:**

* vCloud Director: 9.5 or later
* Cohesity Data Platform: 6.3 or later
* Web browsers: Google Chrome, Firefox, Safari
* Python 3 or higher (to deploy plugin)
   * Python modules required: requests, configparser
* Cohesity cluster should be accessible through port 443
* Signed Certificate should be installed on the Cohesity Cluster

**vCD mandatory user permissions:**

| Permission  | View Access | Manage Access |
| ------------- | ------------- | ------------- |
| View Organizations  | Y  | Y |
| View Group / User | Y  | Y |
| View Tenant Portal Plugin Information | Y | Y |
| Edit Organization Properties| N | Y |

## <a name="install"></a> Installation :hammer_and_pick:

This section describes the detailed steps to install and configure the Cohesity Extension for VMware vCloud Director.

### 1. Enable CORS on the Cohesity Cluster
vCloud Director extension makes cross-origin API requests to the Cohesity Cluster.
To enable support for CORS on the Cohesity Cluster, please follow the steps below.

1) Open the Cohesity Cluster UI and click on the "Download CLI" link in the footer. Download the `iris_cli` binary and make sure it has executable permissions.
2) Connect to the Cohesity Cluster using `iris_cli`
```
iris_cli -server {cohesity_cluster} -username {username} -password {password}
```
3) Once you are connected and in the shell, replace `vcd_hostname` with the hostname of your vCloud Director server in the command below and run it in the shell to enable CORS
```
cluster update-gflag gflag-name="iris_cors_origins" gflag-value="https://{vcd_hostname}" service-name=iris reason="Enabling_CORS"
```
```
cluster update-gflag gflag-name="iris_alllowed_referrer_urls" gflag-value="https://{vcd_hostname}" service-name=iris reason="Enabling_Referer_Url"
```
4) Restart `iris` service by running the command below in the shell
```
cluster restart service-names=iris
```
5) Exit the shell
```
exit
```

### 2. Enable organizations on the Cohesity Cluster
To enable organizations on the Cohesity Cluster please follow the steps below:

1) Login to the Cohesity Cluster UI as a user with Admin role
2) Click on "Admin->Cluster Settings"
3) Click on the toggle buton to "Enable Organizations"
4) Create organizations in Cohesity corresponding to the tenants in vCloud Director by navigating to "Admin->Organizations" and clicking on "Add Organization"
5) Assign the Protection Policies to the organization and also assign the corresponding vCD organization under Sources


### 3. Install Cohesity Extension:

1) Download the latest `dataProtection.zip` file from the [releases](https://github.com/cohesity/cohesity-vcd-extension/releases) page.

2) Extract the contents of this zip to a directory called `dataProtection` and change directory to this directory.
```
unzip dataProtection.zip
cd dataProtection
```

3) Update your vCD details in the `ui_ext_api.ini` file

4) Run the command below to deploy the plugin
```
python deploy.py deploy
```

5) On success, the uploaded extension will be visible under "Data Protection" when you click on the Hamburger Icon in vCD HTML5 UI.

6) Service Provider must first add Cohesity Cluster configuration from the provider view by clicking on "Data Protection" and also specify the mapping of vCloud organizations to Cohesity organizations.

Please note that prior to this "Enable Organizations" setting must be enabled on the Cohesity Cluster and organizations must be present on the Cluster for the mapping to succeed.

7) Once this is done, tenants can start using the extension by clicking on "Data Protection" in the tenant view.

### 4. Upgrade Cohesity Extension:
1) Download the latest `dataProtection.zip` file from the [releases](https://github.com/cohesity/cohesity-vcd-extension/releases) page.

2) Extract the contents of this zip to a directory called `dataProtection` and change directory to this directory.
```
unzip dataProtection.zip
cd dataProtection
```

3) Update your vCD details in the `ui_ext_api.ini` file

4) Run the command below to remove previous version of the plugin
```
python deploy.py remove

```
5) Run the command below to deploy the new version of the plugin
```
python deploy.py deploy
```

6) On success, the uploaded new version of extension will be visible under "Data Protection" when you click on the Hamburger Icon in vCD HTML5 UI.

**Note:** The 'Data Protection' menu item in vCD HTML5 UI will only be visible to the vCD tenants which are mapped to some Cohesity cluster.


## <a name="features"></a> Features: :bulb:

1) Overview
    * Provides dashboard summary of the vCD resources and the associated cohesity protection status. 

2) Protection for vApps/VMs
    * Protect and unprotect VMs and vApps
    * On-demand backups for VMs and vApps.

3) Restore
    * Recover files, folders or VM/vApp from backed up objects

4) Monitor
    * Monitoring backup and restore tasks executed from vCD extension.

## <a name="contribute"></a> Tutorial Video :movie_camera:

* <a href="https://www.youtube.com/watch?v=dhaHRrXg4tM">VCD Tutorial </a>

## <a name="contribute"></a> Contribute :handshake:

* [Refer our contribution guideline](./CONTRIBUTING.md).


## <a name ="suggest"></a> Questions or Feedback :raised_hand:

We would love to hear from you. Please send your questions and feedback to: *cohesity-api-sdks@cohesity.com*

