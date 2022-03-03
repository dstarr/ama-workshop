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

4. Now, from the same directory you may execute the ARM TTK tool like so. Note the path is looking at the **end** folder, which contains the solution to this lab.

```powershell
$AMAPackage = "<PATH TO AMA-WORKSHOP>\ama-workshop\lab-2-deployment-package\assets\end"

Test-AzTemplate -TemplatePath $AMAPackage
```

5. See that test pass. The output from ARM TTK should look something like this.

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

## Exercise 3 - mainTemplate.json

Here you will build out a reasonably complex ARM template that deploys a virtual machine and its needed networking components.

The basic outline of an ARM template looks like this. You will fill in the sections that are currently empty.

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

1. Set up ARM TTK to point at the **begin** for validation.

 ```powershell
 $AMAPackage = "<PATH TO AMA-WORKSHOP>\ama-workshop\lab-2-deployment-package\assets\begin\"
```

2. Run the tests. They should all pass.

 ```powershell
 Test-AzTemplate -TemplatePath $AMAFile
```

# HERE TODO

### Parameters

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
    }
```

2. Run ARM TTK. The tests 

### Variables

### Resources

### Outputs

## Exercise 4 - createUiDefinition.json

This exercise will take you through creating the file that defines the user experience for installing your solution via the `mainTemplate.json` created above.

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

This is what the file in your **begin** folder looks like. You will fill in the pieces as you go.

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
