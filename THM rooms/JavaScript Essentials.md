There are three ways to declare variables in JS: `var`, `let`, and `const`. While `var` is function-scoped, both `let`, and `const` are block-scoped, offering better control over variable visibility within specific code blocks.

For example, you are developing a web application in which you need to print students' results on the web page. The ideal case would be to create a function `PrintResult(rollNum)` that would accept the roll number of the user as an argument.
```javascript
<script>
        function PrintResult(rollNum) {
            alert("Username with roll number " + rollNum + " has passed the exam");
            // any other logic to display the result
        }

        for (let i = 0; i < 100; i++) {
            PrintResult(rollNumbers[i]);
        }
    </script>
```

JS is an **interpreted** language, meaning the code is executed directly in the browser without prior compilation. Below is a sample JS code demonstrating key concepts, such as **defining a variable**, **understanding data types**, **using control flow statements**, and writing simple functions.
```javascript
 // Hello, World! program
console.log("Hello, World!");

// Variable and Data Type
let age = 25; // Number type

// Control Flow Statement
if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}

// Function
function greet(name) {
    console.log("Hello, " + name + "!");
}

// Calling the function
greet("Bob");
```
JS is primarily executed on the client side, which makes it easy to inspect and interact with HTML directly within the browser.

# Integrating JavaScript in HTML

## Internal JavaScript
Internal JS refers to embedding the JS code directly within an HTML document. This method is preferable for beginners because it allows them to see how the script interacts with the HTML. The script is inserted between `<script>` tags. These tags can be placed inside the `<head>` section, typically used for scripts that need to be loaded before the page content is rendered, or inside the `<body>` section, where the script can be utilized to interact with elements as they are loaded on the web page.
```javascript
 <!DOCTYPE html>
<html lang="en">
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script>
        let x = 5;
        let y = 10;
        let result = x + y;
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```
![[Pasted image 20250727104553.png]]

## External JavaScript

External JS involves creating and storing JS code in a separate file ending with a `.js` file extension. This method helps developers keep the HTML document clean and organised. The external JS file can be stored or hosted on the same web server as the HTML document or stored on an external web server such as the cloud.
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <!-- Link to the external JS file -->
    <script src="script.js"></script>
</body>
</html>
```

## Verifying Internal or External JS

When pen-testing a web application, it is important to check whether the website uses internal or external JS. This can be easily verified by viewing the page's source code
This will display the HTML code of the rendered page. Inside the source code, any JS written directly on the page will appear between `<script>` tags without the `src` attribute. If you see a `<script>`tag with a src attribute, it indicates that the page is loading external JS from a separate file.

----
## Alert

The alert function displays a message in a dialogue box with an "`OK`" button, typically used to convey information or warnings to users. For example, if we want to display "`Hello THM`" to the user, we would use an `alert("HelloTHM");`. To try it out, open the Chrome console, type `alert("Hello THM")`, and press Enter. A dialogue box with the message will appear on the screen.

## Prompt

The prompt function displays a dialogue box that asks the user for input. It returns the entered value when the user clicks "`OK`", or null if the user clicks "`Cancel`". For example, to ask the user for their name, we would use `prompt("What is your name?");`.

To test this, open the Chrome console and paste the following that asks for a username and then greets him.
```javascript
name = prompt("What is your name?");
    alert("Hello " + name);
```

## Confirm

The confirm function displays a dialogue box with a message and two buttons: "`OK`" and "`Cancel`". It returns true if the user clicks "`OK`" and false if the user clicks "`Cancel`". For example, to ask the user for confirmation, we would use `confirm("Are you sure?");`. To try this out, open the Chrome console, type `confirm("Do you want to proceed?")`, and press `Enter`.

**How Hackers Exploit the Functionality** 
Imagine receiving an email from a stranger with an attached HTML file. The file looks harmless, but when you open it, it contains JS that disrupts your browsing experience. For example, the following code will show an alert box with the message "**Hacked**" three times:
Imagine if a bad actor sent you a similar file, but instead of displaying the alert three times, the number is set to **500**. You would be forced to keep closing the alert boxes one after another. This is a simple example of how malicious JS can be used to create an inconvenience or worse. Therefore, ensuring you only execute JS files from trusted sources is crucial to avoid such an undesired experience.

# Obfuscation
We have understood how JS works and how we can read it until now, but what if the file is not human-readable and has been **minified**?

Minification in JS is the process of compressing JS files by removing all unnecessary characters, such as spaces, line breaks, comments, and even shortening variable names. This helps reduce the file size and improves the loading time of web pages, especially in production environments. Minified files make the code more compact and harder to read for humans, but they still function exactly the same.

Similarly, **obfuscation** is often used to make JS harder to understand by adding undesired code, renaming variables and functions to meaningless names, and even inserting dummy code.
We can also deobfuscate an obfuscated code using an online tool.
