---
comments: true
layout: default
title: Calculator
description: Calculator made with javascript
type: tangibles
courses: { csse: {week: 1}, csp: {week: 1}, csa: {week: 0} }
---

<link rel="stylesheet" href="{{site.baseurl}}/assets/css/calc.css">

<div class="container">
    <p class="display"></p>
</div>
<div class="container">
    <button class="button" onclick="appendToDisplay('1')">1</button>
    <button class="button" onclick="appendToDisplay('2')">2</button>
    <button class="button" onclick="appendToDisplay('3')">3</button>
    <button class="button" onclick="appendToDisplay('+')">+</button>
</div>
<div class="container">
    <button class="button" onclick="appendToDisplay('4')">4</button>
    <button class="button" onclick="appendToDisplay('5')">5</button>
    <button class="button" onclick="appendToDisplay('6')">6</button>
    <button class="button" onclick="appendToDisplay('-')">-</button>
</div>
<div class="container">
    <button class="button" onclick="appendToDisplay('7')">7</button>
    <button class="button" onclick="appendToDisplay('8')">8</button>
    <button class="button" onclick="appendToDisplay('9')">9</button>
    <button class="button" onclick="appendToDisplay('*')">x</button>
</div>
<div class="container">
    <button class="button" onclick="clearDisplay()">CLEAR</button>
    <button class="button" onclick="appendToDisplay('0')">0</button>
    <button class="button" onclick="calculateResult()">ENTER</button>
    <button class="button" onclick="appendToDisplay('/')">/</button>
</div>

<script>
// Initialize an empty string to store the text to be displayed
let displayText = '';

// Function to append a value to the displayText and update the display
function appendToDisplay(value) {
    displayText += value; // Concatenate the value to the displayText
    document.querySelector('.display').textContent = displayText; // Update the display element with the new displayText
}

// Function to clear the displayText and update the display
function clearDisplay() {
    displayText = ''; // Clear the displayText
    document.querySelector('.display').textContent = displayText; // Update the display element with the cleared displayText
}

// Function to calculate and display the result of the expression in displayText
function calculateResult() {
    try {
        const result = eval(displayText); // Evaluate the expression in displayText
        document.querySelector('.display').textContent = result; // Update the display element with the calculated result
        displayText = result.toString(); // Store the result as the new displayText
    } catch (error) {
        document.querySelector('.display').textContent = 'Error'; // Display 'Error' on the display element
        displayText = ''; // Clear the displayText
    }
}

</script>
    
