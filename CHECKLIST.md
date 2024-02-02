### Challenge 1

-   The application uses Vue.js template syntax to display the movies.
-   The movies are rendered using a `v-for` loop.
-   The name, description, genres and image of each movie is displayed.
-   The rating of the movie is represented by stars, with a maximum of 5 stars.

### Challenge 2

-   There are 5 buttons from 1 to 5 representing the rating on for each movie.
-   Clicking on a button updates the rating of that movie.
-   The button for the rating number is disabled if that rating is the current one of the movie.

### Challenge 3

-   Display each movie rating number within a large yellow `StarIcon` in the top right hand corner of the movie posters
-   For movies with no rating, show a dash instead of the number and color the `StarIcon` gray

### Challenge 4

-   Create an "Add Movie" button
-   Show a form for creating new movies when the button is clicked
-   Include the following fields on the form:
    -   Name: text input & required
    -   Description: textarea
    -   Image: text input
    -   Genre: dropdown & required & user can select multiple
    -   Is in theaters: checkbox
-   Validate the form on submit
-   Once submitted and valid, add the movie to the list
-   Clear and hide the form after the movie is added
-   Add a cancel button to the form
    -   Clicking the cancel button should clean up the form, and any error that it might have.
    -   Clicking the cancel button should hide the form for the user.

### Challenge 5

-   Add a button on each movie allowing users to remove it from the list.
-   Calculate and display the average rating with a computed prop
-   Add a button to reset all movie ratings to zero.
-   Calculate and display the total number of movies in the list
-   Add the ability to edit each movie
    -   Add an edit button to each movie
    -   Re-use the "Add Movie" form to edit movies
    -   Pre-fill the form with the data of the movie being edited
    -   On form submit, replace the old movie values with the new
    -   Ensure that the ratings of that movie are not lost when you edit it

### Challenge 6

-   Move the current template for a movie into the `MovieItem.vue` component.
-   Replace the current part of our template where the movie is displayed with the `MovieItem.vue` component
-   Move the `notRated` value into the `MovieItem.vue` component, create a computed property out of it.
-   The `MovieItem.vue` component should have the following props:
    -   `movie`: It should receive the movie object.
-   Create `3` custom events in the `MovieItem.vue` component.
    -   `edit`: Should be dispatched when the user clicks the `edit` button with the `id` as parameter
    -   `remove`: Should be dispatched when the user clicks the `remove` button with the `id` as parameter
    -   `update:rating`: Should be dispatched when the user updates the rating with the `id` and the new `rating` as parameter
-   Change the behavior of the `updateRating` to accept the movie `id` instead of the `movieIndex`
-   Change the behavior of the `removeMovie` to accept the movie `id` instead of the `movieIndex`
-   Change the behavior of the `editMovie` to accept the movie `id` instead of the `movieIndex`

### Challenge 7

-   Move the current template for the form into the `MovieForm.vue` component.
-   Replace the current part of our template where the form is displayed, with the `MovieForm.vue` component
-   The `MovieForm.vue` component should have the following props:
    -   `modelValue`: It should receive the movie object, it should be optional.
-   Create `2` custom events in the `MovieForm.vue` component.
    -   `update:modelValue`: Should be dispatched when the user clicks the `save` button and include the `form` as the payload
    -   `cancel`: Should be dispatched when the user clicks the `cancel` button and requires no payload.
-   The rest of our application functionality should still work as expected.

### Challenge 8

-   Move the current template for the modal into the `AppModal.vue` component and modify it for general re-use.
-   The `AppModal.vue` component should meet the following specs:
    -   Include a `slot` for the body content
    -   Display a close (x) button
    -   Display a title above the body content
    -   Receive the following props:
        -   `title`: a string for the title, defaults to `null`.
    -   Emit the following events:
        -   `close`: emitted when component is closed
-   Use the `AppModal.vue` component to display the `MovieForm` in `App.vue`
-   Import the `AppModal.vue` and `MovieForm.vue` component asynchronously so that the components' code is only downloaded once it's displayed
-   The rest of the app should continue to work as before

### Challenge 9

-   When the modal component appears and disappears it should be done with a smooth transition.
-   We want to have two different transitions
    1. the modal overlay (the dark background) should fade in
    2. the modal itself should slide in (fade in + slide down 50px)

### Challenge 10

-   When the modal component opens, the name field on the movie form should be focused for the user to start filling the form
-   Use a template ref in the MovieForm component to focus the input when the component mounts to the DOM
