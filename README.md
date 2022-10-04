# Fetch Rewards Front End Challange 

Thank you for taking the time to grade my challenge. This project was built using sveltekit for the framwork, and tailwindcss + daisyui to hadle the ui. I used a small utility library called theme-switcher to handle the dark/light mode tansition and the fetch api to make the GET and POST calls. I used yup for form validation.

## Functionality
This site meets the requirements as listed in the fetch reqards front end challange sheet
 - The occupation and state select fields are populated by the object returened from the GET call to the fetch-reward API
 - The form provides feedback on attempting to submit if there are errors. 
 - Upon succesfully submitting the form you are redirected to a 'success' route to confirm that you have submitted correctly. 

Bonus features: 
 - I added a theme changing feature just to switch between 'light' and 'dark' themes.
 - I added a small navbar to navigate between the success page and the form since I added a page to show successfull form submission.

## Building

 - First clone the repo locally and install all dependancies. 
 - use: npm run dev -- --open 
 - Project should open automatically
 - If the project doesn't open automatically: open http://localhost:5173/ in your browser 
