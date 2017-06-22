---
title: CSS Bundling the jQuery UI Theme
tags: [asp.net, C#, IIS, URL Rewrite]
date: 2014-07-16 20:48:00
---

Its common in the projects I work on to have an images folder separate from the less/css folder.&nbsp; When jQuery UI is used its usually got its own folder as well.&nbsp; Here’s a sample with a number of things removed for clarity.

[![image](http://www.michaelware.net/image.axd?picture=image_thumb_11.png "image")](http://www.michaelware.net/image.axd?picture=image_11.png)

In development while Bundling is turned off accessing an image in the menu.less file might look something like this.&nbsp; 
  <pre class="prettyprint">#contentContainer {
    background: #FFFFFF url('../../images/masterPages/menu/contentContainerBackground.jpg') no-repeat top center;
}</pre>

For context here is a sample bundle from our BundleConfig.cs

<pre class="prettyprint">public class BundleConfig
{
    public static void RegisterBundles(BundleCollection bundles) {

        bundles.Add(new StyleBundle("~/cssBundles/masterPages/root").Include(
                    "~/css/masterPages/reset.css",
                    "~/jQueryUITheme/jquery-ui-theme.css",
                    "~/css/masterPages/root.css"
                    ));
    }
}</pre>

So its a happy accident this just works.&nbsp; It turns out from the bundle path /cssBundles/masterPages if you go back two directories then you hit the root of the site and find the images directory.&nbsp; The CSS works bundled and in development.&nbsp; However, throw a jQuery UI theme in the middle of this and things aren’t so pretty.&nbsp; jQuery by default keeps all its images in a subfolder next to the theme.&nbsp; There really is no way to move the images around without editing the theme.css and **since that file is generated I think its best to leave it alone**.&nbsp; So this left us in a odd spot because we wanted our theme to be bundled in with the rest of our css to keep down on the number of network requests, but we also didn’t want to try to move everything in the project around just to make bundling work.&nbsp; So here’s our answer, **URL Rewrite**. As usual extra information removed for clarity.

<pre class="prettyprint">&lt;configuration&gt;
    &lt;system.webServer&gt;
        &lt;rewrite&gt;
            &lt;rules&gt;
                &lt;rule name="Rewrite jQueryUI Images for Bundling"&gt;
                    &lt;match url="cssBundles/masterPages/images/(.*)" /&gt;
                    &lt;action type="Rewrite" url="jQueryUITheme/images/{R:1}" /&gt;
                &lt;/rule&gt;
            &lt;/rules&gt;
        &lt;/rewrite&gt;
    &lt;/system.webServer&gt;
&lt;/configuration&gt;</pre>

This catch all rule looks for images the jQuery UI theme thinks should be on the end of our bundle url and redirects them to where we want to keep them in the project.&nbsp; It’s a little black magic, but its configure once and forget about it.&nbsp; Now all our bundling works in dev and prod.&nbsp; 