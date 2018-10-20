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
This was initially defaulted to an About and Contact page, but due to the similarities of where the Converter page should take me to, it wasn't difficult to determine what keywords to change. The first element in the ActionLink is simply the text to be displayed on the toolbar, the second is the directory name, and the last is the Controller it is pulling information from. This did take a bit of time to learn when building the second "Color" directory, as, per the requirements of Homework 4, it was not to be pulled from the /Home directory. Once I traced the files and where they originated, I was able to eventually discover how to build a link that takes me to the ColorChooser page.

The resulting product of the homepage can be seen below:

![image text](/CS460/Homework4/hw4home.PNG "Screenshot of my C# Homepage")

# Building the Converter page
Once the homepage is set up, the first task is to build a Converter page that takes a user input for mileage and a selection for what metric unit they wish to convert the mileage to. Ideally, this should look fairly identical to the one seen in the [Homework 4 description](http://www.wou.edu/~morses/classes/cs46x/assignments/images/hw4_mmconverter.png). Starting out, this was fairly smooth sailing. I built a foundational HTML layout for the page that I have gotten some experience with in the first two homework assignments. In fact, using radio buttons for input is something [I had done previously](https://jacewoods.github.io/CS460/Homework2/demo/index.html), so inserting this was no problem. The piece that did take some time to implement is the section that includes the output after the user adds their mileage input and selected conversion radio. Below is how I wrote this:
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

# Building the Color page
Moving onto the next segment of the Web Application, we were to build a Color Mixer that essentially takes two hexadecimal color inputs from the user, combine their elements, and spits out a third, new color. All three colors are to be displayed in small squares once the user hits the Submit button. It again should be [nearly identical to the example given in the homework](http://www.wou.edu/~morses/classes/cs46x/assignments/images/hw4_colorconverter.png). To get started with this, I again began building the .cshtml page. For this one, however, we were to use Razor HTML to create our submission form, versus the standard HTML form we've built in the past few homeworks and in the Converter page. [This](http://www.tutorialsteacher.com/mvc/htmlhelper-textbox-textboxfor) was one resource I used to get help learning how to create the form, which included a large piece of the puzzle as it added a textbox and form-control class. Once I had these, I later found the @pattern and @placeholder features to give me a well-rounded form that ensured the input asked of the user is entered legitimately. The page successfully launched the forms with this final razor html:
```c#
@Html.Label("colorOne", "First Color")
@Html.TextBox("colorOne", null, new { @class = "form-control", @pattern = "#[0-9A-Fa-f]{3,6}", @placeholder = "#12B37F", required = "required" })

@Html.Label("colorTwo", "Second Color")
@Html.TextBox("colorTwo", null, new { @class = "form-control", @pattern = "#[0-9A-Fa-f]{3,6}", @placeholder = "#A13344", required = "required" })
```
Once the form was structured, I moved onto the Color Controller. I kept getting errors launching the site trying to run an ActionResult Color() class and a Create() class, so eventually it worked correctly with two Create() classes. This first was a very plaing [HttpGet] that looks like this:
```c#
[HttpGet]
public ActionResult Create()
{
    ViewBag.show = false;
    return View();
}
```
The second piece uses [HttpPost] as is required in this section of the assignment. It requires the strings to be pulled from the form to be established in the ActionResult, and instead of Request.QueryString, the information can be received with Request.Form. Here is how i set this up:
```c#
[HttpPost]
public ActionResult Create(string firstColor, string secondColor)
{
    firstColor = Request.Form["colorOne"];
    secondColor = Request.Form["colorTwo"];
}
```
Now that I was able to successfully pull the hexadecimal code from the page, I was able to start the process of creating a third color. I used Color and ColorTranslator after learning about them [here](https://docs.microsoft.com/en-us/dotnet/api/system.drawing.colortranslator.fromhtml?view=netframework-4.7.2) to start the process of creating a third mixed color. With ColorTranslator, I was able to convert the hexadecimal into RGB values. This allowed me to build a function with the new Color variable that poured the RGB values into it. Here is some of the code to show how this was done:
```c#
Color inputOne = ColorTranslator.FromHtml(firstColor);
Color inputTwo = ColorTranslator.FromHtml(secondColor);
Color mixedColor = new Color();
int red, green, blue = 0;
if (inputOne.R + inputTwo.R > 255)
{
    red = 255;
}
else
{
    red = inputOne.R + inputTwo.R;
}
```
Here I used the built in Color functionality to select inputOne.R (the red in RGB) and inputTwo.R to make sure their values of red are not greater than 255, and if they are, set 255 to the max. Once I was done with all of this, I finalized the function by building the mixedColor variable using the information from the if-statements and then finally converting it back into hexadecimal and, thanks to ViewBags, sending it to my .cshtml page with some built-in CSS. Here is code for this:
```c#
mixedColor = Color.FromArgb(255, red, green, blue);
string thirdColor = ColorTranslator.ToHtml(mixedColor);

ViewBag.color1 = "background:" + firstColor;
ViewBag.color2 = "background:" + secondColor;
ViewBag.color3 = "background:" + thirdColor;
```
In the HTML Color page, I then invoked this newfound information using the ViewBags, only to be output if there is an initial user input. The code to do this looks like this:
```html
<div class="col-sm-1" style="width:75px; height:75px; @ViewBag.color1; border-style:solid; border-width:1px"></div>
<div class="col-sm-1" style="width:45px;"><h3>+</h3></div>
<div class="col-sm-1" style="width:75px; height:75px; @ViewBag.color2; border-style:solid; border-width:1px"></div>
<div class="col-sm-1" style="width:45px;"><h3>=</h3></div>
<div class="col-sm-1" style="width:75px; height:75px; @ViewBag.color3; border-style:solid; border-width:1px"></div>
```
I also had to figure out a way to create the colored boxes and to make sure they don't split up too much across the page. I accomplished the former using CSS styling for width and height, followed by the invoked background color using ViewBag. The latter was done by stating "width:45px", which forced the boxes closer together. 

With this functionality now complete, my site is now finally running up to par with the requirements. Below is another example picture of the site in action with inputs of #0FB404 and #D20449.

![image text](/CS460/Homework4/hw4color.PNG "Screenshot of my C# Color Page")

# Merging with Git
With all pieces of the project complete, it was time to pull it all back together with Git Merge. I had started off with the very basic template just barely changed in the Master branch. I created a "hw4-converter" feature branch that completed the homepage as well as the converter page. Then, I created a "hw4-color" feature branch that built the functionality of the color mixer page. Once complete I checked back into the barren Master branch, and first merged the hw4-converter branch with no issues. When merging hw4-color, however, I ran into a few merging conflicts. It broke the application inside Visual Studio, so I had to launch Notepad++ in order to fix the conflicts. It was interesting to see what it pushes into the code, as seen below:

![image text](/CS460/Homework4/hw4mergeconflict.PNG "Screenshot of my Merge Conflict")

At first I didn't understand what the \>>> and === lines really meant, but upon looking closer, I realized all it was doing was pushing the differences in code made in two branches and allowing me to decide which difference I should keep. It was somewhat scary seeing my code break down like this, but once I had figured out how to resolve these conflicts, I was able to complete the merge and my site was running at 100%!

Overall, this project did take some time as I had to learn about MVC and how it interacts with the ASP.NET Web Application, but hopefully now that I have a solid understanding of the basics, I will be comfortable enough moving into implementations using a database.
