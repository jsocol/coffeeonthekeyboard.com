---
title: 'Excitement about HTML5: Forms'
author: James
layout: post
permalink: /excitement-about-html5-forms-278/
bitly_url:
  - http://j0.is/14dZNIi
bitly_hash:
  - 14dZNIi
bitly_long_url:
  - http://coffeeonthekeyboard.com/excitement-about-html5-forms-278/
categories:
  - Articles
tags:
  - forms
  - Front-end
  - html5
  - ua
  - ui
  - ux
---
I shouldn&#8217;t imply that [I don&#8217;t like HTML5][1]. I don&#8217;t like certain parts of it—the redundant new elements that add no functionality and are of little use except to [A List Apart][2]. But other parts of the spec are very exciting. [Forms][3], in particular, are getting a much-needed facelift.

Forms will be able to require the user-agent to do [pre-submission validation][4]. Obviously we can&#8217;t rely on this for security reasons, but it will save us from writing JavaScript validation scripts and give users a better experience with our forms.

And the validation is fairly robust. `<input/>` elements gain an attribute called `<a href="http://www.w3.org/TR/html5/forms.html#attr-input-pattern">pattern</a>`, which accepts a simple regular expression, like `"[A-E][0-9]{7}"`.

But you probably won&#8217;t need the `pattern` attribute very often, since there is a whole slew of new [`type`s][5] that the UA will be expected to provide controls for, and validate. Crucial types, like `email` and `url`, and *difficult* types like `datetime-local` and `color`.

[jQuery UI][6] is great, but it doesn&#8217;t compare to this kind of power.

I only realized the sorry state of our current form controls very recently, after getting an iPhone. Built-in form fields on the iPhone can trigger different input methods. A field, like ZIP code, that accepts only numbers, opens the keyboard to the numbers first. A field that wants a URL can open a keyboard with a `.com` button to save time. But web pages can&#8217;t do this. They can only say that an input accepts arbitrary `text`.

For mobile or touch-enabled UAs, knowing that you can open a keypad instead of a full keyboard is a great step forward. For everyone, having date and color pickers built into the UA means saving both developers&#8217; and users&#8217; time. Less time to build and less time to download, consistent experience in the UA across web sites.

Forms also get a new event, [`input`][7], which is a little like the current `change` event, except you don&#8217;t need to wait for the element to lose focus for the event to fire. That&#8217;s just useful.

Controls get new methods, [`stepUp()`][8] and `stepDown()`, to enable, for example, very fast forward/backward selection on a `date` input.

The last fun addition I&#8217;ll mention (go read the links, they&#8217;re all to the same page) is the [`autocomplete`][9] attribute. It doesn&#8217;t do what you hope it does. What it does is specify to the UA that it should not remember and suggest values for this input. It lets the UX designer decide whether to use Firefox or IE&#8217;s built-in autocomplete on a per-field basis. Useful. (Not as useful as, say, an `autocompleteUrl` attribute that could load some JSON or XML, but still useful.)

Building forms is going to be so, so much better once these are widely support.

 [1]: http://coffeeonthekeyboard.com/reservations-about-html5-271/ "I don&#8217;t like HTML5"
 [2]: http://www.alistapart.com/
 [3]: http://www.w3.org/TR/html5/forms.html
 [4]: http://www.w3.org/TR/html5/forms.html#constraints
 [5]: http://www.w3.org/TR/html5/forms.html#attr-input-type
 [6]: http://jqueryui.com/
 [7]: http://www.w3.org/TR/html5/forms.html#event-input-input
 [8]: http://www.w3.org/TR/html5/forms.html#dom-input-stepup
 [9]: http://www.w3.org/TR/html5/forms.html#resulting-autocompletion-state