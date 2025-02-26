
Getting Started - Using the next-generation management libraries of Azure SDK for Javascript/Typescript
=============================================================

We are excited to announce that a new set of management libraries are
now in Public Preview. Those packages share a number of new features
such as Azure Identity support, HTTP pipeline, error-handling.,etc, and
they also follow the new Azure SDK guidelines which create easy-to-use
APIs that are idiomatic, compatible, and dependable. See [Typescript Design Guidelines](https://azure.github.io/azure-sdk/typescript_design.html) for more information.

Currently, we have previewed several packages such as `azure/arm-resources`, `@azure/arm-storage`, 
`@azure/arm-compute`, `@azure/arm-network` for next-generation. See more from npmjs.com and find 
the latest version under `next` tag and have a try. If you are interested in upgrading to the latest new generation of SDK, please refer to this [migration guide](./MIGRATION-guide-for-next-generation-management-libraries.md) for more information.

In this basic quickstart guide, we will walk you through how to
authenticate to Azure and start interacting with Azure resources. There are several possible approaches to
authentication. This document illustrates the most common scenario.

Prerequisites
-------------

You will need the following values to authenticate to Azure

-   **Subscription ID**
-   **Client ID**
-   **Client Secret**
-   **Tenant ID**

These values can be obtained from the portal, here's the instructions:

### Get Subscription ID

1.  Login into your Azure account
2.  Select Subscriptions in the left sidebar
3.  Select whichever subscription is needed
4.  Click on Overview
5.  Copy the Subscription ID

### Get Client ID / Client Secret / Tenant ID

For information on how to get Client ID, Client Secret, and Tenant ID,
please refer to [this document](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)

### Setting Environment Variables

After you obtained the values, you need to set the following values as
your environment variables

-   `AZURE_CLIENT_ID`
-   `AZURE_CLIENT_SECRET`
-   `AZURE_TENANT_ID`
-   `AZURE_SUBSCRIPTION_ID`

To set the following environment variables on your development system:

Windows (Note: Administrator access is required)

1.  Open the Control Panel
2.  Click System Security, then System
3.  Click Advanced system settings on the left
4.  Inside the System Properties window, click the `Environment Variables…` button.
5.  Click on the property you would like to change, then click the `Edit…` button. If the property name is not listed, then click the `New…` button.

Linux-based OS :

    export AZURE_CLIENT_ID="__CLIENT_ID__"
    export AZURE_CLIENT_SECRET="__CLIENT_SECRET__"
    export AZURE_TENANT_ID="__TENANT_ID__"
    export AZURE_SUBSCRIPTION_ID="__SUBSCRIPTION_ID__"

Install the package
-------------------

As an example, to install the Azure Compute module, you would run :

```sh
npm i @azure/arm-compute@30.0.0-beta.1
```
You can always find the latest preview version of our next-generation management libraries via npmjs under the `next` tag of each packages.  

We also recommend installing other packages for authentication and core functionalities :

```sh
npm i @azure/identity
```

Authentication
--------------

Once the environment is setup, all you need to do is to create an authenticated client. Before creating a client, you will first need to authenticate to Azure. In specific, you will need to provide a credential for authenticating with the Azure service.  The `@azure/identity` module provides facilities for various ways of authenticating with Azure including client/secret, certificate, managed identity, and more.

Our default option is to use **DefaultAzureCredential** which will make use of the environment variables we have set and take care of the authentication flow for us.

```typescript
const credential = new DefaultAzureCredential();
```

For more details on how authentication works in `@azure/identity`, please see the documentation for [`@azure/identity`](https://www.npmjs.com/package/@azure/identity).


Creating a Resource Management Client
-------------------------------------

Now, you will need to decide what service to use and create a client to connect to that service. In this section, we will use `Compute` as our target service. 

To show an example, we will create a client to manage Virtual Machines. The code to achieve this task would be:

```typescript
const client = new ComputeManagementClient(credential, subscriptionId);
```

Interacting with Azure Resources
--------------------------------

Now that we are authenticated and have created our clients, we can use our client to make API calls. For resource management scenarios, most of our cases are centered around creating / updating / reading / deleting Azure resources. Those scenarios correspond to what we call "operations" in Azure. Once you are sure of which operations you want to call, you can then implement the operation call using the management client we just created in previous section.


In the following samples. we are going to show

- **Step 1** : How to Create a simple resource Resource Group.
- **Step 2** : How to Manage Resource Group with Azure SDK for Javascript/Typescript
- **Step 3** : How to Create a complex resource Virtual Machine.

Let's show our what final code looks like

Example: Creating a Resource Group
---------------------------------

***Import the packages***  
Typescript
```typescript
import * as resources from "@azure/arm-resources";
import { DefaultAzureCredential } from "@azure/identity";
```
Javascript
```javascript
const resources = require("@azure/arm-resources");
const identity = require("@azure/identity");
```

***Define some global variables***  
Typescript
```typescript
const subscriptionId = process.env.AZURE_SUBSCRIPTION_ID;
const credential = new DefaultAzureCredential();
const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
```
Javascript
```javascript
const subscriptionId = process.env.AZURE_SUBSCRIPTION_ID;
const credential = new identity.DefaultAzureCredential();
const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
```

***Create a resource group***  
Typescript
```typescript
async function updateResourceGroup(resourceGroupName: string) {
    const parameter:resources.ResourceGroupPatchable = {
        tags: {
            tag1: "value1",
            tag2: "value2"
        }
    };
    await resourcesClient.resourceGroups.update(resourceGroupName, parameter).then(
        result => {
            console.log(result);
        }
    )
}
```
Javascript
```javascript
async function createResourceGroup(resourceGroupName) {
    const parameter = {
        location: "eastus",
        tags: {
            tag1: "value1"
        }
    };
    const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
    await resourcesClient.resourceGroups.createOrUpdate(resourceGroupName, parameter).then(
        result => {
            console.log(result);
        }
    )
}
```

Example: Managing Resource Groups with JS/TS SDK
---------------------------------

***Import the packages***  
Typescript
```typescript
import * as resources from "@azure/arm-resources";
import { DefaultAzureCredential } from "@azure/identity";
```
Javascript
```javascript
const resources = require("@azure/arm-resources");
const identity = require("@azure/identity");
```

***Authentication and Setup***  
Typescript
```typescript
const subscriptionId = process.env.AZURE_SUBSCRIPTION_ID;
const credential = new DefaultAzureCredential();
const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
```
Javascript
```javascript
const subscriptionId = process.env.AZURE_SUBSCRIPTION_ID;
const credential = new identity.DefaultAzureCredential();
const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
```

***Update a resource group***  
Typescript
```typescript
async function updateResourceGroup(resourceGroupName: string) {
    const parameter:resources.ResourceGroupPatchable = {
        tags: {
            tag1: "value1",
            tag2: "value2"
        }
    };
    await resourcesClient.resourceGroups.update(resourceGroupName, parameter).then(
        result => {
            console.log(result);
        }
    )
}
```
Javascript
```javascript
async function updateResourceGroup(resourceGroupName) {
    const parameter = {
        location: "eastus",
        tags: {
            tag1: "value1"
        }
    };
    const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
    await resourcesClient.resourceGroups.update(resourceGroupName, parameter).then(
        result => {
            console.log(result);
        }
    )
}
```

***List all resource groups***  
Typescript or Javascript
```typescript
async function listResourceGroup() {
    const result_list = new Array();
    for await (let item of resourceClient.resourceGroups.list()){
        result_list.push(item);
    }
    console.log(result_list);
}
```

***Get a Resource Group***  
Typescript
```typescript
async function getResourceGroup(resourceGroupName: string) {
    const get_result = await resourceClient.resourceGroups.get(resourceGroupName);
    console.log(get_result);
}
```
Javascript
```javascript
async function getResourceGroup(resourceGroupName) {
    const get_result = await resourceClient.resourceGroups.get(resourceGroupName);
    console.log(get_result);
}
```

***Delete a resource group***  
Typescript
```typescript
async function deleteResourceGroup(resourceGroupName: string) {
    await resourcesClient.resourceGroups.delete(resourceGroupName).then(
        result => {
            console.log(result);
        }
    )
}
```
Javascript
```javascript
async function deleteResourceGroup(resourceGroupName) {
    await resourcesClient.resourceGroups.delete(resourceGroupName).then(
        result => {
            console.log(result);
        }
    )
}
```

***Manage Resource Group***  
Typescript or Javascript
```typescript
async function main() {
    const resourceGroupName = "jstest";
    await createResourceGroup(resourceGroupName);
    await listResourceGroup();
    await getResourceGroup(resourceGroupName);
    await updateResourceGroup(resourceGroup);
    await getResourceGroup(resourceGroupName);
    await deleteResourceGroup(resourceGroupName);
    await listResourceGroup();
}
```


Example: Managing Virtual Networks
---------------------------------
In addition to resource groups, we will also use Virtual Networks as an example

***Import the packages***  
Typescript
```typescript
import * as network from "@azure/arm-network";
import * as resources from "@azure/arm-resources";
import { DefaultAzureCredential } from "@azure/identity";
```
Javascript
```javascript
const identity = require("@azure/identity");
const resources = require("@azure/arm-resources");
const network = require("@azure/arm-network");
```

***Define the global variables***  
Typescript or Javascript
```typescript
const subscriptionId = process.env.AZURE_SUBSCRIPTION_ID;
const resourceGroupName = "testRG";
const subnetName = "subnetnamex";
const interfaceName = "interfacex";
const networkName = "networknamex";
const location = "eastus";
```

***Authentication and Setup***  
Typescript
```typescript
const credential = new DefaultAzureCredential();
const networkClient = new network.NetworkManagementClient(credential, subscriptionId);
const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
```
Javascript
```javascript
const credential = new identity.DefaultAzureCredential();
const networkClient = new network.NetworkManagementClient(credential, subscriptionId);
const resourcesClient = new resources.ResourceManagementClient(credential, subscriptionId);
```

***Creating a Resource Group***  
Typescript
```typescript
async function createResourceGroup() {
    const parameter: resources.ResourceGroup = {
        location: "eastus",
        tags: {
            tag1: "value1"
        }
    };
    await resourcesClient.resourceGroups.createOrUpdate(resourceGroupName, parameter).then(
        result => {
            console.log(result);
        }
    )
}
```
Javascript
```javascript
async function createResourceGroup() {
    const parameter = {
        location: "eastus",
        tags: {
            tag1: "value1"
        }
    };
    await resourcesClient.resourceGroups.createOrUpdate(resourceGroupName, parameter).then(
        result => {
            console.log(result);
        }
    )
}
```

***Creating a Virtual Network***  
Typescript
```typescript
async function createVirtualNetwork() {
    const parameter: network.VirtualNetwork = {
        location: location,
        addressSpace: {
            addressPrefixes: ['10.0.0.0/16']
        }
    };
    const virtualNetworks_create_info = await networkClient.virtualNetworks.beginCreateOrUpdateAndWait(resourceGroupName, networkName, parameter);
    console.log(virtualNetworks_create_info);
}
```
Javascript
```javascript
async function createVirtualNetwork() {
    const parameter = {
        location: location,
        addressSpace: {
            addressPrefixes: ['10.0.0.0/16']
        }
    };
    const virtualNetworks_create_info = await networkClient.virtualNetworks.beginCreateOrUpdateAndWait(resourceGroupName, networkName, parameter);
    console.log(virtualNetworks_create_info);
}
```

***Creating a Subnet***  
Typescript
```typescript
async function createSubnet() {
    const subnet_parameter: network.Subnet = {
        addressPrefix: "10.0.0.0/24"
    };
    const subnet_create_info = await networkClient.subnets.beginCreateOrUpdateAndWait(resourceGroupName, networkName, subnetName, subnet_parameter);
    console.log(subnet_create_info)
}
```
Javascript
```javascript
async function createSubnet() {
    const subnet_parameter = {
        addressPrefix: "10.0.0.0/24"
    };
    const subnet_create_info = await networkClient.subnets.beginCreateOrUpdateAndWait(resourceGroupName, networkName, subnetName, subnet_parameter);
    console.log(subnet_create_info)
}
```

Need help?
----------

-   File an issue via [Github
    Issues](https://github.com/Azure/azure-sdk-for-js/issues)  

Contributing
------------

For details on contributing to this repository, see the contributing
guide.

This project welcomes contributions and suggestions. Most contributions
require you to agree to a Contributor License Agreement (CLA) declaring
that you have the right to, and actually do, grant us the rights to use
your contribution. For details, visit <https://cla.microsoft.com>.

When you submit a pull request, a CLA-bot will automatically determine
whether you need to provide a CLA and decorate the PR appropriately
(e.g., label, comment). Simply follow the instructions provided by the
bot. You will only need to do this once across all repositories using
our CLA.

This project has adopted the Microsoft Open Source Code of Conduct. For
more information see the Code of Conduct FAQ or contact
<opencode@microsoft.com> with any additional questions or comments.
