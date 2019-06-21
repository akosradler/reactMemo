# useMemo with dependencies - a case study

When React 16.8 came out, it completely changed how we write our components. State, lifecycle, refs are now managed with hooks, and in turn the code you write became more comprehensive, and lett prone to issues.

Let me start off by stating that this post is not meant to be a thorough explanation of hooks, or useMemo, though I hope it gives you a better understanding of how useMemo works.

useMemo is a React hook, that takes a function, and returns a memoized value. Essentially, it's caching the values the function returns after its initial execution, which makes it your go-to tool for performance optimization.

But wait, don't we already have React.Memo for this?
Kind of...
Unlike React.Memo, which is a higher order component and meant to wrap other components, useMemo memoizes functions.
...and since class components and stateless functional components merged into functional components that are essentially functions themselves, useMemo is a perfect fit for caching our components going forward.

useMemo is also more general, in the sense that it takes an array of dependencies, meaning that the memoized value will only get recomputed if one of the specified dependencies change.

In order to understand its workings better, let's take a look at an example of an issue I was facing before:

We have a list of products, all ready to go into a shopping cart, wrapped in useMemo.

When we do add to the cart though, the productList component re-renders. That is not what we expected to see, our product list did not change at all.

Looking at it with the handy tool why-did-you-re-render, we immidiately see the culprit. Since the shopping card updates the state on the parent component, it causes it to re-render, which recreates the F function. Since F is a dependency on our useMemo in C, it discards the memoized component value.

How do we fix this?
There are a number of approaches:

- Refactor!
  If this was your first thought, congratulations for having the right mindset :)
  Although keep in mind that this is just an example project. In the real world, there are times when you need a quick solution.

- Can't we just remove F from the dependencies? Sure, but you'll be entering a world of pain, caused by headache that you got from hunting bugs. Let me show you what happens.

We'll remove the dependency.

Great, no re-renders this time.

What happens when we add another item into the cart?

That ain't right. What's happening?
