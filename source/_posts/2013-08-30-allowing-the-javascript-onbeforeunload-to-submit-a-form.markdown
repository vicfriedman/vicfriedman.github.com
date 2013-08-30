---
layout: post
title: "Allowing The Javascript onbeforeunload to Submit A Form"
date: 2013-08-30 16:53
comments: true
categories: 
---
<script type="text/javascript">

  var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-38989132-1']);
  _gaq.push(['_trackPageview']);

  (function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();

</script>

***So my last few weeks have been pretty Javascript heavy.*** I came across a situation where I needed to use the `onbeforeunload` method so that a popup would occur when a user tried to navigate away from an edit page. This method works on a global scope and is fired before the window is unloaded, or closed.  My popup was intended to remind a user to save their changes before leaving.

__The only issue with that is that the default functionality of this method__ ensures that a popup appears when a user tries to leave the page, even on a form submission. So clearly you need to be able to work around this. I don't want a popup to appear reminding my user to save their changes when they clicked the save button. 

__So the first step is to define the return value of the function__

      window.onbeforeunload = function(e) { 
        return "You have unsaved changes, please save them."
       };

The popup generated will look like this:
{% img /images/post_images/onbeforeunload_popup.pdf %}



With this included in my javascript file, any link I click will cause this popup to appear. At this point, even submitting the form will induce this popup.