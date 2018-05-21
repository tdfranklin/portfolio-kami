---
layout: post
title: Bloccit
thumbnail-path: "img/bloccit/bloccit.png"
short-description: Bloccit is a basic forum site; a Reddit-lite replica.
---

{:.center}
![]({{ site.baseurl }}/img/bloccit/bloccit-home.PNG)

## Technologies Used

- Rails
- Heroku
- RSpec
- Shoulda
- Factory Girl
- Bootstrap
- Bcrypt
- Figaro

[Click here to view application](https://bloccit-tdf.herokuapp.com/)

[Click here to view the GitHub repo](https://github.com/tdfranklin/bloccit)

## Explanation

Bloccit is the first Rails application that I developed and was used primarily to learn the Rails methodology.  It is a fairly basic forum-type site modeled after Reddit (albeit a very simple version).

## Problem

For anyone who's taken the time to learn Rails, I don't think I really need to explain the difficulties involved.  Even if you are a master of Ruby, Rails has a pretty steep learning curve to master, although it is actually quite easy for the basics.  The problem is that if you don't take the time to understand HOW Rails actually works, you will find yourself in over your head pretty quickly.  Rails is hyper-opinionated with your file names, class names and where they are located in the file structure.  While you are able to go outside of these bounds with some effort, the benefit of staying within them allows you to build impressive applications with a shockingly small amount of code.

With this specific project, I spent a lot of time understanding the basic principles of CRUD and how it worked specifically within Rails, so by the time I got to the later parts of adding authentication, database queries, the voting and favorites model, and user roles, I had to rush through them more quickly than I would have liked.  This left me with a lot of confusion on wrapping up and putting the finishing touches on a Rails project.

## Solution

The solution to this problem was simple - take the time to really learn and understand how Rails works and how it works that Rails magic!  After I finished this project, I took about a week and deep-dived into Ruby and then Rails, trying to make sure I understood the file structure of a Rails application from top to bottom and back to front.  I played around with the various generators, using them to create controllers, models, and other various database migrations and I would study exactly what it was doing, how it was doing it, and then I would recreate a similar process manually.  I made sure to note where things were being named singularly vs plural, studied the migration files (and created some manually) to understand how Rails was interacting with the database.

## Results

The end result was a very functional forum application.  It's quite basic and there are a ton of features I would like to add to it in time, but the real victory was understanding.  Being able to navigate the seemingly complex Rails application structure and actually understand what each files purpose was, how and when to edit them, where to add future files, and more importantly - WHY to add them there was more satisfying than I can accurately describe.

Topic Index View
{:.center}
![]({{ site.baseurl }}/img/bloccit/guest-topic-index.PNG)

Guest Topic View
{:.center}
![]({{ site.baseurl }}/img/bloccit/guest-topic-view.PNG)

Member topic view (Can add new posts)
{:.center}
![]({{ site.baseurl }}/img/bloccit/user-topic-view.PNG)

Guest post view
{:.center}
![]({{ site.baseurl }}/img/bloccit/guest-post-view.PNG)

Member post view of different user's post (Can vote and favorite a post)
{:.center}
![]({{ site.baseurl }}/img/bloccit/user-post-view.PNG)

Member post view of owned post (Can edit and delete)
{:.center}
![]({{ site.baseurl }}/img/bloccit/user-post-view2.PNG)

Admin post view (Can edit and delete other user's posts)
{:.center}
![]({{ site.baseurl }}/img/bloccit/admin-post-view.PNG)

Member profile view
{:.center}
![]({{ site.baseurl }}/img/bloccit/profile-view.PNG)

## Conclusion

Most developers I've talked to either despise or love Rails and there seems to be very little middle ground on that.  It seems like more experienced developers (who've been coding for a decade or more) generally fall in the hate camp while newer developers generally are fans of Rails.  I believe this is due to the opinionated nature of Rails.  Developers who've been coding for a long time have already developed their own way of doing things and seem to have a hard time adjusting to Rails way of thinking.  I'm glad I fall into the latter category because while Rails has many downfalls, I believe it also has a lot to offer.  I'm also glad I took the time to really dive deep into Rails and learn the inner workings because it helped tremendously with my next project.