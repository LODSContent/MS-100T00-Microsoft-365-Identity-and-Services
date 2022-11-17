# Learning Path 3 - Lab 2 - Exercise 3 - Manage a Microsoft 365 Apps for enterprise installation

You have taken on the persona of Holly Dickson, Adatum's Enterprise Administrator, and you have Microsoft 365 deployed in a virtualized lab environment. In this exercise, you will perform the tasks necessary to manage a user-driven Microsoft 365 Apps for enterprise installation. Performing a user-driven Microsoft 365 Apps for enterprise installation is a two-step process: 1) configuring the user account so the user is eligible to download and install the Office 365 deployment tool, and 2) performing the installation. 

In the first two tasks in this exercise, you will verify the following conditions that affect whether a user can be blocked from downloading the Microsoft 365 Apps for enterprise suite: <br/>

- The user does not have an appropriate Office 365 license (which you will verify in Task 1). 
	
- An admin turns off the global Office download setting that controls the downloading of mobile and desktop apps for all users (which you will verify in Task 2).

In the final task in this exercise, you will install the Microsoft 365 Apps for enterprise suite for one of Adatum's users.


### Task 1 – Verify how licensing affects installing Microsoft 365 Apps for enterprise

In this task, Holly will test whether a user who has not been assigned an appropriate Microsoft 365 license can download Microsoft 365 Apps for enterprise. For this test, you can't use any of the existing users that appear in the **Active Users** list in the Microsoft 365 admin center. These users only have Microsoft 365 accounts (xxxxxZZZZZZ.onmicrosoft.com accounts); they do not have corresponding on-premises accounts in the adatum domain. Without an on-premises account, you can't log into the Client 1 (LON-CL1) VM as any of these users to install Microsoft 365 Apps for enterprise on the client machine. 

Therefore, you must use one of Adatum's on-premises user accounts that has been loaded in its on-premises domain (adatum.com) by your lab hosting provider. For this test, you will use **Laura Atkins**. You will create a Microsoft 365 account for Laura, but you will initially not assign her a Microsoft 365 license. This will enable you to see how not having a license affects a user's ability to install Microsoft 365 Apps for enterprise. 

1. You should still be logged into LON-DC1 as **Administrator** and password **Pa55w.rd**. 

2. The **Microsoft 365 admin center** should still be open in your Edge browser from the prior lab, where you should be logged into Microsoft 365 as Holly Dickson. In the navigation pane, select **Users** and then select **Active users**. 

3. You will begin by testing whether a user **without** an appropriate Microsoft 365 license can install Microsoft 365 Apps for enterprise. For this test, you will use **Laura Atkins**. Your lab hosting provider has already created an on-premises user account for Laura, but she does not have a Microsoft 365 user account. You will create a Microsoft 365 account for Laura, but you will NOT assign her a Microsoft 365 license.  <br/>

	At the top of the **Active users** window, select **Add a user** on the menu bar.

4. In the **Set up the basics** window, enter the following information:
	- First name: **Laura**
	- Last name: **Atkins** 
	- Display name: When you tab into this field, Laura Atkins will appear.
	- Username: **Laura**

	**IMPORTANT:** To the right of the Username field is the domain field. You want this value to be Adatum's **xxxxxZZZZZZ.onmicrosoft.com** domain (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider). However, if the custom domain that you added in a prior lab is set as the default domain, then this field will be prefilled with the custom **xxxUPNxxx.xxxCustomDomainxxx.xxx** on-premises domain (where xxxUPNxxx is your UPN number and xxxCustomDomainxxx.xxx is the custom domain). If the custom domain is displayed in this field, you must select the drop-down arrow and select the **xxxxxZZZZZZ.onmicrosoft.com** cloud domain instead.  <br/>

	After configuring this field, Laura’s **Username** should appear as: **Laura@xxxxxZZZZZZ.onmicrosoft.com**

	- Password settings: Clear (uncheck) the **Automatic create a password** option
	- Password: **User.pw1** 
	- Clear (uncheck) the **Require this user to change their password when they first sign in** check box 
	
5. Select **Next**.

6. In the **Assign product licenses** window, select the **Create user without product license (not recommended)** option, and then select **Next**.

7. In the **Optional settings** window, select **Next**. 

8. On the **Review and finish** window, review your selections. If anything needs to be changed, select the appropriate **Edit** link and make the necessary changes. Otherwise, if everything looks good, select **Finish adding**. 

9. On the **Laura Atkins added to active users** page, select **Close**. If a survey form appears, select **Cancel**. 

10. Switch to the Client 1 VM (**LON-CL1**). 

11. You want to log in as **Laura Atkins**. If the Edge browser is still open from the previous lab exercise, then close it now. You should be on the LON-CL1's desktop, where it should indicate that you are logged on as **adatum\administrator**. Since you want to log on as Laura Atkins, select the **Ctrl+Alt+Delete** function for your VM environment. On the menu screen that appears, select **Switch user**. <br/>

	The lower-left portion of the desktop displays the **Administrator** and **Other user** options. Select **Other user**.

12. In the **Other user** log in, enter **adatum\laura** in the **Username** field, enter **Pa55w.rd** as the **Password**, and then select the forward arrow to log in. After logging in, the desktop should indicate the logged on user is **adatum\laura**. 

13. Select the **Microsoft Edge** icon on the taskbar.

14. In **Microsoft Edge**, maximize your browser if necessary. If you receive a **Welcome to Microsoft Edge** diaglog box, select the **Start without your data** button on the first screen, and then on the second page, unselect (clear) the **Make your Microsoft experience more useful to you** check box and select the **Confirm and start browsing** button.  <br/>

	On the Edge browser, go to the **Microsoft Office Home** page by entering the following URL in the address bar: **https://portal.office.com/**

15. In the **Sign in** window, enter **Laura@xxxxxZZZZZZ.onmicrosoft.com** (where xxxxZZZZZZ is the tenant prefix provided by your lab hosting provider) and then select **Next**.

16. In the **Enter password** window, enter **User.pw1** and then select **Sign in.**

17. In the **Stay signed in?** window, select the **Don't show this again** check box and then select **Yes.**

18. In the **Microsoft Office Home** page for Laura, notice that no column of Microsoft 365 app icons appears in the navigation pane on the left-side of the screen; this is because Laura does not have an Office 365 license assigned. <br/>

	Select the **Install Office** button, and then in the drop-down menu that appears, select **Other install options**. This opens the **My account** window for Laura.

19. In Laura's **My account** window, under the **Office apps &amp; devices** tile, select **View apps &amp; devices**. Note the message that appears at the top of page. Laura has not been assigned a license that includes the Office desktop apps, so she’s unable to install Microsoft 365 Apps for enterprise. <br/>
	
	‎**Important:** You have just verified that a user can't download Microsoft 365 Apps for enterprise if they haven't been assigned an appropriate Microsoft 365 license. 
	
20. Leave LON-CL1 open and remain signed into Microsoft 365 as Laura Atkins for the next task. Close the **My account** tab, but leave the **Microsoft Office Home** tab open for the next task.


### Task 2 – Verify how the global Office download setting affects installing Microsoft 365 Apps for enterprise

Microsoft 365 includes a global Office download setting that controls the downloading of mobile and desktop apps for all users. Holly is now going to test whether users can be prohibited from downloading Microsoft 365 Apps for enterprise if an admin turns off this setting. In this test, Holly will once again use Laura Atkins as her test case. However, since you just proved in the prior task that Laura can't install Microsoft 365 Apps for enterprise without a proper license, you must first assign her a license. 
	
1. Switch back to **LON-DC1**. You should still be logged into Microsoft 365 as Holly Dickson, Adatum’s Enterprise Administrator.

2. To turn off the global Office download setting, select the **Microsoft 365 admin center** tab in your browser, and then if necessary, select **...Show all** in the navigation pane. Select **Settings**, and then within the group, select **Org Settings**. 

3. In the **Org settings** window, the **Services** tab is displayed by default. Scroll down through the list of services and select **Office installation options**.

4. In the **Office installation options** pane that appears, the **Feature Updates** tab is displayed by default. Select the **Installation** tab that appears next to it. Then under the **Apps for Windows and mobile devices** section, the **Office (includes Skype for Business)** check box is currently selected. Select this check box to clear it. This turns this feature **Off**, which will prohibit users from downloading Microsoft 365 Apps for enterprise. 

5. Select **Save**. 

6. At the top of the **Office installation options** pane, select the **X** in the upper-right corner of this window to close it. 

7. You should now test whether turning off this global download setting affects a **licensed** user from installing Microsoft 365 Apps for enterprise. In this case, you’re once again going to use **Laura Atkins**, so you must first assign Laura a Microsoft 365 license.  <br/>
	
	In the **Microsoft 365 admin center**, under the **Users** group in the navigation pane, select **Active users**.
	
8. In the **Active users** list, scroll down to **Laura Atkins**. The value in the **Licenses** column for Laura currently indicates that she is **Unlicensed**. Select **Laura Atkins'** display name (do not select the check mark to the left of her name like you did for Adele) to open Laura's properties window.

9. In **Laura Atkins** account pane, the **Account** tab is displayed by default. Select the **Licenses and apps** tab. In the **Licenses** section, select the **Office 365 E5** check box and then select **Save changes**. You can then close Laura’s account pane. In the **Active users** list, note how the value in the **Licenses** column for Laura now displays **Office 365 E5**.

10. You should now check whether Laura can download Microsoft 365 Apps for enterprise on to her client PC when the global Office download setting has been turned Off. <br/>

	To do this, you must first switch back to **LON-CL1**.

11. In **LON-CL1**, your Edge browser should still be open, and you should still be logged into Microsoft 365 as Laura Atkins. In your browser, verify you're on the **Microsoft Office Home** tab. When you left off after the prior lab task, this page didn't display any Microsoft 365 apps in the navigation pane on the left because Laura wasn't assigned an Office 365 license. Let's see what happens now that Laura has been assigned a license. <br/>

	Select the **Refresh icon** that appears to the left of the address bar at the top of your browser. <br/>

	After refreshing the page, notice that the column of Microsoft 365 app icons now appears on the left-side of the screen because Laura has been assigned an Office 365 license. 

12. Select the **Install Office** button, and then in the drop-down menu, select **Other install options**.
	
13. This will open Laura's **My account** window. Under the **Office apps &amp; devices** tile, select **View apps &amp; devices**. 

14. In the **Apps &amp; devices** window, a message is displayed under the **Office** section that indicates the admin has turned off Office installs. <br/>
	
	‎**Important:** You have just verified that a licensed user is unable to download Microsoft 365 Apps for enterprise if the global Office download setting has been turned Off.

15. At this point Holly wants to turn the global Office download setting back On so that Laura can download Microsoft 365 Apps for enterprise. <br/>

	To do this, switch back to **LON-DC1**. 

16. On **LON-DC1**, you should still be logged into Microsoft 365 as Holly Dickson. In the **Microsoft 365 admin center**, under the **Settings** section in the navigation pane, select **Org Settings**. 

17. In the **Org settings** window, the **Services** tab is displayed by default. Scroll down through the list of services and select **Office installation options**.

18. In the **Office installation options** pane, select the **Installation** tab, then under the **Apps for Windows and mobile devices** section, the **Office (includes Skype for Business)** check box is currently blank. Select this check box so that it displays a check mark, which now turns this feature back On.

19. Select **Save**, then select the **X** in the upper-right corner of this window to close it. 

20. Now that this global Office download option is turned back On, you should see if it affects Laura’s ability to download Microsoft 365 Apps for enterprise. <br/>

	To do this, switch back to **LON-CL1**.

21. On **LON-CL1**, Laura's Edge browser should still be open, and the **Office apps and devices** page should be displayed along with the message that indicated your admin has turned off Office installs. Since you just turned this global option back On, you need to refresh this page to see how it affects Laura’s ability to download Microsoft 365 Apps for enterprise. <br/>

	Select the **Refresh icon** that appears to the left of the address bar at the top of your browser. 

22. In the **My account** window that appears, under the **Office apps &amp; devices** tile, an **Install Office** button appears along with a message indicating you can install Office on up to 5 PCs or Macs, 5 tablets, and 5 smartphones.  <br/>
	
	‎**Important:** You have just verified that a user with an Office license is able to download Microsoft 365 Apps for enterprise if the global Office download setting is turned On. Do NOT select the **Install Office** button at this time. You will do that in the next task.

23. Remain on LON-CL1 and continue to the next task to perform the user-driven installation for Laura Atkins.


### Task 3 – Perform a User-Driven Installation of Microsoft 365 Apps for enterprise 

In the prior task, you logged into Laura Atkins’ client PC, and you verified that she could download Microsoft 365 Apps for enterprise once she was assigned an Office 365 license and the global Office download setting was turned On. In this task, you will continue the process by having Laura perform a user-driven installation of the Microsoft 365 Apps for enterprise suite from the Microsoft 365 portal.  

1. On **LON-CL1**, you should still be logged in as Laura Atkins. 

2. You should still be in Laura’s **My account** window since this is where you left off at the end of the prior task. Under the **Office apps &amp; devices** section, the **Install Office** button now appears since Laura is assigned an Office 365 E5 license and the global Office download setting is turned On.<br/>

	‎**Important:** Selecting this **Install Office** button will install the 64 bit, English version of Microsoft 365 Apps for enterprise. However, if you want to install a different language or version, then select **View apps &amp; devices**, which opens the **Apps &amp; devices** page; this enables you to select a different language and version of Microsoft 365 Apps for enterprise to install.  <br/>

	Since Laura wants to install the 64-bit English version of Microsoft 365 Apps for enterprise, select the **Install Office** button now.
		
3. If a **Just a few more steps** window appears, select **Close**.

4. In the **Downloads** window that appears at the top right-side of the page, notice the system is downloading the **OfficeSetup.exe** installation program to the LON-CL1 client PC. This is the 64-bit Microsoft 365 Apps for enterprise installation wizard.

5. Once **OfficeSetup.exe** has finished downloading, select **Open file** that appears below **OfficeSetup.exe** in the **Downloads** window.

6. If a **Do you want to allow this app to make changes to your device?** dialog box appears, enter **adatum\administrator** in the **username** box, type **Pa55w.rd** in the **Password** box, and then select **Yes**. 

7. You may receive a **Continuing could be expensive** dialog box that displays a warning message indicating that it may be expensive to continue downloading because you're connected to a network that limits downloads every month. <br/>

	**Important:** If you receive this dialog box, it may appear in the taskbar but not on the desktop. If this occurs, hover your mouse over the **Office** icon on the taskbar, and then select the **Continuing could be expensive** dialog box if it appears. If you do receive this dialog box, the Office install will NOT proceed until you select **Continue** (the Office window will just keep displaying the **We’re getting things ready** message, but it won’t actually do anything). <br/>
	
	If you received the **Continuing could be expensive** dialog box, select **Continue**.

8. The installation may take several minutes to complete. Once the installation finishes, select **Close** in the **You're all set! Office is installed now** window.

9. To validate Laura's Microsoft 365 Apps for enterprise installation, select the **Start** icon in the lower-left corner of the taskbar. Below the **Recently added** section (at the top of the **Start** menu) select **Expand** to display all the Microsoft 365 enterprise apps that were just installed. These apps should include Word, PowerPoint, Outlook, OneNote, Publisher, Access, Skype for Business, and Excel.

10. In the **Start** menu, select **Word**.

11. On the **Sign in to get started with Office** page, select **Sign in or create account**. On the **Activate Office** page, enter **Laura@xxxxxZZZZZZ.onmicrosoft.com** (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider) with a password of **User.pw1**.

12. On the **Stay signed in to all your apps** window, select **OK**.

13. On the **You're all set!** window, select **Done**.

14. On the **Accept the license agreement** window, select **Accept**, and then select **Close**.

15. Verify that Word is functioning properly by opening a blank Word document, entering some text, and saving the document to the **Documents** folder. 

16. Close Word.

17. Now that you have completed this lab exercise by installing Microsoft 365 Apps for enterprise, you should log out of Microsoft 365 as Laura Atkins. Select Laura's icon in the upper-right corner of the screen (the circle with LA in it), and then in Laura's property window, select **Sign out**.

18. Once Laura is signed out, close your Microsoft Edge browser. 

19. You now want to log out of LON-CL1 as Laura Atkins and log back in as the Adatum administrator. This will prepare LON-CL1 for the next lab. <br/>

	On LON-CL1, select the **Ctrl+Alt+Delete** function in your VM lab environment. 
	
18. On the desktop menu, select **Switch user**. 

19. On the desktop, the **Administrator** is selected by default. Enter **Pa55w.rd** in the **Password** field and then select the forward arrow.  <br/>

	The desktop should now display the logged on user as **adatum\administrator**. LON-CL1 is now ready for the next lab.

# Proceed to Lab 2 - Exercise 4
