---
slug: margin-vs-padding
title: Margin vs Padding
author: Tushar Tambe
author_title: Application Developer @ThoughtWorks
author_url: https://github.com/tushartambe
author_image_url: https://avatars.githubusercontent.com/u/44019295?s=460&u=5029ee2f0952a1ecf522850e1d1b8c6e2f0695f7&v=4
tags: [css, html, styling]
---

<br/>
You might be familiar with both terms, most people generally use it to give space between two html elements. But have you ever thought what’s the real difference between them?

<!--truncate-->
Let’s understand what makes them different.

### Margin

* Margin is space between two html elements.
* Margin can be applied to block elements(like div, header).
* Margin is not calculated in element height.
* Margin is transparent but the background colour and images applied to element will not be applied to margin.

### Padding

* Padding is space from element border to content.
* Padding is calculated in element height.
* Padding is also transparent and the background colour and images applied to element will be applied to padding.

Let’s take an example to understand this. In the example You can see the margin between div1 and div2 is 20 pixels. But height of both div is 50px.

**HTML**

``` html
<body>
    <div class="div1">
        This is DIV 1
    </div>
    <div class="div2">
        This is DIV 2
    </div>
</body>
```

**CSS**

``` css
.div1 {
    width: 100px;
    height: 50px;
    margin-bottom: 20px;
    border: 1px solid black;
    background-color: yellow;
}

.div2 {
    width: 100px;
    height: 50px;
    border: 1px solid black;
    background-color: blue;
}
```

Let’s add margin-top:20px to div2 and see what happens. If you notice nothing is changed the gap between div1 and div2 still same because margin is space between two elements and it doesn't case which element asked for it. A bad thing about margin that it collapses.

Now let’s understand how padding works. Try to add padding:10px; to div1 and notice whats happening. You can see the width and height of div1 increased by 10px each side, also there is space between the content (text ‘This is DIV 1’) of div1 and border of div1.

Now you got the real difference between margin and padding.

### Playground

export const CodePen = ({codePenEmbedUrl}) => (<iframe height="500" width="100%" scrolling="no" title="" src={ `${codePenEmbedUrl}?height=265&theme-id=dark&default-tab=result` } frameborder="no" allowtransparency="true" allowfullscreen="true"></iframe>); 

<CodePen codePenEmbedUrl="https://codepen.io/tushartambe/embed/preview/BXNYxW"></CodePen>
