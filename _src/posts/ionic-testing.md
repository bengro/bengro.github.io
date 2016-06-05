# Writing an ionic 2 application for the app store

## Abstract
We wrote an ionic 2 application that was released to the app store. This post is an attempt to summarise some lessons 
we learnt along the way and hopefully will give you confidence for your own ionic 2 application.

## Checklist

### Hybrid app or not?
Hybrid applications have a reputation for being slow, laggy and crappy in general - not without bad reasons. 
Here is a checklist which might help you decide, whether you get away with building a hybrid app:

* Your application contains heavy animations
* Your application needs to support old devices
* Your application has non-negotiable UX patterns
* Your application has some 3D action going on

If your application checks any of the above, you quite possibly enter a world of pain. ionic does an excellent job
hiding the typical hybrid feel, there are some symptoms that are very hard to get rid of.

On the other hand, hybrid can be a great fit for:
* Your application contains light animations
* Your application is primarly for displaying content that does not require any sophisticated data-crunching 
* Your application targets both app stores
* Your application requires any functionality that cordova provides

There are not too many use cases where hybrid applications shine, but when they do, the result can be impressive!

### TypeScript vs ES2015
We decided to write an ionic 2 application in TypeScript. This turned out to be a good move:
 
* Auto-completion is a huge win to explore the APIs. Often we got away by just clicking through to the declaration or implementation
instead of looking up the documentation.
* Having static analysis does not do any harm and helps you find bugs at compile time.
* TypeScript has some really nice language features, for example union types, which make the code more readable and maintainable.

There are a few painpoints that came with TypeScript too:
* Constantly compiling TypeScript to ES5 takes time and introduces complexity. We had to build a more sophisticated build
pipeline to cater for the transpilation from TypeScript to ES5. Using gulp this is relatively easy, but will take some time
to fine tune the bundling.

If you want to simplify your build pipeline, and you do not care about auto-completion and static analysis, you should go for ES2015.

### Test your cordova plugins in advance
Our application used multiple cordova plugins:
* Facebook integration
* Camera
* Contacts
* Geolocation
* Push notifications
* Console (logging to XCode)

We encountered some disruptive cordova issues. For example, the geolocation plugin crashed our app. 
Given that we were web developers without substantial ios experience, touching the cordova code was a no-go.
We were lucky that the maintainer of the geolocation plugin published a bug fix mid-way through our project.

We suggest to try out the cordova plugins first, ideally in a sandbox. Finding out potential issues, or limitations 
before you start the main application will help you develop your app in a more linear way, avoiding disruptions. 
Also, in case you find an issue, it gives you some time to request a fix or find alternatives in advance.

## Testing ionic 2 applications

We value tests. When assessing ionic 2, it was not clear to us whether ionic 2 provides us with easily testable code.
The ionic 2 documentation falls short on testing. This is also due to the fact that angular 2 neither has any useful
documentation about testing (as of today). 

### Testing pyramid
The **testing pyramid** tells us that our majority of tests should be on unit-level. We found that unit-level testing 
is pretty tough as angular 2 and ionic 2 provide developers with very little information how to go about it.
We inverted the testing pyramid, doing **ice cone** testing. 

### End-to-end tests
We used protractor for our end-to-end tests. Throughout the project Protractor has been a source of anger and frustration. 
Ionic uses css animation and javascript animations which can make it hard to assert about elements being in a page or not.  

We find that the more native features we are adding, the less protractor is. Native features like facebook integration,
camera or accessing contacts do not run in the browser and cannot be tested with protractor. We unit-tested those features 
but still left us no real confidence whether the feature actually work.

For the next project, we would probably use a tool like **appium** to add native end-to-end tests. Appium is still based
on WebDriver and users accustomed to Selenium will have no difficulty making the switch. The difference is that the tests
are actually run on an emulated device rather than a browser.

### Unit and integration tests

We are using karma as our test runner and jasmine as our testing library. Both tools did a great job and are battle-proven.
The karma-browserify plugin has been proven to be useful to get incremental builds working and to reduce the test runs by half.

### Integration tests
We found that writing integration tests for ionic is very difficult as all most @Page dependencies need to be mocked. 
Very often the code is intertwined with angular 2 directives and components which often caused us headaches figuring out 
what exactly we need to mock. Sometimes the error messages that are thrown by angular 2 were sufficient, sometimes they were not.
We suspect that this will improve more and more as angular 2 and ionic 2 will become more stable and better documented.

### Unit tests
Unit tests are quite simple to write. We have unit tests for our services, for our custom components and for some of
the pages. The pages often did not test an isolated unit which turned them into integration tests.

### Testing conclusion
It is possible to write ionic 2 applications with testing in place. Because integration tests are really hard to write,
unit tests provide only little value to assert on user functionality, we found that depending on end-to-end tests
was key for having the peace of mind. Be prepared to battle with mocking your components, and fierce battles with
protractor. Angular 2 is getting closer and closer to a release and with that documentation, also about testing, will
improve quickly. 


### ionic 2 seed
We created an ionic 2 seed which supports unit / integration tests with karma, and end-to-end tests with protractor.

### A possible build pipeline
Our application has some code that is dependant on the environment we are running in. For example for development, we would like
to add additional logging. In order to inject those environment specific variables we introduced an intermediate
step in our build pipeline. Our TypeScript code in source will be transformed to *final TypeScript* by injecting
environment variables. The *final TypeScript* is then used as a basis to bundle it up for running it or running the tests.

[[ graphic ]]

# Conclusion
Most importantly, ionic 2 has made it easy for us to build an ios application which will be released into the app store.  
The pain points that come with writing an ionic 2 application are not related to itself. The main frustration originated from Protractor, Cordova and the 
whole build pipeline including compiling and bundling up TypeScript. Given that ionic 2 is still beta, this is not shocking
and we hope those pain points will be addressed by the open source community once the dust settles.
