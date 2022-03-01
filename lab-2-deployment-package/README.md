# Lab 2 - The Managed Application Deployment Package

In this lab you will edit the artifacts that go into a an Azure Managed Application deployment package. You will run your artifacts through a validation process and package them for use in deployment.

## Exercise 1 - Your initial package artifacts

This exercise just familiarizes you with the artifacts of the lab: the items that go into a Managed Application deployment package.

## Exercise 2 - ARM TTK

This exercise introduces ARM TTK, a tool that runs in PowerShell and validates the artifacts in your deployment package. The ARM TTK tool runs tests that ensure valid `mainTemplate.json` and `createUiDefinition ` files.

## Exercise 3 - mainTemplate.json

Here you will build out a reasonably complex ARM template that deploys a virtual machine and its needed networking components. 

## Exercise 4 - createUiDefinition.json

This exercise will take you through creating the file that defines the user experience for installing your solution via the `mainTemplate.json` created above.

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
