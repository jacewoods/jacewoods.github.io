# Homework 4
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW4_1819.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework4)
* [Back to Homepage](https://jacewoods.github.io/)

In the fourth assignment of CS460, we were introduced to using the ASP.NET MVC 5 to build a simple MVC web application. To get started, I launched Visual Studio and selected the ASP.NET Web Application .NET Framework option to build the site. I also selected the MVC template, rather than an empty one, to create a foundation to build onto for my site.

# Getting Oriented
The first thing I did once the MVC template was built was took a look at the code that already existed on the site. Primarily, the code on the homepage. This code was broken into two pieces. One such piece is the "Index.cshtml" page located under the Home Views tab. This page overall looked pretty standard and familiar compared to the HTML I had written in Homeworks 1 and 2. However the main thing my attention was drawn to was the code shown below:
```
@{
    ViewBag.Title = "Home Page";
}
```
This is an introduction to both the ViewBag element and Razor HTML being invoked to use C# code within the HTML file. The second component of the homepage was built using the Shared view page of "_Layout.cshtml". This page builds the layout of the homepage as well as any other page invoked in the Views tab. In terms of physical buttons to select, the only thing the Layout page added is the toolbar at the top of the screen. This features more Razor HTML and includes links that look like this:
```
<li>@Html.ActionLink("Converter", "Converter", "Home")</li>
<li>@Html.ActionLink("ColorChooser", "Create", "Color")</li>
```
This was initially defaulted to an About and Contact page, but due to the similarities of where the Converter page should take me to, it wasn't difficult to determine what keywords to change. The first element in the ActionLink is simply the text to be displayed on the toolbar, the second is the directory name, and the last is the Controller it is pulling information from. This did take a bit of time to learn when building the second "Color" directory, as it was not to be pulled from the /Home directory. Once I traced the files and where they originated, I was able to eventually discover how to build a link that takes me to the ColorChooser page.

The resulting product of the homepage can be seen below:
![image text](/CS460/Homework4/hw4home.PNG "Screenshot of my C# Homepage")

# Building the Converter page
Once the homepage is set up, the first task is to build a Converter page that takes a user input for mileage and a selection for what metric unit they wish to convert the mileage to. Ideally, this should look fairly identical to the one seen in the [Homework 4 description](http://www.wou.edu/~morses/classes/cs46x/assignments/images/hw4_mmconverter.png). Starting out, this was fairly smooth sailing. I built a foundational HTML layout for the page that I have gotten some experience with in the first two homework assignments. In fact, [using radio buttons for input](https://jacewoods.github.io/CS460/Homework2/demo/index.html) is something I had done previously, so inserting this was no problem. The piece that did take some time to implement is the section that includes the output after the user adds their mileage input and selected conversion radio. Below is how I wrote this:
```html
<form action="Converter" method="get">
    <div style="padding-bottom:240px;">
        <div class="col-sm-6">

            <label for="name"><b>Miles</b></label>
            <p><input type="number" step="0.001" min="0" id="mileNum" name="mileInput"></p>
            <h4 style="color:darkred; margin-top:160px;"><b>@ViewBag.message</b></h4>
```
The thing to note here is the "@ViewBag.message" in the last line. This pulls all of the math I did in the second section of building this page. That is, within the "HomeController.cs" file under the Controllers folder. The Controller files allow you to implement C# code to your project, and with Razor HTML, you can then implement it right into your HTML with the ViewBag. Using [HttpGet] to establish the requirement of using "Get" for this code, I then requested the miles and metric inputs by the users with the following code:
```c#
string miles = Request.QueryString["mileInput"];
string metric = Request.QueryString["chooseMetric"];
```
This code pulls the element identified by "name" in the .cshtml file, and plugs it right into the C# Controller file. This then enabled me to do conversions from string to double and then do math based on what kind of metric input was requested. A quick example of this is shown below:
```c#
mileVal = Convert.ToDouble(miles);
if (metric == "millimeters")
{
  mileVal = mileVal * 1609344.0;
}
```
With this calculation complete, I return the final product with this line:
```c#
ViewBag.message = miles + " miles is equal to " + outputNum + " " + metric;
```
This plugs the message back into the .cshtml code I posted above and outputs it when a request for a conversion is made. The resulting product looks like this on the site:
![image text](/CS460/Homework4/hw4converter.PNG "Screenshot of my C# Converter Page")
