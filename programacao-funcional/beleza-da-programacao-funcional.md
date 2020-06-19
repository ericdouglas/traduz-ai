# The Beauty of Recursion and Pattern Matching in Functional Programming Languages

![Lookup function to retrieve value from binary tree](https://i.imgur.com/lFXKbhd.png)

```erl
%% The lookup function receives a Key and a Node
lookup(_, {node, 'nil'}) ->
    undefined;
lookup(Key, {node, {Key, Val, _, _}}) ->
    {ok, Val};
lookup(Key, {node, {NodeKey, _, Smaller, _}}) when Key < NodeKey ->
    lookup(Key, Smaller);
lookup(Key, {node, {_, _, _, Larger}}) ->
    lookup(Key, Larger).

%% Explanation about parameters and data structure
%%
%% Empty node -> {node, 'nil'}
%% Non-empty node -> {node, {Key, Val, Smaller, Larger}}
%% node -> it's just an atom, like a constant
%% Key and Val -> can be of different types
%% Smaller and Larger -> Can be empty or non-empty node structures as well,
%%                       so you can have your binary tree
```

I want to invite you to admire the beauty, succinctness and power of the code above [1].

It is a function to find some value in a binary tree, using _recursion_ and _pattern matching_.

We can see how simple and extremely declarative it is to write a function with these concepts and techniques from functional programming languages.

**We have absolutely ZERO instructions in order to tell HOW the algorithm should work!** We just expressed **WHAT** it will do, not how.

The code is written in **Erlang**.

## Detailed explanation

This function will search in a binary tree by the `key` you passed to it and will return a tuple with the `value` associated with the key passed, or `undefined` if it was not found.

A great advantage we have in Erlang and Elixir is the possibility to create more cases for the same function just changing the pattern in its parameters.

The function has the same name and arity (number of parameters), but since the pattern is different, it is like it was another function.

The cases will be verified from top to bottom, so we should put more specific cases first, and more general cases in the end. Think about it like a functional way to write `switch` or `if / else` statements.

We have 4 possible cases in our `lookup/2` function, so let's see what which one does:

### `lookup(_, {node, 'nil'})`

The first `lookup` will ignore the first value passed to it (`_`), and if the second value matches, it will return `undefined`. As you can see, the second value is an empty node.

### `lookup(Key, {node, {Key, Val, _, _}})`

The second `lookup` will see if the `Key` we passed is the same `Key` in the node passed. If it is, we just found the node that contains the value we want to retrieve.

In that case, the lookup will return `{ok, Val}`, where `Val` is the value we are interested in.

### `lookup(Key, {node, {NodeKey, _, Smaller, _}}) when Key < NodeKey`

The third `lookup` will match the case where the key we passed is different from the key in the node passed.

We also have a _guard clause_ in the function signature, to keep it more specific. The guard clause `when Key < NodeKey` is important for us to decide in which direction we should go to search the value we expect.

In that case, we know that if the `Key` is smaller than `NodeKey`, we need to continue our search in the `Smaller` node. So we use a pattern match to extract the `Smaller` value and ignore the other data we won't use with `_` again.

To continue our search, when we match the third lookup function, we will call it recursively, now with the following data `lookup(Key, Smaller)`.

_This is **recursion** and **pattern matching** in action! ❤_

### `lookup(Key, {node, {_, _, _, Larger}})`

The last case, and more general one, is when any of the previous `lookup` functions were not matched. We know this will only occur when the `Key` we passed is different from the `Key` in the current node, and the `Key` passed is larger than the key in the present node, so we need to continue our search in the `Larger` node.

In that case, we will call `lookup` recursively with the following data `lookup(Key, Larger)`.

**And that's it!**

With ZERO instructions about WHAT to do, we have a super simple, declarative, and full-featured function that retrieves a value from a binary tree! This is absolutely amazing and mind-blowing <3.

I hope you enjoyed it!

Cheers! :)

# References

1. Code excerpt from chapter 7 - Recursion, section "More than lists", from the book Learn You Some Erlang for Great Good. **[LINK](https://learnyousomeerlang.com/recursion)**
