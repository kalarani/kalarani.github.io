+++
date = '2023-06-19T12:49:01+05:30'
title = 'Paradigm Shift: Immutability in Elixir'
+++

I spent this weekend by writing an [Elixir script to solve the Linear Regression problem](https://github.com/kalarani/mlbook-elixir/blob/main/linear-regression/LinearRegression.exs). And then when I went on to implement Gradient descent to train the model, I hit a road block.

I came up with this code, which looked perfect to me, just that it didn't work. It didn't do what I expected this to do.

```
  def train_from(filename) do
    raw_dataset = read_dataset(filename)
    x = raw_dataset |> Enum.map(fn d -> elem(Integer.parse(Enum.at(d, 0)),0) end)
    y = raw_dataset |> Enum.map(fn d -> elem(Float.parse(Enum.at(d, 1)),0) end)
    training_dataset = %{x: x, y: y}
    w = 100
    b = 100

    for n <- 1..5 do
      IO.inspect("Iteration #{n}: #{w}, #{b}")
      [w, b] = gradient_descent_step(x, y, w, b)
      IO.inspect("End of Iteration #{n}: #{w}, #{b}")
    end

    IO.inspect("End values for w: #{w}, b: #{b}");
  end
```

It spit out this output. 
```
"Iteration 1: 100, 100"
902964882.6662791
4514335.333331471
"Iteration 2: 100, 100"
902964882.6662791
4514335.333331471
"Iteration 3: 100, 100"
902964882.6662791
4514335.333331471
"Iteration 4: 100, 100"
902964882.6662791
4514335.333331471
"Iteration 5: 100, 100"
902964882.6662791
4514335.333331471
"End values for w: 100, b: 100"
```

I was expecting the value of w and b to be updated and passed on in each iteration. Even though, the gradient descent returned back new values for w and b, it wasn't carried forward to the next iteration.

Turned out, I missed an important aspect of Elixir, rather functional programming. It is

> `Immutability`.

All variables are immutable. Ideally, they are constants. You can't change it once they are initialised. 

If that is the case, how am I supposed to do multiple iterations and feed the outputs from the previous iteration to the next one?

> _This is where the **paradigm shift** from object oriented programming is required._

By forcing ourselves to deal with immutable variables and methods that ~~doesn't~~ shouldn't have any side effects, we can come up an alternative way to solve the problem. i.e., to use [recursion for looping](https://elixir-lang.org/getting-started/recursion.html#loops-through-recursion).

## Epilogue:

My relationship with Elixir has always been on and off. Having spent a couple of years building websites in RoR, I've explored [basics of elixir](https://elixirschool.com/ta/lessons/basics/basics) few years back. Recently I came across [this blog](https://dockyard.com/blog/2022/07/12/elixir-versus-python-for-data-science) from [Sean Moriarity](https://seanmoriarity.com/) and that triggered my interest to try out elixir in the ML space.

For curious minds, [Charles Scalfani](https://twitter.com/cscalfani) wrote a [series of blogposts](https://cscalfani.medium.com/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536) that helps with the fundamentals of functional programming.
