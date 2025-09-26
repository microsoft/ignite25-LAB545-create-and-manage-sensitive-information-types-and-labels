## Bonus Task – Create a DLP policy in simulation mode

In this task, you'll create a DLP policy in simulation mode that targets credit card numbers in Teams messages. The policy will notify users when they attempt to share sensitive content and allow them to override with justification.

1. In the Microsoft Purview portal, navigate to **Solutions** > **Data Loss Prevention** > **Policies**.

1. On the **Policies** page, select **+ Create policy** to start the configuration for creating a new data loss prevention policy.

1. On the **Choose what type of data to protect** page, select **Data stored in connected sources**, then select **Next**.

1. On the **Start with a template or create a custom policy** page, select **Custom** as the category, then select **Custom policy** under **Regulations**.

1. Select **Next**.

1. On the **Name your DLP policy** page enter:

   - **Name**: `Block with override project data in Copilot`
   - **Description**: `Detects Contoso project codes in Copilot prompts and blocks sharing, with override option.`

1. Select **Next**.

1. On the **Assign admin units** page select **Next**.

1. On the **Choose locations to apply the policy** page, enable the location for **Microsoft 365 Copilot** only. If any other locations are selected, deselect them.

1. Select **Next**.

1. On the **Define policy settings** page, select **Create or customize advanced DLP rules**, then select **Next**.

1. On the **Customize advanced DLP rules** page, select **+ Create rule**.

1. In the **Create rule** flyout:
    - In the **Name** field, enter `Block project data in Copilot`.

1. Under **Conditions**, select **+ Add condition** > **Content content contains**.

1. In the **Content contains** section, select **Add** > **Sensitivity labels**.








1. On the **Define policy settings** page, select **Create or customize advanced DLP rules**, then select **Next**.

1. On the **Customize advanced DLP rules** page, select **+ Create rule**.

1. In the **Create rule** flyout:
    - In the **Name** field, enter `Credit card information`.

1. Under **Conditions**, select **+ Add condition** > **Content is shared from Microsoft 365**.

1. In the **Content is shared from Microsoft 365** section:
    - Select the option for **with people outside my organization**.

1. Select **+ Add condition** > **Content contains**.

1. In the new **Content contains** section:
    - Select **Add** > **Sensitive info types**.
    - On the **Sensitive info types** page, search for and select `Credit Card Number`, then select **Add**.

1. Under **Actions**, select **+ Add an action** > **Restrict access or encrypt the content in Microsoft 365 locations**.

1. In the **Restrict access or encrypt the content** section:
    - Select **Block only people outside your organization**.

1. Under **User notifications**:
    - Turn on the toggle for **Use notifications to inform your users and help educate them on the proper use of sensitive info.**.
    - Select the checkbox for **Notify users in Office 365 service with a policy tip**.

1. Under **User overrides**:
    - Select the checkbox for **Allow users to override policy restrictions** (Fabric, Exchange, SharePoint, OneDrive, and Teams).
    - Select the checkbox for **Require a business justification to override**.

1. Under **Incident reports**, in the **Use this severity level in admin alerts and reports** dropdown:
    - Select **Low**.

1. At the bottom of the **Create rule** flyout panel, select **Save**.

1. Back on the **Customize advanced DLP rules**, select **Next**.

1. On the **Policy mode** page select **Run the policy in simulation mode** and select the checkbox for **Show policy tips while in simulation mode**.

1. Select **Next**.

1. On the **Review and finish** page review your settings then select **Submit**.

1. On the **New policy created** page select **Done**.

You've created a DLP policy that scans Teams content for credit card numbers and allows overrides with business justification.
