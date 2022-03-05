# Lab 3 - Purchasing an Azure Managed Application

This lab focuses on purchasing the offer and plans you created in [Lab 1](../lab-1-partner-center/README.md) and [Lab 2](../lab-2-deployment-package/README.md).

First, check Partner Center.

> This lab will only work if your offer in is published and visible in the **Publisher signoff** stage of publication as seen on the **Offer overview**. page in **Partner Center**. 
> 
> There should be a **Go live** button on the **Offer overview** page. Do **NOT** click this button at any point today as doing so will submit your offer to the certification team to make it live.

## Exercise 1 - The Silver plan

This exercise takes you through the process of buying your Silver plan, created in [Lab 1](../lab-1-partner-center/README.md). You'll also have a look at your resulting Managed Application.

### Finding the private plan

1. Open the [Azure portal](https://portal.azure.com/) and log in.
2. At the top of the screen click the **Create a resource** button. You are taken to the public Azure marketplace.
    > In Lab 1, you configured your offer to be available to your subscription while in the **Publisher signoff** stage. This makes your offer a private one and you need to navigate to the private offers part of the marketplace to purchase it.
3. Under the search box, click the link that reads **See more in the Marketplace**.
4. On this page you see a pink bar at the top of the screen telling you that you have private offers available. Click the link that reads **View private products**.
5. Locate your offer named **AMA Workshop 1 (preview)** and click on that tile. You are taken to the offer's purchase page, which is in **Preview** mode.

### Purchasing the Silver plan

#### Basics tab

1. In the plan dropdown, ensure Silver is selected.
   > This plan will create a simple **Data storage** service in your subscription.
2. Click the **Create** button. You are taken to the offer creation page, which needs your input.
3. On the **Basics** tab, you should see your current subscription selected. If not, select it from the dropdown.
4. For **Resource group** click the **Create new** link under the dropdown.
5. Name your new resource group **ama_workshop_purchases**.
6. Select a region near you.
7. In the **Managed Application Details** section, enter the following.
   1. **Application name:** workshopapp1
   2. **Managed Resource Group**: workshopapp1-mrg
8. At the bottom of the screen, click the **Next: Storage setting** button.

#### Storage settings tab

1. Enter the following values.
   1. **Storage account name prefix:** amaws01
   2. **Storage account type:** Standard_LRS
2. Click the **Review + create** button at the bottom of the screen.

#### Review + create tab

1. Scroll down to the **Co-Admin Access Permission** section.
2. Click the checkbox **I agree to the terms and conditions above**.
3. Click the **Create** button at the bottom of the screen. You are taken to the **Deployment** page. 
   > Deploying your application may take a couple of minutes.

#### Deployment screen

When deployment is complete, you are met with a message that reads, **Your deployment is complete**.

1. At the bottom of the screen click the **Go to resource** button. 

You are taken to the **managed application** instance.

## Exercise 2 - The managed application

In this exercise you will tour the managed application you've just purchased.

 

## Exercise 3 - The Gold plan

This exercise takes you through the process of buying your Gold plan, created in [Lab 2](../lab-2-deployment-package/README.md). After deployment, you'll use the virtual machine deployed by the application.

### Purchasing the Gold plan

### Examining the managed application

### Remote into the virtual machine

## Conclusion

Congratulations, you have completed Lab 3. In this lab you accomplished the following.

1. Purchased two plans you created using an offer from Lab 1.
2. Examined the artifacts deployed by each plan.
3. Logged into the virtual machine deployed by the Gold plan.