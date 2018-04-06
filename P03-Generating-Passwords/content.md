---
title: "React Redux Generating Passwords"
slug: react-redux-generating-passwords
---

# Generating Passwords

There is a use case for apps that save and store passwords. There is also plenty to 
debate about how secure this is and whether it is a good idea or not. This 
tutorial is less concerned with making a marketable product and more concerned with 
teaching you JavaScript and React! 

We will sidestep the issue of market viablity of the app, and instead focus on the 
code that makes it work. 

## What makes a good password? 

The forst step will be to create some code that generates a random password. If 
we are generating passwords we might as well ask the question: what makes a good 
password? 

Generally speaking the strength of a password is realted to the number of possible 
guesses it would take to solve it. If you used only numbers and made a password 
that was 1 character long there would be 10 possible choices. 

If you increased the number of characters to 2 you'd have 100 possible choices. 

Expanding this to include letters, upper and lower case, and punctuation you'd 
have a character set of 95 characters to choose. Generating a password with a 
length of 8 characters would give you trillions of possibilities. 

We will hold on this idea for now but, the question is more complex since 
passwords are generated by people and people tend to use patterns when they 
generate their passwords. See the links int he resources for more information. 

## Make a component to display a password

Create a new file 'src/password.js'. 

Define a new React component in this file. 

```JavaScript
import React, { Component } from 'react'

class Password extends Component {
  constructor(props) {
    super(props)
    this.state = { password: 'p@ssw0rd' }
  }

  generatePassword() {
    // generate a password here
    console.log("generating password")
  }

  render() {
    return (
      <div>
        <div>{this.state.password}</div>
        <div>
          <button onClick={(e) => {
            this.generatePassword()
          }}>Generate</button>
        </div>
      </div>
    )
  }
}

export default Password
```

Import this into 'App'. Modify 'App.js' to look like this: 

```JavaScript
import React, { Component } from 'react';
import './App.css';
import Password from './password'

class App extends Component {
  render() {
    return (
      <div className="App">
        <Password />
      </div>
    );
  }
}

export default App;
```

With this in place you should see "p@ssw0rd" appear in the browser.
Clicking the button should print: "generating password" to the console. 

This is a terrible password! You need to generate a better password. 

### Math.random() 

To generate a random password we need to generate some random values. JavaScript
provides the `Math.random()` method for this. 

The `Math.random()` method returns a random number between 0 and 1. 

More accurately [MDN - Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) says: 

> The Math.random() function returns a floating-point, pseudo-random number in the range 
> from 0 inclusive up to but not including 1 — which you can then scale to your desired 
> range. The implementation selects the initial seed to the random number generation 
> algorithm; it cannot be chosen or reset by the user.

Let's take this apart. 

**Floating-point** : is a decimal number like: 173.9 or 0.30000000000000004. 

**from 0 inclusive up to but not including 1** : says the possible values could range from
0.0 to 0.999999999999... 

**which you can then scale** : To get a usable range of values you will need to multiply
by a range. For example `Math.random() * 6` would generate numbers from 0.0 to 5.999...

Often you will want a whole number. Since `Math.random()` generates a floating point 
number you will need to use some of the other `Math` functions to convert the floating
point values into Integers. `Math.round()`, `Math.floor()`, `Math.ceil()` among others
can do that. 

`Math.random()` generates a **pseudo-random number**. This means that the numbers generated
are not entirely random. They are random enough but over long observation you might be able 
to point out patterns. For our purposes the numbers will be random enough. 

#### Generating random integers in a range

For this project you will want to generate random integers in a range. For example
if you have an array you might want to get a random element from that array. The index 
of any element in an array is always an integer starting at 0 up to the length of the 
array - 1. 

```JavaScript
function random(n) {
  return Math.floor(Math.random() * n)
}
```

This function returns a whole number from 0 to n - 1. 

`Math.floor(n)` rounds the value `n` down to a whole number. Here is what MDN says: 

> The Math.floor() function returns the largest integer less than or equal to a given number.

Here are a few use examples for the function above. 

Generate a random die roll: `random(6) + 1; // generates a number be 1 and 6`

Get a random element from an array: 

```JavaScript
var names = ['Ann', 'Bob', 'Cat', 'Dan'];
names[random(names.length)] // returns a random name e.g. 'Bob'
```

### Generating a random character

To generate a random character there are several routes you can choose. Which you 
choose depends on your use. I'll leave the final decision to you. 

#### Use an Array 

Use an Array. Create an array of all of the characters and choose a 
random element from the array. 

```JavaScript
var letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g'];
console.log(letters[random(letters.length)])
```

#### Strings are Array like

In JavaScript Strings contain a sequence of characters. JavaScript treats Strings as
"Array Like" objects. This means you can use many of the features of Arrays on Strings. 

For example you can get the length of a String: `"abcd".length // 4`

You can access any character: `"abcd"[2] // c`

```JavaScript
var str = 'abcdefghijklmnopqrstuvwxyz';
console.log(str[random(str.length)]); // prints a random lowercase letter
```

#### Use character code

Another way to generate characters is to use the character code. In short the 
each character is assigned number allowing them to be identified by that number. 

For example: 'a' is character code 97.

Many of the characters are not printable or usable in a password. 

Characters in the range of 33 to 126 include upper case, lower case, numbers, and 
punctuation. 

Try this yourself. The code sample below should print all of the character in the 
character code range 33 to 126. 

```JavaScript
for (let i = 33; i < 127; i ++) {
  console.log(i, String.fromCharCode(i))
}
```

### Challenges 

Your challenge is to write a function that generates a string of 8 random characters. 
Use any of the ideas above the method you choose is up to you. 

Your random password should display in the component when you press the "Generate" 
button. This will happen automatically by calling: 

`this.setState({ password: newPassword })`

Where `newPassword` is the random generated password. 

#### More challenges

Humans understand patterns. Putting the characters in the password into a pattern
would make it easier to remember. Modify the generator to output password in this 
form: 

`xxx-xxx-xxx`

Where each x is a random character. For example: 

`6N1-kMH-d?i`
`8G'-PKe-0Tt`
`rR7-J1U-6zZ`

## Resources 

- Math
  - Math.random()
  - Math.floor()
  - Math.ceil()
  - Math.round()
- Strings and Characters
  - charCode()
  
  
  
  





