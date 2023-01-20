# Debug Log

**Explain how you used the the techniques covered (Trace Forward, Trace Backward, Divide & Conquer) to uncover the bugs
in each exercise. Be specific!**

In your explanations, you may want to answer:

- What is the expected vs. actual output?
- If there is a stack trace, what useful information does it contain?
- Which technique did you use, on which line numbers?
- What assumptions did you have about each line of code, and which ones were proven to be wrong?

_Example: I noticed that the program should show pizza orders once a new order is made, and that it wasn't showing any.
So, I used the trace forward technique starting on line 13. I discovered the bug on line 27 was caused by a typo of '
pzza' instead of 'pizza'._

_Then I noticed another bug ..._

## Exercise 1

After submitting a pizza order, it fails and throws a stack trace. I used the trace backwards technique, starting on line 79 where the problem occurred.

Backtracking, we find that the creation of a PizzaTopping instance is invalid. We see that the "topping" argument does not exist in the class, and should
instead be `topping_type`

Another error originated at the redirect line for the order post. Changed `redirect(url_for('/'))` to `redirect(url_for('home))`

There are no more submission errors, but the order is not showing. I used the divide-and-conquer technique to check the template, post-request route, and database.
My first assumption was that there was a problem with the template. It turned out that there was no problem with the template, but there was a problem with the route. In the route, `ToppingType` was used instead of `toppings_list` to create the pizza.

The `db.session.commit` method was also missing from the route

This time, another stack was thrown showing an issue with the variables.
The use of `request.form`, with both the size and name, is incorrect (None). It should instead, be `order_name` and `pizza_size` 

Fixed the `toppings_list` variable not being an array

## Exercise 2

An internal server error occurred on form submission.

I used the trace backwards technique starting at the problematic line and eventually pointing to the api request.

I checked the api [docs](https://openweathermap.org/current#geocoding) to see what the api expects.

The api call is using incorrect params: `users_city` should be `city` and `requested_units` should be `units`

The api is also expecting the `q` param instead of `place`

Fixing that, the api now gives a valid response.
But there is still a typo in the code. The use of `temperature` instead of `temp` is a typo.

## Exercise 3

The merge sort doesn't work and throws an `IndexError` error when run. Using a trace forwards from the top of the function down
we find a typo at bottom of the `merge_sort` function. `right_side[i]` should be changed to `right_side[j]`

Running the program again, a `TypeError` in thrown in the `binary_search` function with the "mid" variable.
Doing a trace backwards we find that the declaration is using normal division instead of floor division which is creating the problem.