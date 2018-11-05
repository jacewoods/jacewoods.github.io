# Homework 6
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW6_1819.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework6)
* [Back to Homepage](https://jacewoods.github.io/)

The sixth assignment in CS 460 has us working with a pre-existing, very large database (relative to our single-table database in homework 5!). We are also expected to derive from the C# models created using Entity Framework and "Code First with an Existing Database" workflow option, write LINQ queries, and further our use and knowledge of Razor language features.

# Getting Started
The first thing I had to learn was simply restoring the database in order to connect it to my Visual Studio MVC app. I downloaded the .bak file included in the Homework description page, then used [this page](https://www.howtogeek.com/50354/restoring-a-sql-database-backup-using-sql-server-management-studio/) to restore it using SQL Management Studio. The article itself was slightly outdated so I had to do a bit of my own research for figuring out everything, but eventually I was able to get this section of the project done.

Connecting it to Visual Studio and the MVC app was a little more challenging. I ended up downloaded both SQL Management Studio and [SQL Express](https://www.microsoft.com/en-us/sql-server/sql-server-editions-express) in order to get everything connected. In the end I wasn't precisely sure what ended up working, but suddenly it became compliant and was able to connect without too many issues, which I was a fan of. Most likely it had to do with the SQL Express download building in the right connection properties, which I hadn't realized I was missing. Lastly, I used Nuget to add ```Microsoft.SqlServer.Types``` and altered my ```Global.asax.cs``` file in order to enable all of the databases features and allow myself to begin digging into the coding aspect of the project!
