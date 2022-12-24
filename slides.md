---
theme: apple-basic
layout: intro
class: text-left
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
title: Unit testing with Jest
---

# Unit Testing With Jest

Introduction to unit testing in JavaScript

<div class="absolute bottom-10">
  <span class="font-700">
    Ezz Abuzaid  - Senior Software Engineer @Kortext
    <br>
    24/12/2022
</span>
</div>

---

# Agenda

<ul>
  <li>Testing Introduction.</li>
  <li>Why We Should Test?</li>
  <li>Jest</li>
  <li>Unit Testing.</li>
  <li>Best practices.</li>
  <li>FAQ.</li>
</ul>

---

# What is Testing?

  <ul>
    <li ><span v-click>Testing is the process of making sure that a system meets certain predefined requirements</span><span v-click>, by executing it either manualy or by using an automation tool.</span></li>
    <li v-click>The process of identifying defects and errors before shipping a system to the users.</li>
  </ul>
  <p v-click>Requirements?</p>
  <small v-click>A requirement can be as high level as "A user should be able to order a meal and pay for it from within the app itself"
    or low as "When a user clicks on "Add to cart" button the user should receive a notification.</small>
  <br><br>
  <small v-click>Requirements can range to different levels based on the department (Marketing, Sales, Compliance) or based on a speciality (Architect, Product Owner, Developer)</small>

<!--
Before diving into unit testing, let's start by defining testing in general so we can build upon it.

To facilitate the definition I've broken it down into two blocks which will answer three question

What?
How?
Why?

The manual part involves human interaction in almost every step.

The automation tool, may or may not require human interaction at all.

To elaborate on the requirement bit
-->

---

# Types Of Testing

<ul>
  <li>Unit tests.</li>
  <li>Integration tests.</li>
  <li>End-to-end tests.</li>
  <li>Performance testing.</li>
  <li>Smoke testing.</li>
  <li v-click><h2><b>Regression testing</b></h2></li>
</ul>

<!--
We have got a number of types, we won't go over all of them today. The outlined list is most known to the developers and what usually is performed in the software lifecycle. anyway, I'll briefly explain each one and speak about its importance.

Integration: Testing the integration between two or more modules. Usually, in this testing phase, you want to ensure that a set of modules can interact and work well together. in the integration test you'd have to mock the database and any other 3rd party service as you won't be interested in all services outputs. what is important here is that you verify the interaction is happening according to what is correct.

End-to-end: Test the application under real circumstances, like replicating how a user would use it with real data to ensure the application performs regardless of the role of a user. The end-to-end test happens at the User Acceptance phase in SDLC.

Performance: as simple as testing the software under different workloads to help you measure the reliability, speed, scalability, and responsiveness of the software.

Smoke testing: This test ensures that the basic functionality is working before going further. Smoke testing can signal whether.
1. We should perform the E2E test as the E2E test is an expensive operation.
2. or if we should continue the deployment.

The last one and perhaps the major one is regression testing.

Regression test: after any change to the codebase or the software in general we perform this test to ensure that the previously working functionality is still are. in case one of the functionality is not working then we say a regression happened.

A regression test doesn't have to be a separate step, usually, it is associated with any kind of test that you perform. so if we're executing the integration test and the results allude that something that was previously working isn't anymore, then we have done regression testing, the same applies to other types of tests.

The regression test is a bit free form, meaning that it can be defined on a per-company basis, the important thing is that it has to be run on the current functionality and not the new ones.
-->

---

# Why?

<ul>
  <li v-click>To deliver working software to the users.</li>
  <li v-click>To build customer trust and satisfaction.</li>
  <li v-click>To have more resilient (recoverable) and robust software.</li>
  <li v-click><u>It's not unit testing chore to ensure the above, however, it can play huge role in doing so!</u></li>
<br>
  <h2 v-click>Why unit test?</h2>
  <li v-click>Because it's cheaper!</li>
  <li v-click>Living documention.</li>
  <li v-click>Can be source of truth.</li>
  <li v-click>Boost confidance.</li>
  <li v-click>Ease onboarding of new developers.</li>
  <li v-click>Early feedback.</li>
  <li v-click><b>Catch regressions.</b></li>
</ul>

<!--
1. Isn't that everyone's job after all? right!

2. Your user/customer has to trust you in order to pay for your product and use it, if you're to ship software that has major features not working, your customer base will fly away to a competitor.

3. If you do test your product, then you can almost roll back to the last working version without being terrified.

4. Fixing bugs, especially old bugs costly, and sometimes impossible. No one likes to do changes to old features because we know for a fact that it has a big chance of introducing bugs, worst, it can introduce bugs in other areas that no one will know about.

Other than a bug being old, the process of finding a bug is cumbersome, after you push a change that contains a bug, the software tester will have to create a defect, and the team lead or product manager or scrum master has to prioritize it. you, the developer have to fix the bug, the software tester then verifies it got fixed and if not the cycle will go again.

6. Living Documentation: when writing unit tests you'll find that you don't need to write inline comments to explain why a function behaves as it is because writing unit test force you to explain the behaviour in a human-readable approach.

7. Can be the source of truth: after 6 months, or a year, you received a ticket to change something, but you don't recall what this feature was about. so you start to renew your knowledge about it by going to old tickets, but you still can't grasp what the feature is about, because it the time it was written, you had to consult multiple parties to understand it. Thanks to the unit test, it'll shrink the knowledge gap to only learn about the functions you want to change, better, you'll be able to understand faster than a ticket description because the unit test is written specifically for you, the developer.

8. Boost confidence: do you remember that old feature that no one wants to work on it? Unit tests will give you confidence in changing old stuff because you can rely on them to catch regressions. a lot of developers avoid changing old code because of accountablity that comes with change or they end up investing large amounts of time rewriting. Unit test at least can mitigate the burden and time.

9. Ease onboarding of new developers: when you join a new company and learn about the code, the first thing that you'll do when you are stuck at a function or class is to ask, which is fine. but what if there were unit tests that explain what that function does?

10. Early feedback: usually, You receive a ticket, start coding and ask questions that might pop up in your head. those questions most of the time about happy scenarios, but what about edge cases? by writing a unit test, you'll naturally encounter cases that aren't covered in the tickets. so instead of waiting for other people to find those bugs, a unit test will do you a favour.

11. Catch regressions: I'd say that the sole purpose of unit tests is to catch regression, if you're refactoring, your second hand, the unit test will catch regression on the fly, changing old functionality, don't worry, the unit test is here to help you, you're new to the project and afraid that your first PR will introduce a bug, the unit test is here to help.
-->

---

# Why Not To Test

<br>
<h2>Increase development time?</h2>
<br>

<img src="/test-chart.png" width="500" />

<!--
Someone might say that writing test delays or slow development time and shipping features, while that might be true sometimes, however, the development time will get slow in the long term without unit tests due to the number of bugs that will rise, the difficulty of understanding the requirement behind the code, and the difficulty of understanding the code itself.

The Red line represents the amount of work you can do without a unit test. At first, the amount of work can be shipped perhaps quicker in contrary to writing test and shipping code at the same time.

A few months later, you do a change, and it breaks a working feature(s), so instead of focusing on the new feature, you are stuck fixing new bugs.

With a unit test, you might need slightly more time to ship a feature, but in the long term, when compared to not having a unit test it's definitely quicker.
-->

---

# What is Unit testing?

<p v-click>A unit is the smallest piece of a functionality.</p>
<p v-click><small><i>HINT:</i> One function can have multiple unit tests.</small></p>

<p v-click>Unit testing is the process of making sure that a unit</p>
<ul v-click>
  <li><small>Meets requirements.</small></li>
  <li><small>Keeps meeting those requirements overtime.</small></li>
  <li><b>Is a regression free.</b></li>
</ul>

<h3 v-click>A unit test should</h3>
<ul  v-click>
  <li>Verifies a single unit of behavior.</li>
  <li>Does it quickly.</li>
  <li>And does it in isolation from other tests.</li>
</ul>

<small v-click>
Hint: We refer to the thing that is being executed as the "{Thing} Under Test", so in case of testing a System as whole we say, "System Under Test" or if we're taking about unit test then we say, "Unit Under Test".
</small>

<!--
Now that we know what is testing, let's define what is meant by A Unit

-Keeps meeting those requirements over time.- this is where unit tests play a crucial part. think of doing refactoring, you changed something, but the test failed because the changes altered the actual requirements. Thanks to the unit tests, you won't spend your break figuring out an excuse.

anyway, if this happened, we call it Regression. A change that has caused a working functionality to break.

goals

1. A unit test should verify only one thing at a time. if you're writing a test case that verifies more than one thing, then you should think again.
2. A unit test should be fast. CI/CD time
3. Meaning, unit tests shouldn't be dependent on each other. for example, if a unit test verifies that the button does something, the other unit tests should pass without waiting for that unit test to pass.
-->

---

# What Is Jest

Jest is a JavaScript testing framework built on top of **Jasmine** and maintained by Facebook.

- Both test runner and assertion library.
- Support all popular frameworks.
- Have rich API.
- Provide mocking API.
- Can be used with esbuild, webpack, ...etc

---

# Test Suite & Test Case

<p>Test Suite is group of related test cases that speak about a functionallity or behaviour.</p>
<p>Test Case is the setup, invokation, and assertion of the unit under test, <span>it should do one thing: assert single output or verify one behaviour</span></p>

<ul>
<li ><small>Test Suite can be defined using the `describe` function</small></li>
<li ><small>Test Case can be defined using the `it` or `test` function</small></li>
</ul>
<h3>Elements of a test case</h3>
<ul>
<li ><small>Report/Description</small>
<ul>
<li ><small>Test report is a simple and strightforward description that speaks about the unit under test.</small></li>
<li ><small>Test report should clearly tell what is being tested and under which cirumstance. Anyone with fair bit of knoweldge in the team should be able to understand the test suite and test case from its report.</small></li>
</ul>
</li>
<li ><small>Setup: Prepare the inputs, mocks, and whatever the unit under test needs in order to get invoked.</small></li>
<li ><small>Invocation: Invoke the unit under test.</small></li>
<li ><small>Assertion: Verifies that the unit under test behaves as expected.</small></li>
</ul>

<!--
To put everything into action, let's see the following example and how each element is applied.
-->

---

# Your First Test

```js
function sum(...numbers) {
	return numbers.reduce((sum, current) => (sum += current), 0);
}
```

<small v-click>
```js
describe("Sum", () => {
 it("sums up the provided argument", () => {
  expect(sum(1, 2, 3, 4, 5)).toEqual(15);
 });
});
```
</small>
<small v-click>
```js
describe("Sum", () => {
 it("sums up the provided argument", () => {
  // Arrange
  const expected = 15;
  const numbers = [1, 2, 3, 4, 5];
  // Invocation
  const actual = sum(...numbers);
  // Assertion
  expect(actual).toEqual(expected);
 });
});
```
</small>

<!--
So use describe to define the test suite and `it` to define the test case. since the sum function has only one behaviour we wrote only one test case.

As you can see, we didn't separate the elements of the test case. the setup invocation and assertion are all in one line.
-->

---

# Your First Real Test

<p v-click>
 - <b>Requirements</b> <br> <br>
  Given a counter widget <br>
  When a user click on the "Increase Button" <br>
  Then the counter label increases by 1 <br>
</p>

<p v-click>Implementation</p> <br>
<small v-click><br>
```jsx
function Counter() {
 const [counter, setCounter] = useState(0);
 return (
  <>
   <div>{counter}</div>
   <button onClick={() => setCounter(counter + 1)}>Increase</button>
  </>
 );
}
````

</small>

<!--
create project with npx create-react-app counter example after this slide
-->

---

# AAA

Arrange, Act, Assert (AAA) is a pattern to write a unit test to make them readable, structured and clean.

<ul>
<li ><small><s>Setup</s> Arrange: Prepare the inputs, mocks, and whatever the unit under test needs in order to get invoked.</small></li>
<li ><small><s>Invocation</s> Act: Invoke the unit under test.</small></li>
<li ><small><s>Assertion</s> Assert: Verifies that the unit under test behaves as expected.</small></li>
</ul>

Querying the elements

- CSS selectors
- textContent
- tagName
- data-testid attribute

<!--
AAA: These labels are standardised among the community, so it's better to use common names than reinvent them.

Querying the elements: There are many ways to query an element, but in the unit test we need to make sure that the method we use doesn't affect the unit test.

A unit test shouldn't be coupled to the code under test, meaning removing elements that are made for design purposes shouldn't affect the unit test. if we are to use a class name to query an element in the unit test then any changes to the class attribute will affect the unit test.

add bg-black class to the text label and use it query to show them.

using data-testid tells that this attribute is used only for unit tests and nothing else.
-->

---

# New Requirement

- Requirement <br>
  Change the counter widget to increase based on the user provided input

  Given user enterd a number in the "Increase By" input<br>
  When a user clicks on the "Increase Button" <br>
  Then the counter label increases by the user entered number <br>

<small v-click>

- <b>Implementation</b> <br>

```jsx
function Counter() {
	const [counter, setCounter] = useState(0);
	const [increaseBy, setIncreaseBy] = useState(1);

	return (
		<>
			<input
				placeholder="Increase By"
				value={increaseBy}
				onChange={(event) => setIncreaseBy(+event.target.value)}
			/>
			<div>{counter}</div>
			<button onClick={() => setCounter(counter + increaseBy)}>Increase</button>
		</>
	);
}
```

</small>

<!--
Change the `increaseBy` to 2 and show them how the unit test caught the change.
-->

---

# Write Your First Test Suite

1. Open https://github.com/ezzabuzaid/testing-workshop-sample
2. Run `npm install` in the command line
3. Run `npm test` in the command line
4. Open `TodoList.js` file

---

# Test Double

Test Double is a generic term for any case where you have to replace a production object with fake one for testing purposes.

Types

- Stub, Mock, Spy, Fake, and Dummy.

<p v-click>Stub: help to emulate incoming interactions. These interactions are calls the Unit Under Test makes to its dependencies to get input data.</p>
<ul>
  <li v-click>Provide a predetermined response.</li>
  <li v-click>Take a predetermined action, like throwing an exception.</li>
</ul>
<p v-click>Mock: Is stub on steroids.</p>
<ul>
  <li v-click>verify the collaborator's method is called the correct number of times.</li>
  <li v-click>verify the collaborator's method is called with the correct parameters.</li>
  <li v-click>verify the collaborator's method is called with the correct parameters throw an exception if they receive a call they don't expect.</li>
</ul>

<!--
When it comes to testing a function that uses HTTP, does extensive calculations or sends an email, we need to mimic the behaviour so the test doesn't depart from its purpose.

We have to mimic all edge case if the unit test about sending an email then we need to mimic a case where the event will be sent successfully and when it'll throw an exception.

This thing is named Test Double. The closest example to real life is the actor's double. where a movie has someone who looks exactly like the actor but is hired to do stuff that the actor cannot.

Test double have types.
-->

---

# New Requirement

- Requirement <br>
  Change the counter widget to increase based on value from the API.

  Given user-saved "Increase By" value<br>
  When a user clicks on the "Increase Button" <br>
  Then the counter label increases by the user-saved number <br>

<small v-click>

- <b>Implementation</b> <br>

```jsx
function Counter() {
	const [counter, setCounter] = useState(0);
	const [increaseBy, setIncreaseBy] = useState(1);
	const { get, response } = http.useFetch("https://counter-real-api.com");

	useEffect(() => {
		get("/increase-by").then((increaseBy) => setIncreaseBy(increaseBy));
	}, []);

	return (
		<>
			<div>{counter}</div>
			<button onClick={() => setCounter(counter + increaseBy)}>Increase</button>
		</>
	);
}
```

</small>

---

# Test Hooks

A test hook is function that run before or after a test case. Often, while writing tests, you have some setup work that needs to happen before tests run, and you have some finishing work that needs to happen after tests run.

<ul>
  <li v-click>`beforeEach`.</li>
  <li v-click>`afterEach`</li>
  <li v-click>`beforeAll`</li>
  <li v-click>`afterAll`</li>
</ul>

---

# Parameterized Tests

Parameterized tests are a good way to define and run multiple test cases, where the only difference between them is the data.

Very useful to test validation functions.

<small v-click>

```js
describe("Sum", () => {
	it("sums up to 15", () => {
		expect(sum(1, 2, 3, 4, 5)).toEqual(15);
	});
	it("sums up to 20", () => {
		expect(sum(1, 2, 3, 4, 5, 5)).toEqual(100);
	});
	it("sums up to 100", () => {
		expect(sum(50 + 50)).toEqual(100);
	});
});
```

</small>
<small v-click>

```js
describe("Sum", () => {
	it.each([
		// [{ numbers: [1, 2, 3, 4, 5] }, { expected: 15 }],
		[{ numbers: [10, 9, 1] }, { expected: 20 }],
		[{ numbers: [50, 50] }, { expected: 100 }],
	])("sums up to $expected", ({ numbers, expected }) => {
		expect(sum(...numbers)).toEqual(expected);
	});
});
```

</small>

<!--
Going back to the sum function, let's say we want to add more test cases to ensure different scenarios are working correctly.

The first thought is to replicate the test case with different inputs. another way is to create a loop. a better way is to use parameterized tests using `it.each` function.

When ever you're calling the same unit under test but with a different argument then use parameterized test.
-->

---

# Matchers

Matcher are assertion api to test values.

Ref: https://jestjs.io/docs/using-matchers

---

# New Requirement

- Requirement <br>

  Given a todo list<br>
  When a user add new todo <br>
  Then it should avoid adding todo with white spaces<br>

<!--
/^\S*$/
-->

---

# Post Talk

- When to write unit test?
- When not to write unit test?
- Disadvantages.
- Code Coverage.
- Best Practices.
- Consistancy.
- FAQ.
- Discussion!

---

# Write unit test when

- Refactoring code
- A bug is discovered - <i>Once a human tester finds a bug, it should be the last time a human tester finds that bug. The automated tests should be modified to check for that particular bug from then on, every time, with no exceptions, no matter how trivial, and no matter how much the developer complains and says, “Oh, that will never happen again.” Because it will happen again. And we just don't have the time to go chasing after bugs that the automated tests could have found for us. We have to spend our time writing new code—and new bugs. - Excerpt from The Pragmatic Programmer</i>

- The answer is yes to the following questions:

  - Is this functionality part of what makes the product (Identify the core product features)?
  - Would changing a functionality cause other things to break?
  - Would changing a functionality make the feature unusable (either UX or functional)? _-point. a-_

- _To stop as soon as you are confident that, if someone comes to introduce a regression in the code you have just produced, one of the tests in place will fall into error. - Excerpt from_ **Refactoring: Improving the Design of Existing Code** by **Martin Fowler**

<!--

I'll tell you an example of a trivial case happened few days ago. one of my team members changed an anchor tag to a button.

For instance, Change the anchor tag to a button - you would still have the same functionality but the user will not be able to open the link in a new tab, depending on the page context, changing it to a button might not affect the user at all, but in case the page has information, then a user might want to open this link in a different tab to avoid losing the state of the page, therefore, you should write a test case to prevent a developer from changing the anchor tag to a button.


1. If you want to refactor a piece of functionality, make sure it does have a unit test first, and if it doesn't, then right before doing the refactor -Remember, a unit test is your second hand, you should rely on it to catch regressions.-
-->

---

# Write unit test when

- Don't test anything that can't fail.
- Don’t test anything that will be caught by the compiler.
- Don’t test 3rd party tools/libraries
- Don’t test boilerplate code.

<!--

-->

---

# Disadvantages

<ul>
  <li>It requires more time <b>initially</b>.</li>
  <li>Maintanace Liability.</li>
  <li>Hard to agree on standards.</li>
  <li>Often, no direct result.</li>
  <li>Wrong setup for test might incur high expenses ^^.</li>
</ul>

<!--
1. that's the case with everything, once you past the initial phase, it'll get easier.

2. Writing code is not difficult but maintaining it is. I mean, one of the unit testing selling points is helping you to maintain the code. see how difficult it is!
Unit testing itself is code so it comes with the burden of maintaining it. That's why you should strive to write clean and dead simple unit tests.

3. This is often due to the team members' different expertise.

4. Unit test is great in catching regressions, upon writing the tests you might find it's worthless.

5. If you let
-->

---

# F.I.R.S.T Principle

- Fast: They do not do much, so obviously they are quick to run.
- Independent: A test shouldn't mutate outer state or cause other tests to fail.
- Repeatable: Tests should be repeatable in any environment without varying results. If they do not depend on a network or database, then it removes the possible reasons for tests failing, as the only thing they depend on is test double. If the test fails, then the unit test is not working correctly or the test is set up wrong.

- Self-validating: Each test has a single assert or two at most, which will determine whether the test passes or fails. (Look at the picture below)
- Timely: Unit tests should be written just before the production code
  that makes the test pass. This is something that you would follow if you
  were doing TDD (Test Driven Development).

<img src="/single-assert.png" width="500" />

---

# Code coverage

Code coverage is a metric that can help you understand how much of your sourcecode is tested.

Code Coverage Formula = (Number of lines of code executed by a testing algorithm / Total number of lines of the code source) \* 100.

Code Coverage aim is to answer the following question: <b>Does the project have enough tests?</b>

---

# Best Practices

<ul>
  <li>Run the unit test as part of Continuous Integration.</li>
  <li>Enfore (%) coverage.</li>
  <li>Don't write unit test for everthing.</li>
  <li>Agree with your team on one standard to write tests.</li>
  <li>ONE UNIT TEST FOR ONE BEHAVIOUR.</li>
  <li>Favor stateless objects.</li>
  <li>Don't write if statments, loops, or logic. SIMPLE UNIT TEST.</li>
  <li>Concise yet informative report.</li>
  <li>Don’t “foo”, use realistic input data.</li>
  <li>Don’t catch errors, expect them.</li>
  <li>It's okay to write verbose test, don't over reuse!</li>
</ul>

---

# Glossry

- Testing: is the process of evaluating and verifying that a software product or application does what it is supposed to do.

- Unit testing: is the process of breaking down feature to individual behaviours to ensure they do what they're intended to.

- Techincal dept: is the cost of choosing the fast way to develop a feature. where if you spend a bit more time to refine your work you might reduce that techincal dept. Usually, the technical dept is cumulative, the longer it lasts, the more costly it gets.

- Regression: is bug introduce is a type of software bug where a feature that has worked before stops working.

# FAQ

- Unit testing take a lot of time?
  1. Unit testing is not an addition to your work, It is mandatory, the best thing is to consult with team lead to have you more time.
  2. You might need to rethink of the tools you use to write test, the tooling you use can greatly help in reducing code boilerplate.
- Where the unit test should be executed -the old tests-?
  It's better to run all unit tests in your machine, however, there should be setup in place in CI server to run all tests and possipply a thereshold for code coverage.
- What if I have to push the code rightaway?
  I got you, don’t write test code, just write test reports, lucky you, jest supports test only report which is a test to be written later on, you can do that by `it(a report that will remind you later what this test case shall verify')` without providing a callback function

---

# Closing notes

- Always strive to write maintable unit test.
- Ensure that you and your team are on the same page regarding writing unit test.
- You still can have bugs even with unit test.
- Identify what features are the most imporaant and write unit test according to that.
- Always remember that the more test you write the more maintiance they will require.
- Use ESLint, prettier and any other static analysis tool that will help in avoding silly bugs.
- Before writing unit test, make sure that the project architecute allow that. not all architcure testable, at least not all of them can be easily tested.
- Modularize! a modulare project is testable project.

---

# Book Recommendation

1. The Art of Unit Testing.
2. Unit Testing Principles, Practices, and Patterns.
