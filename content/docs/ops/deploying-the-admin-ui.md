---
menu:
  docs:
    parent: deployment
title: Deploying the admin UI
---

# Deploying the admin UI
This guide covers the high level stops required to configure, deploy and add users to the Admin UI as part of an existing Cloud Foundry environment.

#### Background

The Cloud Foundry Admin UI provides a high level overview of metrics pulled from the Cloud Foundry UAA, Cloud Controller, NATS and databases.

#### Prerequisites

- Knowledge of the secrets and other properties specified in the initial deployment manifest.
- The [Spiff](https://github.com/cloudfoundry-incubator/spiff) templating tool.
- The [uaac](https://github.com/cloudfoundry/cf-uaac) command line client.
- A reachable CF UAA instance.

#### Procedure

1. [Deployment]({{< relref "#deployment" >}})
1. [Configuration]({{< relref "#configuration" >}})
1. [Operation]({{< relref "#operation" >}})

#### Deployment

As of this writing, instructions for deploying the Admin UI bosh release default to the v6 release.  We have deployed the v6 release as the included deployment template covers required properties and job definitions not present in v3.

1. Obtain the current release source.

		git clone git@github.com:cloudfoundry-community/admin-ui-boshrelease.git
		cd admin-ui-boshrelease
		git checkout tags/v6

1. Upload the v6 release to do be used during deployment.

		bosh upload release releases/admin-ui-6.yml

1. Create the deployment manifest

		./make_manifest warden

1. Initiate the deployment.

		bosh deploy

1. Run the registration errand.

		bosh run errand register_admin_ui

#### Configuration

Admin UI users and administrators must be part of the groups `admin_ui.users` or `admin_ui.admins` respectively.  Add new members with the uaac cli:

	uaac member add admin_ui.user USERNAME

Or...

	uaac member add admin_ui.admins USERNAME


#### Operation

Access the Admin UI at the uri specified in the deployment manifest. By default this is `https://admin.YOUR-SYSTEM-DOMAIN` as generated by `meta.subdomain "." properties.system_domain` in the `templates/admin-ui-deployment.yml`.  If you did not change this setting, try `https://admin.bosh-lite.com`.