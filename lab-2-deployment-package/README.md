# Lab 2 - The Managed Application Deployment Package

In this lab you will edit the artifacts that go into a an Azure Managed Application deployment package. You will run your artifacts through a validation process and package them for use in deployment.

## Exercise 1 - Your initial package artifacts

This exercise just familiarizes you with the artifacts of the lab: the items that go into a Managed Application deployment package.

## Exercise 2 - ARM TTK

This exercise introduces **Azure Resource Manager Template Toolkit** (ARM TTK), a tool that runs in PowerShell and validates the artifacts in your deployment package. The ARM TTK tool runs tests that ensure valid `mainTemplate.json` and `createUiDefinition ` files.

You will clone the ARM TTK repository and bring the code to your local machine where it can be executed using PowerShell. 

If you are not on Windows you may download PowerShell for your platform.

- [Linux](https://github.com/Azure/arm-ttk/blob/master/arm-ttk/README.md#running-tests-on-linux)
- [macOS](https://github.com/Azure/arm-ttk/blob/master/arm-ttk/README.md#running-tests-on-macos)

1. Clone the [ARM TTK repository](https://github.com/Azure/arm-ttk) from GitHub.
2.
3. Set up ARM TTK to run on your machine.

> ```powershell
> cd "<PATH TO ARM TTK REPO>\arm-ttk\arm-ttk"
> 
> Import-Module .\arm-ttk.psd1
> ```

1. Now, from the same directory you may execute the ARM TTK tool like so. Note the path is looking at the `end` folder, which contains the solution to this lab.

```powershell
$AMAPackage = "<PATH TO AMA-WORKSHOP>\ama-workshop\lab-2-deployment-package\assets\end"

Test-AzTemplate -TemplatePath $AMAPackage
```

2. See that test pass. The output from ARM TTK should look something like this.

```
Validating end\createUiDefinition.json
  AllFiles
    [+] JSONFiles Should Be Valid (61 ms)
  CreateUIDefinition
    [+] Allowed Values Should Actually Be Allowed (190 ms)

    ...

Validating end\mainTemplate.json
  deploymentTemplate
    [+] adminUsername Should Not Be A Literal (90 ms)
    [+] apiVersions Should Be Recent In Reference Functions (171 ms)

    ...
```

> You will use the **ARM TTK** tool to check the status of your package files several times during this lab. Do not close the terminal window.

3. Set up ARM TTK to point at the `begin` directory for validation.

 ```powershell
 $AMAPackage = "<PATH TO AMA-WORKSHOP>\ama-workshop\lab-2-deployment-package\assets\begin\"
```

4. Run the tests. They should all pass although there is essentially nothing in the `mainTemplate.json` file in the directory.

 ```powershell
 Test-AzTemplate -TemplatePath $AMAFile
```

## Exercise 3 - `mainTemplate.json`

Here you will build out a reasonably complex ARM template that deploys a virtual machine and its needed networking components.

The basic outline of an `mainTemplate.json` ARM template looks like this. You will fill in the sections that are currently empty.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {}
}
```

There are several parameters the ARM template expects. You will fill them in and take note of the different types of parameters.

1. Add the following parameters to the ARM template.

```json
"adminUsername": {
  "type": "string",
  "metadata": {
    "description": "Username for the Virtual Machine."
  }
},
"adminPassword": {
  "type": "securestring",
  "metadata": {
    "description": "Password for the Virtual Machine."
  }
},
  "itemPrefix": {
    "type": "string",
    "metadata": {
      "description": "Unique string used to prefix resource names created in the project."
    }
  },
"storageAccountName": {
    "type": "string"
},
"storageAccountType": {
    "type": "string"
},
"windowsOSVersion": {
  "type": "string",
  "defaultValue": "2022-Datacenter",
  "allowedValues": [
    "2019-Datacenter",
    "2022-Datacenter"
  ],
  "metadata": {
    "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version."
  }
},
"vmSize": {
  "type": "string",
  "defaultValue": "Standard_D2s_V3",
  "metadata": {
    "description": "Size of the virtual machine."
  }
},
"location": {
  "type": "string",
  "defaultValue": "[resourceGroup().location]",
  "metadata": {
    "description": "Location for all resources."
  }
}
```

> Take a moment to review the `type` attribute of each parameter. It should be evident you can pass in multiple data types.

1. Run ARM TTK. The following test fails.

```cmd
Parameters Must Be Referenced
```

2. Make the variables section look like the following JSON.

```json
"variables": {
  "addressPrefix": "10.0.0.0/16",
  "domainNameLabel": "[concat(parameters('itemPrefix'), '-dnl')]",
  "networkSecurityGroupName": "[concat(parameters('itemPrefix'), '-nsg')]",
  "nicName": "[concat(parameters('itemPrefix'), '-nic')]",
  "publicIPAddressName": "[concat(parameters('itemPrefix'), '-ip')]",
  "storageAccountName": "[concat(parameters('itemPrefix'), uniquestring(resourceGroup().id))]",
  "subnetName": "Subnet",
  "subnetPrefix": "10.0.0.0/24",
  "virtualNetworkName": "[concat(parameters('itemPrefix'), '-vnet')]",
  "vmName": "[concat(parameters('itemPrefix'), '-vm')]"
}
```

3. Run ARM TTK. What differences do you see?

    > Now there are **parameters** and **variables** that are not being referenced. This means they are not being referenced in the `resources` section which defines the actual resources to be created by the ARM template.
    >
    > Note the `itemPrefix` parameter is no longer reported as an unreferenced parameter since it being used by several variables.

4. make the `references[]` section look like the following.
```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2021-08-01",
    "name": "[parameters('storageAccountName')]",
    "location": "[parameters('location')]",
    "sku": {
      "name": "[parameters('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
  },
  {
    "type": "Microsoft.Network/publicIPAddresses",
    "apiVersion": "2020-06-01",
    "name": "[variables('publicIPAddressName')]",
    "location": "[parameters('location')]",
    "properties": {
      "publicIPAllocationMethod": "Dynamic",
      "dnsSettings": {
        "domainNameLabel": "[variables('domainNameLabel')]"
      }
    }
  },
  {
    "comments": "Default Network Security Group for template",
    "type": "Microsoft.Network/networkSecurityGroups",
    "apiVersion": "2021-08-01",
    "name": "[variables('networkSecurityGroupName')]",
    "location": "[parameters('location')]",
    "properties": {
      "securityRules": [
        {
          "name": "default-allow-3389",
          "properties": {
            "priority": 1000,
            "access": "Allow",
            "direction": "Inbound",
            "destinationPortRange": "3389",
            "protocol": "Tcp",
            "sourcePortRange": "*",
            "sourceAddressPrefix": "*",
            "destinationAddressPrefix": "*"
          }
        }
      ]
    }
  },
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2020-06-01",
    "name": "[variables('virtualNetworkName')]",
    "location": "[parameters('location')]",
    "dependsOn": [
      "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroupName'))]"
    ],
    "properties": {
      "addressSpace": {
        "addressPrefixes": ["[variables('addressPrefix')]"]
      },
      "subnets": [
        {
          "name": "[variables('subnetName')]",
          "properties": {
            "addressPrefix": "[variables('subnetPrefix')]",
            "networkSecurityGroup": {
              "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroupName'))]"
            }
          }
        }
      ]
    }
  },
  {
    "type": "Microsoft.Network/networkInterfaces",
    "apiVersion": "2020-06-01",
    "name": "[variables('nicName')]",
    "location": "[parameters('location')]",
    "dependsOn": [
      "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
      "[resourceId('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
              "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
              "id": "[variables('subnetRef')]"
            }
          }
        }
      ]
    }
  },
  {
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2020-06-01",
    "name": "[variables('vmName')]",
    "location": "[parameters('location')]",
    "identity": {
      "type": "systemAssigned"
    },
    "dependsOn": [
      "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
      "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
      "hardwareProfile": {
        "vmSize": "[parameters('vmSize')]"
      },
      "osProfile": {
        "computerName": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
      },
      "storageProfile": {
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "[parameters('windowsOSVersion')]",
          "version": "latest"
        },
        "osDisk": {
          "createOption": "FromImage"
        },
        "dataDisks": [
          {
            "diskSizeGB": 1023,
            "lun": 0,
            "createOption": "Empty"
          }
        ]
      },
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
          }
        ]
      },
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": true,
          "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob]"
        }
      }
    }
  },
  {
    "apiVersion": "2020-04-01-preview",
    "name": "[guid(resourceGroup().id)]",
    "type": "Microsoft.Authorization/roleAssignments",
    "dependsOn": [
      "[resourceId('Microsoft.Compute/virtualMachines', variables('vmName'))]"
    ],
    "properties": {
      "roleDefinitionId": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
      "principalId": "[reference(variables('vmName'),'2021-11-01', 'Full').identity.principalId]",
      "delegatedManagedIdentityResourceId": "[resourceId('Microsoft.Compute/virtualMachines', variables('vmName'))]", 
      "scope": "[resourceGroup().id]",
      "principalType": "ServicePrincipal"
    }
  }
]
```

> Take moment to examine the different resources that will be created by this ARM template.

6. Run ARM TTK. One test should fail.

    > ```cmd
    > apiVersions Should Be Recent
    > ```

7. The resource type of `Microsoft.Storage/storageAccounts` has the wrong API version. Fix this problem as recommended by ARM TTK.

8. Ensure ARM TTK passes with no errors.

Your ARM template is complete.

## Exercise 4 - `createUiDefinition.json`

This exercise will take you through creating the file that defines the user experience for installing your solution via the `mainTemplate.json` created above.

Remember the role of the `createUiDefinition.json` file is to do the following.

   1. Provide parameters to the `mainTemplate.json` ARM template.
   2. Provide an installation user experience for the customer.

The main components of a `createUiDefinition.json` file looks like this.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [],
        "steps": [],
        "outputs": {}
    }
}
```

1. Create the file `createUiDefinition.json` in your `begin` folder.
2. Paste the above JSON into the new file.
3. Run ARM TTK. You get several errors on the `createUiDefinition.json` file. Take a moment and ensure you understand the errors being reported.
3. Open the URL SANDBOX TODO


## Exercise 5 - Publishing your deployment package

In this exercise, you will create a second plan to go along with the offer you created in [Lab 1](../lab-1-partner-center/README.md).

## Exercise 6 - Publish your offer

Now that you have two plans in your offer (created in Lab 1), it's time to publish your offer.

1. Click the **Review and publish** button at the bottom of the page, which takes you to the **Review publish changes** page.
2. Once all indicators are green, click the **Publish** button at the bottom of the page. You will be taken to the **Offer overview** page where you can monitor the status of your offer as it progresses to the **Publisher signoff** stage. This process can take some time to complete.

> **DO NOT** go past the **Publisher signoff** stage by clicking a **Go live** button.


## Conclusion

Great job, you've finished lab 2! 

In this lab you accomplished the following.

1. Explored initial deployment package artifacts.
2. Used ARM TTK to validate your deployment files.
3. Fleshed out an ARM template in `mainTemplate.json`.
4. Created the user experience of installing the solution via the ARM template using `createUiDefinition,json`.
5. Created a deployment package from the artifacts in this lab.
6. Created a new plan in Partner Center and used your new deployment package in that plan.
7. Published your offer with its new plan.
