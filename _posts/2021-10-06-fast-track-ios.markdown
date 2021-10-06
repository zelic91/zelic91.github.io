---
layout: post
title: "Fast track iOS"
date: 2021-09-21 22:32:21 +0700
categories: iOS
image: /assets/img/fast3.jpeg
---

Everytime I'm back to create new thing with iOS, it's quite tough to search for a set of necessary libraries. I'm too lazy for that, so in this post I will try to cover my basic setup that I would use for all of my iOS apps.

<!--more-->

# 1. Setup

We will have to create a folder set to cope with the architecture. At this point, I stick with MVVM, but I don't want to use the commonsense folder set. Open the terminal (I use iTerm) and run the following command:

{% highlight bash %}
mkdir -p Resources Common Features Resources/Fonts Common/Persistence Common/Network Common/Design
{% endhighlight %}

Feel free to add more folders as you wish. Remember to add those folders in your XCode as groups, you just need to add the root folders.


# 2. Dependencies

We will use Cocoapod to manage the dependencies. Still in the terminal:

{% highlight bash %}
pod init && vim Podfile
{% endhighlight %}

Add following dependencies (versions may change in future):

{% highlight ruby %}

pod 'Alamofire', '~> 5.4'

pod 'Firebase/Analytics'
pod 'Firebase/Messaging'
pod 'Google-Mobile-Ads-SDK'

pod 'SnapKit', '~> 5.0.0'

pod 'SQLite.swift', '~> 0.13.0'

pod 'SwiftyBeaver'

{% endhighlight %}

Then of course, install the dependencies:

{% highlight bash %}
pod install
{% endhighlight %}

# 3. Base classes

I will finish this part later.

# 4. Conclusion

Now you have a nice place to start playing with your idea.