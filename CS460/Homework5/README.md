# Homework 5
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW5_1819.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework5)
* [Back to Homepage](https://jacewoods.github.io/)

For our fifth assignment in CS 460, we were to continue developing our knowledge in ASP.NET MVC 5, as well as to include a simple database to start learning how to integrate this into our site. Utilizing Razor HTML and T-SQL also helped to round out the completion of this assignment. The specific assignment was to have a database that stored information that Apartment Renters submitted, so that the appropriate maintenance team could view the requests and proceed based on the information provided.

# Getting Started
The first step I took to getting this assignment going was to get a local database connected and running within the code. To do this, I took a few steps. First off, I created a database under the App_Data folder, and then included the ```up.sql``` and ```down.sql``` files in order to run queries for the database (the query itself to launch the table was simply ```SELECT * FROM dbo.Renters;```). Once the basic elements of the database were created, I was able to seed some starting inputs into it that I would later use for the actual website. Seeding turned out like this:
```sql
INSERT INTO [dbo].[Renters] (FirstName, LastName, PhoneNumber, ApartmentName, UnitNumber, Explanation, Permission) VALUES
	('Sam', 'Samson', '503-444-5555', 'Westfield Apartments', '280', 'Freezer Exploded', 0),
	('John', 'Roberts', '971-888-8889', 'Clearwater Apartments', '300', 'Bathroom door split in half', 1),
	('Theresa', 'King', '503-999-9990', 'Ridgeview Apartments', '1002', 'The pool water is green', 0),
	('Jane', 'Thomas', '971-111-1112', 'Ridgeview Apartments', '80', 'Washing Machine is giving attitude', 1),
	('Bill', 'Smith', '503-333-3336', 'Westfield Apartments', '85', 'The Carpet has been stolen', 1)
GO
```

Once seeded, the previously mentioned query then built the table based on my seeded information, and I was at a good point to be able to proceed with the project.

# Building the MVC Site without a Database
Now that my database was internally running correctly, I moved on to structuring the site. Initially, I began with a site that contained no database, referenced from [this code](https://bitbucket.org/morses/seniorproject_2018-19/src/c7bb8a6d0b64a6383e1b33f8766c6baf2cee3597/mvc/User-NoDB/User-NoDB/?at=master), just to ensure everything was running smoothly. This included seeding the information within my DAL folder. Once this ran okay, I removed the seeded information from the DAL folder and included a new line of code: ```public RenterContext() : base("name=OurRenters")```

With this code. I was now attempting to connect the internal database to the local site I was running, which requires a few steps.

# Connecting the Database to the Site

Using NuGet to install the EntityFramework package, I was able to use the ```public class RenterContext : DbContext``` line of code effectively. I also had to alter my ```Web.Config``` file in order to allow everything to see each other. With this complete, I rebuilt my current code solution and then scaffolded a new RentersController that utilized my DAL Context file, as well as my Model. My model has some of the following sample code:
```c#
        [Required (ErrorMessage = "Please input a First Name"), StringLength(20, ErrorMessage = "Input can be no longer than 20 Characters")]
        public string FirstName { get; set; }

        [Required (ErrorMessage = "Please input a Last Name"), StringLength(20, ErrorMessage ="Input can be no longer than 20 Characters")]
        public string LastName { get; set; }
 ```
And with this, the scaffolding process automatically built multiple views that successfully revealed the pre-seeded database, as well as offering a form to fill out to include new items in the database. It did take a few tries to successfully scaffold, as I had issues with some of the syntax and how I had built the models page, but with some troubleshooting, the process did run smoothly over the multiple times I attempted this scaffold to change a few elements.

# Adjusting the Views to a Cleaner Look
The Scaffolding did not do justice to the views pages. It also included many views and elements in the controller that I did not need for this specific assignment, such as Delete and Edit. I simply removed these files and reconfigured my controller to match that of the one I had designed previously in my HomeController. It was helpful to take a look at the pre-built Razor HTML and I continued to work with it in order to design the page in a way that I was happy with.

I started with the table view page, which I honestly did not mind the look of at the start. I made some tweaks to it to make it feel more sleek, such as styling within the page: ```<tr style="border: 3px solid black; background-color: #E2E2E2;">``` and changing the output from the first row to more well formatted text ("First Name", rather than the variable inserted "FirstName"). When I was happy with the way it looked, I moved on to the form page.

The Form did not look well designed. There were strangely writted col-md-2's and col-md-10's that spaced the variables and their text boxes in an odd way. I shuffled this around to allow the input text to appear above the input box, as well as designed it with col-md-4 to allow for 3 input boxes in each row. I also made a change to the "Explanation" box to give more room for writing in a description of the issue the renter was having. Below contains some code that shows my Explanation styling sandwiched between some of the more generic styling of the Apartment Name and Unit Number inquiries:

```html
        <div class="col-md-4">
            @Html.LabelFor(model => model.ApartmentName, "Apartment Name")
            @Html.EditorFor(model => model.ApartmentName, new { htmlAttributes = new { @class = "form-control" } })
            @Html.ValidationMessageFor(model => model.ApartmentName, "", new { @class = "text-danger" })
        </div>
        <div class="col-md-4">
            @Html.LabelFor(model => model.Explanation, "Short Explanation of Issue")
            @Html.TextAreaFor(model => model.Explanation,
     new { @cols = "100", @rows = "5", @style = "width:100%; resize:none;"})
            @Html.ValidationMessageFor(model => model.Explanation, "", new { @class = "text-danger" })
        </div>
        <div class="col-md-4">
            @Html.LabelFor(model => model.UnitNumber, "Unit Number")
            @Html.EditorFor(model => model.UnitNumber, new { htmlAttributes = new { @class = "form-control" } })
            @Html.ValidationMessageFor(model => model.UnitNumber, "", new { @class = "text-danger" })
        </div>
```

I also cut away at the margin between the above text boxes and the submit/agree to contact buttons, using ```margin-top:-50px```.

I've gotten very comfortable integrating CSS into my html code at this point which I am happy to see, as I was very overwhelmed with it just four weeks ago at the start of class.

There are a few more differences included in this view page that I made, including that of error messages. Referencing to my second snippet of code above, I included my own personalized error messages in two cases. That of no input, and that of an input that is invalid (too much text in a string, for example). In doing this, I learned a lot about how the Model was working with this View page, as it directly transferred onto the site once I built and ran the code with the personalized error messages.

# The Final Product
Overall, I am comfortable with how a simple database works, and pretty much already was going into this assignment. My issues just involved getting it connected successfully, which did take some time, and I am sure that I will run into more issues in the future when we move into more complex database tables that include foreign keys and other elements I'm still not fully comfortable working with at this point.

Below is a video that sees my database and site in action!

<a href="http://www.youtube.com/watch?feature=player_embedded&v=Ia9-NO1SMSE
" target="_blank"><img src="http://img.youtube.com/vi/Ia9-NO1SMSE/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="920" height="690" border="10" /></a>
