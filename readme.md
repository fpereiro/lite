# Unfancy software architecture

If you wonder why software seems to be so unnecessary complicated, and you suspect that implementing and maintaining good software could be easier, you're not alone.

My perception is that the field of software architecture is mostly comprised of high-sounding, complex conceptual edifices that offer scarce practical guidance on how to create simple and powerful software systems, particularly in the context of small teams. For the most part, software architctures are impressive and well-reasoned, but tend to leave the implementer in a lurch.

Software architectures, however, also embody a written guideline on how to co-create software in the context of a team. *Why code things in a certain way and not another?* This question is not merely interesting, it is the essential question that determines the quality of the technical execution of a sofware team. And software architecture answers, or at least hints at how to answer, that very question. So we cannot do without a software architecture.

If we need a software architecture, let's make it an unfancy one. One that answers the big questions directly, is focused on helping you deeply understand what's going on, and one that helps you gets stuff done today (while not painting yourself into a corner tomorrow).

Here I will outline just that: a sketch of an unfancy software architecture.

The usual counterargument against an unfancy software architecture will be: why not trust the standards, the best practices, the way things are already being done? Here, it all comes to your (dis)satisfaction with the way software is currently developed and your appetite or aversion to risk. Not going on the well-worn path is guaranteed to be bumpy. But, if you are dissatisfied with the status quo, a bumpy ride sure sounds better than spending your career plodding along a path that, no matter how popular, just doesn't feel right.

A better counterargument against an unfancy software architecture will be: this won't work at scale. And it might be right, at the edge of the scale: I don't know if Google or Meta can use an unfancy architecture. I haven't worked or built systems that have billions of users. But I venture to say that you can create software that serves millions of users just by using unfancy architecture. I did it once myself (and I hope to have the chance to do it again). I hope this will also help you get your first million users while developing something that still comfortably fits in your head.

Onwards!

## 1. Software only exists to store, retrieve and transform data.

Before we start, it is important to understand that the purpose of all software is solely concerned with data. Software instructs computers to do certain things. But those "certain things" are merely reading, transforming and storing data.

Without data, software makes no sense, and has no economic value. The end goal of any software system is to enable the reading, transforming and storing of data.

Correct software is software that correctly performs operations on data. Performant software is software that quickly, and with few computing resources, performs operations on data.

That's the whole game!

## 2. Think concrete first.

> "I had a scheme, which I still use today when somebody is explaining something that I'm trying to understand: I keep making up examples. For instance, the mathematicians would come in with a terrific theorem, and they're all excited. As they're telling me the conditions of the theorem, I construct something which fits all the conditions. You know, you have a set (one ball)--disjoint (two balls). Then the balls turn colors, grow hairs, or whatever, in my head as they put more conditions on. Finally they state the theorem, which is some dumb thing about the ball which isn't true for my hairy green ball thing. Then the fun begins. I keep explaining why their theorem isn't true, and they keep explaining why it is true. They're all excited because they think they have a counterexample to their theorem, and I keep trying to push it over the edge to see if it really does contradict something else. In that way, I can understand things very quickly." -- Richard Feynman

When designing a new system, or understanding an existing one, always start with the data. What are the precise data flows that are going to go through the system? How is the data going to be stored?

Going from the abstract to the concrete, when building software, is the main reason that makes it difficult to have a solid understanding of the system. By thinking inductively, that is, by going from the specific to the general, or from the concrete to the abstract, we can grasp a system with greater ease, depth and speed.

Abstraction is merely a range of concrete possibilities. You should not feeel ashamed of spending some time to design/understand exactly how the plain data that goes into a function comes out of it afterwards. This is incredibly valuable work, and it tends to be dismissed as "details". Foolishness! These details are the starting point of the understanding of your system.

Are you building an endpoint? Clearly outline the possible inputs to it, as well as all the outputs. State them as plain data - plain as in: strings, numbers, nulls, arrays and objects. Can you serialize it into a JSON? Then it is concrete enough.

Are you designing the database structure? Specify all of the tables and their columns. Remove unnecessary items, add the ones that you feel you will need.

Do not delay decisions. Rather, strive to make them in the right order, and revisit them when you need to.

Gain domain knowledge by playing around with different inputs and seeing their expected outputs. That's the whole game! Remember that it is all about data transformations.

Always think concrete. Ultimately, the digital is physical: data is physically stored in media, and it moves around as electrons through wires. Think of data as matter changing shape and moving from one place to the other, because that's exactly what it is! If you respect the concrete, physical nature of a software system, you will cut through the conceptual fog that usually envelops us when building software.

Do not worry about your design being to concrete, or becoming a slave to details. From the concrete and the understanding that you gain from mastering it, you will perform accurate, powerful generalizations (abstractions) that will allow your system to grow and adapt. The details are essential. The database is not a detail. HTTP is not a detail.

Specifying a system from a vague abstraction is the way to confusion. Think concrete first.

## 3. Use functions as the building blocks.

Functions are the most powerful way to express computation and, therefore, to organize software. It is worth our time to stop for a minute and understand what they are and why they are so powerful.

A function is, at its most essential:

1. A block of code.
2. That receives inputs.
3. That returns a single output.
4. That can be called from other parts of code.
5. That can define its own variables inside of it, without affecting the code outside of it.

Why are functions so useful?

1. By having a named block of code, we can avoid copying and pasting the code of the function everywhere that we need it.
2. By passing inputs to the block of code, we can cover different cases - basically, we can generalize from a single case to multiple ones.
3. By getting back a value, we can use this transformed piece of data to perform further computations.
4. By being able to define local variables, functions do not have to worry about creating unintended side effects and modifying a variable with the same name that happens to exist in another function.

A function is a concrete way to express computation. You can see a function as a machine that takes a set of inputs and produces an output. It combines both mathematical elegance (because it enables abstraction) and the physicality of a machine that takes things in and spits them out. Each function execution has a clear beginning and end; clear inputs and clear outputs.

This already is a lot. But there's more.

I am coming to the conclusion that what makes programming difficult is the amount of context that code needs. Context is merely data, just data, that is there "in the background". Without a way to provide context to a function, it would be impossible to create large programs. Here's why:

Imagine that a function could only access its inputs, and nothing else (besides the standard functions and operators from the language). Very quickly, you'd have to pass five, ten, fifteen arguments (inputs) to each function, to get something done! Even if a function X doesn't use a certain argument, but needs to invoke a function Y that does need that argument, then you'd have to pass that argument to X so that it can pass it onwards to Y!

This is where free variables come to our rescue. A free variable is a variable that has been defined outside of the function, but that can still be accessed by the function itself. These free variables are precisely the "context" of a function. Rather than passing them in explicitly as arguments, free variables are available to the function.

In languages where you cannot define a function inside another, like C (and C++ before 2011), the only free variables that are available to a function are those that are in the "global scope" of the program, that is, at the top level of it. (Note: I consider "file scope" to be a variant of global scope, in this context).

But in languages such as Javascript and Python, which support nested functions and closures, the inner function can access the variables of the outer function. Functions act as *context boundaries*. This allows for what is, in my view, the most elegant and definitive way of creating and handling context within a program. A function operates as a cell membrane, letting in whatever the cell needs (a certain free variable), and leaving things out that it doesn't (free variables that are not accessed, or local variables that the function creates for its own use).

Nested functions, or also functions that return other functions, quickly become useful in nontrivial programs. They are very powerful abstractions because they allow to control context freely and elegantly, while remaining understandable.

My suspicion is that most of Object Oriented Programming is an exercise in trying to create and handle context. The context is held inside objects, which are instances of classes; these objects combine data and functions. Despite its popularity, OOP is not as powerful at handling context as functions with closures can be; and OOP's conceptual edifice is much larger and complex than that of functions. Stated plainly: functions will take you farther than OOP, and it will be much easier for you to understand and maintain a program composed of functions rather than a program composed of classes and objects.

I think this is also because, at the very beginning, OOP seems more intuitive because it is based on something concrete: an object itself. Whereas a function, really, is a transformation, so it is a bit harder to picture concretely. However, the moment you see a function as something that turns an input into an output, the main conceptual leap to understand functions is done; whereas with OOP, when you understand a plain object, that is the beginning of your conceptual travails: classes, inheritance, polymorphism, and so forth.

Lest I avoid not offending yet another subset of programmers in this section, I also am convinced that purely functional programming is also restrictive and tends to complicate things. Orthodox functional programming is fixated on making functions *pure*, that is, not letting functions modify any values that exist inside of it, and merely perform its work through its return value. But, as we just saw briefly before, the power of functions with closures is in accessing context; and said access need not be read-only. It is mightily useful for an inner function to be able to modify the value of a variable defined outside of it - I believe this allows you to do things in Javascript that in C you'd only be able to do with pointers.

Further orthodox functional stands also forbid modifying the values of any variables *inside* a function, and require the programmer to create new variables based on older ones. This can quickly verge on silliness. In essence, programs are written to *modify* data. Data is not immutable; what we need to figure out is exactly when, where and how to modify it. And while most of the time, functions should not perform side-effects (that is, modify their context), sometimes that is exactly what they should do. The emphasis on purity of functions and immutability of data smacks of moralism and confers a rigidity to software that is even greater than the rigidity imposed by vanilla OOP.

(*By the way, I really do not intend to offend anyone. But in order to say what I believe is the truth, I must bear with the discomfort of saying things that can potentially enrage almost anyone who reads them*).

If you go with an unorthodox, straightforward way of functional programming in a language that supports nested functions and closures, you can pass around and modify context in an elegant way. And you can throw away all of these concepts:

- Dependency injection.
- Inheritance.
- Polymorphism.
- Instantiation of objects from classes.
- Public/private/protected variables.
- Immutability of data.
- Functional purity.

You will likely need some form of organization for your functions. You can put them inside modules. I would argue here for a light approach: create a plain object, and put your functions in there. For example, if you create a `DB` module to perform database operations, you can then write functions such as `DB.read` and `DB.write`. This sounds simple enough to be simplistic, but it is not. You can build large and demanding programs with this level of organizational simplicity.

I also recommend using, wherever possible, expressions instead of statements. An expression is anything you can put inside a data structure. For example:

- A number.
- A string.
- A null.
- A function.
- A function execution.
- A ternary.

Expressions can be combined and passed around. They are the gateway to metaprogramming, that is, a program that generates its own execution path at runtime. This is not something that is necessary most of the time, but some of the time it will be the best way to do something. That something, likely, is something both critical and almost impossible to do without metaprogramming. And when that time comes, you will have the power to do it.

Going from the unusual to the usual: use functional looping constructs (for example, `[].map`) rather than plain `for`/`while` loops. They are easier to read and can be nested.

## 4. Strictly validate everything that you expose, at runtime.

The single most important metric of the quality of a software system is how strictly it validates its inputs. At first glance, this is not intuitive. In principle, the quality of a system is just another name for its correctness (that is, producing the right outputs when receiving certain inputs). But by thoroughly specifying its inputs and rejecting invalid ones, more than half the quality battle is won.

If you think of a program as a set of functions, and if the goal of the program is to transform data, then a high quality program is one that transforms the data correctly. The starting point of transforming data is to understand what data comes in. And this is where validation becomes essential.

By specifying exactly what the inputs to a function should be, you will be thinking hard about how to transform that data as well in order to generate a correct output. It is very difficult to have a sloppy validation and not have a sloppy output, or to have a strict validation and have a bad quality output. In other words, good system design starts with thorough validation of inputs.

Any function that you expose to the outside of the program should thoroughly validate its inputs. It doesn't matter if you are the only one calling that function from another system that you yourself design. Do not trust the caller. Let your function raise the standard of quality and specify exactly what's acceptable and what is not.

As for internal functions, it is up to you. If you're building a library, you should probably validate the inputs to your functions, even if that costs you some performance - validation checks can be somewhat costly. If you're writing helper functions that you'll use in a couple of places only, you might want to skip the validations. But you should still clearly specify what their inputs should be.

The main obstacle in doing this is the same obstacle to thinking concretely: designing and specifying inputs in a thorough manner is seen as "details" that are non-essential. But remember #2, and think concrete first. In my experience, the act of designing the inputs will send me back to the stakeholders to ask interesting, nontrivial questions about the domain logic of what we're building. Start with the details, and the important domain questions will happen sooner than later - which is exactly when you want them to happen.

Once you strictly define the inputs to a function, how do you enforce their correctness? A typical way to do this is through a type system. And while this is perfectly possible, I am of the minority opinion that using types to do this is a straightjacket. A type system requires you to create types for everything; and any variation requires you to create a new type. Even though you can extend or modify an existing type, nontrivial validations are expressed as a maze of types. In essence, my contention against type systems is that they reify (that is, make a thing out of) something that is dynamic. Whether a certain key should be present or not is best expressed as a conditional, not as two types "WidgetWithPrice" and "WidgetWithoutPrice". Optional values don't cut it when you want, for example, two properties to be simultaneously present or absent, but not the intermediate cases.

Runtime checks are superior to types because:

1. You can use code to express them (that is, you have variables, conditionals, loops and functions to express them).
2. You don't require a separate syntax to write them.

The extra syntax really gets in the way of metaprogramming. It also makes the code more verbose and harder to read.

Runtime checks can be repetitive, but they can be compressed by utility functions. Many years ago I wrote [teishi](https://github.com/fpereiro/teishi) for exactly this purpose. But there's nothing magic about that implementation: just find out what are the most common validations, and write a succint syntax (purely made of expressions) that can validate and print precise errors when one is found. That's it!

The performance hit of validations is small. And never forget that in any system that you expose to the public, *you need to perform runtime validations* on all the inputs you receive, whether you use types or not. So you might as well do it dynamically everywhere, and keep the expressivity of code while you are at it.

The one thing you will lose without a type system are static typing checks, which can detect many errors before you run your code. That's to me the main benefit of static type systems: in a way, they are "running" your code, at least partially, and detecting errors on it. That's why we'll need to test thoroughly (see #6 below).

## 5. When you find an error, stop.

Let's take a trip back to post-war Japan. At the time, Toyota and a few other manufacturing companies were trying to vastly improve the quality of their products. Before and during the war, Japanese industrial manufactures were considered of low quality. In about a decade, the situation was completely turned around, and Japan started producing industrial goods that were of superior quality and lower cost than that of the United States.

How was this possible? Japan's industry went through a *quality revolution*. The impact of that process not only catapulted Japan to becoming the third economy of the world, but thoroughly transformed industrial production everywhere. From that revolution we also got concepts such as "just in time", "lean" , "kanban" and "kaizen". Many current software development practices actually come from adaptations to the core principles of the industrial quality revolution in Japan.

Here I would like to go to the sources. At Toyota, an industrial engineering named [Taiichi Ohno](https://en.wikipedia.org/wiki/Taiichi_Ohno) developed the main two concepts of the Toyota Production System, which is considered to be the crown jewel of Japan's quality revolution. The two concepts are:

- Just in Time.
- Jidoka (also known as "autonomation" or "auto-activation").

Here we'll focus on jidoka. The essence of jidoka is: an automatic process that doesn't detect when something goes wrong is **useless**. An automatic process that produces quality will immediately stop whenever an abnormality is found, and either report the abnormality so that it can be corrected immediately, or even self-correct the abnormality if it has the ability to do so.

The notion behind jidoka is that errors are not just "a fact of life", but rather sources of waste that can progressively and fruitfully be eliminated. If a system is properly constructed and improved, here's how its error curve can look like:

```
 |  |
 |  |
E|  \
R|   \
R|    \
O|     \
R|      \
S|       \
 |        \
 |         ---------
 +----------------------
       TIME
```

That is: a lot of errors at the beginning, a sharp decline afterwards, and an almost zero rate of errors as times goes by. This is what I call the "asymptotic expectation" towards error. Errors will probably never be banished, but their probability can be vastly reduced over time.

The beginning of this attitude is to sharply distinguish normal from abnormal operation. This means that you have to first develop a clear notion of what is an error and what is normal. Errors can be exceptions thrown by code, but they can also be unexpected errors in properly formed requests, or inconsistencies on the database.

Once clear definitions of what is normal and what is not are in place, there should be automated checks to detect errors.

Do not ignore any errors or unexpected behaviors in your logs. Chase them. When an error happens, stop everything else you are doing and investigate it. See errors the way that the FAA sees plane crashes. Investigate the deep causes for an error (which might be two, three levels deeper than the immediate cause for error). Then, eliminate that. Easier said than done, but it is almost always possible, and almost always very valuable.

When you experience an incident in a production system, make an incident report. Investigate why things happened like they did.

It is very demanding to work like this. But increasing the reliability of your system is what will let you avoid being forever stuck in "maintenance" mode of a piece of software that is constantly and unexpectedly failing, which requires constant patching of both the code and the data that it mangles. You can do better than that!

## 6. Test the real thing.

The main principle of unfancy testing is to assume that any part of your software that is not tested, is broken. If it is untested and yet it ends up to work, you were lucky.

This is quite curious. In other human contexts, a keen eye and a certain degree of rework will produce high quality outputs. This can be the case for both a novel or a financial statement. Why do we have to have a thorough and executable set of assertions (tests) that ensure that our code is, for the most part, correct?

I think the answer to this lies in that a small error in a software system can generate large issues in the data that it handles. There's a sort of butterfly effect at work: a small error can cause large disturbances; whereas a novel, or a financial statement, when parsed by a human, most small errors will not disturb the overall picture.

This is compounded by the fact that any nontrivial software handles a large amount of possible inputs and outputs; and even if your code is straightforward and unfancy, it is very common to make mistakes.

We can boil this down to one point:

1. It is almost impossible to create quality software without thorough testing.

We can also add two more points:

2. Thorough testing will often reveal deep design issues that will send you back to the drawing board.
3. Thorough testing will allow you to modify and rewrite your system fearlessly.

As for #2: like thorough validation of inputs, writing thorough tests will make you think hard about what you are building. You will start to think of corner cases; lurking in this "detailed" activity will be usually a couple of important domain learnings. All of this will increase the quality of what you are designing.

Writing tests is essentially about 1) creating a test case; 2) executing assertions after the case is executed. In other words, you are coming up with families of inputs and outputs, that your system should handle in a concrete way.

As for #3: if your tests are thorough, and you run them automatically (rather than manually), you can perform nontrivial modifications to your system, or tackle outright rewrites. A team that is maintaining a production system with weak tests will go extreme duress if it needs to perform nontrivial modifications or rewrites, because they cannot know what they are breaking until things break for real, in production, with production data. A thorough test suite covers most of the families of inputs and outputs that your system is supposed to provide, so that you can quickly figure out whether your system broke or not.

Testing operates a lot like double-entry bookkeeping: if the code is the first entry, the tests are the second. The code performs the data transformations, the tests then executes the code for all imaginable variations of them.

The standard approach to testing, nowadays, is the following:
- Test each of the parts of your software in isolation.
- Rely on sophisticated tooling to do most of the effort of testing for you.

From an unfancy perspective, this approach is very time consuming and leads to questionable results. This is for the following reasons:
- **Mocking considered harmful**: when you test the parts of your system in isolation, you need to mock every other part of your system that interacts with the part that you are testing. These mocks are not the real other parts of your system, but rather more like cardboard cutouts that return one or two values that "look like" what the other parts of your system would return. Often, mocks are incorrect approximations of the other components, which render tests unreliable.
- **You miss out on the holistic potential of tests**: when you are testing your software from the outside, like a client would use it, you can understand entire data flows. These complete data flows are more understandable than small parts of them.

As for the sophisticated tooling, for the most part it provides the following facilities:
- Run single tests to save time.
- Provide assertion mechanisms after each test.
- Report code coverage (the % of lines that was executed at least once during the entire test suite).

The first two are indeed useful, but can also quickly be implemented without a test runner. As for code coverage, this is a metric that provides an illusion of correctness: a single line of code can actually have multiple behaviors depending on its inputs. The important thing is to test all possible families of inputs, not to pick the least amount of inputs that will make sure that all the lines of your code are executed once.

The type of coverage one is looking for is to approximate a correctness proof for whatever you are testing. This is not easy and requires hard thinking about all possible inputs and outputs. You cannot specify all possible inputs, since the amount can well be infinite; you need to infer the families of inputs, and from there map the families of valid outputs.

If focusing on unit tests and tooling is training with machines at the gym, unfancy testing is like training with free weights. Simple, difficult and (in my view) vastly more effective.

Here are some principles for unfancy testing:

**1. Aim asymptotically to cover all possible inputs and outputs.**

We've just been through this. Think of many test scenarios.

To condense the tests, you can often create a list of inputs and outputs, and iterate through them, rather than manually writing each test with all its boilerplate.

**2. Test things in as real a situation as you can: integrated, not isolated.**

If you are building an integration between two systems, test your integration running against those two systems. Strive to have a sandbox version of those systems that approximates as much as possible their production counterparts, so you gain more confidence that whatever works in your test environment, will also work in production.

Rather than spending your time creating mocks, spend your time figuring out how to test something that looks like the real thing as much as possible. It will be much more rewarding, for the simple reason that the mocks don't exist in production: only the real parts of the system do.

Unit tests are best saved for self-contained, involved pieces of logic. A good example is a function that implements an algorithm. This function can profitably be unit tested, because it takes only a few inputs and has also a straightforward output.

At the other end of the spectrum, you have systems that are composed of integrations, whose job is to process information, pass it along and also perhaps generate side effects in the form of events. The dependencies of these systems take enormous effort to be mocked, and the exercise is mostly futile. Unit testing, for these systems, is akin to snapshot testing but at the level of code - something that is only better than no testing at all. Figure out a way to do better, and test the real thing.

In summary: test the system from the outside, like a real user would.

**3. First test for invalid inputs, and check that you get proper errors.**

Invalid inputs are also inputs. Divide them into families. Make sure that your system not only rejects them, but that the proper error messages are returned.

Since the validations of your code are executed linearly, you will probably have to write your invalid inputs in the right order, so that certain errors can be "reached". For example, if a function requires two arguments, X and Y, and validates them in that order, then you need to pass a correct X so that you can test whether Y will be properly validated.

It is possible and fruitful to create code to generate invalid inputs. Rather than generating random values, the generator should take a list of validations, and generate invalid inputs in the correct order; the furthest you go, the more "valid" an input is. Ultimately, we might get to a point where our validation functions directly generate invalid inputs. At that point, one might start to question the value of testing in this manner. I don't have a good answer for that. All I know is that we're not even remotely there today, at least in mainstream software development.

**4. Don't use fixed waits for asynchronous assertions; rather, retry automatically until the assertion passes.**

When you are dealing with integrations with asynchronous processing, your tests need to wait for an unspecified amount of time until something passes. For example, if you build a file service that uploads files to S3 in the background, when you get back a 200 the file will still not be in S3. How do you test then that the file is in S3?

The default way to do this is to wait an arbitrary amount of seconds. But this is both inefficient (because the amount of time is usually excessive) and unreliable (because if the external service or the network are too slow just this time, the test will fail). Rather, use a function that performs a retry mechanism for your assertions - for example, test your assertions for a given test every 20 milliseconds, for up to 20 seconds. The first time that the assertions pass, the test passes. This will reduce the runtime and increase the reliability of your tests.

**5. Test the backend first, then test the frontend assuming the backend is correct.**

It is much more fruitful to test the backend first, which is ultimately responsible for the correctness of the data flows of the system. Once you have this down, you can test the frontend, fully using the backend.

In this way, you also avoid the expensive and mostly futile exercise of mocking a backend when developing a frontend.

There's no good name to summarize this philosophy of testing: end to end (e2e) is usually focused on testing a backend through a frontend; integration testing is focused on just testing integrations, rather than the whole thing. For now, I am going either with *unfancy testing* or *whole system testing*. I'm open to suggestions.

## 7. Design the data flows to be continuous

Let's go back to Taiichi Ohno's first pillar of quality production: "just in time". This is also a very fruitful concept when applied to software.

If we think of data as the material on which software works, we can apply also the "just in time" principle. We have already defined precisely where the value of the transformations is (the possible inputs and desired outputs). We can then specify a value chain, where each incoming piece of data is transformed and sent back (or stored).

Throughout the value chain - which, for software, is the sequence of transformations for a piece of data - the data can be made to smoothly flow. Rather than storing it in intermediate locations, the data should be worked on continuously, in the right order, until it reaches the desired state.

Data should be readily available when it is requested by the user, but ideally exactly at that time and not before. This means that most data transformations should happen as a result of a user "pulling" data from the system; and those transformations should happen as fast as possible, to avoid keeping the user waiting.

Over time, possibilities of improvement for the data flows can be detected and implemented, so that the data flows converge to perfection over time.

The above condenses lean manufacturing, applied to software. If we go to a standard definition of lean manufacturing:

> 1. Value: Specify the value desired by the customer. "Form a team for each product to stick with that product during its entire production cycle", "Enter into a dialogue with the customer" (e.g. Voice of the customer)
> 2. The Value Stream: Identify the value stream for each product providing that value and challenge all of the wasted steps (generally nine out of ten) currently necessary to provide it
> 3. Flow: Make the product flow continuously through the remaining value-added steps
> 4. Pull: Introduce pull between all steps where continuous flow is possible
> 5. Perfection: Manage toward perfection so that the number of steps and the amount of time and information needed to serve the customer continually falls

We can readily see that, by seeing data as the product processed by software, these principles can be applied to system design.

Avoid pre-computing things that don't take much time to be computed. This is the equivalent of inventory. Avoid holding your information in objects in memory, if you already store your information in efficient and readily queriable databases. Keep things lean and minimize your digital inventory. Be very aware of what you cache.

## 8. Minimize the big four: dependencies, lines of code, files and abstraction layers.

> "The price of reliability is the pursuit of the utmost simplicity." -- Tony Hoare

If most software is difficult to write, it is not because we are implementing difficult algorithms. Most of the difficulty of software relies in keeping the complexity of the system at bay. A system that can grow while remaining simple enough is perhaps the operational definition of software engineering success.

The pursuit of software simplicity is the way in which we can build systems that we don't want to throw away a couple of months after they are written. Systems *can* get better with time, provided that we design them properly. This is difficult, perhaps very difficult, but worth it.

From an implementation perspective, all other aspects can and should be subordinated to the pursuit of simplicity. While it is not easy to have a consensus about what's simple and what's not, a few variables do stand out:

- Number of dependencies.
- Number of lines of code.
- Number of files.
- Number of abstraction layers.

All of these should be aggresively minimized, subjected to the following constraints:

- The system should be functional (ie: do what it's supposed to do).
- The system should be correct (ie: return the right outputs).
- The system should be performan (ie: use a decent amount of computing resources to create results in a short amount of time).

Dependencies are double edged swords, swords that more often than not inflict damage on those who wield them. Dependencies should be very carefully considered and picked. The benefits of a dependency should be great in order to use it. Every library that you use should be worth its weight in silver, if not gold. Why? Because dependencies are merely code that someone else wrote, which might not be well tailored to your use case, which mmight be incorrect, and which might break because in turn it depends on other dependencies.

The best dependencies are those that:

1. Solve a difficult problem.
2. Have very few dependencies themselves.
3. Have been around for a while.
4. Are clearly documented.
5. Are comparatively small in lines of code.

The long term reliability of a system, I dare say, is an inverse exponential function of the amount of its dependencies. If you have +1000 direct and indirect dependencies, good luck running your system in five years without major surgery.

As for lines of code, Fred Brooks points out that they are the main "cost" of a software system. The largest the system is, the more bugs it will have. Perhaps his main empirical discovery (which came from managing OS/360, the creation of the first operative system which supported multiprogramming) was that the bugs were proportional to the lines of code of a system. This warranted the use of high level languages, since you could write more functionality with them in a smaller amount of lines, therefore reducing defects proportionally.

This is also valid for contemporary, high-level language code. The less lines of code you have, the less bugs you will have, and the easier your system will be understood. Your code should not be cryptic, and I'm not saying you should stuff as many statements as possible in a single line. What I am saying is that succintness is extremely valuable in code, and that it will take you very far.

Which brings me to reading code. Any literate human is capable to read entire *books*. It is, to me, plainly ridiculous that source code for a medium project (say, 10k lines of code) has to be scattered into hundreds of files. It doesn't matter if you use a modern IDE to jump from file to file. That level of fragmentation does not help to udnerstand the software; it merely inflicts a death of a thousand cuts on whoever is trying to understand what you wrote. The only benefit of this fragmentation is that it promises "bite-size" portions.

Stop writing software in fragmentary Sumerian tablets; start trying to write masterpieces, or at least decent novels, that have extension and context. Write a system in two, five, perhaps twenty files, but not a hundred. Trust that if you write things properly, succintly and in a reasonable order, people (including the future you) will be able to read it; a thousand lines should not daunt you, but encourage you.

Last, but not least: minimize the amount of abstraction layers in your system. For example, if you are building a CRUD app exposed to the web, you can have three layers:

1. The HTTP routes.
2. Application routes that validate, process and store data, interacting with layers #1 and #3.
3. A database layer.

That's it. You could even compact it into one layer without much loss of generality. You definitely do not need five or ten layers. Each abstraction layer has also to be worth its abstraction weight in silver, if not gold. Don't make your data go through hoops; keep the data flows lean and understandable.

The most effective way to create fancy software is to add multiple abstraction layers and handle their complex interdependencies; conversely, to effectively create unfancy software, use few but essential abstraction layers. And do it while minimizing the overall code that you write, the amount of files in which you write them, and the dependencies on which you rely. Aim to write software that you will be able to run, read and perhaps even admire ten years from now.

## 9. Write your own documentation

Writing documentation is hard work. That is a feature, not a bug. Documentation is there to be read by humans. You don't want to make someone spend minutes of their precious existence poring over remedial, or - even worse - autogenerated documentation. Documentation is there to explain the purpose of your software, the main aspects of its data flows, and the most important (or curious) decisions that you took in the process of implementing it.

A lot of documentation will be both detail-oriented and repetitive. If you are documenting your API endpoints, you'll have to put a lot of detail in there. But, again, this is OK. When you write those docs, you'll probably also come out with insights on how to improve your code, even sometimes going back to the drawing board.

If you want to take this to the extreme, practice literate programming, where you annotate your entire code with prose explaining it. I've taken to a more limited approach which I call "annotated source code" (and which I borrowed from Jeremy Ashkenas). It is a pain in the rear to do it, but always worth it. I've lost count of the amount of bugs and optimizations that I found while annotating code.

## 10. Think calmly about security.

Security is an essential aspect of software. No one wants to be responsible for compromising one's users and being liable to ridicule, loss of trust and potential legal procedures. In an economy where data is the main value, the security of software determines to a great extent the security of the entire economic system.

But if something is so important, and so difficult to get right, it is all the more important to have a calm approach to security. Rushing to implement security without understanding what you are doing is a bad idea, because you might be creating new attack surfaces as you close others.

Calm doesn't mean slow or lazy. Calm means that you will determine what are the few yet essential decisions that can prevent most security issues. For example:

- Hash all user passwords.
- Encrypting all the data flows that leave a machine in your control.
- Do not commit secrets to a version control system.
- Safeguard against both XSS and CSRF attacks by having both HTTP only cookies *and* CSRF tokens.
- Authenticate each and every incoming request.
- Pick a secure operating system.
- Minimize dependencies.
- Keep your software updated.

But the list emerges from a calm approach. If you don't understand why you should implement a security best practice, dig deeper until you do. You don't have to understand how encryption works, but you do need to understand why it is needed.

Above all, avoid the cargo culte of security; that is, doing actions that seemingly increase the security of a system, but actually do not do so and instead create an illusion of security. For example, there is nothing wrong with having configuration variables inside your code, as long as none of these is a secret key.

To slay security cargo culting, use Kerckhoff's Principle: what protects your system is not obfuscation, but rather a clear, shining design, that also uses a few secrets. All that differentiates you from an attacker is that you are in possession of those secrets and you are not. Things that you do "just in case" to make it more secure actually make your system more complex and therefore can add, over time, potential attack vectors.

Security cannot be bought either; you can use money to buy and leverage existing systems, but if you buy complex systems that you don't understand well, you are not buying just solutions, but problems too. If your integration with a big, existing security system is buggy, you will have multiple vulnerabilities. If security was just a matter of throwing money at the problem, we wouldn't see the amount of software vulnerabilities that we do see in rich and established tech companies.

If you are building a backend, I'd recommend storing your cookies in the database, and checking every request against the database. JWT tokens are generally a bad idea, because you are distributing secrets to the client and then having to trust them based on a single secret key. This prevents you from a more fine-grained approach to making secrets stale.

If you can, use a high level language where you don't have to manage your own memory. Even in 2023, so many vulnerabilities arise from exploitable memory overflows.

In general, where possible, aim to eliminate *families* of security issues.

And remember Steve Yegge's dictum: Security and Accessibility are nemesis of each other. It is an art to build a secure system that is also easy to work with, both as user and implementer.

## 11. Choose between consistency and availability.

If you are building a service, and that service is used by many users around the globe, you will have to partition your service to run in multiple places (servers) simultaneously.

The CAP theorem states that you can only choose two out of three: consistency, availability and partitioning. If you partition your system, therefore, you can only choose between consistency and availability.

This choice means the following: when things go wrong, you can either make your system always be **consistent and unavailable**, or **inconsistent and available**. Consistent means that you will not violate your data constraints, either in your database or in the data that you return to an user. Available means that you will keep on processing data always.

Usually, system designers choose availability, although in areas like banking there is a much stronger emphasis on consistency.

Partitioning your API nodes is usually not an issue, because each API node holds a very small amount of state, which is the data in transit that's currently being processed. What *is* a big deal is to properly partition your database. If you are building a non-trivial system, it is very important to understand how your DB is partitioned.

Do not think that using the best and latest practices and cloud services, you'll be able to create a system that can scale infinitely while remaining consistent *and* available. Face this hard choice early on, so that when the unexpected strikes (a network partition, a datacenter outage) you will have something to work with.

## 12. Avoid change for change's sake.

> "Men desire novelty to such an extent that those who are doing well wish for a change as much as those who are doing badly." -- Niccolò Machiavelli

When a new tool, language, paradigm or architecture comes along, your default answer shouldn't be "that wouldn't hurt". Rather, it should be "no". Be extremely selective about the tools, languages, paradigms and architectures that you pick. It is one thing to play with new things; it is entirely another one to do it at work, when your entire system is at stake.

Pursue a radical conservatism: say "no" almost always, and when you say "yes", make it so because you are willing to transform your system through the innovation.

If your software remains correct, performant and flexible, you won already. Disregard the popularity contest. Focus on the signal, not the noise.

## 13. Always pick the unfancy alternative.

If you are in doubt about whether to implement something or not, consider whether you are doing it because you need it, or because it is fancy and it sounds like what everyone is doing these days. It might be that a few things are just fancy but have no clear benefits.

Many other things, like an event system, are initially not necessary and would be plainly fancy in the context of a small system with no integrations. But if your system grows and interacts with multiple integrations (and especially if those integrations are unreliable because of reasons outside of your control), the event systems becomes essential; then it is no longer fancy, and you can use it.

Same goes for layers: if you are 100% sure that you will only serve HTTP requests, you might want to have a single layer for both HTTP routes and application logic. But if you will support webhooks, it is actually unfancier to split the outermost functions (one for HTTP and one for websockets), and then let each of those functions call the corresponding application function with the same inputs.

The phrase "keep it simple, stupid" is singularly unhelpful. To say that to a software developer is like saying to a composer: "keep it beautiful, hack". Simplicity is the emergent result of a grounded and laborious process of software development. It is the end result, not the means to that result.

For that reason, I'd rather say instead: "keep it concrete, smart one". You have to be quite smart to develop software; this is not an empty compliment, but a fact. If you rprofession consists of building and maintaining complex logic that interacts with vastly more complicated systems also made of logic, you rely on your wits just to earn your keep.

Unfanciness is vulnerable, because you cannot hide behind a burst of concepts that momentarily confuses others. You are exposing yourself, your concepts, and because you are trying to make yourself and your software as simple and understandable as possible, it is much easier for others to spot your mistakes.

Unfanciness is not popular, because it induces vulnerability in those who practice it.

But unfanciness lifts the weight of confusion from your shoulders. It lets you walk towards clarity. And even if you take many hits, because you are such an exposed target, every hit can make you grow. Isn't that the point, anyway? Try to follow the path that makes you correct in the long run, rather than the one that reduces the chance of you being found out wrong in the short term.

## Question this!

Anything I learned, I learned from others or direct experience. Please challenge this, especially if you have direct experience that supports your challenge. Open an [issue](https://github.com/fpereiro/unfancy/issues) or comment on the HN thread.

Thank you for reading. May you find joy and improve the world through your code!
