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

__So the first step is to define the the function, and it's return value.__

			window.onbeforeunload = function(e) { 
        		return "You have unsaved changes, please save them."
       		};

The popup generated will look like this:<br>
{% img /images/post_images/popup.png %}


The only thing about this popup that can be customized is the message, which is defined in the return value of the function. The rest of it is a generic browswer popup. The same popup in Safari will look like this:<br>

{% img /images/post_images/safari-popup.png %}

__Now, any link I click will cause this popup to appear.__ At this point, even submitting the form will induce this popup. Obviously not what I want. 

__So the next thing I did was attach a click event to my submit button__ so that I could customize `onbeforeunload`'s typical functionality. 

		$('#save-button').live('click', function(){
			window.onbeforeunload = function(){};
		});

I am basically redefining onebeforeunload in the scope of that specific click event. Here, nothing will happen. But if I click any other link on my page, the popup will still appear. I only defined `onbeforeunload` to do nothing in the scope of that one specific click event.

Resources:<br>
1. https://developer.mozilla.org/en-US/docs/Web/API/window.onbeforeunload <br>


			