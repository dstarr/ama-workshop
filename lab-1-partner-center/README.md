# Lab 1 - Publishing an Azure Managed Application Offer in Partner Center

## Mastering the Marketplace: Azure Managed Apps Workshop

In this lab you will configure and publish an Azure Application offer in the Azure Marketplace. To do this, you must have an account in Partner Center, the portal for marketplace publishers. You must also have an Azure account where you can create resources.

## Exercise 1 - Create the offer

In this exercise you will create an Anure application offer.

1. Log in to [Partner Center](https://partner.microsoft.com/en-us/dashboard/home).
1. Go to the **Marketplace offers** page.
1. Select the **New offer** dropdown and choose **Azure application** for the type of offer you will create.
1. You will be prompted for an **Offer ID** and **Offer alias**. Use the following values.
    1. **Offer ID:** ama-workshop-01
    1. **Offer alias:** AMA Workshop 1
    1. Click the **Create** button. Your new offer is created and you are redirected to the **Offer setup** page.

You now have an offer to work with. Proceed to the next exercise.

## Exercise 2 - Offer setup

In this exercise you will set up a leads management section and connect it to a CRM.

### CRM leads - Part 1

In this section you will fill our the **Offer setup** portion of your offer. You will connect your offer to a CRM so you collect leads of people using your solution, or interested in it.

1. Navigate to the **Offer setup** page via the left hand navigateion
1. Scroll down to the **Customer leads** section.
1. Click the **Connect** link.
1. Click the **Lead destination dropdown** and note you may integrate your lead collection with several types of CRMs. In this exercise you wlil connect to an **Azure table**, so select that option from the dropdown.
1. Put in your email address.

You must create the Azure table storage account in the Azure portal to connect to for lead management.

### Create the Data storage account in the Azure Portal

In this section you will create an Azure **Data Storage account** using the Azure portal.

1. Log in to the [Azure portal](https://portal.azure.com/).
1. Click **+ Create a resource** in the left-most side menu. The **Create a resource** page appears.
1. In the search box, type **Storage account**.
1. Select **Storage account** from the resulting dropdown.
1. On the **Storage account** page, click the **Create** button. You should now be on the **Create a storage account** page.
1. Under the **Project details** section, select **Create new** for your resource group.
1. Create a new Resource group named **ama-workshop-leads**.
1. For storage account name, type in a string that will be unique accross azure. Try something like **amaworshopleads002**, where **002** is a random number between 001 and 999.
1. For the **Region** select a region near you.
1. Under **Redundancy**, select **Locally-redundant storage (LRS)**. This is the leaast expensive storage account type.
1. Select the **Review and create** at the bottom of the page.You are taken to teh final review page and a message should appear at the top of the page, **Validation passed**.
1. Slect the **Create** button.


### CRM leads - Part 2

In this section you will finish configuring leads management by connecting it to the new Data Storage account you just created.

1. In the Azure portal, navigate to the Data Storage account you just created.
1. In the left hand menu under **Security + networking** select **Access keys**.
1. Near the top of the Access keys screen, click the **Show keys** link. This makes the hidden keys visible.
1. Under **key 1** copy the **Connection string** into your clipboard.
1. Return to the **Partner Center** browser tab and paste the connection string in the **Storage account connection string** text box.
1. Click the Validate link. You should see a positive confirmation that the connection string worked.
1. Click the **Connect** button.
1. Click the **Save draft** button at the bottom of the page.

You have completed the **Offer setup** screen. Proceed to the next exercise.

## Exercise 3 - Properties

In this exercise you will fill out the **Properties** section of your offer. 

1. Navigate the to **Properties** page via the left hand Nav.

### Categories

You must select a category for your Managed App. You want your app to show up in the Azure Marketplace, so you will select a category that will ensure it will appear there.

1. Click the **Categories** link.
1. Under the **Primary category** dropdown, select **Developer tools**.
1. Under the **Subcategory** dropdown, select **Developer Service**.

You have selected a category for your application. Note you could select one more, but that's not nedded for this lab.

### Legal

1. In the **Legal** section selec the checkbox next to "Use the Standard Contract for Microsoft’s commercial marketplace?"
1. A dialog box appears. Click the **Accept** button.
1. Click the **Save draft** button at the bottom of the page.

You have completed the **Properties** screen. Proceed to the next exercise.

## Exercise 4 - Offer listing

In this exercise, you fill fill out the **Offer listing** page.

1. In the left-hand menu, select **Offer listing**. You are taken to the **Offer listing** page.
1. In the **Search results summary** textbox, enter "**AMA Workshop 1 search results summary**".
1. In the Short description text area, enter "**AMA Workshop 1 short description**".
1. In the **Description** text area, enter "**AMA Workshop 1 description**".
1. In the **Search keywonrds** section, you may enter up to 3 keywords.
1. For the **Privacy policy link** textbox, you may use Microsoft's privacy policy for this workshop. Enter "https://privacy.microsoft.com/en-us/privacystatement".
1. Scroll down to the **Support contact** section and enter the required information. For the field **Support website for Azure Global customers**, enter the link "https://partner.microsoft.com/en-US/support/v2/".
1. Scroll down to the **Engineering contact information** section.
1. Enter the required information.
1. Scroll down to the **Marketplace media > Logos** section.

Here you will need to upload two logos that must be of the exaact right size. Examples are provided for you in the [./lab-1-partner-center/assets](./lab-1--partner-center/assets) folder. You can click to naviagate to the images or you can drag and drop them.

1. Upload the two images.
1. For the screenshot, click the **Add image caption** link. Enter whatever you like for a caption.
1. Click the **OK** button.
1. Click the **Save draft** button at the bottom of the page.

You have completed the **Offer listing** screen. Proceed to the next exercise.

## Exercise 5 - Preview audience

In this exercise, you will add your subscription id to the list of subscriptions that may access this offer before it is live. This is to provide access to your test team and anyoen else who would like to preview the offer before it goes live. In this case, you will enter the subscription ID you are using in theis workshop.

1. In the Azure portal, find your subscription ID. If you aren't sure how to do this take the following steps.
    1. In the Azure portal, in the Searh box at the top of the screen, enter the word, "Subscriptions."
    1. Select the **Subscriptions** icon in the dropdown.
    1. On the **Subscriptions** page, click the link to the subscription you are using.
    1. On the **Subscription** page, copy the **Subscription ID** into your clipboard.
1. In the Partner Center portal, **Preview audience** page, enter your subscription ID.
1. Provide a brief description for this subscription.

You have completed the **Preview audience** screen. Proceed to the next exercise.

## Exercise 6 - Resell through CSPs

In this excercise, you will indicate that your solution will not be re-sold by Cloud Solution Providers.

1. From the list of radio buttons, select **No partners in the CSP program**.
1. Click the **Save draft** button at the bottom of the page.

You have completed the **Resell through CSPs** screen. Proceed to the next exercise.

## Exercise 6 - Plan overview

Every offer must include at least one plan. In this exercise, you will add two plans to your offer.




