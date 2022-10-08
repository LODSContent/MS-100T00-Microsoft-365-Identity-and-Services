# Learning Path 3 - Lab 2 - Exercise 1 - Manage Administration Delegation

In this exercise, you will continue in your role as Holly Dickson, Adatum's Enterprise Administrator. As part of Adatum's Microsoft 365 pilot project, you will manage administration delegation by assigning Microsoft 365 administrator roles to several of the Microsoft 365 user accounts that were created by your lab hosting provider. You will assign these roles using both the Microsoft 365 admin center and Windows PowerShell; this will give you the added experience of using PowerShell to perform these administrative functions. Once you have assigned Microsoft 365 admin roles to several of the existing user accounts, you will then test those assignments by verifying the users have the permissions to act in accordance with their roles. 

### ‎Task 1 - Assign Delegated Administrators in the Microsoft 365 Admin Center

As Holly Dickson, Adatum’s Enterprise Administrator and Microsoft 365 Global Admin, you will use the Microsoft 365 admin center to assign administrator rights to several users. 

1. If you’re not logged into LON-DC1 as **ADATUM\Administrator** and password **Pa55w.rd**, then please do so now.

2. In the **Microsoft 365 admin center** in your Edge browser, you should still be logged in as Holly Dickson. In the navigation pane, select **Users** and then select **Active Users**. 

3. In the **Active users** list, select **Diego Siciliani**.  <br/>

	**Note:** Select Diego’s name; do not select the checkbox to the left of his name. The box with the check mark is typically used for selecting multiple users when you want to perform one of the user-related actions on the menu bar that appears above the list of users, such as **Manage product licenses** and **Manage roles**. Selecting a user’s name opens a property pane specifically for that user.

4. In the **Diego Siciliani** pane that appears, the **Account** tab is displayed by default. In this tab, scroll down to the **Roles** section and select **Manage roles**. 

5. In the **Manage roles** window, the **User (no admin center access)** option is currently selected by default. Now that you want to assign Diego an administrator role, select the **Admin center access** option. This enables the list of common admin roles for selection. 

6. Diego has been promoted to Billing Administrator, but since this role does not appear in the list of commonly used roles, scroll down and select **Show all by category**. 

7. In the list of roles that are sorted by category, scroll down to the **Other** category, select **Billing Administrator**, and then select **Save changes**. 

8. On the **Manage roles** window, select the **X** in the upper-right corner of the screen to close it. This returns you to the **Active users** list. 

9. In the **Active users** list, select **Lynne Robbins**. 

10. In the **Lynne Robbins** pane that appears, the **Account** tab is displayed by default. In this tab, scroll down to the **Roles** section and select **Manage roles**. 

11. In the **Manage roles** window, select the **Admin center access** option. This enables the list of common admin roles for selection. 
 
12. In the list of common admin roles, select the **User Administrator** role and then select **Save changes**.

13. Remain logged into LON-DC1 and the Microsoft 365 admin center as Holly Dickson.


### Task 2 - Assign Delegated Administrators with Windows PowerShell  

This task is similar to the prior one in that you will assign administrator rights to users; however, in this case, you will use Windows PowerShell to perform this function rather than the Microsoft 365 Admin Center. This will give you experience performing this management function in PowerShell, since some administrators prefer performing maintenance such as this using PowerShell. 

To add a user to an admin role using the Azure Active Directory PowerShell for Graph module (AzureAD), you must first obtain the Object ID of the user and the Object ID of the role. If the role has not yet been enabled (meaning that it hasn't been assigned to a user or it hasn't been physically enabled), then you must enable the role first before you can assign it to a user using PowerShell. In this task, you will enable the Service Support Administrator role first before assigning it to Patti Fernandez.

PowerShell also enables you to display all the users assigned to a specific role, which can be very important when auditing your Microsoft 365 deployment. In this task, you will learn how to use PowerShell to display all the users assigned to a specific role. 

1. On LON-DC1, select the Windows PowerShell icon on the taskbar that you left open from the previous lab. If you closed the PowerShell window, then open an elevated instance of it using the same instruction as before. 

2. Your PowerShell session should still be connected to the Azure Active Directory PowerShell for Graph module (AzureAD) from the prior lab. However, if you previously closed PowerShell and just repopened it, then connect to the AzureAD modle using the steps from the prior lab exercise. 

3. PowerShell's execution policy settings dictate what PowerShell scripts can be run on a Windows system. Setting this policy to **Unrestricted** enables Holly to load all configuration files and run all scripts. At the command prompt, type the following command, and then press Enter:   <br/>

		Set-ExecutionPolicy unrestricted

	‎If you are prompted to verify that you want to change the execution policy, enter **A** to select **[A] Yes to All.** 

4. Holly wants to assign **Patti Fernandez** to the **Service Support Administrator** role. To assign a role using the AzureAD PowerShell module, you must first obtain the Object ID of the user and the Object ID of the role. <br/>

	To obtain Patti's Object ID, type the following command and then press Enter: <br/>

		Get-AzureADUser -ObjectID "PattiF@xxxxxZZZZZZ.onmicrosoft.com" (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider)

5. You must now obtain the ObjectID of the Service Support Administrator role so that you can assign it to Patti using the role's ObjectID. However, in the AzureAD PowerShell module, you can only assign roles that have been "enabled". Enabled roles are roles that were either enabled from a role template, or they're roles that have already been assigned to users through PowerShell or the Microsoft 365 admin center. To view all the enabled roles in Microsoft 365, enter the following command at the command prompt and then press Enter: <br/>
	
		Get-AzureADDirectoryRole

	**Note:** This command displays the three roles that have been enabled thus far in Microsoft 365 - the Global admin, the User admin, and the Billing admin. These roles were enabled when you manually assigned them to users in the Microsoft 365 admin center in prior labs. If the Service Support Administrator role appeared in this list, you could proceed directly to step 10 to assign the role to Patti.  <br/>

	However, since the Service Support Administrator is not included in this list of enabled roles, you must perform steps 6-9 to enable the role before you can assign it to Patti in step 10.

6. To enable a role in the AzureAD PowerShell module, you must first locate the role template to verify its Display Name. You need to know the exact spelling of the role template in order to assign it to the role. To view the list of role templates, type in the following command and then press Enter: <br/>

		Get-AzureADDirectoryRoleTemplate
	
7. This command displayed the templates for all the possible Microsoft 365 roles. In the list of role templates, locate the template record for the **Service Support Administrator** role. Verify you have the correct spelling of the role (Note: the word Administrator must be spelled out. It can't be abbreviated to "admin" as we often times do in our documentation). <br/>

	You should then retrieve the role template object for the Service Support Administrator role. To do so, type in the following command, which retrieves this template in the **$ServiceSupportRole** variable, and then press Enter:  <br/>

		$ServiceSupportRole = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.DisplayName -eq "Service Support Administrator"}

8. You now want to verify that you captured the correct role template by verifying the contents of the $ServiceSupportRole variable. To do so, type the following command and press Enter: <br/>

		$ServiceSupportRole

9. You're now ready to enable the Service Support Administrator role by basing it on its predefined template, which is stored in the $ServiceSupportRole variable. To do so, type the following command and press Enter: <br/>

		Enable-AzureADDirectoryRole -RoleTemplateId $ServiceSupportRole.ObjectId

	**Note:** This command enables the Service Support Administrator role and displays the role's ObjectID. You will need to copy and paste in this ObjectID in the next command, along with the ObjectID of Patti's user account.

10. Now that you know the ObjectID of the recently enabled Service Support Administrator role and the ObjectID of Patti's user account, you can assign the role to Patti. Here's the format of the command that you will eventually run: <br/>

	**Important:** Do NOT perform the following command just yet – this is an informational step whose purpose is to describe what you will be doing later in this step.  <br/>
	
	Add-AzureADDirectoryRoleMember -ObjectID "paste in the role's ObjectID here" -RefObjectId "paste in Patti's object ID here" <br/>

	To run this Add-AzureADDirectoryRoleMember command, you must copy  the ObjectID of the Service Support Administrator role (not the ObjectID of the template, but the ObjectID of the role you enabled, which was displayed when you ran step 9) and paste it in the command following the **-ObjectID** parameter. <br/>

	You should then copy the ObjectID of Patti's user account (which was displayed when you ran step 4) and paste it in the command following the **-RefObjectID** parameter. **NOTE: Both ObjectID values must appear within quotations marks.**  <br/>

	For example (these are NOT actual ObjectID values - this example is simply displayed here to give you an idea as to what the command should look like): <br/>

	Add-AzureADDirectoryRoleMember -ObjectID "03618579-3c16-4765-9539-86d9163ee3d9" -RefObjectId "a4a9ed46-369c-4b69-9e47-d2ac6029485d" <br/>

	**Note:** To copy an ObjectID, highlight the ObjectID and select Ctrl+C to copy it. Then place your cursor in the appropriate spot in the command and press Ctrl+V to paste it. <br/>
	
	Now that you know where each ObjectID goes, type in the following command, copy each ObjectID and paste it within quotation marks in its appropriate location within the command, and then press Enter: <br/>

		Add-AzureADDirectoryRoleMember -ObjectID "paste in the role's ObjectID here" -RefObjectId "paste in Patti's object ID here"

11. You now want to verify that Patti has been assigned to the Service Support Administrator role. Displaying the users assigned to a role is a two-step process in PowerShell. You will begin by creating a macro command ($role) that states that anytime $role is used in a cmdlet, it should retrieve all users assigned to whichever role name you are validating.  <br/>

	You will create this $role variable for the Service Support Administrator role. To do so, type the following command and then press Enter:  <br/>

		$role = Get-AzureADDirectoryRole | Where-Object {$_.DisplayName -eq "Service Support Administrator"}
		
12. After creating the macro in the prior step, you will then run the following command that directs PowerShell to display all object IDs for the users who have been assigned to the name of the role that you invoked in the previous $role macro.  <br/>
	
		Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
				
13. Verify that **Patti Fernandez** is in the list of users who have been assigned the **Service Support Administrator** role. As you can see, Patti is the only user assigned to the role. Let's now repeat this process to see all the users assigned to the Global admin role.

14. Repeat steps 11-12 to verify which Adatum users have been assigned to the **Global Administrator** role.  <br/>

15. Verify that there are multiple user accounts that have been assigned the Global Administrator role. In a real world scenario, you would use these two PowerShell commands to monitor how many global admins exist in your Microsoft 365 deployment. You would then remove the Global Administrator role from any users who truly shouldn't have it (remember, the best practice guideline is to have between 2 to 4 global admins in a Microsoft 365 deployment - depending on the size of the organization).  <br/>

	In the case of this lab, while your lab hosting provider assigned the Global Administrator role to users other than the MOD Administrator (and you assigned it to Holly Dickson), you'll leave these users as is. In this fictitious Adatum deployment, there's no point in wasting your time removing this role from their accounts. Plus, some of the future lab tasks are based on these users being assigned the Global Administrator role. <br/>

	**Important:** Just remember that in your real world deployments, the Enterprise Administrator should monitor the Global Administrator role on a periodic basis to keep the number of assigned users between 2 and 4.
	
16. Leave your Windows PowerShell session open for future lab exercises; simply minimize it before going on to the next task.


### Task 3 - Verify Delegated Administration  

In this task, you will begin by examining the administrative properties of two users, Joni Sherman and Lynne Robbins. You will then log into the Microsoft 365 home page on the Client 1 VM (LON-CL1) as each user to confirm several of the changes that you made when managing their administrative delegation in the prior tasks. Finally, as Lynne Robbins, you will perform several user account maintenance tasks, such as resetting passwords and blocking a user account.

**Password Note:** When logging into Microsoft 365 as any of the existing user accounts that were created for you in the Microsoft 365 trial tenant by your lab hosting provider (for example, Joni Sherman, Lynne Robbins, and so on), you must use the same Tenant Password that you used in Lab 1 when you signed in using the MOD Administrator account to set up your organization profile. All of the existing Microsoft 365 user accounts in your tenant have been assigned this same Tenant Password, which your instructor will provide for you. Only Holly Dickson has a different password, since you entered **User.pw1** as Holly's password when you created her user account.

1. In LON-DC1, you should still be logged into the Microsoft 365 admin center as Holly Dickson. If not, then do so now.

2. In the **Microsoft 365 admin center**, if you are not displaying the **Active Users**, then navigate to there now.  

3. In the **Active users** list, select **Joni Sherman**. 

4. In **Joni Sherman's** properties window, the **Account** tab is displayed by default. Under the **Roles** section, it should indicate that Joni has **No administrator access**. Select the **X** in the upper right corner to close Joni's properties window.

5. In the **Active users** list, select **Lynne Robbins**. 

6. In **Lynne Robbins's** properties window, it should indicate that Lynne has been assigned the **User Administrator** role. Close Lynne's properties window.

7. In your VM lab environment, switch to the Client 1 VM (**LON-CL1**).

8. On the log-in screen, you will log in as the **Administrator** account with a password of **Pa55w.rd**.

9. If a **Networks** window appears, select **Yes**.

10. On the taskbar, select the **Microsoft Edge** icon. Maximize your Edge browser window if necessary.

11. In your **Edge** browser navigate to **https://portal.office.com**. 

12. You will begin by signing into Microsoft 365 as **Joni Sherman**. In the **Sign-in** window, enter **JoniS@xxxxxZZZZZZ.onmicrosoft.com** (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider). In the **Enter password** window, enter the Tenant Password provided by your instructor (this is the same password used by the MOD Administrator account). If you are signed in to another account, sign out and sign back in using **Joni Sherman's** credentials .

13. On the **Stay signed in?** window, select the **Don't show this again** check box and then select **Yes**.

14. If a **Welcome to your new Office, Joni** window appears, select the right-arrow (>) three times to close it.

15. In the **Find more apps** window that appears, select the **X** in the upper right-hand corner of the window to close it.

16. On the **Microsoft Office Home** page, select the **App launcher** icon (the square made up of 3 rows of dots) that appears above the **Home** icon in the top left corner of the screen. In the **Apps** pane that appears, note how the **Admin** option is not available in the list of apps. This is due to the fact that Joni was never assigned an administrator role. 

17. You will now sign out of Microsoft 365 as Joni. In **Microsoft Edge**, at the top right of the **Office 365 home** page, select the user icon for **Joni Sherman** (the circle in the upper right-hand corner with Joni's picture in it), and in the **Joni Sherman** window that appears, select **Sign out.** In the **Pick and account** window, you want to specify which account you want to sign out of. Select **Joni Sherman**.  

18. You will now sign into Microsoft 365 as **Lynne Robbins**. In your current **Edge** browser tab, it should display a message indicating **Joni, you're signed out now**. In this window, it gives you the option of signing back in as Joni, or signing in as a different user. Select **Switch to a different account**, and in the **Email address** field that appears, enter **LynneR@xxxxxZZZZZZ.onmicrosoft.com** (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider) and then select **Sign in**. In the **Enter password** window, enter the Tenant Password provided by your instructor.

19. If a **Welcome to your new Office, Lynne** window appears, select the right-arrow (>) three times to close it.

20. In the **Find more apps** window that appears, select the **X** in the upper right-hand corner of the window to close it.

21. On the **Microsoft Office Home** page, select the **App launcher** icon (the square made up of 3 rows of dots) that appears above the **Home** icon in the top left corner of the screen. In the **Apps** pane that appears, note how the **Admin** icon appears; this is because Lynne was assigned to a Microsoft 365 administrator role. Select the **Admin** icon to open the Microsoft 365 admin center.

22. In the **Microsoft 365 admin center**, select **Users** on the navigation pane and then select **Active users**. 

23. As the **User Administrator**, Lynne has permission to change user passwords. Lynne was recently contacted by **Diego Siciliani** and **Joni Sherman**, who each reported that their passwords may have been compromised. Per Adatum's company policy, Lynne must reset their passwords to a temporary value, and then force them to reset their password at their next login.   <br/>

	‎In the **Active users** list, as you move your mouse from one user account to another, notice the **key (Reset a password)** icon that appears to the right of each user's name. Select the key icon that appears to the right of **Diego Siciliani's** name.

24. In the **Reset password** window for Diego, if the **Automatically create a password** check box displays a check mark, then select this box to clear it. This will enable Lynne to manually assign Diego a temporary password. Enter **diego** in the **Password** field. Note to the right of the password, the system displays a message indicating this is a **Weak** password. Also note the message that appears below the field indicating the requirements for a strong password. Finally, note how the **Reset password** button at the bottom of the pane is not enabled; **this button will only be enabled once you enter a strong password**.  <br/>

	To correct this situation, enter **User.pw1** in the **Password** field. Note how **Strong** now appears next to this password, and the **Reset password** button at the bottom of the pane is now enabled. <br/>
	
	**Note:** This is just a temporary password because Lynne wants to force Diego to change it the next time he logs in. Therefore, verify the **Require this user to change their password when they first sign in** check box displays a check mark; if the box is clear, then select it so that it displays a check mark.

25. Select **Reset password**.

26. If prompted to save password, select **never** to close the window.

27. You should receive an error message indicating that you cannot reset Diego’s password because he has been assigned an admin role. In Diego’s case, he was assigned to the Billing Administrator role. Since only Global admins can change another admin’s password, and because Lynne is not a Global admin, she will have to ask Holly Dickson to make this change. Select **Close**. 

28. If a survey request window appears, select **Cancel**.

29. In the **Active users** list, select the **key (Reset a password)** icon for **Joni Sherman**. 

30. In the **Reset password** window for Joni, if the **Automatically create a password** check box displays a check mark, then select this box to clear it. Lynne wants to manually assign Joni a temporary password, so enter **User.pw1** in the **Password** field.  <br/>

	This is just a temporary password because Lynne wants to force Joni to change it the next time she logs in. Therefore, verify the **Require this user to change their password when they first sign in** check box displays a check mark; if the box is clear, then select it so that it displays a check mark.
	
31. Select the **Reset password** button.

32. On the **Password has been reset** window, you should receive a message indicating that you successfully reset the password for Joni. Select **Close**.

33. Management has recently discovered that Alex Wilber's username may have been compromised. As a result, Lynne has been asked to block Alex's account so that no one can sign in with his username until management is able to determine the extent of the issue. In the **Active users** list, select the check box to the left of **Alex Wilber's** name (do NOT select Alex’s name itself).  <br/>

	**Important:** Since you are going to run a global command on Alex's account rather than a command associated with his account, you only want Alex's account selected in the list of active users. If any other user account is selected, you must unselect that user account before proceeding. Examine Joni Sherman's account, since you just reset her password. If the check box to the left of **Joni Sherman** is selected, then select this check box to unselect Joni's account. **Only Alex Wilber's account should be selected.**

34. In the menu bar at the top of the page, select the **ellipsis icon (...)** to display a drop-down menu of additional options. In the menu that appears, select **Edit sign-in status**.

35. In the **Block sign-in** pane that appears, verify Alex's email address appears below the **Block sign-in** heading. Select the **Block this user from signing in** check box, and then select **Save changes.** 

36. The **Block sign-in** window should display a message indicating that Alex is now blocked from signing in (and no one can sign in with Alex's username in the event that his username was actually compromised). In addition, Alex will automatically be signed out of Microsoft services within 60 minutes. Select the **X** in the upper right-hand corner of the pane to close it. 

37. Lynne has just been informed that **Nestor Wilke's** username has also been potentially compromised. Repeat steps 30 through 33 to block Nestor from signing in (and to block anyone else from using his username to sign in). 

38. When you tried to block Nestor's sign in, you should have received an error message indicating **Changes could not be saved**. The reason that you received this error is that Nestor is a Global Administrator, and Lynne is not. Only a Global Administrator can block another Global Admin from being able to sign in. Lynne will need to ask Holly Dickson to make this change. <br/>

	Close the **Block sign-in** pane.

39. You previously blocked Alex Wilber from being able to sign in. To verify whether he is blocked, you will attempt to sign in as Alex. Log out of Microsoft 365 by selecting the user icon for **Lynne Robbins** (the circle with Lynne's picture in the upper right-hand corner), and in the **Lynne Robbins** window that appears, select **Sign out.** 

40. As a best practice, close all your browser tabs except for the **Sign out** tab once you have been signed out. On the **Sign out** tab, navigate to **https://portal.office.com**. 

41. In the **Pick an account** window, select **Use another account**. In the **Sign in** window, enter **AlexW@xxxxxZZZZZZ.onmicrosoft.com** (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider). In the **Enter password** window, enter the Tenant Password provided by your instructor.  <br/>

	The **Pick an account** window should appear, and it should display an error message indicating **Your account has been locked. Contact your support person to unlock it, then try again.** You have just verified that Alex (or someone who has obtained Alex's username and password) cannot log in. <br/>

	**Note:** It can take a few minutes for the account block to fully implement. Until that time Alex may be able to log on, but none of the Microsoft 365 services are available to the user, and will not appear in the portal. Once the account is unblocked, the services will become available again. <br/>

	If you are able to sign in as Alex, sign back out and wait a few minutes. Then attempt to sign back in as Alex. By this time, you should hopefully receive the error message that indicates Alex's account has been blocked from signing in. 
	
42. Switch back to LON-DC1, where you should still be logged into **Microsoft 365** as Holly Dickson. The **Active users** list should be displayed in the **Microsoft 365 admin center** from earlier in this task. 

43. Upon further investigation, Adatum's CTO has determined that Alex Wilber's account has, in fact, not been compromised; therefore, the CTO has asked Holly to remove the block on Alex's sign in. Repeat steps 30 through 33 to unblock his account. Note how the **Block sign-in** window from step 32 now displays the **Unblock sign-in** window instead.  <br/>

	In the **Unblock sign-in** window, the **Block this user from signing in** check box is currently selected. Select this check box to clear it, and then select **Save changes**. <br/>
	
	**Note:** A warning message is displayed indicating it can take up to 15 minutes before Alex can sign in again. As such, you will **NOT** try to log back in as Alex on LON-CL1. Instead, remain on LON-DC1 and simply close the **Unblock sign-in** window.
	
44. On LON-DC1, leave your browser and all tabs open and proceed to the next exercise. 


# Proceed to Lab 2 - Exercise 2

