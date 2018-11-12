# Homework 7
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW7_1819.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework7)
* [Back to Homepage](https://jacewoods.github.io/)

The seventh assignment in CS460 involves working with APIs. The Giphy API to be specific. To utilize the API within our MVC Web Application, we use Javascript and JSON to gather the required information and AJAX for responsive, quick results. The Giphy API itself is a "translater" that essentially takes the user input and, if it isn't a generic word like "to" or "she", will convert the word into a image relevant to the input, via the JSON that is returned using the Giphy API.

# Getting Started
To set this project up, I created an empty MVC app, versus the standard pre-built one that features a sample "About" and "Contact" page. Since this app would only be using the landing page, I ensured it would not have any extra startup views or a navbar at the top, since both of these would be unnecessary. Since I also named my initial Controller "Giphy" I went into the ```RouteConfig.cs``` section and changed my default landing page from the Home controller to:
```c#
defaults: new { controller = "Giphy", action = "Index", id = UrlParameter.Optional }
```
Once this was done, I built a simple looking landing page that consists of just a header and a textbox for input.

# Setting up Javascript and AJAX
Before I dive into utilizing the API itself, I set up my landing page to return just a simple "Hello" each time I hit the spacebar. To do this, I set up a ```translator.js``` page in my scripts, and connect it to the view by using the following code:
```html
@section TranslateScripts
{
    <script type="text/javascript" src="~/Scripts/translator.js"></script>
}
```
This code is rendered in the Shared layout page with ```@RenderSection("TranslateScripts", required: false)```. With the view connected to my javascript code, I had to set up a basic conditional to make sure it would only react to my javascript code if I hit the spacebar after an input. To do this, I wrote a conditional directly following a function:
```js
$("#UserWord").bind('keypress', function (e) {
    if (e.key === ' ' || e.key === 'Spacebar') {
```
With this, I was able to get a successful response when hitting the spacebar in my textbox defined with ```#UserWord``` when watching the network diagnostics in the [Firefox Developers](https://www.mozilla.org/en-US/firefox/developer/) browser. This function then gets funneled into my ```displayData``` function via AJAX. It took me a while to fully understand AJAX at this stage of my code, and it ended up doing more than initially required until more of my code was developed, but it does do the job.
```cs
$.ajax({
    type: "GET",
    dataType: "json",
    url: source,
    success: displayData,
    error: errorOnAjax
});
```
With the success result pushed into the displayData function, all I had to do to output text onto my view was:
```js
function displayData(info) {
    $("#message").append("hello ");
}
```

With hello's appearing in my view, I was ready to move onto replicating the actual input I typed in. For this, I had to employ assistance from a new controller to produce JSON for my javascript to utilize. The process I, and the code, goes through for this, is first gathering the word in javascript using this line of code:
```js
var word = $("#UserWord").val().split(" ").slice(-1);
```
This grabs all the information currently typed into the #UserWord textbox, and "slices", or grabs the last word found in the box, denoted by spacebar with ```split(" ")```. This word then goes into ```var source = "/Word/BoringWords/" + word;```, and, if you look at the AJAX call above, you can see the ```source``` variable is gathered in the AJAX function. Word/BoringWords is where we delve into the Controller created to use alongside the javascript. All it is doing right now is taking the input string that we send to it and converting it to JSON so that the displayData function can use it. The code at this point is:
```cs
public JsonResult BoringWords(string id)
{
    var data = new
    {
        inputText = id
    }
    return Json(data, JsonRequestBehavior.AllowGet);
}
```
This then sends JSON back to displayData that looks something like this:
```json
inputText	: "TextInputHere"
```
With this information in the displayData function, we can now call it fairly easily. The displayData code I showed above can now be updated to look like this:
```js
if (info.inputText != null) {
    $("#message").append(info.inputText + " ");
}
```
The if-null I included before the output will prevent any output if there is no text added since the last time the spacebar was tapped.

# Using the Giphy API to "Translate" words
Now that the basic functionality of the application is complete, I was ready to employ the Giphy API to create image results based on the user input. To get started, I created a key on the [Giphy Developers site](https://developers.giphy.com/) to utilize in my code when fetching my input query from Giphy. It took a lot of trial and error building the code to find the image Giphy was sending to me. Entering this sample URL (`https://api.giphy.com/v1/stickers/translate?api_key=XXXYourKeyHereXXX&s=lobster`) into my browser with my unique key, I found that it was a lot of JSON information, so I knew I had to somehow get this information into my displayData function, since it has already shown its capability of parsing JSON. I decided on the WebClient function in my Controller to gather the JSON data, and initially, the code I had written to do this looked like this:
```cs
public JsonResult FindGiphy(string id)
{
    var data2 = new WebClient().DownloadString("https://api.giphy.com/v1/stickers/translate?api_key=" + apiKey + "&s=" + id);
    
    return Json(data2, JsonRequestBehavior.AllowGet);
}
```
The apiKey variable the URL takes is my hidden key for the Giphy API, which I grabbed by calling a local file in the Web.config settings, and the ID is the search query sent to me from the javascript grabbing my textbox information. The returned message did have the information I was looking for, however, what I saw in the JSON when doing this was what appeared to be double-wrapped JSON and unparsable. This made sense to me, since I was essentially taking JSON data and trying to convert it into JSON again, using the ```JsonResult``` and ```return Json()``` commands. I was able to fix this by changing my ```JsonResult``` to ```ActionResult``` and changing the return line to ```return Content(data2, "application/json");```. Doing this allowed for me to gather the singly-wrapped JSON data and successfully transferring it to my displayData function.

One thing to note here, is that I opted for a second public Result option in my controller. I found it more efficient to basically give my javascript two seperate options to branch off to based on if the word that is being input is a "Boring Word" or not. I did this by creating a list of boring words in my javascript file, running a for-loop against the word, and, if it is found in the list, marking a boolean value as true. Branching to the correct controller was done simply like this:
```js
if (isBoring == true) {
    var source = "/Word/BoringWords/" + word;
}
else {
    var source = "/Word/FindGiphy/" + word;
}
```

With this method, my displayData function will now either receive a very simple JSON object of just the ID, or a complex JSON object received by the Giphy API. Since we already discussed the former, I was able to apply the latter with the following code:
```js
else if (info.data.type != "undefined") {
    $("#message").append("<img src='" + info.data.images.original_still.url + "' style='height:140px; width:140px; margin:25px;'/>");
}
```
This first makes sure the word being translated was sent data by Giphy, and then html image syntax is hard-coded into the javascript, all the while using the correct JSON parsing to grab the image (that is, ```info.data.images.original_still.url```). I control the size of the image with the style included after. That's it. The view was now successfully sending in the ID, and getting Giphy Images back out. While I was tempted to make Giphy GIFs, it appeared a bit unbearable in the view, so I just stuck with still pictures.

# Using a Local Database to Store Information
Adding a database to grab information was a simple process, as we have already done something similar in the [5th Homework Assignment](https://jacewoods.github.io/CS460/Homework5/). This includes creating a DAL folder with a Context file, downloading Entity Framework using NuGet, and setting variables with ```up.sql``` and a model class. However, I had initially failed in making sure everything was connected properly, which caused a few hours of debugging and trying to figure out what went wrong. The overall process of setting up a database is simple enough, but I overlooked how delicate the process is, and a single typo that Visual Studio is unable to detect can leave you stranded. My first issue was forgetting to alter my ```Web.config``` file to connect the database. Another was a typo in setting my database elements in the models class where one variable didn't match with what I set up in my ```up.sql```. Lastly, I had set logging the browser used to a 50 character string limit. I had to use the Visual Studio Debugger to realize the browser information being returned was much longer than that. These were the three standout issues in my database, but there may have been others, as I tried a lot of unique things to try and get it working to store the information from my application. In the future I'll be much more cautious when setting up a database, hopefully so there is one or fewer problems to work out once I get it running.

As for storing the information, the code was very simple.
```cs
DBLog.InputTerm = id;
DBLog.IPAddress = Request.UserHostAddress;
DBLog.BrowserType = Request.UserAgent;
db.Giphies.Add(DBLog);
db.SaveChanges();
```
It gathers my inputTerm just like the rest of the Controller does, as well as my local IP address and my previously mentioned Web Browser. It also includes a unique ID and Timestamp, set in the Models page.

# Takeaways
With the project complete, I feel much more confident with interacting with a few debugging elements. This was the first time I had utilized the Firefox Developer, which I used to see what kind of information I was receiving in the JSON. I also used it to see how information was being logged in javascript using ```console.log```. I used the Visual Studio debugger which I previously mentioned helped me realize my string for the Browser was much longer than what I was allocating, causing a break in storing the information to my database. Moving forward, I now have experience with troubleshooting database issues, using debuggers and ```console.log```, and working with API's. All this alongside the experience using AJAX and furthering my knowledge with Javascript and jQuery makes me more confident moving forward in the class.

Check out my video that demonstrates the application in action!

<a href="http://www.youtube.com/watch?feature=player_embedded&v=RWiahMu55pQ
" target="_blank"><img src="http://img.youtube.com/vi/RWiahMu55pQ/0.jpg" 
width="920" height="690" border="10" /></a>
