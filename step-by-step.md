# Step By Step (Oh Baby)

For those who don't want to figure it out all on their lonesome, here's a guide for how to make our Todata app.

You can do it... [Step By Step](https://www.youtube.com/watch?v=ay6GjmiJTPM).


### General Architecture

What is the structure of our app?

* The code we already wrote for `back-end.js` is in charge of all the mapping, filtering, and sorting.
* Our sort of middle level is files like `addTodo`, `printTodo`, `refreshTodos`, and so on. These functions handle adding and removing todos from the data and the DOM and are MOSTLY done, though we'll consider tweaking it now and again.
* The final level is all up to you now: button event listeners that use the lower two levels appropriately for the respective button. So for the Hide Low Priority button, we'll need an event listener function that sets current todos to the filtered version (using the back end function for filtering) and then refreshes the DOM to display that updated list.

So: let's make that final level happen.


### Part One: Adding Todos

Create a new `add-todos-button-controller.js` file and let's get cracking!

* Query the button for adding todos and add an event listener function. This can be done in one line using 1) dot-chaining and 2) an anonymous function as the second parameter to `addEventListener`.
* Inside that event listener function, query the todo user input box and save it to a variable. We'll need to use it a few times!
* Now we're ready to create our todo object. It needs four fields: the `text`, `completeness`, `id`, and `priority`. You can grab the `priority` and `text`'s values from the DOM. Where would they be located? I'll leave you to think about the values should be for `complete` (easy to figure out) and `id` (a bit harder).
* Now pass that todo object to `addTodo`. We'll have to go in and play with `addTodo`... should it add to `todos` or `currentTodos`? For now, change it to `currentTodos`, but you may consider revisiting this.
* Finally, let's update the DOM. Call `refreshTodos`--it takes no parameters and simply erases the list and rewrites it to bring it in line with whatever's in `currentTodos`.
* And finally finally, let's clear the input box so our user has an empty field, and put the browser focus there so that they can start typing another todo if they want.

You should now have the ability to add to your `todos` and your `currentTodos` by typing in the text field and hitting "ADD". If you do so, you should see the DOM update. Additionally type `todos` and `currentTodos` into your console to make sure our data has been updated as well!


### Part Two: Toggling Completeness

`print-todo.js` is MOSTLY complete, but it's missing something important. Click on a todo item in our app; it gives it some strikethrough (changeable in `style.css` in the rules for `.complete`!), but type `todos` into the console... Nothing has changed in our data! We won't be able to sort or filter by completeness if we don't know what's complete. So let's make that happen.

* The main problem we must solve is finding which todo in our data matches up with the todo `li` that was clicked. They might not be in their original order (given the sorting and filtering we'll add soon), so we'll need to match them up in some way? What identifies a todo? The `id` field! (Which, hopefully, we found a way to always make unique in Part One.) 
* SO! Let's hop into that `li` click listener and create, with a `let`, a `foundTodo` variable. When we find the todo object that matches the `id` of the `li` the user clicked, we'll set `foundTodo` to equal that todo, and then toggle its completeness.
* Now we can loop through our global `todos` (I'd recommend a `for of` loop since we don't need the `i`), and when we find an `id` property that matches the property of the clicked-on element (how do we find that, again?!), save that particular todo to `foundTodo`.
* Possible issue to be aware of: the `id` field of our todo objects are numbers. But DOM elements' `id` fields are... not. `===` won't work if the things compared are of different types! There are several different fixes for this. And I'm sure you'll find (at least) one!
* Now that you've got the todo you need, you can toggle its completeness property to be the opposite of whatever it currently is. Toggle some todos on the DOM and check `todos` in the console to see if it worked!

A couple important notes before we move on:
1. While `todos` and `currentTodos` might not have the same number or order of objects in them thanks to filtering and sorting, the objects that are in them are actually literally the same objects. So if you change a property of an object in the `todos` array (like `.complete`), that will be reflected in the `currentTodos` array as well. This isn't some work being done under the hood, either--again, they're literally the same object. It's a bit like working in a cloud document shared by several people; no one has to download or send copies of the document around, you're all acting on the same file.
2. There are a couple more advanced ways to find the todo with that particular `id`. One is [`Array.find()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find), which is a lesser-known but awesome array callback function. Another is the much more advanced concept of closures. (Here's [an article on closures](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8). Here's [a great Stack Overflow answer on closures](https://stackoverflow.com/a/111111) as well.) Thanks to closures, each event listener actually still has access to the todo object that was created in the scope above it (our `printTodo` function) when it was added as an event listener, and since that `todo` variable is exactly the one we were looking for with the loop or `.find()`, we can really just use `todo` within the event listener. (Don't worry if you don't get closures yet. As I said, it's an advanced concept!) Feel free to give that a try, even if you may not get how it's working.
