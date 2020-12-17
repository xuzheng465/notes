# Chapter 1



它是以最相关的格式来展示网页内容，以适应访问它的视口和设备。

It's the presentation of web content in the most relevant format for the viewport and device accessing it.



Namely, that everything from design to content management and development worked better when starting with the smallest screens first, and then "progressively enhancing" the design and content for larger screens and/or more capable devices. 

，从设计到内容管理和开发，如果先从最小的屏幕开始，然后 "逐步增强 "到大屏幕和/或能力更强的设备的设计和内容，效果会更好。



```html
<meta name="viewport" content="width=device-width,initial-scale=1.0" />
```

我们的视口元标签实际上是在说 "让内容以设备的宽度呈现"。



We want a responsive design—something that is agnostic of the screen size viewing it, responding to any size viewport it finds itself in, not something that only looks at its best at specific sizes.



我们希望有一个响应式的设计--它与查看它的屏幕尺寸无关，对任何尺寸的视口都能做出反应，而不是只有在特定尺寸下才看起来最好。



```css
@media screen and (min-width: 800px) { 
  /* styles */ 
}
```

the `screen` part tells the browser these rules should be applied to all screen types.

`(min-width: 800px)`. That tells the browser that the rules should also be limited to all viewports at least 800px wide.

这告诉浏览器，规则也应该限制在所有至少800px宽的视口

"The absence of support for media queries is in fact the first media query."

What he meant by that is that the first rules we write outside of a media query should be our starter or "base" rules for the most basic devices, which we then enhance for more capable devices and larger screens.

他的意思是，我们在媒体查询之外写的第一条规则应该是我们针对最基本设备的启动规则或 "基础 "规则，然后针对能力更强的设备和更大的屏幕进行增强。

## Summary

* A `meta` tag is needed in the head of your HTML so browser knows how to render the page.
* You'll want all images to be set with a `max-width` of 100% in the CSS by default
* When you write CSS for a responsive design, start with base styles that can work on any device—typically the smallest screen and then use media queries to adapt it for larger screens