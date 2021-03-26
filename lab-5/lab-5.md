# Lab 5: Implementing custom providers

Azure Managed Applications Workshop

## Lab overview

In this lab you will customize and deploy a managed application with functionality to manage and store custom resources.

# Exercise 1: Deploy the function package

In this exercise you will deploy a packaged Azure function to a storage account, making it ready for deployment by an ARM template.

## Create a storage account

1. Create a new Azure storage account
2. Put it in a new resource group named "AMALab5"

## Upoad the function application

1. In the blob storage of your new storage account, create a container named "function".
2. Upload the `lab-5/begin/functionpackage.zip` file to the new container.
3. Click on the ZIP file and then copy the URL for that blob.
4. Paste the URL to a location for use later.

# Exercise 2: Set up the ARM template

In this exercise you will add the custom provider resources to the `mainTemplate.json` ARM template for a managed application. 

## Add the function package URL

1. Open the `lab-5/begin/mainTemplate.json` file for editing.
2. Find the following JSON.

```json
"zipFileBlobUri": {
    "type": "string",
    "defaultValue": "CHANGE_THIS",
    "metadata": {
        "description": "The Uri to the uploaded function zip file"
    }
}
```
3. Replace `CHANGE_THIS` with the URL of the function app package you copied in the previous step.

## Add the custom provider resource

1. Find the resources section and note there are already 3 resources defined.
2. Add the following JSON to the array of resources so the ARM template will create the custom resource provider.

```json
{
    "apiVersion": "[variables('customrpApiversion')]",
    "type": "Microsoft.CustomProviders/resourceProviders",
    "name": "[variables('customProviderName')]",
    "location": "[parameters('location')]",
    "properties": {
        "actions": [],
        "resourceTypes": []
    },
    "dependsOn": [
        "[concat('Microsoft.Web/sites/',parameters('funcname'))]"
    ]
}

```

## Add custom actions

1. Add the following actions to the `actions` array.

```json
{
    "name": "ping",
    "routingType": "Proxy",
    "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
},
{
    "name": "users/contextAction",
    "routingType": "Proxy",
    "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
}
```

## Add the custom resource type

1. Find the `resourceTypes` array.
2. Add the following resource type to the array.

```json
{
    "name": "users",
    "routingType": "Proxy,Cache",
    "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
}
```

# Exercise 3: Set up the view definition

In this exercise you will add the custom provider elements to the `viewDefinition.json` that defines the interface for the managed application. You will add UI elements that make use of the function application for customizing the managed application.

## Add the overview command

1. Open the `lab-5/begin/viewDefinition.json` file for editing.
2. Find the **overview** section and add the following JSON after the **description** to declare a command. This command will appear at the top of the managed application.

```json
"commands": [
    {
        "displayName": "Ping Action",
        "path": "/customping",
        "icon": "LaunchCurrent"
    }
]
```
The **overview** view should now look like this.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to ...",
        "description": "This Managed application ...",
        "commands": [
            {
                "displayName": "Ping Action",
                "path": "/customping",
                "icon": "LaunchCurrent"
            }
        ]
    }
}
```

## Add the custom resources UI

1. Find the `"resourceType": "users"` value.
2. Paste the following `createUIDefinition` section below `"resourceType": "users"`.

```json
"createUIDefinition": {
    "parameters": {
        "steps": [
            {
                "name": "add",
                "label": "Add user",
                "elements": [
                    {
                        "name": "name",
                        "label": "User's Full Name",
                        "type": "Microsoft.Common.TextBox",
                        "defaultValue": "",
                        "toolTip": "Provide a full user name.",
                        "constraints": {
                            "required": true
                        }
                    },
                    {
                        "name": "location",
                        "label": "User's Location",
                        "type": "Microsoft.Common.TextBox",
                        "defaultValue": "",
                        "toolTip": "Provide a Location.",
                        "constraints": {
                            "required": true
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "name": "[steps('add').name]",
            "properties": {
                "FullName": "[steps('add').name]",
                "Location": "[steps('add').location]"
            }
        }
    }
}
```
Note the above is a fully formed createUIDefinition as you might find in a dedicated file of the same name.

## Add commands

1. After the `createUIDefinition` property, add the following JSON to define the command that will be executed.

```json
"commands": [
    {
        "displayName": "Custom Context Action",
        "path": "users/contextAction",
        "icon": "Start"
    }
]
```
## Add columns for layout

1. Under the commands array, add the following JSON to define the layout for showing any custom resources.

```json
"columns": [
    {
        "key": "properties.FullName",
        "displayName": "Full Name"
    },
    {
        "key": "properties.Location",
        "displayName": "Location",
        "optional": true
    }
]
```
You now have a fully formed `viewDefinition.json`.    


# Exercise 4: Deploy the managed application package

In this exercise you will add the managed application artifacts to a storage account, making them ready for deployment.

## Create the deployment package

ZIP the following files into a ZIP file with the files at the root.

- createUiDefinition.json
- mainTemplate.json
- viewDefinition.json

## Upload the package to the storage account

1. In the storage account you created earlier, create a new container named "arm".
2. Upload your ZIP file into this container.
3. Copy the URL for this blob and paste it somewhere so you have easy access to it later.

# Exercise 5: Deploy the managed application

In this exercise you will create a new Managed Application Definition and use it to deploy the managed application.

## Create the managed application definition



## Deploy the managed application

# Exercise 6: Using custom resources

In this exercise you will use the custom provider functionality you added to the managed applciation.

## Running the ping action

## Managing users

### Add new users

### Inspecting the user resource id

### Inspecting user storage

### Deleting a user