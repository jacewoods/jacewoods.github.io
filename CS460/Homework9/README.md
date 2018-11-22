# Homework 9
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW9_1819.html)
* [Back to Homepage](https://jacewoods.github.io/)

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

# Select SQL databases

![image text](/CS460/Homework9/SetupSQLDBonAzure.PNG)

1. Input a "Database name", the Pay-As-You-Go Subscription, and the resource group that was previously created in the above step.
1. "Blank database" for Select source
1. For Server, Configured required settings
⋅⋅1. Input a "Server name"
⋅⋅1. Create a Server admin login and password
⋅⋅1. Set location to "West US"
⋅⋅1. Allow Azure services to access server (make sure this is checked!)
⋅⋅1. Select "Select" in the bottom right
1. "Not Now" for SQL elastic pool
1. Set pricing tier to the $5/month, Basic 2GB Option
1. Collation can be left alone
1. Select "Create", database should appear in the SQL databases section after a few seconds
