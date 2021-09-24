---
layout: post
title: "Fast track NodeJS"
date: 2021-09-21 22:32:21 +0700
categories: nodejs
image: /assets/img/fast2.jpeg
---

In this post, let's start very quickly on a NodeJS project. Of course we will have enough functionalities to work: express, log, jwt, database, ...

<!--more-->

# 1. Setup

I use MacOS with `zsh` installed, so I would use `take` instead of mkdir && cd:

{% highlight bash %}
take project_name && npm init && touch index.js && touch router.js
{% endhighlight %}

Add following folders to build the basic structure:

{% highlight bash %}
mkdir db handlers middlewares models
{% endhighlight %}

# 2. Dependencies

We will need quite a few dependencies:

{% highlight bash %}
yarn add body-parser connect-history-api-fallback cors dotenv express express-fileupload jsonwebtoken morgan sequelize
{% endhighlight %}

Add nodemon for smoothier development experience:

{% highlight bash %}
yarn add --save-dev nodemon
{% endhighlight %}

# 3. The famous index.js

Let's get started with some stuff in index.js:

{% highlight javascript %}

require("dotenv").config();
const express = require("express");
const fileUpload = require('express-fileupload');
const morgan = require("morgan");
const history = require("connect-history-api-fallback");
const bodyParser = require("body-parser");
const cors = require("cors");
const app = express();
const apiRouter = require("./router");

// Logging
app.use(morgan("tiny"));

// Enable file upload
app.use(fileUpload({
    createParentPath: true
}));

// JSON
app.use(bodyParser.json());

// FormData
app.use(
    bodyParser.urlencoded({
        extended: true,
    })
);

// CORS
app.use(cors());

// Serve static files
app.use(express.static("public"));

// Port
const port = process.env.PORT || 3200;

// Landing page
app.get("/", (req, res) => {
    res.send("This is my voice, clear and strong.");
});

app.use("/api", apiRouter);

app.listen(port, () => {
    console.log(`App is now running on port ${port}.`);
});

{% endhighlight %}

# 4. The less famous router.js

Add following initial stuff to router.js

{% highlight javascript %}
const express = require('express');
const router = express.Router();
const middleware = require('./middlewares/middleware');

router.post('/sign_in', authHandlers.signIn);

module.exports = router;
{% endhighlight %}

# 5. And start it

{% highlight bash %}
yarn start
{% endhighlight %}