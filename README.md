
<a href='https://pledgie.com/campaigns/30176'><img alt='Click here to lend your support to: Integration test recorder for Angular/Ember/React/Vanilla web applications. and make a donation at pledgie.com !' src='https://pledgie.com/campaigns/30176.png?skin_name=chrome' border='0' ></a>

# test-recorder
This is an ambitious project that aims to record integration tests for different frameworks.

It currently supports Emberjs and am working on angular version

This records the ways you interact with your application, and then generates the code to playback these actions inside a test runner like 
qunit or protractor. The idea is to save you time writing these tests by hand.
 
You should only add the relevant script to your app when your app behaves as
expected (happy flow) as then you will have the tests generated for expected behaviour.

#Setup

If you want to run the protractor tests you will need to do the following:
* Install protactor `npm install -g protractor` 
* Update webdriver `webdriver-manager update`
* Start webdriver `webdriver-manager start`


#Current UI interactions that are recorded for acceptance tests:

* Button clicks, they also generate andThen code blocks. 
* records text input fillins
* Record any changes to route
* Changes in DOM additions/removals, only items with ID's are recorded and that don't have the doNotRecord css class.

#Notes

The test recorder mode is specified by the user in the script tag with the data-framework attribute and then is read 
using `document.currentScript.dataset`. This works in Chrome, Edge and FF. If IE support is needed I'll have to add some
extra functionality.

If an element doesn't have an id then a dom path selector will be generated to click on this button in a test, ie
```js
click("html>body>div>div:eq(0)>button");
andThen(function () {
 equal(find("#foo").length, 0, "foo removed AFTER user [INSERT REASON]");
});
```

If you don't want an element to be recorded, and any of its children add this class to it `doNotRecord`

#Vanilla (No framework/native js)

This just records actions in a non framework environment in the /demo/VanillaJsApp application. 
This will always be here, other frameworks come and go.


##Running the Vanilla demo app

The first thing you need to do is install the local npm dependencies in ./demo/VanillaJsApp

You can run 

`npm run demo-angular`


##Running the Vanilla test


This uses the basicConf.js to run a single file ('test-recorder-spec.js'), you can paste the generated test code into there

You need to start webdriver:
`npm run webdriver`

then run protractor with :
`npm run vanilla-test`

#Angular

Include this line in your page

`<script src="dist/emberTestRecorder.js" data-framework="angular"></script>`

##Running the Angular demo app

The Angular todo app has been copied to demo/angular-1-TODO. The test recorder has been added to this also via a symlink 
to the /dist folder which contains the webpack build of the test recorder.

The first thing you need to do is install the local npm dependencies to the angular 1 demo app.

You can run the angular todo app with this, which will start up an express server

`npm run demo-angular`


##Running the Angular protractor test


This uses the basicConf.js to run a single file ('test-recorder-spec.js'), you can paste the generated test code into there

You need to start webdriver:
`npm run webdriver`

then run protractor with :
`npm run demo-angular-test`

# React

Include this line in your page

`<script src="dist/emberTestRecorder.js" data-framework="react"></script>`

##Running the React demo app

`npm run demo-react`

##Running the React protractor test

This uses the react/basicConf.js to run a single file ('test-recorder-spec.js'), you can paste the generated test code into there

You need to start webdriver:
`npm run webdriver`

then run protractor with :
`npm run demo-react-test`


# Ember

To use this, simply include this line of code someone in your ember-cli, I recommend in your `app.js`:

```js
import main from 'ember-cli-test-recorder/main';// jshint ignore:line
```
Note: we use jshint ignore:line as we don't actually do anything with the main object, it sets everything up by itself


# Angular 2 

comming soon!

## Roadmap
* Allow selects to be automated
* Allow more complex click actions like the steps to click on inputs like select2 to be recorded
* Ignore clicks on ember elements with no effect
* Create codes for key-presses 
* Get mutations to work with async effects more accurately with performance api
* create tests for changes of lengths in lists
* Generate cucumber specs

##TIPS

Avoid making multiple button clicks (or other interactions that cause asynchronous) updates until DOM has 
finished updating. This will allow code generated by the mutations observer to be placed in the in the
generated code. 

#Browser compatibility

Chrome + Firefox + Edge
