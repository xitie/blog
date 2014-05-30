---
layout: post 
title: Easy Scroll To Everywhere 
keywords: jQuery 
tags: jQuery 
description: jQuery make scroll become so easy, I recommend you to have a look.
---
<h3>Getting started</h3>
<p><b>Step 1:</b> add javascript to your html page</p>
<pre>
&lt;script language="javascript"&gt;  
  $(function() {
    $('a[href*=#]:not([href=#])').click(function() {
      if (location.pathname.replace(/^\//,'') == this.pathname.replace(/^\//,'') && location.hostname == this.hostname) {
        var target = $(this.hash);
        target = target.length ? target : $('[name=' + this.hash.slice(1) +']');
        if (target.length) {
          $('html,body').animate({
            scrollTop: target.offset().top
          }, 1000);
          return false;
        }
      }
    });
  });
&lt;script/&gt;
</pre>

<p><b>Step 2:</b> create a link like this</p>

<pre>
&lt;a href="http://example/#tab1"&gt;Go to tab1&lt;/a&gt;
</pre>

<p><b>Step 3:</b> create a tag which should equal 'tab1'</p>

<pre>
&lt;div id="tab1"&gt; Tab1 &lt;/div&gt;
</pre>

<p>Now you can try it !</p>
<p>Then you will love it !</p>
