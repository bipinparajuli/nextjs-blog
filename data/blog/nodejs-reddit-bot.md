---
title: 'Creating and Deploying a Reddit Bot in Node.js: A Step-by-Step Guide'
date: '2023-10-08'
tags: ['reddit', 'Nodejs', 'bot', 'reddit bot', 'aws']
draft: false
summary:
images: [/static/images/node-reddit-banner.jpg]
---

# Creating and Deploying a Reddit Bot in Node.js: A Step-by-Step Guide

Reddit is a thriving online community with millions of users discussing a wide range of topics. Reddit bots are automated scripts that can perform various tasks, such as posting comments, upvoting or downvoting content, and more. In this tutorial, we will learn how to create a simple Reddit bot using Node.js, interact with the Reddit API, and deploy it to a cloud platform like AWS.

<img src="/static/images/node-reddit-banner.jpg" />

## Table of contents

- Prerequisites
- What the final bot would look like
- Registering bot on Reddit
- Creating app on nodejs

  - Importing and Installing snoowrap package
  - Posting to the subreddit from our bot
  - Adding replies to comments on the subreddit from the bot

- Deploying bot to AWS
- Conclusion

## Prerequisites (Tools Used)

Before we dive into creating our Reddit bot, make sure you have the following tools and knowledge in place:

- You should have Node.js installed on your computer. You can download it from the official Node.js [website](https://nodejs.org/en/download).

- To access the Reddit API, you must create a Reddit developer account.

- You'll need an AWS account to deploy your bot. Sign up for an account if you don't have one.

## What the Final Bot Will Look Like

Our Reddit bot will add a post to the subreddit and add comments to posts with a predefined message on the subreddit. Here's an overview of what the final bot will do:

**Authentication:** The bot will authenticate itself with the Reddit API using the API credentials obtained from your Reddit developer account.

**Adding a post to a Subreddit:** The bot will add a new post to the subreddit with predefined text.

**Replying to comments on the subreddit:** When a new comments is detected on the subreddit, the bot will post a predefined message as a reply to a comment.

Now, let's explore the steps to create and deploy this bot.

## Registering a bot on Reddit

Before directly jumping into code you need to register your bot in Reddit. To register your bot you need to sign in to Reddit from the account from which you would like to operate your Reddit bot. After signing in go to the [link](https://www.reddit.com/prefs/apps/) and click on Create App. Then you are prompted to enter the app name, then select the button script, and enter a redirect URL that can be any dummy URL for personal use. You can leave other fields blank as they are optional.

<img src="/static/images/reddit-create-app.jpg" />

After successfully creating an app you will get a client_id(personal-use-script) and secret which are required to authenticate your bot.

<img src="/static/images/reddit-create-app-success.jpg" />

## Creating app on nodejs

For this tutorial we are using Nodejs to create a Reddit bot but above process remains the same for any language. Initialize a brand new Nodejs project with npm init. Reddit offers a comprehensive set of APIs that allow you to interact with its platform programmatically. Nodejs provides a Reddit API wrapper snoowrap which interacts with Reddit API in the easiest way.

### Importing and Installing snoowrap package

Snoopwrap is a fully-featured JavaScript wrapper for the Reddit API. It provides a simple interface to access every Reddit API endpoint. To include snoopwrap into the project install it using

```

npm install snoowrap --save

```

Now it's a time to create a separate .env file to store Reddit API credentials. These include client_id and client_secret from the Reddit app and the user name and password of your Reddit account, user agent is just a random name given to your project.

<img src="/static/images/reddit-env.jpg" />

After defining credentials to the .env file pass it to snoopwrap constructor by creating index file in root folder to authenticate Reddit API.

```

import Snoowrap from 'snoowrap';

const reddit = new Snoowrap({

    userAgent: process.env.USER_AGENT,

    clientId: process.env.CLIENT_ID,

    clientSecret: process.env.CLIENT_SECRET,

    username: process.env.USER_NAME,

    password: process.env.PASSWORD,

});

```

### Posting to the subreddit from our bot

After authenticating the Reddit API you can easily get a subreddit using the Reddit instance.getSubreddit(‘subreddit name’) gives the subreddit you passed. After that, you can chain with submitSelfPost with the title and text to post to the subreddit. Simply pass a title and text you want to post a subreddit on the submitSelfpost method.

```

reddit.getSubreddit('test').submitSelfpost({

    title:"my demo title",

    text:'my demo text',

})

```

The above code calls to the reddit.getSubreddit('test').submitSelfpost() method. This method takes an object as an argument, which specifies the title and body of the self-post. The title and text properties of the object specify the title and body of the self-post, respectively. Once the code is executed, the self-post will be submitted to the subreddit test.

Now if you run the application and visit [https://www.reddit.com/r/test/](https://www.reddit.com/r/test/) subreddit you can see a post from the bot with title my demo title and my demo text.

<img src="/static/images/reddit-post.jpg" />

### Adding replies to comments on the subreddit from the bot

Replying to a comment on a subreddit is as simple as creating a post to a subreddit.

```

reddit.getSubreddit('test').getNewComments().then(comments=>{

    comments.map(comment=>{

    comment.reply('test reply from bot')

    })

})

```

The code starts with a call to the reddit.getSubreddit('test').getNewComments() method. This method returns a promise that resolves to a list of the newest comments in the subreddit test. The .then() method is used to handle the promise returned by the getNewComments() method. The callback function passed to the .then() method is executed when the promise is resolved. The callback function takes a list of comments as an argument. The callback function iterates over the list of comments and replies to each one with the message test reply from the bot.

Now if you run the application and visit https://www.reddit.com/r/test/ subreddit you can see a reply to a comment from the bot with a text test reply from the bot.

<img src="/static/images/reddit-reply.jpg" />

## Deploying bot to AWS

For this tutorial, we will be deploying a bot to aws lambda. AWS Lambda is a serverless, event-driven compute service that lets you run code for virtually any type of application or backend service without provisioning or managing servers. Below is a step-by-step procedure to deploy the Reddit bot on aws lambda:

- **Create an AWS account:** If you don't already have an AWS account, you can create one for free at [https://aws.amazon.com/](https://aws.amazon.com/).
- **Create a Role:** These roles will provide write permissions to CloudWatch Logs and interact with other AWS services. Go to the IAM console at https://console.aws.amazon.com/iam/. Click on Roles and click on Create Role. Select the AWS service role radio button after that select lambda and after clicking next choose role AWSLambdaBasicExecutionRole and AWSXRayDaemonWriteAccess. Then give a name to the role and click on the create role button.
- **Create lambda function:** To deploy the bot to lambda go to https://eu-north-1.console.aws.amazon.com/lambda/home and click on Create lambda, then select create from scratch, give function name, and select Nodejs version on runtime option, then on permission select to use an existing role and choose one that you have created before. Then click on the Create function.

<img src="/static/images/create-function.jpg" />

- **Deploy your bot to AWS Lambda:** After creating the lambda function next step is to deploy the lambda function. For deploying you will need to upload a zip of the code that you have written in the code section. Then select the handler that is the entry point for code execution in my case it is index.handler. You will also need to define the handler in code. Wrap the whole bot logic that needs to be executed on the handler function.

```

export const handler = async (event, context) => {

// Your code here

};

```

After defining the handler you will need to set up the environment variable from the configuration tab on your function. Once your bot is deployed, you will need to configure it to run on a schedule. To do this, click on the Triggers tab for your Lambda function and click on the Add Trigger button. Select CloudWatch Events and enter a rate expression in the Schedule expression field. For example, to run your bot every minute, you would enter the rate(1 minute). Click on Add to finish configuring the trigger. After doing this your bot should function properly. You could test it from the test section or by monitoring on cloud watch logs.

<img src="/static/images/deploy-function.jpg" />

## Conclusion

Creating a Reddit bot in Node.js is a fun and educational project that allows you to interact with a vibrant online community. In this tutorial, we've covered the basic steps to create a simple Reddit bot, and you can further enhance it by adding more features.

To get started, check out the GitHub repository for the complete source [code](https://github.com/bipinparajuli/node-reddit) of the bot created in this tutorial. Feel free to explore the Reddit API documentation for more advanced features and possibilities.
