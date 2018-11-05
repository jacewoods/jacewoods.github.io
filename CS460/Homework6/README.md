# Homework 6
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW6_1819.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework6)
* [Back to Homepage](https://jacewoods.github.io/)

The sixth assignment in CS 460 has us working with a pre-existing, very large database (relative to our single-table database in homework 5!). We are also expected to derive from the C# models created using Entity Framework and "Code First with an Existing Database" workflow option, write LINQ queries, and further our use and knowledge of Razor language features. The objective of the assignment is to use a "World Wide Importers" database that contains a lot of information within the tables, and query the correct elements within the table and apply the queries to the view.

# Getting Started
The first thing I had to learn was simply restoring the database in order to connect it to my Visual Studio MVC app. I downloaded the .bak file included in the Homework description page, then used [this page](https://www.howtogeek.com/50354/restoring-a-sql-database-backup-using-sql-server-management-studio/) to restore it using SQL Management Studio. The article itself was slightly outdated so I had to do a bit of my own research for figuring out everything, but eventually I was able to get this section of the project done.

Connecting it to Visual Studio and the MVC app was a little more challenging. I ended up downloaded both SQL Management Studio and [SQL Express](https://www.microsoft.com/en-us/sql-server/sql-server-editions-express) in order to get everything connected. In the end I wasn't precisely sure what ended up working, but suddenly it became compliant and was able to connect without too many issues, which I was a fan of. Most likely it had to do with the SQL Express download building in the right connection properties, which I hadn't realized I was missing. Lastly, I used Nuget to add ```Microsoft.SqlServer.Types``` and altered my ```Global.asax.cs``` file in order to enable all of the databases features and allow myself to begin digging into the coding aspect of the project!

# First Steps once Connected
Once I had the database connected to Visual Studio, I took to creating the View to make sure it was ready for taking in Person strings, that is, the strings that are used to find people and eventually relay their contact information and other required information in a table. To do this, I started with a simple banner to display at the top of the page. As seen in the video, the banner shows a picture of the globe, followed with "World Wide Importers". I initially set this on the main client lookup page, but later realized it would be redudant to copy and paste this into my client info page, when I could simply paste it at the top of the shared layout page, which is what I had decided to do. Next, I used Razor HTML to build most of the actual client lookup page, with the final amount of code written being surprisingly minimal:

```html
<h2 style="text-align:center; margin-top:-10px; font-family:'Bodoni MT'">Client Search</h2>

@using (Html.BeginForm("Index", "Home", FormMethod.Get, new { @style = "margin-top:30px;" }))
{
    <div class="form-group">

        @Html.TextBox("input", "", new { @class = "form-control", @placeholder = "Search using Client Name", required = "required", @style = "margin: 0px auto;" })
        <div style="text-align:center; margin-top:5px;">
            <button class="btn btn-default" type="submit" style="background-color:#FF2F2F; color:white; text-align:center;">Search</button>
        </div>
    </div>
}
```

With the MVC app, it tends to refresh much quicker when building the CSS into the html page, versus into a seperate CSS page, so I have gotten comfortable with making small changes to the website via CSS by using ```style="StuffHere;"```. The rest of the code in this excerpt was pretty simple to build as well, with some backtracking to previous projects and some research online to add in the Razor HTML form request and textbox input bar.

Past this, the steps to populate the home page with the actual client names wasn't too bad to figure out, either. The lookup page in my controller just needed to receive the inputted string, and then returns back to the View with essentially just this line of code:

```c#
return View(db.People.Where(search => search.FullName.Contains(input)).ToList());
```

To receive this information in the view, I used more Razor html to check if the input string had indeed been received with ```@if (ViewBag.show)``` and then, if the input string was a match with any of the Persons in the database, the following code:

```html
        <h4>Names containing the search input:</h4>
        foreach (var searchResult in Model)
        {

            <div style="margin-bottom:3px;">
                    <a class="btn btn-primary btn-md btn-block" href="Home/Info/@searchResult.PersonID" role="button"> @searchResult.FullName (@searchResult.PreferredName) </a>
            </div>
        }
```

Surprisingly, putting parentheses around the code of the Client's preferred name worked to reflect putting them around the name in the website, which is how it was demo'd in the example video of Homework 6's requirements. Now that the index page is complete and doing everything it should be doing correctly, we are ready to move onto the first feature of the project!

# Building the First Feature
There isn't a whole lot to cover in how I went about building the first feature. I found it was a lot of trial and error, as well as searching various forums and talking with other students to figure out how to go about gathering and showing the connection of Person information into the View. One thing I did do to discover how interconnected the database really is, was to download [LINQPad](https://www.linqpad.net/) and query the database with some commands such as ```People.Find(22)``` and ```Customers.Find(22)``` to make sure I was using the correct syntax when querying within Visual Studio. Later on, in Feature 2, it was slightly more difficult to understand what LINQPad was querying and how to translate it to my View, so I mostly stuck with trial and error in my Controller to discover how to query the correct elements. Focusing on the first feature, though, the code wasn't too complex once I finally figured out what I needed to write, in fact, it was just three lines of code in my Controller:

```cs
ViewModel vm = new ViewModel();
vm.Person = db.People.Find(id);
return View(vm);
```

This, and adding a ```public Person Person { get; set; }``` to my manually written Models page, was all I needed to do to start getting Person information into my view.

In the view itself, I used the ```dl``` and ```dd``` html elements to build the table with the necessary information. and Razor HTML such as ```@Html.DisplayFor(model => model.Person.PreferredName)``` to gather the appropriate information from the Person table and plug it into the View. The main catch of plugging this information into the view was figuring out how to remove the timestamp from the date in the "Member Since:" category. After some more research, I eventually discovered this slightly messy but effective method of converting the information into a string and then only calling a piece of the datetime to go into the view. The final result of that looks like this:

```cs
@string.Format("{0:MM/dd/yyyy}", Convert.ToDateTime(Html.DisplayFor(model => model.Person.ValidFrom).ToString()))
```

That's the gist of it. With this information included, and with some CSS to make the tables look nice, the first feature was less about extensive coding, and more about figuring out how to code it.

# Building the Second Feature
The second feature is more about extensive coding AND figuring out how to code it. This section was pretty difficult for me to wrap my head around, and I'm still not fully comfortable with querying foreign keys and other elements in order to reflect the querying in the view, something I'm sure just takes practice to get comfortable with. Nonetheless, let's dive into how I went about getting this feature to work!

To be more precise, the final segments of the second feature was the most difficult, due to the extensive querying to build the last two tables, the first table wasn't too bad and were mostly similar to that of feature one. The first thing I had to do is restructure the code to allow for using this second feature where applicable. I did this by setting a ```ViewBag.NotEmployee = false;``` variable, and later setting it to true if an if-statement is met. That is, ```if (vm.Person.Customers2.Count() > 0)```. This statement checks for whether the Client is a Customer or an Employee, if they are a Customer, they will have at least one person in their Customers2 data table. ```vm.Person``` had a predetermined ID that was selected in the view. For the first table in feature 2, I just needed to add ```public Customer Customer { get; set; }``` to my model, ```vm.Customer = db.Customers.Find(custID);``` to my controller, and then I was ready to add it to the view. In the view, after checking if the person is indeed a Customer, it becomes a very similar coding process to that of the first feature. Replacing the ```Person``` with ```Customer``` for getting information from their relationship with the company they work for.

The second feature took some time to establish how to get the information. I had to first select the invoices, then the invoice lines, for the Gross Sales and Gross Profit, which looks like this:

```cs
ViewBag.GrossSales = vm.Customer.Orders.SelectMany(i => i.Invoices).SelectMany(il => il.InvoiceLines).Sum(ep => ep.ExtendedPrice);
ViewBag.GrossProfit = vm.Customer.Orders.SelectMany(i => i.Invoices).SelectMany(il => il.InvoiceLines).Sum(lp => lp.LineProfit);
```
And converted to my view using a method like this:
```cs
@Html.DisplayFor(model => model.Customer.Orders.Count)
@string.Format("{0:C}", ViewBag.GrossSales)
@string.Format("{0:C}", ViewBag.GrossProfit)
```

When I managed to get the queries working correctly for this table, it was pretty similar to do the same in the final segment of feature 2, where I selected the invoices and the invoice lines. The addition to this code I had to make was converting it to a list, taking only the first 10 invoices, and order it in descending order to match that of the demo. In the view I used ```@foreach``` again, and populated the table with the four elements of information required just 10 times, utilizing the ```.Take(10)``` code as to not overpopulate this table. Querying these four elements looks like this:

```cs
@Html.DisplayFor(result => results.StockItemID)
@Html.DisplayFor(result => results.Description)
@string.Format("{0:C}", results.LineProfit)
@Html.DisplayFor(result => results.Invoice.Person4.FullName)
```

# The Final Product
In the end, I was happy with how it turned out, but I, again, am not fully comfortable with querying tables at this point in time. It did take a long time, as well as some collaboration from other students, to figure out what was necessary to query to get the information I needed. Given time, I believe this will be something I will be comfortable with. 

I made a demo video showing my site in action! Check it out:

<a href="http://www.youtube.com/watch?feature=player_embedded&v=HO4kw7QeEP8
" target="_blank"><img src="http://img.youtube.com/vi/HO4kw7QeEP8/0.jpg" 
width="920" height="690" border="10" /></a>
