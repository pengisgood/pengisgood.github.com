---
title: [译] Sass 和 SCSS 到底是什么关系？
date: 2016-08-11 16:51:37
tags: [Sass, SCSS]
categories: Web 前端
---
> 原文链接：[https://www.sitepoint.com/whats-difference-sass-scss/](https://www.sitepoint.com/whats-difference-sass-scss/)

I’ve written a lot lately on Sass, but some recent comments made it clear that not everyone knows exactly what Sass refers to. Here’s a bit of clarity:

When we talk about Sass, we usually refer to the preprocessor and the language as a whole. We’ll say, for example, “we are using Sass”, or “here is a Sass mixin”. Meanwhile, Sass (the preprocessor) allows two different syntaxes:

Sass, also known as the indented syntax
SCSS, a CSS-like syntax
History
Initially, Sass was part of another preprocessor called Haml, designed and written by Ruby developers. Because of that, Sass stylesheets were using a Ruby-like syntax with no braces, no semi-colons and a strict indentation, like this:

```sass
// Variable
!primary-color= hotpink

// Mixin
=border-radius(!radius)
    -webkit-border-radius= !radius
    -moz-border-radius= !radius
    border-radius= !radius

.my-element
    color= !primary-color
    width= 100%
    overflow= hidden

.my-other-element
    +border-radius(5px)
```

As you can see, this is quite a change compared to regular CSS! Even if you’re a Sass (the preprocessor) user, you can see this is pretty different from what we are used to. The variable sign is ! and not $, the assignment sign is = and not :. Pretty weird.

But that’s how Sass looked like until version 3.0 arrived in May 2010, introducing a whole new syntax called SCSS for Sassy CSS. This syntax aimed at closing the gap between Sass and CSS by bringing a CSS-friendly syntax.

```scss
// Variable
$primary-color: hotpink;

// Mixin
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    border-radius: $radius;
}

.my-element {
    color: $primary-color;
    width: 100%;
    overflow: hidden;
}

.my-other-element {
    @include border-radius(5px);
}
```

SCSS is definitely closer to CSS than Sass. That being said, Sass maintainers have also worked to make both syntaxes closer to each other by moving ! (variable sign) and = (assignment sign) from the indented syntax to $ and : from SCSS.

Now, when starting a new project, you may wonder which syntax you should use. Let me enlighten the path and explain the pros and cons of each syntax.

Pros for Sass indented syntax
While this syntax might look weird, it has some interesting points. First of all, it is shorter and easier to type. No more braces and semi-colons, you don’t need all that stuff. Even better! No need for @mixin or @include, when a single character is enough: = and +.

Also the Sass syntax enforces clean coding standards by relying on indentation. Because a wrong indent is likely to break the whole .sass stylesheet, it makes sure the code is clean and well formatted at all times. There is one way to write Sass code: the good way.

But beware! Indenting means something in Sass. When indenting a selector, it means it is nested into the previous selector. For instance:

```sass
.element-a
    color: hotpink

    .element-b
        float: left
```

… will output the following CSS:

```css
.element-a {
    color: hotpink;
}

.element-a .element-b {
    float: left;
}
```

The simple fact of pushing .element-b one level to the right means it is a child of .element-a, changing the resulting CSS. Be very careful with your indentation!

As an aside, I feel like the indentation based syntax will probably suit a Ruby/Python team more than a PHP/Java team (although this is debatable, and I’d love to hear opinions to the contrary).

Pros for SCSS syntax
For starter, it is fully CSS compliant. It means, you can rename a CSS file in .scss and it will just work. Making SCSS fully compatible with CSS has always been a priority for Sass maintainers since SCSS was released, and this is a big deal in my opinion. Moreover, they try to stick as close as possible to what could become a valid CSS syntax in the future (hence @directives).

Because SCSS is compatible with CSS, it means there is little to no learning curve. The syntax is already known: after all, it’s just CSS with a few extras. This is important when working with inexperienced developers: they will be able to quickly start coding without knowing the first thing about Sass.

Moreover, it is easier to read because it actually makes sense. When you read @mixin, you know it is a mixin declaration; when you see @include, you are calling a mixin. It doesn’t make any shortcuts, and everything makes sense when read out loud.

Also, almost all existing tools, plugins and demo for Sass are developed using the SCSS syntax. As time goes, this syntax is becoming pro-eminent and the default (if only) choice, mostly for the above reasons. For instance, it is getting quite hard to find a clean syntax highlighter for Sass indented syntax; usually, there is only SCSS available.

Final thoughts
The choice is up to you, but unless you have really good reasons to code using the indented syntax, I would strongly suggest using SCSS over Sass. Not only is it simpler, but it is also more convenient.

I once tried the indented syntax myself and liked it. I liked how short and easy it was. I was actually about to move an entire code base to Sass at work before changing my mind at the last minute. I thank my past-self for halting that move, because it would have been very difficult to work with several of our tools had we used the indented syntax.

Also, please note Sass is never in uppercase, no matter whether you’re talking about the language or the syntax. Meanwhile, SCSS is always in uppercase. Need a reminder? SassnotSASS.com to the rescue!
