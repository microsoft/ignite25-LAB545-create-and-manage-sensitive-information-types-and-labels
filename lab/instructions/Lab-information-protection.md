---
lab:
    title: 'Exercise 1 - Create and manage sensitive information types'
    module: 'Lab 1 - Implement Information Protection'
---


# Lab 1 - Exercise 1 - Create and manage sensitive information types

Megan Bowen, the Information Security Administrator at Contoso Ltd., is updating the organization's information protection strategy after previous incidents involving the unintentional sharing of personal data in support tickets. She needs to create and test custom sensitive information types that help detect employee IDs and references to personal health information in documents and emails.

**Tasks**:

1. Create custom sensitive information types
1. Modify confidence level to reduce false positives
1. Create a security group and assign roles to create an EDM classifier
1. Create keyword dictionary
1. Test custom sensitive information types

## Task 1 - Enable Audit in the Microsoft Purview portal

Audit records user and administrator activity, and you'll need it enabled to continue with creating auto-apply sensitivity labels in this session.

1. Log into Client 1 VM (SC-401-CL1) with the **Admin** account.

1. Open Microsoft Edge.

1. In Microsoft Edge, navigate to **https://purview.microsoft.com** and sign in as Megan Bowen (MeganB@WWLxZZZZZZ.onmicrosoft.com, where ZZZZZZ is your unique tenant prefix). Use the password provided by your lab host.

1. Select **Get started** on the welcome message for the new Microsoft Purview portal.

    ![Screenshot showing the Welcome to the new Microsoft Purview portal screen.](./media/welcome-purview-portal.png)

1. From the left sidebar, select **Solutions**, then select **Audit**.

1. On the **Search** page, select **Start recording user and admin activity** to enable audit logging.

    ![Screenshot showing the Start recording user and admin activity button.](./media/enable-audit-button.png)

1. Verify that the blue bar disappears from the page after you enable auditing.

You've successfully enabled Audit in Microsoft Purview. With auditing active, you're ready to move on to creating custom sensitive information types and sensitivity labels.

## Task 2 – Create a custom sensitive information type

In this task, you'll create a custom sensitive information type (SIT) to detect Contoso project codes. The built-in SITs don't match this format, so you need a custom one to ensure project codes can be consistently identified and protected.

1. Stay signed in to **Client 1 VM (SC-401-CL1)** with the **SC-401-CL1\admin** account.

1. In **Microsoft Edge**, navigate to `https://purview.microsoft.com` and sign in as **Megan Bowen** (`MeganB@WWLxZZZZZZ.onmicrosoft.com`, where ZZZZZZ is your unique tenant ID). Use the password provided by your lab host.

1. From the left sidebar, select **Solutions**, then select **Information Protection**.

1. Expand **Classifiers**, then select **Sensitive info types**.

1. On the **Sensitive info types** page, select **+ Create sensitive info type**.

1. On the **Name your sensitive info type** page, enter:

   - **Name**: `Contoso Project Codes`
   - **Description**: `Pattern for Contoso project code identifiers.`

1. Select **Next**.

1. On the **Define patterns for this sensitive info type** page, select **Create one now**.

1. On the **New pattern** flyout, select **+ Add primary element** > **Regular expression**.

1. On the **+ Add a regular expression** flyout, enter:

   - **ID**: `Contoso project code regex`
   - **Regular expression**: `[A-Z]{2}-[0-9]{4}-[0-9]{3}`
   - Select the radio button for **String match**.
   - Select **Done**.

1. Back on the **New pattern** flyout, under **Supporting elements**, select **+ Add supporting elements or group of elements**, then select **Keyword list**.

1. On the **Add a keyword list** flyout, enter:

   - **ID**: `Project code keywords`
   - In **Keyword Group #1** under **Case insensitive**:

   ```text
   Project
   Code
   Identifier
   ```

   - Select the radio button for **Word match**.
   - Select **Done**.

1. Back on the **New pattern** flyout, set **Detect primary AND supporting elements** to **100** characters.

1. Select **Create** at the bottom of the flyout.

1. Back on the **Define patterns for this sensitive info type** page, select **Next**.

1. On the **Choose the recommended confidence level to show in compliance policies** page, keep the default value, then select **Next**.

1. On the **Review settings and finish** page, review your configuration, then select **Create**. After the SIT is created, select **Done**.

You've successfully created a new sensitive information type to identify Contoso project codes. Custom SITs allow you to detect and classify organization-specific data so it can be protected through Microsoft Purview policies.















## Task 2 – Modify confidence level to reduce false positives

You've received reports that some documents containing employee IDs aren't being detected. To improve detection coverage, you'll lower the confidence level of the pattern in the Contoso Employee IDs SIT so it triggers even when only partial evidence is found, increasing the likelihood of detection.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account, and logged into Microsoft Purview as Joni Sherman.

1. In Microsoft Edge, navigate to `https://purview.microsoft.com`.

1. In the left navigation, select **Solutions** > **Information protection** > **Classifiers** > **Sensitive info types**.

1. Search for `Contoso Employee IDs` in the list and select the SIT name to open the details page.

1. Select **Edit** at the top of the page to modify the SIT.

1. On the **Name your sensitive info type** page, select **Next**.

1. On the **Define patterns for this sensitive info type** page, expand **Pattern #1** and review the settings.

1. Select the pencil icon on the right to edit the pattern.

1. In the **Edit pattern** flyout, set the **Confidence level** dropdown to **Medium confidence**, which allows matches with less supporting evidence than high confidence.

1. Select **Update** at the bottom of the flyout.

1. Select **Next** until you reach the **Review settings and finish** page.

1. Select **Save**, then select **Done** to update your sensitive info type.

1. Sign out of Joni's account by selecting the profile picture of Joni Sherman in the top right. Select **Sign out**, then close the browser window.

You have successfully reduced the confidence level to increase the sensitivity of your custom SIT, helping ensure documents with partial matching content are more likely to be flagged.

## Task 3 – Create a security group and assign roles to create an EDM classifier

In this task, you'll create the role group to create an EDM classifier and add Joni to the new role group.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account.

1. Open **Microsoft Edge** then navigate to **`https://admin.microsoft.com`**.

1. When the **Pick an account** page is displayed, select **Use another account** and sign in as **MOD Administrator** `admin@WWLxZZZZZZ.onmicrosoft.com` (where ZZZZZZ is your unique tenant ID provided by your lab hosting provider). Admin's password should be provided by your lab hosting provider.

1. From the left pane, expand **Teams & groups** then select **Active teams & groups**.

1. On the top of the **Active teams and groups** page, select **Security groups** then select **+ Add a security group**.

    ![Screenshot of the Add a group button.](../Media/add-security-group.png)

1. On the **Set up the basics** screen, enter:

    - **Name**: `EDM_DataUploaders`
    - **Description**: `People who upload data for EDM.`

1. Select **Next**.

1. On the **Edit settings** page, leave the default settings, then select **Next**.

1. On the **Review and finish adding group** page, review your settings and select **Create group**.

1. On the **EDM_DataUploaders group created** page, select **Close**.

1. Back on the **Active teams and groups** page, ensure the **Security** tab is selected from the top navigation ribbon, then select the **Refresh** button to display the newly created security group. Select the **EDM_DataUploaders** group from the list to open the **EDM_DataUploaders** flyout panel on the right.

1. Select the **Members** tab then select **View all and manage members**.

1. On the **Members** page select **+ Add members**.

1. On the **Add members** page, select the checkbox to the left of **Joni Sherman**, then select the **Add (1)** button at the bottom of the flyout panel.

1. Verify **Joni Sherman** is listed below **Members**, then close the flyout panel by selecting the **X** on the top right of the flyout panel.

1. Sign out of the Mod Administrator account by selecting the MA icon on the top right of the window, then selecting **Sign out** and closing the browser window.

You have successfully created the **EDM_DataUploaders group** and assigned Joni access to create an EDM classifier.

## Task 4 – Create keyword dictionary

Several violations of personal information leakage happened when users sent out emails after colleagues reported on sick leave. In those cases, the reason for illness or disease was disclosed. We don't want that to happen. In this task, you'll create a keyword dictionary to prevent personal information leakage in emails.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account, and you should be logged into Microsoft 365 as **Joni Sherman**.

1. The Microsoft Purview portal should still be to the EDM classifiers page in Microsoft Edge. If not, in Microsoft Edge, navigate to `https://purview.microsoft.com` > **Solutions** > **Information protection**.

1. In the left sidebar, expand **Classifiers** then select **Sensitive info types**.

1. Select **+ Create sensitive info type** to open the configuration for a new sensitive information type.

1. On the **Name your sensitive info type** page, enter:

    - **Name**: `Contoso Diseases List`
    - **Description**: `List of possible diseases of employees.`

1. Select **Next**.

1. On the **Define patterns for this sensitive info type** page, select **+ Create pattern**.

1. On the **New pattern** flyout panel on the right, under **Primary element** select **+ Add primary element**, then select **Keyword dictionary**.

1. On the **Add a keyword dictionary** page enter:

   - **Name**: `Diseases Dictionary`
   - **Keywords**:

    ```text
    flu
    influenza
    cold
    bronchitis
    otitis
    ```

1. Select **Done** at the bottom of the flyout panel.

1. Back on the **New pattern** page, under **Supporting elements**, select **+ Add supporting elements or group of elements**, then select **Keyword list** to add additional support for the keyword dictionary.

1. On the **Add a keyword list** page enter:

   - **ID**: `Employee absence`
   - **Case insensitive**:

    ``` text
    employee
    absence
    reason
    ```

1. Select **Done** at the bottom of the flyout panel.

1. Back on the **New pattern** page, review the configuration and select **Create**.

1. Back on the **Define patterns for this sensitive info type**, select **Next**.

1. On the **Choose the recommended confidence level to show in compliance policies**, leave the default value, then select **Next**.

1. On the **Review settings and finish** page, review your settings and select **Create**. Once your sensitive info type is created, select **Done** on the **Your sensitive info type is created** page.

1. Leave the browser window in the Microsoft Purview portal open.

You have successfully created a new sensitive information type based on a keyword dictionary and added more keywords to decrease the false positive rate.

## Task 5 – Test custom sensitive information types

Always test custom sensitive information types before using them in policies. Otherwise, data loss or leakage may occur if the pattern is misconfigured.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account, and you should be logged into Microsoft 365 as **Joni Sherman**.

1. In your task bar, search for `Notepad` in the search field. Select the **Notepad** app from the **Best match** section of the search.

1. In Notepad, enter:

    ``` text
    Employee Joni Sherman EMP123456 is absent because of the flu/influenza.
    ```

1. Select **File** > **Save As**.

1. Select **Documents** on the left side pane and enter `SickTestData.txt` as the **File name**, then select **Save**.

1. Close the Notepad window.

1. Back in **Microsoft Edge**, Microsoft Purview portal should still be open on the **Sensitive info types** page.

1. In the **Search** bar on the upper right, enter `Contoso` and press Enter.

1. Select **Contoso Employee IDs**.

1. Select **Test**.

1. On the **Upload file to test "Contoso Employee IDs"** flyout panel on the right, select **Upload file**.

1. Select **Documents** from the left pane, select the _SickTestData.txt_ file, then select **Open**.

1. Select **Test** to start the analysis.

1. On the **Match results** page, review the matches, then select **Finish** to end the test.

1. Navigate back to **Sensitive info types** and search for `Contoso` again.

1. This time select the **Contoso Diseases List** sensitive info type, then select **Test**.

1. On the **Upload file to test "Contoso Diseases List"** flyout panel on the right, select **Upload file**.

1. On the **Upload file to test** pane, select **Upload file**.

1. Select **Documents** from the left pane, select the _SickTestData.txt_ file, then select **Open**.

1. Select **Test** to start the analysis.

1. On the **Match results** page, review the matches, then select **Finish** to end the test.

You've successfully tested the two custom sensitive information types and validated that the search patterns work as expected.



---
lab:
    title: 'Exercise 2 - Create and manage sensitivity labels'
    module: 'Module 1 - Implement Information Protection'
---

# Lab 1 - Exercise 2 - Create and manage sensitivity labels

Joni Sherman, an Information Security Administrator at Contoso Ltd., is rolling out a sensitivity labeling strategy to help protect sensitive data across departments. As part of this effort, she's configuring manual and automatic labeling, sublabels, and encryption options, including support for Double Key Encryption (DKE) and integration with Microsoft Defender for Cloud Apps.

**Tasks**:

1. Enable support for sensitivity labels
1. Create a sensitivity label
1. Create a sublabel
1. Publish sensitivity labels
1. Configure auto labeling

## Task 1 – Enable support for sensitivity labels

In this task, you'll enable co-authoring for sensitivity labels, which also enables sensitivity labels for files in SharePoint and OneDrive.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account and logged into Microsoft Purview as Joni Sherman.

1. Open **Microsoft Edge**, then navigate to `https://purview.microsoft.com`.

1. In the left navigation, select **Settings** > **Information Protection**.

1. On the **Information Protection settings** ensure you're on the **Co-authoring for files with sensitivity labels** tab.

1. Select the checkbox for **Turn on co-authoring for files with sensitivity labels**.

1. Select **Apply** at the bottom of the screen.

You have successfully enabled support for sensitivity labels for files in SharePoint and OneDrive.

## Task 2 – Create a sensitivity label

In this task, you'll create a parent sensitivity label for internal content. This label includes basic settings and acts as a parent label for department-specific sublabels.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account.

1. In **Microsoft Edge**, navigate to `https://purview.microsoft.com`.

1. In the Microsoft Purview portal, select **Solutions** from the left sidebar, then select **Information Protection**.

1. On the **Microsoft Information Protection** page, on the left sidebar, select **Sensitivity labels**.

1. On the **Sensitivity labels** page select **+ Create a label**.

1. The **New sensitivity label** configuration will start. On the **Provide basic details for this label**, enter:

    - **Name**: `Internal`
    - **Display name**: `Internal`
    - **Description for users**: `Internal sensitivity label.`
    - **Description for admins**: `Internal sensitivity label for Contoso.`

1. Select **Next**.

1. On the **Define the scope for this label** page, select **Files** and **Emails**. If the checkbox for **Meetings** is selected, make sure it's deselected.

1. Select **Next**.

1. On the **Choose protection settings for labeled items** page, select **Next**.

1. On the **Auto-labeling for files and emails** page, select **Next**.

1. On the **Define protection settings for groups and sites** page, select **Next**.

1. On the **Review your settings and finish** page, select **Create label**.

1. On the **Your sensitivity label was created** page, select **Don't create a policy yet**, then select **Done**.

You've created a sensitivity label for internal use. This label will act as a parent label for more specific sublabels used across different departments.

## Task 3 – Create a sublabel

Now that you have a base label, you'll create a sublabel for HR-related documents. This sublabel includes protection settings and visible content markings to support internal data handling practices for the HR department.

1. On the **Sensitivity labels** page, find the newly created **Internal** sensitivity label. Select the vertical ellipsis (**...**) next to it, then select **+ Create sublabel** from the dropdown menu.

    ![Screenshot showing the Action menu to create a sublabel for a sensitivity label.](../Media/create-sublabel-button.png)

1. The **New sensitivity label** wizard will start. On the **Provide basic details for this label** page enter:

   - **Name**: `Employee data (HR)`
   - **Display name**: `Employee data (HR)`
   - **Description for users**: `This HR label is the default label for all specified documents in the HR Department.`
   - **Description for admins**: `This label is created in consultation with Ms. Jones (Head of HR department). Contact her if you need to change the label settings.`

1. Select **Next**.

1. On the **Define the scope for this label** page, select **Files** and **Emails**. If the checkbox for **Meetings** is selected, make sure it's deselected.

1. Select **Next**.

1. On the **Choose protection settings for labeled items** page, select the **Control access** and **Apply content marking** options, then select **Next**.

1. On the **Access control** page, select **Configure access control settings**.

1. Configure the encryption settings with these options:

   - **Assign permissions now or let users decide?**: Assign permissions now
   - **User access to content expires**: Never
   - **Allow offline access**: Only for a number of days
   - **Users have offline access to the content for this many days**: 15
   - Select the **Assign permissions** link. On the **Assign permissions** flyout panel, select the **+ Add any authenticated users**, then select **Save** to apply this setting.

1. On the **Access control** page, select **Next**.

1. On the **Content marking** page, select the toggle to enable **Content marking**.

1. For each of the following marking types, select the checkbox, then select the edit icon to enter the text:

   |Marking type|Text|
   |:---|:---|
   |Add a watermark|`INTERNAL USE ONLY`|
   |Add a header|`Internal Document`|
   |Add a footer|`Contoso Confidential`|

1. Select **Next**.

1. On the **Auto-labeling for files and emails** page, select **Next**.

1. On the **Define protection settings for groups and sites** page, select **Next**.

1. On the **Review your settings and finish** page, select **Create label**.

1. On the **Your sensitivity label was created** page, select **Don't create a policy yet**, then select **Done**.

You've created a sublabel that applies encryption and content markings to HR documents. This label helps ensure HR data is only accessible to authenticated users and can be identified by visual markings.

## Task 4 – Publish sensitivity labels

You will now publish the Internal and HR sensitivity label so that the published sensitivity labels will be available for the HR users to apply to their HR documents.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-cl1\admin** account, and you should be logged into Microsoft Purview as **Joni Sherman**.

1. In **Microsoft Edge**, the Microsoft Purview portal tab should still be open. If not, navigate to **`https://purview.microsoft.com`** > **Solutions** > **Information Protection** > **Sensitivity labels**.

1. On the **Sensitivity labels** page select **Publish labels**.

1. The publish sensitivity labels configuration will start.

1. On the **Choose sensitivity labels to publish** page, select the **Choose sensitivity labels to publish** link.

1. On the **Sensitivity labels to publish** flyout panel, select the **Internal** and **Internal/Employee data (HR)** checkboxes, then select **Add** at the bottom of the flyout page.

1. Back on the **Choose sensitivity labels to publish** page, select **Next**.

1. On the **Assign admin units** page, select **Next**

1. On the **Publish to users and groups** page, select **Next**.

1. On the **Policy settings** page, select **Next**.

1. On the **Default settings for documents** select **Next**.

1. On the **Default settings for emails** select **Next**.

1. On the **Default settings for meetings and calendar events** select **Next**.

1. On the **Default settings for Fabric and Power BI Content** page, select **Next**.

1. On the **Name your policy** page, enter:

   - **Name**: `Internal HR employee data`

   - **Enter a description for your sensitivity label policy**: `This HR label is to be applied to internal HR employee data.`

1. Select **Next**.

1. On the **Review and finish** page, select **Submit**.

1. On the **New policy created** page, select **Done** to finish publishing your label policy.

You have successfully published the Internal and HR sensitivity labels. Note that it can take up to 24 hours for changes to replicate to all users and services.

## Task 5 – Configure auto labeling

In this task, you'll create a sensitivity label for financial data and configure it to apply automatically to content containing specific financial identifiers, such as credit card numbers and bank routing information.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-cl1\admin** account.

1. In **Microsoft Edge**, navigate to `https://purview.microsoft.com` and log into the Microsoft Purview portal as **Joni Sherman**.

1. In the Microsoft Purview portal, select **Solutions** > **Information protection** > **Sensitivity labels**.

1. On the **Sensitivity labels** page, find the **Internal** sensitivity label. Select the vertical ellipsis (**...**), then select **+ Create sublabel** from the dropdown menu.

1. On the **Provide basic details for this label** page, enter:

   |Details|Text|
   |---|---|
   |**Name**|`Financial Data`|
   |**Display name**|`Financial Data`|
   |**Description for users**|`This content contains financial data that must be labeled and protected.`|
   |**Description for admins**|`This label is used for content that includes sensitive financial identifiers.`|

1. Select **Next**.

1. On the **Define the scope for this label** page, select **Files** and **Emails**. If the checkbox for **Meetings** is selected, make sure it's deselected.

1. Select **Next**.

1. On the **Choose protection settings for labeled items** page, select **Next**.

1. On the **Auto-labeling for files and emails** page, set the **Auto-labeling for files and emails** to enabled.

1. In the **Detect content that matches these conditions** section, select **+ Add condition** > **Content contains**.

1. In **Content contains** section select the **Add** > **Sensitive info types**.

1. In the **Sensitive info types** flyout page, search for and select these sensitive info types:

   - `Credit Card Number`
   - `ABA Routing Number`
   - `SWIFT Code`

1. Select **Add**.

1. Back on the **Auto-labeling for files and emails** page, select **Next**.

1. On the **Define protection settings for groups and sites** page, select **Next**.

1. On the **Review your settings and finish** page, select **Create label**.

1. On the **Your sensitivity label was created** page, select **Automatically apply label to sensitive content**, then select **Done**.

1. On the **Create auto-labeling policy** flyout page, select **Review policy**.

1. On the **Name your auto-labeling policy** page, leave the default, then select **Next**.

1. On the **Choose a label to auto-apply** page, review to ensure the _Internal/Financial Data_ label is selected, then select **Next**.

1. On the **Assign admin units** page, select **Next**.

1. On the **Choose locations where you want to apply the label** page, select the options for:

   - Exchange email
   - SharePoint sites
   - OneDrive accounts

1. Select **Next**.

1. On the **Set up common or advanced rules** page, leave the default **Common rules** selected, then select **Next**.

1. On the **Define rules for content in all locations** page, expand the rules for _Financial Data rule_ to ensure the expected rules are defined, then select **Next**.

1. On the **Additional settings for email** page, select **Next**.

1. On the **Decide if you want to test out the policy now or later** page, select **Run policy in simulation mode**, and select the checkbox for **Automatically turn on policy if not modified after 7 days in simulation.**

1. Select **Next**.

1. On the **Review and finish** page, select **Create policy**.

1. On the **Your auto-labeling policy was created** page, select **Done**.

You have successfully created a sensitivity label for financial data and configured an auto-labeling policy to detect and label content that contains sensitive financial information.

