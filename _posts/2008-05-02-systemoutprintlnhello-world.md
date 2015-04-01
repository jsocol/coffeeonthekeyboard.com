---
title: 'System.out.println(&#8220;Hello World!&#8221;)'
author: James
layout: post
permalink: /systemoutprintlnhello-world-79/
bitly_url:
  - http://j0.is/14dZzkr
bitly_hash:
  - 14dZzkr
bitly_long_url:
  - http://coffeeonthekeyboard.com/systemoutprintlnhello-world-79/
categories:
  - Articles
tags:
  - block level
  - Code
  - java
  - programming
  - scope
  - syntax
---
So I&#8217;ve been getting used to a couple new languages lately, mostly Ruby and Java but also Python and C++ for comparison. Coming from PHP, VBScript and JavaScript has confused the hell out of me.<!--more-->

The hardest part so far is not strict types, which I expected would mess me up. It&#8217;s block-level scope. For example, in PHP you could:

    <?php
    if ( $_GET['name'] ) {
        $name = $_GET['name'];
    } else {
        $name = "World";
    }
    echo "Hello $name!";
    ?>

But in C++ and Java, variables defined within *any* block (section wrapped in {}), are only visible in that block and its descendant blocks, so when I tried

    
    class HelloWorld
    {
        public static void main ( String[] args )
        {
            if ( args.length > 0 ) {
                String name = args[0];
            } else {
                String name = "World";
            }
            System.out.println("Hello "+name+"!");
        }
    }

it wouldn&#8217;t compile, saying &#8220;name cannot be resolved.&#8221; It took me a while to figure this one out.

Fortunately I had some help from a friend but honestly it still seems counter-intuitive to need to declare a value before copying a fixed value. For the record, here&#8217;s a version of the last class that works:

    
    class HelloWorld
    {
        public static void main ( String[] args )
        {
            String name;
            if ( args.length > 0 ) {
                name = args[0];
            } else {
                name = "World";
            }
            System.out.println("Hello "+name+"!");
        }
    }