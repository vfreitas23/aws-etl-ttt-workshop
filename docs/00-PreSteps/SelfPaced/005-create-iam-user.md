<h1 id="toc_0" align="center">
CREATE IAM USER
</h1>

Services in AWS, such as **AWS Glue**, require that you provide **credentials** when you access them, so that the service can determine whether you have permission to access its resources.

The console requires your password. You can create **access keys for your AWS account** to access the **command line interface or API**. However, we don't recommend that you access **AWS** using the credentials for your **AWS account**; we recommend that you use **AWS Identity and Access Management (IAM)** instead. Create an **IAM user**, and then add the user to an **IAM group** with **administrative permissions** or grant this user **administrative permissions**. You can then access **AWS using a special URL and the credentials for the IAM user.**

If you signed up for **AWS** but have not created an **IAM user** for yourself, you can create one using the **IAM console**. If you aren't familiar with using the console, see [Working with the AWS Management Console](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/learn-whats-new.html) for an overview.  

###To create an IAM user for yourself and add the user to an Administrators group:


**1\.** Use your **AWS account email address and password** to sign in as the **AWS account root user to the IAM console** at [IAM Console](https://console.aws.amazon.com/iam/)

> We strongly recommend that you adhere to the best practice of using the **Administrator IAM user** below and **securely lock away the root user credentials**.

**2\.** Sign in as the **root user** only to perform a **few account and service management tasks**.

**3\.** In the navigation pane of the console, choose **Users**, and then choose **Add user**.

**4\.** For **User name**, type **Administrator**.

**5\.** Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the **new user's password** in the text box. You can optionally select **Require password reset** to force the user to create a new password the next time the user signs in.

**6\.** Choose **Next**: **Permissions**.

**7\.** On the **Set permissions page**, choose **Add user to group**.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**7\****.1** Choose **Create group**.

**8\.** In the **Create group** dialog box, for **Group name** type **Administrators**.

**9\.** For **Filter policies**, select the check box for **AWS managed - job function**.

**10\.** In the policy list, select the check box for **AdministratorAccess**. Then choose **Create group**.

**11\.** Back in the list of groups, select the check box for your new group. Choose **Refresh** if necessary to see the group in the list.

**12\.** Choose **Next**: **Tags to add metadata** to the user by attaching tags as **key-value pairs**.

**13\.** Choose **Next**: Review to see the list of **group memberships** to be added to the new user. When you are ready to proceed, choose **Create user**.

**14\.** You can use this same process to create more **groups and users**, and to give your **users access to your AWS account resources**.

To sign in as this new **IAM user**, sign out of the **AWS console**, then use the following **URL**, where **your_aws_account_id** is your **AWS account number without the hyphens** *(for example, if your AWS account number is 1234-5678-9012, your AWS account ID is 123456789012)*:

[https://your_aws_account_id.signin.aws.amazon.com/console/](https://your_aws_account_id.signin.aws.amazon.com/console/)

Enter the **IAM user name** (not your email address) and **password** that you just created. When you're signed in, the navigation bar displays **"your\_user\_name @ your\_aws\_account\_id".**

If you don't want the **URL** for your sign-in page to contain your **AWS account ID**, you can create an **account alias**. From the **IAM console**, choose **Dashboard** in the navigation pane. From the dashboard, choose **Customize** and enter an **alias** such as your company name. To sign in after you create an **account alias**, use the following **URL**:

[https://your_account_alias.signin.aws.amazon.com/console/](https://your_account_alias.signin.aws.amazon.com/console/)

To verify the **sign-in link for IAM users** for your account, open the **IAM console** and check under **IAM users sign-in link** on the dashboard.
