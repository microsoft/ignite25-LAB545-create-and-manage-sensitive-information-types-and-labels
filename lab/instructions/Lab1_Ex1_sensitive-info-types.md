---
lab:
    title: 'Exercise 1 - Create and manage sensitive information types'
    module: 'Lab 1 - Implement Information Protection'
---


# Lab 1 - Exercise 1 - Create and manage sensitive information types

Joni Sherman, the Information Security Administrator at Contoso Ltd., is updating the organization's information protection strategy after previous incidents involving the unintentional sharing of personal data in support tickets. She needs to create and test custom sensitive information types that help detect employee IDs and references to personal health information in documents and emails.

**Tasks**:

1. Create custom sensitive information types
1. Modify confidence level to reduce false positives
1. Create a security group and assign roles to create an EDM classifier
1. Create keyword dictionary
1. Test custom sensitive information types

## Task 1 – Create custom sensitive information types

In this task, you'll create a new custom sensitive information type that recognizes the pattern of employee IDs near the keywords "Employee" and "ID".

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account.

1. In **Microsoft Edge**, navigate to **`https://purview.microsoft.com`** and log into the Microsoft Purview portal as `JoniS@WWLxZZZZZZ.onmicrosoft.com` (where ZZZZZZ is your unique tenant ID provided by your lab hosting provider). Joni's password was set in a previous exercise.

1. On the left sidebar, select **Solutions** then select **Information Protection**.

1. On the left sidebar, expand **Classifiers** then select **Sensitive info types**.

1. On the **Sensitive info types** page, select **+ Create sensitive info type** to start the sensitive information type configuration.

1. On the **Name your sensitive info type** page, enter:

    - **Name**: `Contoso Employee IDs`
    - **Description**: `Pattern for Contoso employee IDs.`

1. Select **Next**.

1. On the **Define patterns for this sensitive info type** page, select **Create pattern**.

1. On the **New pattern** flyout panel on the right, select **+ Add primary element** > **Regular expression**.

1. On the **+ Add a regular expression​** flyout panel on the right, enter:

   - **ID**: `Contoso IDs`
   - **Regular expression**: `[A-Z]{3}[0-9]{6}`
   - Select the radio button for _String match_.

1. Select **Done** at the bottom of the flyout panel.

1. Back on the **New pattern** flyout panel, under **Supporting elements**, select **+ Add supporting elements or group of elements** drop-down menu and select **Keyword list**.

1. On the **Add a keyword list** flyout panel on the right, enter:

   - **ID**: `Employee ID keywords`
   - **Case insensitive**:

      ```text
      Employee
      ID
      ```

   - Select the radio button for _Word match_

1. Select **Done** at the bottom of the flyout panel.

1. Back on the **New pattern** flyout panel, under **Character proximity**, decrease the **Detect primary AND supporting elements** value to `100` characters.

1. Select the **Create** button at the bottom of the flyout panel.

1. Back on the **Define patterns for this sensitive info type** page select **Next**.

1. On the **Choose the recommended confidence level to show in compliance policies** page use the default value and select **Next**.

1. On the **Review settings and finish** page review the settings and select **Create**. When successfully created select **Done**.

You have successfully created a new sensitive information type to identify employee IDs in the pattern of three uppercase characters, six numbers, and the keywords 'Employee' or 'IDs' within a range of 100 characters.

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
