# Writing an ionic 2 application for production

## Abstract
We spent three months writing an ionic 2 application that was released to the app store.
There are still only a few resources that share insights about how to write ionic 2 applications.
This post is an attempt to summarise some lessons we learnt along the way and hopefully will give you 
confidence for your project.


## General considerations

### Hybrid app or not?
Hybrid applications have a reputation for being slow, laggy and crappy in general - not without bad reasons. 
If an application consists of heavy animations, needs to support old devices, has some non-negotiable UX patterns, 
renders a lot of data or has some 3D action going on, stay away from hybrid applications. 
A hybrid application can deal with light animations, content-driven applications that are light on the client. We found 
that building a hybrid application sometimes also means to embrace the bottlenecks and to work around them by changing UX.

### TypeScript for the win
We decided to write an ionic 2 application with TypeScript and we are glad we did so. Auto-completion is a huge win,
to explore the documentation. The static analysis you get for free is nice to have. The extra compilation step from 
TypeScript to ES5 is painful at times and adds additional complexity and considerations. Using gulp and the ionic 
gulp plugins were very helpful in building a build pipeline.

### Think about your build pipeline
Our application has some code that is dependant on the environment we are running in. For example for dev, we would like
to add additional logging. In order to inject those environment specific variables we introduced an intermediate
step in our build pipeline. Our TypeScript code in source will be transformed to *final TypeScript* by injecting
environment variables. The *final TypeScript* is then used as a basis to bundle it up for running it or running the tests.

### Cordova works okay
There have been strange issues with the cordova plugins. The geolocation plugin for example has crashed our app
consistently until a bug fix was published. After a few hiccups, cordova is a powerful library that helped us adding native
functionality very easily. 

### Focus on one platform
We focused on building an ios app first. This helped us to not get slowed down by platform discrepancies and only worry 
about one UX flow. For the most part of the time we were able to build the app and get a pretty decent android experience. 
We think that if we wanted to fine-tune it for android we would probably have to invest a couple days work to align 
the UX with android. 


## Testing ionic 2 applications

If there was one thing where I think the ionic documentation falls short, it is testing.

### Testing pyramid
The **testing pyramid** tells us that our majority of tests should be on unit-level. We found that unit-level testing 
is pretty tough as angular 2 and ionic 2 provide developers with very little information how to go about it.
We inverted the testing pyramid, doing **ice cone** testing. That means we focused mainly on end-to-end tests
and integration tests rather than unit tests. 

### End-to-end tests
We used protractor for our end-to-end tests. Protractor has been a source of anger and frustration and we would 
not use it again. Protractor is heavily based around promises. We would have wished that our tests could have been
written in a more synchronous way. Getting promises right are definitely a requirement for using protractor effectively
and even if you do, be prepared to debug and fiddle with tests. Ionic uses css animation and javascript animations 
which can make it hard to assert about elements being in a page or not. As they are often on the DOM but not visible 
but protractor can not differentiate sometimes. End-to-end are expensive and take time to run, this will start biting you
once your application reaches the critical size. Parallelisation can alleviate that pain to some extent, but we did 
not investigate any further.
We find that the more native features we are adding, the less protractor is. Native features like facebook integration,
camera or accessing contacts do not run in the browser and cannot be tested with protractor. We are therefore looking
into **appium** to add native end-to-end tests.

### Integration tests
Integration tests are proof that components behave a specified. We found that writing integration tests for ionic is
very difficult as all most @Page dependencies need to be mocked. Very often the code is intertwined with angular 2 
directives and components which often caused us headaches figuring out what exactly we need to mock. Sometimes the 
error messages that are thrown by angular 2 were sufficient, sometimes they were not.

### Unit tests
Unit tests are quite simple to write. We have unit tests for our services, for our custom components and for some of
the pages. The pages often did not test an isolated unit which turned them into integration tests.
The test run takes about 30 seconds, which is slightly frustrating and could be sped up. 
We are using karma as our test runner and jasmine as our testing library. Both tools did a great job and are battle-proven.
The karma-browserify plugin has been proven to be useful to get incremental builds working and to reduce the test runs by half.

### Testing conclusion
It is possible to write ionic 2 applications with testing in place. Because integration tests are really hard to write,
unit tests provide only little value to assert on user functionality, we found that depending on end-to-end tests
was key for having the peace of mind. Be prepared to battle with mocking your components, and most of all battling with
protractor. Angular 2 is getting closer and closer to a release and with that documentation, also about testing, will
improve quickly. 


### ionic 2 seed
We created an ionic 2 seed which supports unit / integration tests with karma, and end-to-end tests with protractor.

# Conclusion
Most importantly, ionic 2 has made it easy for us to build an ios application which will be released into the app store.  
The pain points that come with writing an ionic 2 application are not related to itself. The main frustration originated from Protractor, Cordova and the 
whole build pipeline including compiling and bundling up TypeScript. Given that ionic 2 is still beta, this is not shocking
and we hope those pain points will be addressed by the open source community once the dust settles.
