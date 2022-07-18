# Magazine-issues

Thinking process
================

Display mode
------------

The display mode can be made using one prop type string, where you can pass 'grid' | 'featured' | 'both'.

But it can also be made using two props: grid type boolean, and featured type boolean.

I'll go with the second option because it will probably be more readable throughout the process.

Available space
---------------

The available space can't use media queries, and nowadays CSS doesn't have support for block-size queries.

> There is a spec for adding this in the future, (@container queries).  
> Check [https://github.com/w3c/csswg-drafts/issues/5796](https://github.com/w3c/csswg-drafts/issues/5796).  
> But since we are in the present, we need to solve this with JavaScript.

The component will have a prop named size; with that, we can calculate our inner sizings.

Also, it is probably good to have an **onresize** handler, to avoid overflows.

Issues
------

For the issues, my approach will be an array of images with the following structure.

```plain
image = {
name: the name of the issue
datePublished: when it was published (for sorting)
src: url of the image
featured?: if the image should appear on front
}
```

Layout of the component
-----------------------

There are are two approaches for the main layout of this component:

*   Flexbox
*   Grid

I'll choose Flexbox because I'm more experienced with it. But maybe it's better to go with Grid.

  

Tech stacks / Frameworks
========================

For this project my choice of frameworks/tech stacks was:

*   Nuxt 3 SSR
*   Vue 3 Composition API
*   TypeScript
*   Flui
    *   Flui my own CSS Toolkit, it works like Bootstrap and TailWind.
    
    [
    
    Flui - Toolkit generator
    
    ![](https://www.google.com/s2/favicons?domain_url=https%3A%2F%2Fflui-css.netlify.app%2F)https://flui-css.netlify.app/
    
    ](https://flui-css.netlify.app/)
    

  

Directory structure
===================

The **components** folder has 2 folders

*   a
    *   These components were imported from another project, and created by me. They are helpers.
    *   The image component is used in the magazine-issues component, but the other ones are not necessary.
*   main
    *   This folder has only one component, that is the **magazine-issues**, where you'll find the main logic.

The **pages** folder, only has one file, where the main purpose is to test the component.

  

Additional/Over-engineered features
===================================

*   The magazine-issues component can have a relative width **% and vw**
*   The image component shows a loading bar when the image hasn't loaded
*   The page has dark/light mode that is set accordingly to the user preference
*   The **images** prop is reactive, meaning that you can request the images after the page has loaded.

  

Demo
====

  

Running the project
===================

To run this project, open your favorite command line tool, and run:

```plain
git clone **this repo**
cd magazine-issues
npm install
npm run dev
```