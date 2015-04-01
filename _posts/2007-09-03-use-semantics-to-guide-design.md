---
title: use semantics to guide design
author: James
layout: post
permalink: /use-semantics-to-guide-design-53/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2007/09/use-semantics-to-guide-design.php
bitly_url:
  - http://j0.is/16FvoHG
bitly_hash:
  - 16FvoHG
bitly_long_url:
  - http://coffeeonthekeyboard.com/use-semantics-to-guide-design-53/
dsq_thread_id:
  - 1746519808
categories:
  - Articles
---
<div class="image right">
  <img src="/blog/images/semantics-illustration-1.jpg" height="120" width="160" alt="A sample of CSS code using semantic descriptions." /><br /> An example.
</div>

For the longest time, I wondered how exactly people chose the layouts for gutters. Why did box X end up in the left gutter while box Y was on the right? Was it random chance?

Looking back, I realize that, yes, much of it was essentially chance. Or trying to make the columns the same height. Or what the designer had eaten that morning. But not all sites were done this way, and the sites that had other reasons for grouping were usually better.

Then I looked at the code for one of my favorite sites, and I was shown The Way: **describe semantic containers**.

By describing the containers semantically, instead of geographically (ie: `#user_context` instead of `#left_gutter`) you can give yourself a map and create a layout that&#8217;s both easy and logical.

#### my example

For example, on my site, I wanted to break the [footer][1] into sections. But where to put the copyright notice, which had previously been centered on the bottom? Easy: put it in the `#footer_legal` section, with the other legalese.

Another one of the four footer columns was described as `#footer_standards`, but then I wanted to add a link to an "about the site" page, which is not a coding standard. Instead I changed the column to `#footer_techy` and included all the technical links, including standards and site-information, divided into sub-sections.

#### logical design is ease of use

By describing semantically, you not only make your own design choices easier, you make navigation easier for your users.

Semantic descriptions force you to group semantically related content. So if your readers are looking for your privacy policy, but find your copyright notice first, they should be in a "legal-looking" section, and be able to find your privacy policy easily.

Of course, this isn&#8217;t limited to design elements within a page. On a larger scale, you can use semantic groupings to make user-interaction easier. As Eric at [Internet Duct Tape][2] suggested, you can [use mind-maps][3] to model user interaction, and also to model site organization.

So pay attention to your content. Use semantic `id`s and `class`es to your, and your users&#8217;, advantage.

 [1]: #footer "Look at the footer."
 [2]: http://internetducttape.com "Visit Internet Duct Tape."
 [3]: http://internetducttape.com/2007/08/31/mind-mapping-user-interface-complexity/trackback/ "Read about mind-mapping."