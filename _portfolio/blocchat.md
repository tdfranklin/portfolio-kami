---
layout: post
title: BlocChat
thumbnail-path: "img/blocchat/blocchat.png"
short-description: BlocChat is a chat room where you can create your own room's and send messages.
---

{:.center}
![]({{ site.baseurl }}/img/blocchat/chat3.png)

## Explanation

BlocChat is the first Angular program I created pretty much on my own.  As part of the Bloc curriculum, they simply gave me assignments on what functionality they wanted the application to have and what technologies to use and it was up to me to create it however I decided was best.

## Problem

I think the biggest problem for me was trying to learn new technologies while working on the project.  I had just finished refactoring another application I had created in Javascript/jQuery for Angular so the framework was still relatively new to me as well.  However, Bloc wanted me to create a backend database for the application so I was introduced to Firebase and Angularfire to handle the job.  Also, during the process, I was introduced to UI-Bootstrap that was needed to create the Modal's used to set the username and create a new chatroom.  Previously, Bloc had walked me through new technologies when introducing them and gave plenty of examples on how they worked.  However, in this project, they pretty much just gave me the link to the documentation and I was left to figure out how they worked on my own.

{:.center}
![]({{ site.baseurl }}/img/blocchat/chat2.png)

{:.center}
![]({{ site.baseurl }}/img/blocchat/chat1.png)

## Solution

Trial and Error - that was pretty much the name of the game.  I knew in my head how I wanted it to look and function, but to display that vision took a lot of trial and error.  One of the lengthier parts for me, I believe, was the HTML/CSS.  I decided to use Bootstrap to help get a cleaner layout and I hadn't used that before so adjusting to that took a little effort.  Also, design isn't really my forte nearly as much as logic, but once I got the layout working, then the actal coding wasn't too bad.  I typically had a pretty good idea on how I wanted everything to work, but more than once the interraction between Angular and Firebase caused me delays.

On particularly challenging bit of code was adding the functionality to delete chat rooms once they were created.  Luckily, my mentor, Allen, and I sat down one afternoon and figured it out.

{% highlight javascript %}
var ref = firebase.database().ref().child('rooms');

var remove = function(id){
    var removalTarget = $firebaseObject(ref.child(id));
    removalTarget.$remove().then(function() {
    console.log("removed");
    });
};
{% endhighlight %}

With that, it was simply a matter of creating a function on the controller to call that and then creating an ng-click in the HTML.

## Results

In the end, I was able to create a mostly functional chat program.  Due to time constraints, I didn't finish all of the functionality I wanted to add or get everything looking just like I wanted, but it is fully functional.  As my first true creation on my own, I am quite happy with how it turned out.  And while there are many more things I would want to add or change before releasing to the public, I can say that I am proud of it.

## Conclusion

In conclusion, while there were certainly challenges along the way and quite a few things that were frustrating - in the end I learned so much.  I now feel much more confident about Angular, I have a strong understand of Firebase (and Angularfire), and also got a lot of practice with UI design using Bootstrap.  The application isn't perfect, but the knowledge I gained when creating it will carry forward to my next project and I am much stronger developer because of it.