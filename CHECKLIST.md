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

