---
layout: post
title: Newton's Apple
thumbnail-path: "img/newtons-apple/newtons-apple.png"
short-description: Newton's Apple is a CLI to generate boilerplate code for React components
---

{:.center}
![]({{ site.baseurl }}/img/newtons-apple/main.PNG)

## Technologies Used

- NodeJS
- NPM
- ES6
- Commander
- Inquirer
- Configstore
- fs-extra

[Published to NPM](https://www.npmjs.com/package/newtons-apple)

[Click here to view the GitHub repo](https://github.com/tdfranklin/newtons-apple)

## Explanation

Newton's Apple is a Command Line Interface(CLI) tool to simplify component creation for developers working on ReactJS projects. Running a simple command, it will create a file with the name you specify and generate the boilerplate code for a typical React component.

## Problem

Up until this project, I had only used Ruby for command line tools.  I briefly considered using Ruby to create Newton's Apple, but quickly decided that since this was going to be strictly for developers using React, a Javascript framework, that it would be best for the CLI to be written in Javascript as well.  That way, any developer who was using the tool would easily be able to look at the source code, understand what was going on, and even make modifications for themselves without needing to know another language.  And thus, the biggest problem I had with this project was learning how to create a CLI with Javascript.

## Solution

And Node came to the rescue!  I had only used Node, with Express, as a backend server before when creating Retrocade, so figuring out how to get it to work as a CLI took a little research.  Lucky for me, there are some wonderful developers out there who took the time to write up some guides that really got me moving in the right direction:

* [Building Command Line Tools with Node.js](https://developer.atlassian.com/blog/2015/11/scripting-with-node/)
* [Writing Command-Line Applications in NodeJS](https://medium.freecodecamp.org/writing-command-line-applications-in-nodejs-2cf8327eee2)
* [Creating a Project Generator with Node](https://medium.com/northcoders/creating-a-project-generator-with-node-29e13b3cd309)

Turns out, it's pretty much as easy as putting this at the top of your entry file:

{% highlight javascript %}
#!/usr/bin/env node
{% endhighlight %}

Then, to give your app a command, simply add this to your package.json file:

{% highlight javascript %}
"bin": {
    "command": "./path/to/entryFile.js"
},
{% endhighlight %}

With those simple instructions, that was pretty much all I needed to get going!

## Problem

Next, I needed to figure out what tools I wanted to use in the project.  I knew in the back of my mind that I wanted to keep it as lightweight as possible, but at the same time it just didn't make sense not to take advantage of some great tools that other developers were providing to the community.

## Solution

* [Commander](https://www.npmjs.com/package/commander)
* [Inquirer](https://www.npmjs.com/package/inquirer)

I saw that both of these were being in many projects similar to mine, so after reading through the docs and examples, I knew they would both be essential for what I wanted to do with Newton's Apple.

With Commander, I could not only easily parse arguments passed back from the user, but also build a nice help guide at the same time.  Commander also allows for very clean, easy to read code.

{% highlight javascript %}
const program = require('commander');

program
    .version(pkg.version, '-v, --version')
    .command('new <component-name>')
    .description('create new component in either current directory or provided path')
    .option("-a, --all", "enable all methods")
    .option("-n, --none", "disable all methods")
    .option("-c, --create", "creates directories if they don't exist")
    .option("-o, --overwrite", "overwrites file if it exists")
    .action( (component, options) => {
        if (options.all)
            changeAllSettings(true);
        if (options.none)
            changeAllSettings(false);
        createComponent(component, options.create, options.overwrite, conf.all)
    });
{% endhighlight %}

Inquirer, on the other hand, helps by allowing the application to "communicate" with the user by asking them questions.  I wanted to allow the user to select from a list of lifecycle methods that they wanted to include and Inquirer delivers!

Simply passing a few lines of code:

{% highlight javascript %}
const inquirer = require('inquirer');

const QUESTIONS = [
    {
        type: 'checkbox',
        message: 'Choose lifecycle methods',
        name: 'methods',
        choices: [
            'componentWillMount',
            'componentWillReceiveProps',
            'shouldComponentUpdate',
            'componentWillUpdate',
            'componentDidMount',
            'componentDidUpdate',
            'componentWillUnmount',
            'componentDidCatch'
        ]
    }
]

inquirer.prompt(questions).then((answers) => {
    changeAllSettings(false);
    for (let answer of answers.methods) {
        conf.set(answer, true);
    }
});
{% endhighlight %}

Will provide you with a nice, easy list to select from when the command is passed:

{% highlight javascript %}
? Choose lifecycle methods (Press <space> to select, <a> to toggle all, <i> to inverse selection)
❯◯ componentWillMount
 ◯ componentWillReceiveProps
 ◯ shouldComponentUpdate
 ◯ componentWillUpdate
 ◯ componentDidMount
 ◯ componentDidUpdate
 ◯ componentWillUnmount
(Move up and down to reveal more choices)
{% endhighlight %}

## Problem

The next problem I encountered was how to persist settings for a user.  I did not want a user to have to select which lifecycle methods they wanted to include everytime they created a component.  Also, I already had more advanced features I wanted to include in the future, so I needed a way to save options.

## Solution

* [ConfigStore](https://www.npmjs.com/package/configstore)

Enter ConfigStore - the perfect solution to my problem.

{% highlight javascript %}
const Configstore = require('configstore');

const conf = new Configstore('napp-config', {
    componentWillMount: true,
    componentWillReceiveProps: true,
    shouldComponentUpdate: true,
    componentWillUpdate: true,
    componentDidMount: true,
    componentDidUpdate: true,
    componentWillUnmount: true,
    componentDidCatch: true
});
{% endhighlight %}

Now upon install, a small text file is created on the user's computer that will save config options.  Plus, ConfigStore offers a host of methods that allow for easy CRUD updates of those options.

## Problem

The last problem I encountered was one I struggled with weather to write methods myself or use a library.  When creating these files for components, the fs module built into Node was great for creating a file in the current directory, a directory that already existed, or even creating a directory and a file inside that directory.  But once I started trying to pass a long path that didn't exist, I quickly ran into issues.

## Solution

* [fs-extra](https://www.npmjs.com/package/fs-extra)

While I certainly could have called Node's fs functions recursively and written in error checking myself, it seemed silly when such a great library exists.  This is actually a direct replacement for the default fs as you can call the fs methods from this library as well.  And it's as easy as:

{% highlight javascript %}
const fs = require('fs-extra');

fs.pathExists(file, (err, exists) => {
    if (err) {
        console.error(err.message);
        return;
    }
    if (exists && !overwriteFile) {
        console.error('A file with that name already exists.  Rerun the command with -o or--overwrite to overwrite the file');
        return;
    } else {
        if (createDir) {
            fs.outputFile(file, template, (err) => {
                if (err)
                    console.error(err.message);
                    return;
            });
        } else {
            fs.writeFile(file, template, (err) => {
                if (err)
                    if (err.code === 'ENOENT') {
                        console.error('That path does not exist.  Rerun the command with-c or --create to create the path');
                        return;
                    } else {
                        console.error(err.message);
                        return;
                    }
            });
        }
    }
});
{% endhighlight %}

## Conclusion

Overall, I am very happy with the current progress of Newton's Apple.  I only released version 1.0.0 a couple of weeks ago and I already have over 600 downloads and 9 stars on Github, so I feel confident that it's a tool people want to use.  I still have big plans to add Redux boilerplate, custom methods, and default file structure support so I believe it will continue to grow.