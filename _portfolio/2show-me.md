---
layout: post
title: Show Me The TV!
thumbnail-path: "img/show-me/show-me-the-tv.png"
short-description: Show Me The TV! is a web app to search for information on TV Shows and Movies using The Movie Database API
---

{:.center}
![]({{ site.baseurl }}/img/show-me/main.PNG)

## Technologies Used

- NodeJS
- ExpressJS
- Axios
- The Movie Database API
- ES6
- ReactJS
- Material-UI
- Webpack
- Babel
- Jest
- Enzyme
- Supertest
- Amazon Web Services (Elastic Beanstalk)

[Deployed to AWS](http://showmethetv-env.uqrc23xqzq.us-east-2.elasticbeanstalk.com/)

[Click here to view the GitHub repo](https://github.com/tdfranklin/show-me-the-tv)

## Explanation

Show Me The TV is a web application that can be used to search and view information about TV Shows and Movies. It is a Full Stack Javascript web application built with Node/Express on the back end and React on the front end and deployed using AWS Elastic Beanstalk. It queries The Movie Database API using Axios to get information on the shows and it is styled using Material UI (React components that implement Google's Material Design).

## Problem

My first, and by far the biggest, problem was Webpack. It's such a complex tool and has so many various plugins and settings. I probably spent close to half of my time simply trying to get my webpack config set up correctly (and I'm sure there are still many optimizations that could be made).

First, I wanted to make sure there was a clear separation between my back end API and the front end view.  Second, I was having issues making API calls on the front end. Since the front and back were running on separate servers/ports, when I was making calls, it was calling the wrong port.  Lastly, I was using the webpack dev server to run my front end on port 8080 (the default port for WDS) on one terminal and running my back end Express server on port 3000 on another, but was quickly getting frustrated with having to stop both servers, create a build, then restart the servers to see changes made on the back end.

## Solution

I was finally able to get Webpack and the Dev Server working as I wanted after a lot of trial and error, reading the Webpack docs and various articles, and watching several videos on YouTube of developers setting up Webpack.

For my first problem, I discovered that when you export your config file for Webpack, you can send it an array of multiple objects, so I used that to my advantage to have it bundle two files - one for the front end and one for the back end.  As for the API call issue, I had to go implement proxy settings in the webpack config for the dev server and this fixed the port issue.  Finally (and what saved me the most headaches), I was able to resolve the issue. A few minor tweaks to my webpack config, adding nodemon as a dev dependency, and then creating a new start script got everything up and running just how I wanted!

{% highlight javascript %}
//package.json
"scripts": {
    "server": "nodemon ./server/server.js",
    "devstart": "npm run server & webpack-dev-server --mode development --open --hot"
}

//webpack.cofig.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const nodeExternals = require('webpack-node-externals');

const commonModule = {
    rules: [
        {
            test: /\.js$/,
            exclude: /node_modules/,
            use: {
                loader: 'babel-loader'
            },
        },
    ],
};

module.exports = [
    {
        entry: path.resolve(__dirname, 'src', 'index.js'),
        output: {
            path: path.resolve(__dirname, 'public'),
            filename: 'app_bundle.js',
        },
        module: commonModule,
        plugins: [
            new HtmlWebpackPlugin({
                template: './src/index.html'
            })
        ],
        devServer: {
            contentBase: path.join(__dirname, '/public'),
            watchContentBase: true,
            proxy: [
                {
                    context: ['/api'],
                    target: 'http://localhost:3000',
                    secure: false,
                },
            ],
        }
    },
    {
        entry: path.resolve(__dirname, 'server', 'server.js'),
        output: {
            path: path.resolve(__dirname, 'public'),
            filename: 'server_bundle.js',
        },
        module: commonModule,
        target: 'node',
        externals: [nodeExternals()],
    },
];
{% endhighlight %}

## Problem

Next, I was getting compile errors with trying to use ES6 arrow functions (something I thought was supposed to be handled by babel's presets env and react).

## Solution

A quick Google search and I discovered this was resolved by simply adding a plugin (babel-plugin-transform-class-properties) to the .babelrc file:

{% highlight javascript %}
//.babelrc
{
    "presets": ["env", "react"],
    "plugins": [
        "babel-plugin-transform-class-properties"
    ]
}
{% endhighlight %}

## Results

I'm pretty happy with how well the app turned out given the short amount of time I was able to spend on it (~15 hours). In the future, I would like to add a few additional features to the application, like seeing related shows/movies and allowing users to login and save favorites and get recommendations based on favorites.

Home Screen/Popular TV Shows
{:.center}
![]({{ site.baseurl }}/img/show-me/main.PNG)

Searching for TV Shows
{:.center}
![]({{ site.baseurl }}/img/show-me/search.PNG)

More Info (TV Show)
{:.center}
![]({{ site.baseurl }}/img/show-me/tv-modal.PNG)

Popular Movies
{:.center}
![]({{ site.baseurl }}/img/show-me/movies.PNG)

More Info (Movie)
{:.center}
![]({{ site.baseurl }}/img/show-me/movie-modal.PNG)

## Conclusion

One of the biggest lessons I learned is just how much work went into CREATE-REACT-APP and why it is so popular when using React.  Still, I'm glad I took the time to really dive into Webpack and Babel and learn as much about it as I did.  Also, this is my first project using an external API so heavily and I learned quite a lot from that as well.