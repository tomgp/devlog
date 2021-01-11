---
subjects: []
title: Feature detection in React apps
subtitle: 'Short version: Use the context API'
blurb: ''
date: 2021-01-11 00:00:00 +0000

---
Short version: Use the [context API](https://reactjs.org/docs/context.html)

## Problem

You want to add some wizzy WebGL (or other not-universally available thing) to your React website. You need to make sure the client running your code supports this feature. For sanity’s sake you want to do this once, in one place in the code, and probably, separate from the [domain logic](https://en.wikipedia.org/wiki/Business_logic).

## Solution

React’s [Context API](https://reactjs.org/docs/context.html) is a good fit fo this kind of problem.

The way context work works in React is that you create a context provider component and any sub component of that context provider at any point in you component hierarchy can access data from that context.

First lets make the context provider, it’s a normal react component…

    import {createContext} from 'react';
    
    export const FeatureDetectionContext = createContext(); [1]
    
    export function FeatureDetectionWrapper({children}){ // [2]
      const features = {
      	webGL:isWebGL(), // [3]
        // detect some other features here maybe?
      };
      
      return (
        <FeatureDetectionContext.Provider value={features}> [4]
          {children}
        </FeatureDetectionContext.Provider>
      )
    }
    
    function isWebGL(){	// [5]
      const canvas = document.createElement("canvas");
      const ctx = (canvas.getContext("webgl") || canvas.getContext("experimental-webgl")); 
      return ((window.WebGLRenderingContext !== undefined) && ctx && ctx instanceof WebGLRenderingContext);
    }

\[1\] here is where the context object  is created, it needs to exported from the module so it can be consumed by other components

\[2\] the context wrapper component is just a normal functional React component, it too needs to be exported so that things can be wrapped in it

\[3\] when the component is instantiated you can add data to it in the usual ways, either by passing props or, in this case by running a function to find something out about the browser environment in which the code is running \[5\]

\[4\] finally we return the component markup, the \`Provider\` from the context object  is the outer bit of this markup

OK, so how do you use this component? In the case of feature detection you probably want the context wrapper component at the highest level of the app

    import {FeatureDetectionWrapper} from ‘FeatureDetectionContext'; [1]
    
    function RootComponent(){
      return <FeatureDetectionWrapper> // [2]
        <TheRestOfTheApp />
      </FeatureDetectionWrapper>
    }

\[1\] Import the wrapper component

\[2\] Put the component around the component sub-tree that needs access to the info contained in the context

Finally to use the information stored in the context…

    import { FeatureDetectionContext } from 'FeatureDetectionContext'; // [1] import the context 
    
    function ComponentThatNeedsContext(){
    	const features = useContext(FeatureDetectionContext); // [2] to use the context you need to pass it through the useContext function
    	if(features.webGL){
    		return <WizzyWebGL />
    	}
    	return <NonWebGL />
    } 

And that’s about it.

Warning, as the React docs say, using the context API may make components less easy to reuse, if a component relies on a context it probably won’t be reusable without that context. I've found that you maybe better off accesing the context data in the component one level above those that require the feature in question that way the individual features can be context free and you have context being accessed in the fewest possible places YMMV.

***

[BBC Responsive News, Cutting the Mustard](https://responsivenews.co.uk/post/18948466399/cutting-the-mustard) an old but still relevant article explaining the rationale and best praxctice behind feature detection as opposed to user agent detection. 