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

  <small v-click>
  Hint: We refer to the thing that is being executed as the "{Thing} Under Test", so in case of testing a System as whole we say, "System Under Test" or if we're taking about unit test then we say, "Unit Under Test".
  </small>

<!--
Before diving into unit test, les't start by defining testing in general so we can build upon it.

To faciltate the defention I've broken it down to two blocks which will answer three question

What?
How?
Why?

Those requirments usualy comes from buisness analyst or someone on the team, typically, a tem lead, for example "When the app first opened, send an event using backend API"

The manual part involve human interaction in almost every step.

The automation tool, may or may not require interaction at all.


To elaporate on the requirment bit
-->

---

# Why?

<ul>
  <li v-click>To deliver working software to the users.</li>
  <li v-click>Cumlativlty build trust in the users.</li>
  <li v-click>More resilient (recoverable) and robust software.</li>
</ul>

<v-clicks>
<h2>Why not test?</h2>
<p>Increase development time?</p>
</v-clicks>

<!--
Someone might argue that writing test delay or slow development time and shipping features, while that might be true sometimes, however, the development time will get slowly in the long term without unit test due to the number of bugs that will rise.
-->

---

# Chart

<img src="/test-chart.png" width="620" />

<!--
The Red line, represents the amount of work you can do without unit test. At first, the amount of work can be shipped perhaps quicker in contrary to writing test and shipping code in the same time.

A few month later, you do a change, and it breaks a working feature(s), so instead of focusing on the new feature, you stuck fixing new bugs. 

With unit test, you might need a slightly more time to ship a feature, but in the long term,
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

# What is Unit testing?

<p v-click>A unit is the smallest part of a software. it can as small as function or a class.</p>
<p v-click>Unit testing is the process of making sure the unit under test does what expected.</p>
<p v-click>Unit testing is the process of detecting defects in the unit under test.</p>

<p v-click>Example</p>
<small v-click>
 - <b>Requirements</b> <br> <br>
  Givin a counter widget <br>
  When a user click on the "Increase Button" <br>
  Then the counter label increases by 1 <br>
</small>
<small v-click><br>
 - <b>Implementation</b> <br>
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

<!--
Now that we know what is testing, let's define what is meant by A Unit

So we apply the defention of testing but on a class or function.

To put the concept in action, let's see the following example.

To not to bore you more, let's write our first test suite.
-->

---

# Test Suite & Test Case

<p v-click>Test Suite is group of related test cases that speak about a functionallity or behaviour.</p>
<p v-click>Test Case is the setup, invokation, and assertion of the unit under test, <span>it should do one thing: assert single output or verify one behaviour</span></p>

<v-clicks>
<ul>
<li v-click><small>Test Suite can be defined using the `describe` function</small></li>
<li v-click><small>Test Case can be defined using the `it` or `test` function</small></li>
</ul>
</v-clicks>

<p v-click>Test report is the description that speaks about your test suite and cases</p>
<small  v-click>
Test Report should clearly tell what is being tested and under which cirumstance. The test report should be simple and strightforward. Anyone with fair bit of knoweldge in the team should be able to understand the test suite and test case from its report.
</small>

<!--
Before going further, let's write our test suite for the counter widget.
-->

---

# Attributes of unit testing

<ul>
  <li>It should be fast.</li>
  <li>It should be small.</li>
  <li>It should be isolated.</li>
</ul>

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

# Quick Example

```jsx
function Counter(props: readonly { startAt: number, increaseBy: number } = {startAt:0, increaseBy:1}) {
	const [counter, setCounter] = useState(props.startAt);
	return (
		<>
			<div data-testid="counter-label">{counter}</div>
			<button data-testid="increase-button" onClick={() => setCounter(counter + props.increaseBy)}>Increase</button>
		</>
	);
}
```

```typescript
import userEvent from "@testing-library/user-event";
import { render, screen } from "@testing-library/react";
import "@testing-library/jest-dom";
it("increases the counter on click by 1", () => {
	// ARRANGE
	render(<Counter startAt={0} />);
	const expected = 1;

	// ACT
	await userEvent.click(screen.getByTestId("increase-button"));

	// Assert
	expect(screen.getByTestId("counter-label")).toEqual(expected);
});

it("increases the counter on click by 5", () => {
	// ARRANGE
	render(<Counter startAt={0} increaseBy={5} />);
	const expected = 1;

	// ACT
	await userEvent.click(screen.getByTestId("increase-button"));

	// Assert
	expect(screen.getByTestId("counter-label")).toEqual(expected);
});
```

---

<small>it should sum up a set of numbers</small>

---

# Code

Use code snippets and get the highlighting directly![^1]

```ts {all|2|1-6|9|all}
interface User {
	id: number;
	firstName: string;
	lastName: string;
	role: string;
}

function updateUser(id: number, update: User) {
	const user = getUser(id);
	const newUser = { ...user, ...update };
	saveUser(id, newUser);
}
```

<arrow v-click="3" x1="400" y1="420" x2="230" y2="330" color="#564" width="3" arrowSize="1" />

[^1]: [Learn More](https://sli.dev/guide/syntax.html#line-highlighting)

<style>
.footnotes-sep {
  @apply mt-20 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

---

# Components

<div grid="~ cols-2 gap-4">
<div>

You can use Vue components directly inside your slides.

We have provided a few built-in components like `<Tweet/>` and `<Youtube/>` that you can use directly. And adding your custom components is also super easy.

```html
<Counter :count="10" />
```

<!-- ./components/Counter.vue -->
<Counter :count="10" m="t-4" />

Check out [the guides](https://sli.dev/builtin/components.html) for more.

</div>
<div>

```html
<Tweet id="1390115482657726468" />
```

<Tweet id="1390115482657726468" scale="0.65" />

</div>
</div>

---

## class: px-20

# Themes

Slidev comes with powerful theming support. Themes can provide styles, layouts, components, or even configurations for tools. Switching between themes by just **one edit** in your frontmatter:

<div grid="~ cols-2 gap-2" m="-t-2">

```yaml
---
theme: default
---
```

```yaml
---
theme: seriph
---
```

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-default/01.png?raw=true">

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-seriph/01.png?raw=true">

</div>

Read more about [How to use a theme](https://sli.dev/themes/use.html) and
check out the [Awesome Themes Gallery](https://sli.dev/themes/gallery.html).

---

## preload: false

# Animations

Animations are powered by [@vueuse/motion](https://motion.vueuse.org/).

```html
<div v-motion :initial="{ x: -80 }" :enter="{ x: 0 }">Slidev</div>
```

<div class="w-60 relative mt-6">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-square.png"
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-circle.png"
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-triangle.png"
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 40, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[Learn More](https://sli.dev/guide/animations.html#motion)

</div>

---

# Post Talk

- Testing categories
  1. Funtional testing.
  2. Non functional testing.

---
