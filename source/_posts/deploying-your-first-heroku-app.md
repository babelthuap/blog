---
title: Deploying Your First Heroku App
id: 53
categories:
date: 11-10-2015 17:49:38
tags:
  - heroku
  - node
  - coding house
---

![](https://babelthuap.files.wordpress.com/2015/11/heroku_logo.png "Heroku logo")

I deployed [my first Node.js app on Heroku](https://secure-earth-5739.herokuapp.com/) last night! (Source code [here](https://github.com/babelthuap/markdown-to-html).) _After_ I figured it out, it seemed easy and straightforward. However, it took a little time to get there. I'd like to share what worked for me.

The [tutorial](https://devcenter.heroku.com/articles/getting-started-with-nodejs) on the Heroku site seems beginner-friendly, and it mostly is. There are a few key steps, however, without which your first deployment will not be easy. Here's a quick summary of what you actually need to do:

1.  Make sure your project directory is a git repo.

2.  Make sure your package.json correctly lists all your npm package dependencies.

3.  Add a "start" script to your package.json. That is, supposing your server is called app.js, add
```javascript
"scripts": {
  "start": "node app.js"
}
```

4.  Specify, in your package.json, the engine you're using. Supposing you want to use 5.0.0, you need
```javascript
"engines": {
  "node": "5.0.0"
}
```

5.  You need to be able to test your server locally and also have it work on Heroku. Set it to listen at
```javascript
process.env.PORT || 3000
```

6.  Set up Heroku Toolbelt via [these instructions](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up).

7.  Finally, deploy your app with [these commands](https://devcenter.heroku.com/articles/getting-started-with-nodejs#deploy-the-app). If all goes well, you should now have an app deployed to Heroku! To update it, you just need to commit your changes via git, then do
```bash
git push heroku master
```

_See [here](https://scotch.io/tutorials/how-to-deploy-a-node-js-app-to-heroku) for a more in-depth walkthrough._