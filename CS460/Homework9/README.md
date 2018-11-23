# Homework 9
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW9_1819.html)
* [Back to Homepage](https://jacewoods.github.io/)
* [Deployed Site](http://hw8jaceapp.azurewebsites.net/)

The ninth and final assignment in CS460 involves deploying both the web application and the database that was built in Homework 8 to Azure. Below are the steps to accomplished this.

# Getting Started

First off, I needed to create an account on Azure. I did so [here.](https://azure.microsoft.com/en-us/free/search/?&OCID=AID719825_SEM_ZQCt71E6&lnkd=Google_Azure_Brand&gclid=CjwKCAiAodTfBRBEEiwAa1haugjwERN0m3H-c0adVw4y707sdO6hvjy5v0oAnYTEuLjpd-9kZOw51RoCRRwQAvD_BwE) Next, I need to create a Resource group on Azure. To do this, start on the Azure Portal:

# Create a Resource group

![image text](/CS460/Homework9/ResourceGroup.PNG)

1. Select the "Resource groups" option on the left-hand side
1. Select "Add"
1. Type in a Resource group name
1. Select a Subscription (Pay-As-You-Go) and a Resource group location (West US)
1. Select "Create" at the bottom of the page

# Create an Empty Database on Azure

![image text](/CS460/Homework9/SetupSQLDBonAzure.PNG)

1. Select the SQL databases tab
1. Input a "Database name", the Pay-As-You-Go Subscription, and the resource group that was previously created in the above step.
1. "Blank database" for Select source
1. For Server, Configured required settings
    1. Input a "Server name"
    1. Create a Server admin login and password (make sure you don't forget these credentials!)
    1. Set location to "West US"
    1. Allow Azure services to access server (make sure this is checked!)
    1. Select "Select" in the bottom right
1. "Not Now" for SQL elastic pool
1. Set pricing tier to the $5/month, Basic 2GB Option
1. Collation can be left alone
1. Select "Create", database should appear in the SQL databases section after a few seconds

# Add a Firewall Rule

1. Select the "Resource groups" tab
1. Select the previously created resource
1. Select the SQL server you created, not the SQL database
1. Select "Show fireewall settings" from Firewalls and virtual networks
1. Name your rule (HomeDev in my example), and give it a Start/End IP that matches the Client IP address
1. Confirm this and select Save to make the changes.

![image text](/CS460/Homework9/firewallsuccess.PNG)

# Connect your created database to SSMS

1. Select the SQL databases tab
1. Select your database
1. Copy the Server name which should be <YourSelectedName>.database.windows.net
1. Open SSMS (Microsoft SQL Server Management Studio)
1. Enter the credentials you provided in the "Create an Empty Database" section as shown in the figure below
    
![image text](/CS460/Homework9/SSMStoAzure.PNG)
    
# Connect your created database to Visual Studio

1. In Visual Studio, select the Server Explorer
1. Under Azure, right click SQL Databases, and select "Open SQL Server Object Explorer"
1. In the Object Explorer, select the "Add SQL Server" option
1. Fill in the credentials, similarly to in SSMS, and connect

![image text](/CS460/Homework9/connectVStoAzure.PNG)

If successful, the Server Object Explorer should look something like this:

![image text](/CS460/Homework9/VSconnectsuccess.PNG)

# Use up.sql to upload information into the new database

1. Go to up.sql and repeat the previous instructions by:
1. Changing the connection from your default local database if necessary
1. Entering the credentials of your azure database
1. Once connected, run the up script
1. Your database should now be populated. You can check by refreshing it in Visual Studio or SSMS and viewing the content

# Get the Connection String

1. In Azure, select SQL databases, and then your created database
1. Select "Show database connection strings" under the Connection strings option
1. This will show the string we need to plug into our Web.Config to connect our Azure database to our local web app, copy this string
1. In Web.Config, replace the `<local code here>` information in `connectionString="<local code here>"` with the string you copied just now
1. In your pasted string, make sure to change `{your_username}` and `{your_password}` to the credentials you've been providing with the azure database. We can see if this works and then hide the actual password from the Web.Config in a future step
1. Use IIS Express and create a new item in the app and check the Azure database to make sure the item inserted is now reflected in the database

# Create Web App on Azure

1. On the Azure portal, select the "App Services" tab, then select "Add"
1. Under Web Apps, select "Web App", then select Create
1. Enter a name for the web app, Pay-As-You-Go subscription, Use existing resource group that we previously made
1. Under App Service plan, make sure to have a free plan that you can add by setting the pricing tier to the Dev/Test Free (F1) option
1. Click Create

![image text](/CS460/Homework9/CreateWebApp.PNG)

Now, if you go to App Services and select your app, you can select the URL that will open your yet-to-be connected web app

# Configure Connection String

1. Remain in App Services and select the Application settings option within your app
1. Scroll down to Connection strings and select "Add new connection string"
1. For the Connection String input, it must match the name of the context class
1. The Value input must be the same string that was plugged into `Web.Config`
1. The Type is "SQLServer"
1. Make sure to "Save"

![image text](/CS460/Homework9/ConnectionStringConfigure.PNG)

# Deploying the Local Web App

1. In Visual Studio, select "Build", then "Publish <projectname>"
1. Under App Service, choose "Select Existing" and hit Publish
1. Tap "OK" after the prompt looks something like this:
    
![image text](/CS460/Homework9/publishapp.PNG)

The website should now be deployed!
