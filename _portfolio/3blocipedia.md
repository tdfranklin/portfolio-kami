---
layout: post
title: Blocipedia
thumbnail-path: "img/blocipedia/blocipedia.png"
short-description: Blocipedia is a Wikipedia-lite replica.
---

{:.center}
![]({{ site.baseurl }}/img/blocipedia/blocipedia-home.PNG)

## Technologies Used

- Rails
- Heroku
- Bootstrap
- Devise
- Pundit
- Stripe
- RedCarpet
- RSpec
- Shoulda
- Faker
- Factory Girl
- Figaro

[Click here to view application](https://blocipedia-tdf.herokuapp.com/)

[Click here to view the GitHub repo](https://github.com/tdfranklin/blocipedia)

## Explanation

My second foray into Rails was an entirely different experience.  With Bloccit, it felt like a bit of a guided tour with the Bloc program holding my hand through a lot of the process.  However, with Blocipedia, I was given the keys to the kingdom and pretty much left to my own devices.  Bloc told me what kind of application they wanted built and provided the wireframes for me to get started as well as giving me some advice on which gems I could use to perform certain tasks, but other than that I was given complete freedom to build as I saw fit.

## Problem

Setting up the basic CRUD was very simple for the most part and I didn't have any real challenges with that.  However, I used the Devise gem for authentication so I ran into a big challenge in regard to testing.  I set up Devise so that you had to verify your email to create an account and I set up the site so that you could not access anything but the homepage until you set up an account.  Well, I had a constant issue with tests failing because I could not figure out how to allow a user to skip confirming their email address when running tests.

## Solution

After a lot of time spent Googling and researching how the confirmation works with Devise, the solution actually ended up being quite simple.

{% highlight ruby %}
FactoryGirl.define do
  factory :user do
    name 'Bob'
    sequence(:email){|n| "user#{n}@factory.com"}
    password 'helloworld'
    password_confirmation 'helloworld'
    confirmed_at Time.now
  end
end
{% endhighlight %}

The key line being setting the "confirmed_at" to any time.  After doing a lot of reading and looking at how Devise stores the user information in the database, turns out there's just a column called "confirmed_at" that get's set to the current time when the user clicks on the link in the email.  So when I set up the factory to create a user, by manually setting that, it flagged the account as confirmed and then I was able to simply log the user in to perform the tests:

{% highlight ruby %}
let(:my_user) { create(:user) }

before do
  sign_in my_user
end
{% endhighlight %}

## Problem

I think my biggest problem I had developing this application was with the design and layout.  This was my first time really seeing the big flaw with Bootstrap in that it can be quite a challenge differentiating one Bootstrap application from another.  One of my first iterations of the layout for Blocipedia had it looking strikingly similar to Bloccit and I knew I needed to dive a little deeper into Bootstrap to make sure I could make Blocipedia look more unique.

## Solution

To solve the issue, I just had to take a little time and experiment with various Bootstrap classes and practice a little more with their grid system.  My first order of business was getting the functionality working.  I kept the view fairly simple at first and got my data structure, model, and controller's up and running.  Once that was functional, I began to customize the view more.  My wife was actually a big help here.  While I was developing, I had her take a look at it to get another person's perspective on what she did or didn't like.  Her first question to me was
<blockquote><em>What is a wiki?</em></blockquote>

I actually had to think about it for a minute to figure out how to explain it to her... this led to a Google search.  What was the actual definition of a wiki?  Then I knew I had my homepage.

{% highlight html %}
<p class="list-group-item list-group-item-info">
    <big><strong>wi&#8729ki</strong></big> <br />
    /'wik&#275/ <br />
    <em><small>noun</small></em> <br />
    a website that allows collaborative editing of its content and structure by its users.
</p>
{% endhighlight %}

From here, the rest of the layout started coming naturally.  I just assumed that people would know what a wiki was or how to write Markdown because I did.  I quickly threw that assumption aside and got to work.

## Results

The end result came together quite well I think.  It was simple, yes, but I tried to have the application explain itself as it went for those who may not know what a wiki was or how to write Markdown.

User Sign Up/Sign In Page
{:.center}
![]({{ site.baseurl }}/img/blocipedia/devise.PNG)

User Profile Page
{:.center}
![]({{ site.baseurl }}/img/blocipedia/profile.PNG)

User Edit Page
{:.center}
![]({{ site.baseurl }}/img/blocipedia/user-edit.PNG)

Upgrade Account Page
{:.center}
![]({{ site.baseurl }}/img/blocipedia/upgrade.PNG)

Wiki Index (Only shows Wikis you have access to)
{:.center}
![]({{ site.baseurl }}/img/blocipedia/user-home.PNG)

New Wiki
{:.center}
![]({{ site.baseurl }}/img/blocipedia/wiki-new.PNG)

Wiki Show
{:.center}
![]({{ site.baseurl }}/img/blocipedia/wiki-view.PNG)

## Conclusion

This was a great experience for me.  When I was developing BlocChat, I didn't have nearly as much confidence as a developer and struggled quite a bit on design since I didn't really have a vision of what I wanted the application to look like.  With Blocipedia however, I really learned how to shape a vision for an application and then bring it to life.