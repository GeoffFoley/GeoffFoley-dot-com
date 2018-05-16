---
layout: post
title:  "Color Fader"
date:   2018-04-17
categories: code sass mixin
---

The following is a Sass mixin I recently wrote to tackle an issue for a project on GitHub where they wanted to have their header background fade between a number of colors.

I set it up so a list of colors could be passed in and it wouldn't matter how many colors you wanted; the interval would always be evenly divided amongst them.

**_colorFader.scss:**
{% highlight sass %}
// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ //
//                                                                //
// mixin: colorFader                                              //
// params: $animationName, $colorList                             //
//   $animationName: The name you want to give your animation     //
//   $colorList: The list of colors you want to cycle through     //
//                                                                //
// Divides 100% by the color count to get the $interval and       //
//   increments the $frame by ($frame + $interval)                //
//                                                                //
// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ //

@mixin colorFader($animationName, $colorList) {
    $colorCount: length($colorList);
    $interval: 100 / $colorCount;

    //  Keeping this block here until @ interpolation is added
    //    Open issue: https://github.com/sass/sass/issues/429
    //  Then, either pass the prefixes in, or use the below
    //    $prefixes list
    //  Until then, each vendor block will have to be written separately
    //
    //$at: @;
    //$prefixes: "", "-webkit-", "-moz-";
    //@each $prefix in $prefixes {
    //    #{$at}#{$prefix}keyframes #{$animationName} {
    //        $frame: 0;
    //        @each $color in $colorList {
    //            #{$frame}% {background: $color;}
    //            $frame: $frame + $interval;
    //        }
    //    }
    //}

    @keyframes #{$animationName} {
        $frame: 0;
        @each $color in $colorList {
            #{$frame}% {background: $color;}
            $frame: $frame + $interval;
        }
    }

    /* Safari */
    @-webkit-keyframes #{$animationName} {
        $frame: 0;
        @each $color in $colorList {
            #{$frame}% {background: $color;}
            $frame: $frame + $interval;
        }
    }

    /* Firefox */
    @-moz-keyframes #{$animationName} {
        $frame: 0;
        @each $color in $colorList {
            #{$frame}% {background: $color;}
            $frame: $frame + $interval;
        }
    }
}
{% endhighlight %}

**Of Course...** the above vendor prefix blocks are unnecessary if you're using an auto-prefixer such as the one in css-next.

### Usage

**styles.scss:**
{% highlight sass %}
// Import the fader mixin
@import "colorFader";

// Apply to an ID (or class)
#colorFaderDIV {
    background: red;
    -webkit-animation: colorfader linear infinite;
    -webkit-animation-duration: 90s;
    -webkit-animation-iteration-count: infinite;
    -moz-animation: colorfader linear infinite;
    -moz-animation-duration: 90s;
    -moz-animation-iteration-count: infinite;
    -o-animation-iteration-count: infinite;
    color: #fff;
}

// Set the list of colors to cycle through
$colorList: #76c261 #fdc345 #f8943 #e65453 #a759a8 #32afe2;

// Call the fader, passing in the animation name and colors
@include colorFader(colorfader, $colorList);
{% endhighlight %}

**styles.css:**
{% highlight css %}
#colorFaderDIV {
    background: red;
    -webkit-animation: colorfader linear infinite;
    -webkit-animation-duration: 90s;
    -webkit-animation-iteration-count: infinite;
    -moz-animation: colorfader linear infinite;
    -moz-animation-duration: 90s;
    -moz-animation-iteration-count: infinite;
    -o-animation-iteration-count: infinite;
    color: #fff;
}

@keyframes colorfader {
    0% {background: #76c261;}
    16.66667% {background: #fdc345;}
    33.33333% {background: #f8943;}
    50% {background: #e65453;}
    66.66667% {background: #a759a8;}
    83.33333% {background: #32afe2;}
}

/* Safari */
@-webkit-keyframes colorfader {
    0% {background: #76c261;}
    16.66667% {background: #fdc345;}
    33.33333% {background: #f8943;}
    50% {background: #e65453;}
    66.66667% {background: #a759a8;}
    83.33333% {background: #32afe2;}
}

/* Firefox */
@-moz-keyframes colorfader {
    0% {background: #76c261;}
    16.66667% {background: #fdc345;}
    33.33333% {background: #f8943;}
    50% {background: #e65453;}
    66.66667% {background: #a759a8;}
    83.33333% {background: #32afe2;}
}
{% endhighlight %}

**example.html:**
{% highlight html %}
<link href="/css/styles.css" rel="stylesheet" />
...
<div id="colorFaderDIV">Example</div>
{% endhighlight %}

The above code renders the following:
<style>
#navbar
{
    width: 100%;
    background: #76c261;
    -webkit-animation: colorfader linear infinite;
    -webkit-animation-duration: 90s;
    -webkit-animation-iteration-count: infinite;
    -moz-animation: colorfader linear infinite;
    -moz-animation-duration: 90s;
    -moz-animation-iteration-count: infinite;
    -o-animation-iteration-count: infinite;
    color: #fff;
    text-align: center;
}
@keyframes colorfader {
    0% {background: #76c261;}
    16.66667% {background: #fdc345;}
    33.33333% {background: #f8943;}
    50% {background: #e65453;}
    66.66667% {background: #a759a8;}
    83.33333% {background: #32afe2;}
}

/* Safari */
@-webkit-keyframes colorfader {
    0% {background: #76c261;}
    16.66667% {background: #fdc345;}
    33.33333% {background: #f8943;}
    50% {background: #e65453;}
    66.66667% {background: #a759a8;}
    83.33333% {background: #32afe2;}
}

/* Firefox */
@-moz-keyframes colorfader {
    0% {background: #76c261;}
    16.66667% {background: #fdc345;}
    33.33333% {background: #f8943;}
    50% {background: #e65453;}
    66.66667% {background: #a759a8;}
    83.33333% {background: #32afe2;}
}
</style>
<div id="navbar">Example</div>
<br />
[You can grab the code from this Gist, as well.](https://gist.github.com/GeoffFoley/78b92f3a32cba5ad5d714e870c24d3ce)
