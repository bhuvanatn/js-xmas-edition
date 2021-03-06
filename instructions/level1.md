How does a web form work?
=========================


1. A visitor visits a web page that contains a form.
2. The web browser displays the HTML form.
3. The visitor fills in the form and submits
4. The browser sends the submitted form data to the web server
5. A form processor script running on the web server processes the form data.
   The processing steps can include:
      - sending the form submission by email
      - saving the submission to a database table or a file.
6. A response page is sent back to the browser.

The parts of a web form
A standard web form has the following parts:
1. The HTML code for the form (read in more details at
   html-form-description.txt file)
2. Input validations.
3. Form processor script.



Input Validations.
=================


* Client-side validation:
Input validations are essential for any web form since it helps the web site
visitor submit the right input. Input validations are often written in the
client-side language – JavaScript.
Validating form input with JavaScript is easy to do and can save a lot of
unnecessary calls to the server as all processing is handled by the web
browser. It can prevent people from leaving fields blank, from entering too
little or too much or from using invalid characters.

For an alternative approach to client-side form validation, without
JavaScript, check on HTML5 Form Validation which is available now in most
modern browsers. But we cannot style it, that is why we are doing js validation
in this workshop.
https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms/Data_form_validation

* Server-side validation:
When form input is important, it should always be verified using a secure
server-side script. Otherwise a browser with JavaScript disabled, or a hacker
trying to compromise your site, can easily submit invalid data.



To validate our form we will need to follow next steps:
    1. Get value from form
    2. General validation:
        - not empty
        - min length 2
        - max length 250
        - only letters
        - only numbers
        - letters and numbers
    3. Individual validation
        - for name
        - drop down - city
        - description
        - file type
        - file size
    4. Validate whole form when we submit it
    5. Clean the code
    6. Css:
        - change for unvalid field - red
        - warning messages
        


1. Get value from the form.
===========================


   First of all we will need to access our HTML, to do so we will use `document`.
   If you don't know how DOM works you can check on info here:
   https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction
   Or if you tried our `Intro to Javascript` workshop it is in more details in
   part 3.
   https://github.com/node-girls-australia/js-intro-workshop

   So let's get one value out to see how it works, for that you need to check on
   index.html file as well - to see how we are getting element out knowing classes:
   `var name = document.letterToSanta.myName.value;`

   TODO: now it is your turn to get other values out. Create variables called
   `city`, `behavior`, `description` and store in them appropriate values from the form.
   
   
2. Validate one value to be right.
==================================


  Let's continue our example `name` and validate it.
  First of all let's validate that our `name` value is not empty:

  `function nameValidation (value) {
    if(value == '') {
      return 'This field cannot be empty';
    }
  }`

  TODO: add inside of `nameValidation` function few more validations:
  - If name is shorter then 2 characters print error
  'This field should be longer then 1 character'
  - If name is longer then 250 characters print error
  'This field cannot be longer then 250 characters'


  But there is one problem with this function - it will always give us
  only one error. What if would have few errors same time?
  (Not in this case of course).
  It would be better to create a variable `error` for each single error
  and a variable `errors` - an array that will keep a list of all errors.
  PS: If you do not remember much about arrays and what methods you can
  use to work with them you can read more here:
  https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array

  TODO: create an variable `error` that equal to empty string and an empty
  array named `errors`.


  Now when we have this containers to keep pieces of information we can use
  them inside of our function `nameValidation`, to give them some values. As so:

  `function nameValidation (value) {
    if(value == '') {
      error = 'This field cannot be empty';
      errors.push(error);
    }
  }`

  So with this changes we gave a value to variable `error` that is equal to
  'This field cannot be empty', and we pushed this `error` to the array of
  all our errors. If you console.log(errors) after running the function you
  should see our error.

  TODO: Do same to our minimum and maximum length validations. And in the end
  of our function `nameValidation` we need to return `error`.


  Now it is time to do one more interesting validation. For example, we want
  our name to contain only from letters, how we can check on it? One of very
  handy tool that we can use for this kinds of checks is Regular Expressions
  (RegEx/RegExp for short) - an object that describes a pattern of characters.
  RegExps are used to perform pattern-matching and "search-and-replace" functions
  on text. So we can check if our text(or part of the text) is only letters or
  only numbers, or if we have particular set of letters etc.

  RegEx can get very complex, but we will deal with a simple patterns today.
  PS: check on regular-expressions.txt for more information about RegExp and
  links to cheat sheet and interesting games(!!!) where you can practice your
  new skill. Very recommended!

  Regular expression objects have a number of methods. The simplest one is
  `test`. If you pass it a string, it will return a Boolean telling you whether
  the string contains a match of the pattern in the expression.

  `console.log(/abc/.test("abcde"));
    // → true
  console.log(/abc/.test("abxde"));
    // → false`

  To validate that our name will have only letters we can test our value as
  follows:

  `var onlyLetters = /^[A-z]+$/.test(value);`

  This test will return us boolean value (true or false), if it does match our
  pattern `onlyLetters` will be equal to `true`.

  TODO: Add one more validation to `nameValidation` function. That will validate
  that we have letters only, if not return us an error 'This field can have only
  letters'
  

3. Submitting the form.
========================
  
  
  Now, when we have full validation on name field let's check
  up how it works when we submit the form. If you go to the
  `index.html` file and check on the form, on the bottom, where
  we have submit button `Send your letter to Santa` you can see
  that `onclick` we call function `validateForm`.

  TODO: 1.So let's create function `validateForm`. It will not take
  any arguments.
  TODO: 2. Move variable `name` with it's value inside of the function.
  TODO: 3. Call function `nameValidation` inside our `validateForm`
  function and pass `name` as an argument.
  TODO: 4. console.log the `errors` so we can check if it works.

  So, what happened now - when we click submit button we get the value
  of the name field (if there is any) and after we call function
  `nameValidation` - that will validate if our name field is empty,
  will check if it's of an appropriate length and if our name consist
  only from letters.

  If you try it now nothing will happen. And the reason for that is
  the default behaviour of the submit button.
  
  
4. Events. Submit button.
=========================

  The standard behaviour of the form on submit - all input will be send
  to server and page will be reloaded - it was reasonable long time ago
  when it was no single page application concept. Nowadays we creating 
  single-page applications with javascript and we want to prevent that 
  default behaviour.

  TODO: 1. At `index.html` page add `event` as an argument of the
  `validateForm` function.
  TODO: 2. In your javascript file add `event` as an argument to
  the `validateForm` function.
  TODO: 3. Inside of the `validateForm` function call:
  `event.preventDefault();`


  Now if you try to submit form with empty name field it should print
  you all errors.
  

5. Display success or errors
============================
  
  Let's create an error handler, that will be handling our errors
  and if
  
  TODO: 1. Create function `handleErrors` that takes 1 argument `errors`
  and if we have no errors console.log 'Success', else - console.log errors.
  TODO: 2. Replace console.log errors in function `validateForm` with calling
  on function `handleErrors` and pass in it `errors`.
  
  Run the form again and if name field is filled in correctly it should
  print us 'Success', else any error or all errors that we get.
  
  
6. Clean errors
===============

  If you run form multiple times you will see that the array of our errors
  is keep growing, to avoid that we can clean the errors. So every time we
  submit form we start with an empty errors array.
  
  TODO: on the first line inside of `validateForm` function reset `error`
  by making it equal to empty array.


7. Do validation for other fields
=================================

  Now, when you know all steps on how to validate 1 field do same for other
  fields.
  
  TODO: 1. Get value from field city and validate that it is not empty.
  TODO: 2. Get value from field description and validate that it is not empty,
  has more then 2 characters, less then 250 characters and consist only from
  letters and numbers.
  TODO: 3. Call on validation in `validateForm`.
   
  
  Now three of your fields should be validating in a proper way.
  
  
8. Refactoring/cleaning
=======================

  As you can see now we have a lot of repetitions in our code. We are checking
  for the same validations in some fields. Now, when all code works, it is
  time to do some cleaning and refactor the code.
  
  TODO: 1. Let's subtract a check if field is empty to separate function
  named `emptyFieldError`, which will take one argument `value`. Inside will do
  the if check and in the end of the function return array of `errors`.
  TODO: 2. Subtract check of minimum length of the field into function named
  `minLengthError`, which will take one argument `value`. Inside it will do same
  as previous one with it's own check.
  TODO: 3. Do same for new function `maxLengthError`.
  TODO: 4. Do same for new function `onlyLettersError`.
  TODO: 5. Do same for new function `onlyNumbersLettersError`.
  TODO: 6. Now, when we have those separate functions you can call on them inside
  of `nameValidation`, `descriptionValidation` and `cityValidation`, depending
  on what we were checking in which field.
  
  
  Now when you submit form it all should work the same. And as you can see, now
  we do not have that many repetitions in our code and it consist of smaller functions
  that does some particular action.


9. If field is empty do not do other checks
===========================================



 




/*
9. If field is empty do not do other checks
 function nameValidation (value) {
    if(emptyFieldError(value)){
        return;
    }
    minLengthError(value);
    maxLengthError(value);
    onlyLettersError(value);
 }
10. If errors change css + append errors messages into page
11. If no errors redirect to wish list page
- and save all values to local storage
- clean the fields
 */





////////////////////////////////////////////////////////////////////////
//                                                                    //
// Congratulations! You have finished Part 1!                         //
// Stand up, stretch your legs, celebrate your achievement.           //
// Next step will be following up the instructions in level2.js file. //
//                                                                    //
////////////////////////////////////////////////////////////////////////
