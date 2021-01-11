---
subjects:
- react
- nextjs
- javascript
- accesibility
title: Adding an HTML language attribute to a NextJS app
subtitle: ''
blurb: ''
date: 2021-01-08T00:00:00.000+00:00

---
Short version [use a custom document class](https://nextjs.org/docs/advanced-features/custom-document "NextJS custom document documentation").

## The Problem

Recently when setting up a new [NextJS](https://nextjs.org/ "The next JS hompage") app I wanted to include accessibility checking right from the start. It's much easier to stay on top of basic accessibility good practice with automated checking in place _before_ you start rather than trying to fix things up once you already have a bunch of code and UI in place (though better late than never!).

Running [pa11y](https://pa11y.org/ "Pally docs") on the output of [create-next-app](https://nextjs.org/docs/api-reference/create-next-app "Create Next App docs") I was surprised that I got an error telling me I should have specified a `lang` attribute on the `html` element to tell clients what language the content was going to be in i.e. the rendered html tag looked like this `<html>` when it should ideally have looked like this `<html lang="en">` or indeed `<html lang="cy">` etc. Thinking about it, this makes sense, it would be parochial for NextJS devs to assume all people using the framework are making sites in english or any other language probably shouldn't. However, for a NextJS newbie like me it's not obvious how to go about setting it up properly.

## The Solution

So! NextJS has a document file which specifies how the outermost elements of the application's page's markup should be rendered. You can override this default by creating a file, `_document.js`, in the `pages` directory of your NextJS project.

![](/uploads/document-js.png)

The content of this file is as follows

    import Document, { Head, Html, Main, NextScript } from 'next/document';
    
    const language="en"; // [1]
    
    class InternationalDocument extends Document { // [2]
      render() {
        return (
          <Html lang={language}> // [3]
            <Head /> // [4]
            <body>
              <Main />
              <NextScript />
            </body>
          </Html>
        )
      }
    }
    
    export default InternationalDocument; // [5]

**\[1\]** I've just hardcoded the value in my file but this constant might change later based on an environment variable on the server or the requesting URL structure

**\[2\]** My document class is called `InternationalDocument` you can call it whatever you want as long as you extend Next's in build `Document` class

**\[3\]** Here's the place where you add your `lang` attribute

**\[4\]** All the rest of the file is just recreating the standard NextJS `Document` class

**\[5\]** Don't forget to export the correct class name

And that's it!

You don't need to explicitly import this `InternationalDocument` anywhere. NextJS figures out it's there and does the necessary "magic". Aside: I think NextJS is comfortably my favourite way to build all but the most simple React app and the documentation is great but things like this do feel a bit out of my control for my taste, a bit too much "magic".

***

## Tangents

There's a [translate](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/translate "Mozdev translate docs") global attribute

> an enumerated attribute that is used to specify whether an element's _translateable attribute_ values and its [`Text`](https://developer.mozilla.org/en-US/docs/Web/API/Text) node children should be translated when the page is localized, or whether to leave them unchanged.

[A big list of lang attribute values](http://www.toffeemilkshake.co.uk/devlog/2021/01/04/a-big-list-of-lang-attribute-values.html)