# Homework 2
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW2.html)
* [Demo](https://jacewoods.github.io/CS460/Homework2/demo/index.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework2)
* [Back to Homepage](https://jacewoods.github.io/)

The second homework assignment for CS460 involved learning the following things:
1. Javascript
1. jQuery
1. The DOM
1. Merging branches with Git

First off, I drew a sketch to try and visualize what I was looking for in my website design.

![image text](/CS460/Homework2/hw2image1.jpg "Page 1 of Sketch")
![image text](/CS460/Homework2/hw2image2.jpg "Page 2 of Sketch")

My goal initially was to grab multiple inputs in a "Burger Bar" and create a detailed table based on those inputs. One thing you'll notice is that I had a very barren table result in my initial sketch. I had a tough time brainstorming ideas to add information aside from the ingredients into the table simply because there isn't a whole lot you can add to a table with just the inputs I sketched, and adding anything unique aside from ingredients would defeat the purpose of a "Burger Bar". I decided to go for a list approach, as you'll see in the demo, and actually created two seperate lists; one for the ingredients, and one that creates messages based on unique input combinations. More on that below.

# (More) HTML and CSS
I was much more comfortable writing HTML and CSS code this week than the previous, having some experience under my belt. The challenges were mostly in adding inputs like radios and checkboxes, which I did like this:

```
<!--Select Type of Patty-->
  <div class="col-sm-4" id="thepatty">
    <h4>The Patty</h4>
    <ul>
      <li>
        <input type="radio" name="choosePatty" checked="checked" id="beef" value="Beef Patty">
        <label for="beef">Beef</label>
      </li>
      <li>
        <input type="radio" name="choosePatty" id="veggie" value="Veggie Patty">
        <label for="veggie">Veggie</label>
      </li>
      <li>
        <input type="radio" name="choosePatty" id="vegan" value="Vegan Patty">
        <label for="vegan">Vegan</label>
      </li>
    </ul>
  </div>
```

The other thing that took some time, but was enjoyable, was making a fun button to click on when all of the inputs are complete, and I made a lot of edits to the CSS of the button to shape it up the way I wanted it to appear on the screen, such as changing the outline of the box, the color to appear like a textbox version of a burger, and adding hover and clicking functionality. Below is the final product behind the process I took:

```
.burgerBuilder {
    font-size: 20pt;
    font: bold;
    margin-top: 10px;
    margin-bottom: 50px;
    padding: 25px;
    border-radius: 20%;
    border-color: black;
    background-color: #ffcc66;
    color: #663300;
    outline: none;
}

    .burgerBuilder:hover {
        box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24), 0 17px 50px 0 rgba(0,0,0,0.19);
    }

    .burgerBuilder:active {
        box-shadow: 0 5px #666;
        transform: translateY(4px);
    }
```

# Javascript and jQuery
Due to the condensed and accelerated learning structure of the class, I combined my learning of javascript and jQuery simultaneously. The concept of (simple) javascript coding is very familiar to me and was not hard to understand. Especially considering the basic elements of Javascript are similar to C++ or Python in that they are very Object Oriented.

The main lesson of this assignment for me was getting acquainted with jQuery. I used [this series of tutorials](https://www.youtube.com/watch?v=hMxGhHNOkCU&list=PLoYCgNOIyGABdI2V8I_SWo22tFpgh2s6_) to help me understand the basic concepts, as well as [this page](https://oscarotero.com/jquery/) to get a nice overview of the commands to look at while building my webpage.

I started out just having some fun with jQuery commands. I wanted the page to pop out to the viewer in a way, which is why I added the following code to start off the launch of the page:

```
$(function () {
  $('#burgerBar').hide().fadeIn(300);
  $('#burgerBar2').hide().delay(300).fadeIn(300);
  $('#thebun').hide().delay(600).fadeIn(300);
  $('#thepatty').hide().delay(900).fadeIn(300);
  $('#thecheese').hide().delay(1200).fadeIn(300);
  $('#thetoppings').hide().delay(1500).fadeIn(300);
  $('#premiumtoppings').hide().delay(1800).fadeIn(300);
  $('.burgerBuilder').hide().delay(2100).fadeIn(300);
});
```

This code utilizes hiding my columns and button, and displaying them in a quick manner (every .3 seconds) until the full page has loaded. I spent a lot of time working with different jQuery commands as well, such as .empty() to ensure my page doesn't get overpopulated with each button press, .each() to traverse all elements from my inputs, and .css() to change the elements in a table from within the jQuery command. Below is an example of the usage of the .each() command to look at all inputs that used a checkbox.
```
$('div input[type=checkbox').each(function () {stuff})
```

# The DOM
The DOM, or Document Object Model, was a pretty simple concept to understand, or at least its basic principle was. I ended up using the DOM for much of my jQuery script, primarily append(), and had it interact with my inputs in order to push out information for my lists. Below is an excerpt of my code with the append() and empty() functions utilized.

```
function burgerList() {
  $(".listBuilder").empty().append("<h4>Your Burger Ingredients:</h4><ul>").css("padding-bottom", "10px");
  $('div input[type=radio').each(function () {
    if ($(this).is(":checked") && $(this).attr('value') != "No Cheese") {
      $(".listBuilder").append("<li>" + $(this).attr('value')).append("</li>");
    }
  })
```

In the first line of this code I used the append() function to begin building onto a div I had created in my HTML section with the class "listBuilder". I also used empty() to ensure that the output information is not repeated. Without including empty(), the page can become filled with repetitive and unnecessary information. I also used some javascript if-statements to find out if a certain input was avoided. If it was, I append to my list of ingredients. (In this example, I add the type of cheese selected, unless the option of "No Cheese" was selected)

Overall, I felt confident in my understanding of the DOM as I progressed through the assignment.

# Merging branches with Git
Merging seemed like a daunting and complicated process to grasp, but after this week I became comfortable with merging and realized it is simpler process than I had made it out to be. I was very careful in my steps taken to merge, as not to impact the way the code was pushed out to the repository. Here are roughly the steps I took to ensure this procedure was done correctly:

1. git branch hw2-main (creates the new branch)
1. git checkout hw2-main (goes to the new branch)
1. git branch (verify I am on the new branch)
1. vi homeworkfiles.html (create a file in new branch)
1. git add homeworkfiles.html
1. git commit -m "added homework files"
1. git checkout master (go back to master branch)
1. vi README.md (add change to make merge visible)
1. git add README.md
1. git commit -m "showing commit on master"
1. git merge hw2-main (merges hw2-main into master branch)
1. git log --oneline --graph (visual of hw2-main being merged into master branch, and to make sure it worked)
1. git push origin master
1. git push origin hw2-main

# The Final Product
In the end, I altered the input displays slightly from my initial sketch to a more clean and concise look, and I found it to be more fun to take an approach of first generating a generic list that displays all of the options you selected, as well as a second (and more interactive) list that sends you notes based on what you selected. For instance, if you select "Bacon", it will essentially tell you in a personalized note that bacon was a good idea, or if you select both "pineapple" and "egg", it will question the combination. This is a way to play with the inputs in order to see what kind of messages the Kitchen will deliver! Below is a screenshot of one combination of inputs.

![image text](/CS460/Homework2/hw2pagedemo.PNG "Screenshot of my webpage")
