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
  persist: true
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
  <li>Types of testing.</li>
  <li>What is Unit Testing.</li>
  <li>Test Case & Test Suite.</li>
  <li>Pros & Cons</li>
  <li>Attributes of unit testing.</li>
  <li>Anti Patterns.</li>
  <li>Two Different Kind Of Testing.</li>
  <li>TDD?</li>
  <li>Should I always write test?</li>
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
Before diving into unit test, les't start by defining testing in general so we can build upon it.

To facilitate the definition I've broken it down into two blocks which will answer three question

What?
How?
Why?

Those requirements usually comes from business analyst or someone on the team, typically, a team lead, for example, "When the app first opened, send an event using backend API"

The manual part involves human interaction in almost every step.

The automation tool, may or may not require interaction at all.


To elaborate on the requirement bit
-->

---

# Why?

<ul>
  <li v-click>To deliver working software to the users.</li>
  <li v-click>To build customer trust and satisfaction.</li>
  <li v-click>To have more resilient (recoverable) and robust software.</li>
  <li v-click>Because it's cheaper!</li>
</ul>

<v-clicks>
<h2>Why not to test?</h2>
<p>Increase development time?</p>
</v-clicks>

<!--
1. Isn't that everyone's job after all? right!

2. Your user/customer has to trust you in order to pay for your product and use it, if you're to ship software that has major features not working, your customer base will fly away to a competitor.

3. If you do test your product, then you can almost roll back to the last working version without being terrified.

4. Fixing bugs, especially old bugs costly, and sometimes impossible.

Someone might argue that writing test delays or slow development time and shipping features, while that might be true sometimes, however, the development time will get slow in the long term without unit tests due to the number of bugs that will rise.
-->

---

# Chart

<img src="/test-chart.png" width="620" />

<!--
The Red line represents the amount of work you can do without a unit test. At first, the amount of work can be shipped perhaps quicker in contrary to writing test and shipping code at the same time.

A few months later, you do a change, and it breaks a working feature(s), so instead of focusing on the new feature, you are stuck fixing new bugs.

With a unit test, you might need slightly more time to ship a feature, but in the long term, when compared to not having a unit test it's definitely quicker.
-->

---

# What is Unit testing?

<p v-click>A unit is the smallest piece of a functionality.</p>
<p v-click><small><i>HINT:</i> One function can have multiple unit tests.</small></p>

<p v-click>Unit testing is the process of making sure that a unit</p>
<ul>
  <li><small>Meets requirements.</small></li>
  <li><small>Keeps meeting that requirements overtime.</small></li>
  <li><b>Is a regression free.</b></li>
</ul>

<h3 v-click>A unit test should</h3>
<ul v-click>
  <li>Verifies a single unit of behavior.</li>
  <li>Does it quickly.</li>
  <li>And does it in isolation from other tests.</li>
</ul>

<small v-click>
Hint: We refer to the thing that is being executed as the "{Thing} Under Test", so in case of testing a System as whole we say, "System Under Test" or if we're taking about unit test then we say, "Unit Under Test".
</small>

<!--
Now that we know what is testing, let's define what is meant by A Unit

So we apply the definition of testing but on a class or function.

-Keeps meeting those requirements over time.- this is where unit tests play a crucial part. think of doing refactoring, you changed something, but the test failed because the changes altered the actual requirements.
We call this Regression. A change that has caused a working functionality to break.

goals

1. A unit test should verify only one thing at a time. if you're writing a test case that verifies more than one thing, then you should think again.
2. A unit test should be fast. CI/CD time
3. Meaning, unit tests shouldn't be dependent on each other. for example, if a unit test verifies that the button does something, the other unit tests should pass without waiting for that unit test to pass.
-->

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
To put everything in action, let's see the following example.
-->

---

# Your First Test

<p v-click>
 - <b>Requirements</b> <br> <br>
  Givin a counter widget <br>
  When a user click on the "Increase Button" <br>
  Then the counter label increases by 1 <br>
</p>

<p>Implementation</p> <br>
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
```
</small>

---

# New Requirement

- Requirement <br>
  Change the counter widget to increase based on the user provided input

  Givin user enterd a number in the "Increase By" input<br>
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

End-to-end: Test the application under real circumstances, like replicating how a user would use it with real data to ensure the application performs regardless of the role of a user. End-to-end test happens at the User Acceptance phase in SDLC.

Performance: as simple as testing the software under different workloads to help you measure the reliability, speed, scalability, and responsiveness of an application.

Smoke testing: This test ensures that the basic functionality is working before going further. Smock testing can signal whether.
1. We should perform the E2E test as the E2E test is an expensive operation.
2. We should continue the deployment.

The last one and perhaps the major one is regression testing. We'll talk about it a lot today as it's related to unit test.

Regression test: after any change to the codebase or the software in general we perform this test to ensure that the previously working functionality is still are. in case one of the functionality is not working then we say a regression happened.


A regression test doesn't have to be a separate step, usually it is associated to any kind of test that you perform. so if we're executing the intergration test and the results allude that something that was previously working that isn't any more, then we have done regression testing, the same applies on other types of tests.

Regression test is a bit free form, meaning that it can be defined as per company basis, the important thing that it have to be run on the current functionallity and not the new ones.

Companies, of course, might have a regression test as a separate step by only running the test on the current functionality and not the new one. it is a matter of how the teams communicate.
-->

---

# Pros & Cons

Pros

<ul>
  <li>Reduce techincal dept.</li>
  <li>Boost developer confidance in doing changes.</li>
  <li>Reduce regressions.</li>
</ul>

Cons

<ul>
  <li>It requires more time <b>initially</b>.</li>
  <li>Maintanace Liability.</li>
  <li>Hard to agree on standards.</li>
  <li>Often, no direct result.</li>
</ul>

<p>Techincal dept</p>
<small>Techincal dept is the cost of choosing the fast way to develop a feature. where if you spend a bit more time to refine your work you might reduce that techincal dept</small>

<!--
Usually, the technical dept is cumulative, the longer it lasts, the coster it gets
-->

---

# Post Talk

- Testing categories
  1. Funtional testing.
  2. Non functional testing.

---
