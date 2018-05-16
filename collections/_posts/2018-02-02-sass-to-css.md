---
layout: post
title:  "What I Learned Moving from Sass to CSS"
date:   2018-02-02
categories: code sass mixin
---

So in a test build-out of a tool (built in Vue) I'm working on at the day job, I came across this [post by Tyler Gaw on his move from Sass to (post)CSS](https://tylergaw.com/articles/sass-to-postcss/). I liked what I was reading enough to give it a shot myself.

I might not have done this if not for the fact that we're moving a good portion of our front-end development into a *better* workflow with [webpack](https://webpack.js.org/).

So I guess some backstory might help here. Currently I have a handful of batch files (a different one for each individual VS site we have) that I have to run in the CLI to keep our Sass files compiled since the Visual Studio implementation of Gulp is just an absolute and abject failure. It would work for about ten minutes and then just stop, for no good reason. Annoying is an understatement. But now we have some younger blood on the team who is well versed in the latest workflows for front-end development. This has led to an influx of better methodology for how we do things here. And I for one, am happy.

Back to the topic at hand, I have converted the whole host of Sass files back into CSS using current-spec features such as custom properties (`variables` in Sass), nested rules (thanks to css-next), and left the Sass `@import` statements in place (thanks to css-import). A couple things I've learned:

A) either css-next, or webpack itself, converts `rem` values into their `px` equivalents and outputs both to the rendered css (for browser compatibility?) and...

B) such conversions are picked up (and watched) only after an initial build i.e. changing from Sass to CSS requires the `npm run dev` to be restarted (this is probably a duh).

Oh, and...

C) while we can still do nested `@media` queries inside the rules, doing so to change a declaration of a custom property requires the query to be nested inside the initial custom property, and does not work if you have a media query out by itself and have a custom property changed inside of it.

For example this...
 {% highlight css %}
:root {
  --box: {
    margin-bottom: 20px;
    padding: 15px 0;
    @media screen and (max-width: 991px) {
      padding: 15px;
    }
  };
}
{% endhighlight %}

...works just fine, while this...

{% highlight css %}
:root {
  --box: {
    margin-bottom: 20px;
    padding: 15px 0;
  };
}
@media screen and (max-width: 991px) {
  :root {
    --box: {
      padding: 15px;
    };
  }
}
{% endhighlight %}

... does not.

I'm not totally sure as to why it works that way, but I'm only just beginning on my journey (yes, far too late, perhaps) on the way to learning this *new to me* workflow. Then again, the first example is more concise and clean, so maybe that's all the reason I need.
