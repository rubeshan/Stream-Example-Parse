GetStream.io, parse cloud code & EmberJS
========================================

This example helps you create activity streams & newsfeeds with [Parse Cloud Code](https://parse.com/docs/cloud_code_guide) and [GetStream.io](https://getstream.io).

###Activity Streams & Newsfeeds

![](https://dvqg2dogggmn6.cloudfront.net/images/mood-home.png)

What you can build:

* Activity streams such as seen on Github
* A twitter style newsfeed
* A feed like instagram/ pinterest
* Facebook style newsfeeds
* A notification system

###Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Activity Streams & Newsfeeds](#activity-streams-&-newsfeeds)
- [Table of Contents](#table-of-contents)
- [Gem installation](#gem-installation)
- [Setup](#setup)
- [Model configuration](#model-configuration)
  - [Activity fields](#activity-fields)
  - [Activity extra data](#activity-extra-data)
- [Feed manager](#feed-manager)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### About Parse Cloud & GetStream.io

Parse Cloud allows you to write server side application logic and extend the functionality of your existing Parse installation. GetStream.io allows you to build scalable newsfeeds and activity streams. This example app shows how you can get them to work together.

The example app is based on [EmberJS](http://emberjs.com/) and showcases a Twitter/Facebook style community.

### Tutorial

#### Installing the Stream library

The Parse Cloud ecosystem is unique. You can find the adapted client for getstream.io in [cloud/getstream.js](https://github.com/tschellenbach/Stream-Example-Parse/blob/master/cloud/getstream.js).
The documentation for the client can be found on [getstream.io/docs](https://getstream.io/docs/)

```
// initialize the getstream.io client
var stream = require('cloud/getstream.js');
var client = stream.connect(settings.streamApiKey, settings.streamApiSecret, settings.streamApp);
```

The easiest way to get started is to copy over this entire repo.

#### Setting up the backend & syncing to getstream.io

To build your activity stream you need to notifity getstream.io of 2 things:

1. When activities are added/removed
2. When follow relationships are changed

If you haven't tried out getstream.io before I recommend you try the getting started first:
https://getstream.io/get_started/#intro
The interactive API tutorial will get you up to speed in a few minutes.

The code to notify GetStream.io of these events can be found in cloud/main.js
For each model defined in settings.activityModels we'll listen to the Parse.Cloud.afterSave and Parse.Cloud.afterDelete methods.

Furthermore we also listen to the changes in settings.followModel and sync the changes.

#### Creating activities

Now that our backend is correctly setup we can create activity's via Parse and they will get automatically send to getstream.io

```
// define your parse models
Activity = Parse.Object.extend("Activity");
Tweet = Activity.extend("Tweet");
var tweet = new Tweet();
tweet.set('actor', Parse.User.current());
tweet.set('verb', 'tweet');
tweet.set('tweet', 'Happy times');
tweet.save()
```

Now when you call tweet.save the Parse object is stored. You can verify by visiting the parse data browser.
If you open your GetStream.io data browser you'll see the newly created feed containing the first activity.

#### Reading feeds

You can read feeds by using a Parse.Cloud.run('feed') call, for example:

```
var promise = Parse.Cloud.run('feed', {
	feed : 'user:all'
});
```

### Running this example app

#### Settings

If you want to install this example app on your own parse instance you'll need to update a few setting files

0. signup for parse, getstream.io and create a github app (for the github login)
1. edit cloud/settings.js
2. edit config/global.json
3. edit public/index.html (the inline config variable)

#### Dev tools

0. install gulp, compass and bower
1. bower install
2. gulp
3. parse develop <your application name>

gulp will monitor for changes and livereload the index.html file
parse develop will upload your cloud code and host it at <yourconfiguredvalue>.parseapp.com.





