# Homework 1
* [Assignment Page](http://www.wou.edu/~morses/classes/cs46x/assignments/HW1.html)
* [Demo](https://jacewoods.github.io/CS460/Homework1/demo/index.html)
* [Repository](https://github.com/jacewoods/CS460/tree/master/homework1)
* [Back to Homepage](https://jacewoods.github.io/)

The first homework assignment of CS460 was based on getting acquainted with a few things:
1. Git
1. GitHub
1. HTML
1. CSS
1. Bootstrap

I had a decent enough knowledge of how all of the languages and software listed above worked, with bootstrap being the 
exception. Due to this, bootstrap gave me the biggest hurdle to pass in order to get my site up and running how I wanted it to, with
the other listed items needing more of a refresher/review.

# Git and GitHub
First off, I had to get started with Git. I used [git-scm](https://git-scm.com/downloads) to download the Git Bash and use this for commits and pushing to GitHub. Having experience using GitHub and git commits through the command line, I didn't experience too many issues with this. The main thing I had to try and focus on was using "git status" to ensure my computer and laptop were in sync at all times during the process of working on the html files. 

Below is the general process I went through when committing and pushing out updates to my files using git and github:

```
git status
git add .
git status
git commit -m "commit message here"
git push -u origin master
git status
```

# HTML and CSS
My HTML experience goes way back to when I was about 11 years old! I didn't realize I was coding at the time as an online gaming community I was in to allowed for customizable webpages using HTML, but I believe it sparked my ambition to code. That being said, a lot has changed since then and now. Aside from some brief time spent with refresher tutorials during the summer, I've never technically dealt with HTML5 or CSS before so it did take some time to get a feel for the environment, but overall the process ran fairly smoothly. I'm still actively trying to get used to how CSS selectors interact with the HTML element attributes, but with time I'm confident I'll understand how these are applied more clearly. 

Below are coding snippets where I tried to get my top navbar links to appear as white, but this refused to work until I added an "!important" tag following the "white" label.

```
<div class="navbar-nav ml-auto">
  <a class="nav-item nav-link active" href="index.html">Home</a>
  <a class="nav-item nav-link" href="bonvine.html">Bo & Vine</a>
  <a class="nav-item nav-link" href="8ozburger.html">8oz Burger</a>
  <a class="nav-item nav-link" href="cheesystuffed.html">Cheesy Stuffed Burgers</a>
</div>
 ```
 
 ```
 a {
    color: white !important;
    text-decoration: underline;
}
```

# Bootstrap
Bootstrap was the most difficult process to navigate for this assignment. I had not previously heard of bootstrap so I had to take some time just to understand what it was and how it worked (I used [thenewboston bootstrap tutorial videos for this!](https://www.youtube.com/watch?v=qIULMnbH2-o&list=PL6gx4Cwl9DGBPw1sFodruZUPheWVKchlM)). Eventually I understood it is designed to be a simple solution to building websites that are compatible on small smartphone screens all the way to widescreen desktops. I also initially downloaded bootstrap files [here](https://getbootstrap.com/docs/4.0/getting-started/download/) and eventually found out it could be pushed directly to the html file through a series of links that I found scattered around on the bootstrap site [here](http://getbootstrap.com/docs/4.1/getting-started/introduction/) and [here](http://getbootstrap.com/docs/4.1/getting-started/download/) to result in:

```
<!--Bootstrap CSS-->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js"></script>
```

When playing with the functionality of bootstrap, I did find it bewildering that if I used Bootstrap 4, it actually broke my initial code, whereas Bootstrap 3 did not. I had to build some workarounds to fix this issue (as mentioned above using "!important" to make my link text white), but eventually I became accustomed to bootstrap and its functionality enough to where I was able to complete a website I was happy with! I also took advantage of a few of the bootstrap components, such as [Navbar](https://getbootstrap.com/docs/4.0/components/navbar/), [Jumbotron](https://getbootstrap.com/docs/4.0/components/jumbotron/), and [Alerts](https://getbootstrap.com/docs/4.0/components/alerts/), the latter I used to open up my index.html page with a green alert on the top of the screen using the following code:

```
<div class="alert alert-success alert-dismissible fade show" role="alert">
    You've found the best burger-related content in the world!
    <button type="button" class="close" data-dismiss="alert" aria-label="Close">
        <span aria-hidden="true">&times;</span>
    </button>
</div>
```
