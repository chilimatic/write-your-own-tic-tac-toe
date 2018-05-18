# Basic functionality marking a field on our board

our first question is how do we preserve state. 
state is a fancy word we like to use to how do we remember/write things down
so we can reuse/resume.

in other words the if the no player has set a symbol/sign the state is empty
as soon as one symbol is on the field we need to remember where the field has been set. 

This can be done in many ways. To make our calculations easier we will use a matrix
what is a matrix? a matrix is a an array that contains other arrays

what is an array? an array is something where we can group information together.

What does this mean? 

Think about it like this: if you got 4,5,6,7,8,9 the ',' symbols those numbers belong together. 

This is basically an array we only write it [4,5,6,7,8,9] and now instead of always needing to write 
4,5,6,7,8,9 we can just write the array name 

as an example: 

```javascript
// you need to open the developer tools in your web-browsers usually it's ctrl + shift + i on windows and linux
var array = [4,5,6,7,8,9];
console.log(array);  
```
The only weird thing we do in computer science we don't start counting with 1, we start with 0

that is why you will see Row 0 and Field 0 which just means the first symbol in the array and the column 

we need to connect our displayed fields with our internal state!
Most of us know the classic map coordination systems 



hallo mein name ist michael, und bin teil des hackathons im CoderDojo


 
        -------------

We will represent this as 

```javascript
 
var board = [[null,null,null],[null,null,null],[null,null,null]]; // this looks confusing so let me format this into a more 2D way so it's more logical

// we start by adding the rows
var board = [
    [], // row 0
    [], // row 1
    [], // row 3
];

// we now add the fields 
// we start by adding the rows
var board = [
    [null, null, null], // row 0
    [null, null, null], // row 1
    [null, null, null], // row 3
];
// why the null? this is just for visualization it means there is no symbol/sign.
// in general in programming null or nil means 'no value'  
```

this is our state representation within our memory lets now connect our display with our internal state
if we look at our display (step2.html)
we see our html markup we now need to make our markup accessible from the outside
```html
    <div class="board">
        <div class="row">
            <div class="field"></div>
            <div class="field"></div>
            <div class="field"></div>
        </div>
        <div class="row">
            <div class="field"></div>
            <div class="field"></div>
            <div class="field"></div>
        </div>
        <div class="row">
            <div class="field"></div>
            <div class="field"></div>
            <div class="field"></div>
        </div>
    </div>
```
we want to associate (connect) our matrix with the display
we have as in every 2 dimensional coordinate system we got 2 points we need to address 

the X and the Y 

X represents ROW in our case 
Y represents the Column 

Row 0 / Field 0
Row 0 / Field 1
Row 0 / Field 2

we want to directly address our html which means we need an ID per element we want to access. 
an id has to unique within the whole HTML.

But if we think about it we actually don't need the rows to access the fields  

```html
    <div class="board">
        <div data-row="0" class="row">
            <div data-field="0" class="field"></div>
            <div data-field="1" class="field"></div>
            <div data-field="2" class="field"></div>
        </div>
        <div data-row="1" class="row">
            <div data-field="0" class="field"></div>
            <div data-field="1" class="field"></div>
            <div data-field="2" class="field"></div>
        </div>
        <div data-row="2" class="row">
            <div data-field="0" class="field"></div>
            <div data-field="1" class="field"></div>
            <div data-field="2" class="field"></div>
        </div>
    </div>
```

Now we can directly connect the fields with our state array 

```javascript
document.addEventListener('DOMContentLoaded', function() { // this is that our script won't be executed before our html is ready
    var field = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];

    // this has only to be done once
    for (var rowNumber in field) {
        for (var fieldNumber in field[rowNumber]) {
            var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
            
            document.getElementById(fieldId).addEventListener('click', function(e) {
                console.log(this)
            }); // this is how we connect the html to our script by adding "event listeners" 
            // in our case we tell the fields 'if someone click on you log yourself'
        }
    }
});
```
As the next step we want to get row and the field so we can actually mark our field with the current player symbol


```javascript
document.addEventListener('DOMContentLoaded', function() { // this is that our script won't be executed before our html is ready
    var field = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];

    // this has only to be done once
    for (var rowNumber in field) {
        for (var fieldNumber in field[rowNumber]) {
            var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';

            document.querySelector(fieldId).addEventListener('click', function(e) {
                var fieldNumber = this.getAttribute('data-field'); // we look at our <div data-field=.....>
                var rowNumber = this.parentNode.getAttribute('data-row'); // we need the row as well -> the row is the parent, the element that has the field inside
                console.log(rowNumber);
                console.log(fieldNumber);
            });
        }
    }
});
```

Now we want to mark our fields that we click! :) with this change we can finally see an X on the fields we click

```javascript
document.addEventListener('DOMContentLoaded', function() { // this is that our script won't be executed before our html is ready
    var field = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];

    // this has only to be done once
    for (var rowNumber in field) {
        for (var fieldNumber in field[rowNumber]) {
            var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';

            document.querySelector(fieldId).addEventListener('click', function(e) {
                var fieldNumber = this.getAttribute('data-field'); // we look at our <div data-field=.....>
                var rowNumber = this.parentNode.getAttribute('data-row'); // we need the row as well -> the row is the parent, the element that has the field inside
                var field = document.querySelector('div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]');
                field.innerHTML = 'X';
            });
        }
    }
});
```

we want to clean this a little so we have less repeating code and so it's easier to read


We want to to actually be able to just have something that will give use the html element we want
so we define a function that gives us the element we want 

So we can just say give us the field 0 in the row 0.

```javascript
/**
 * @param rowNumber
 * @param fieldNumber
 * @returns Element | null
 */
function getField(rowNumber, fieldNumber) {
    // we build our selector so we can say give us the the field with the fieldNumber within the row with the rowNumber
    var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
    var element = document.querySelector(fieldId);
    
    if (!element) { // if we cannot find it in the HTML we scream that we cannot find it.
        throw new Error('Cannot find the field ')
    }

    return element;
}
```

this will change our code to look as follows

```javascript
document.addEventListener('DOMContentLoaded', function() {
    var field = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];

    for (var rowNumber in field) {
        for (var fieldNumber in field[rowNumber]) {
            field = getField(rowNumber, fieldNumber); // we replaced it here

            field.querySelector(fieldId).addEventListener('click', function(e) {
                var fieldNumber = this.getAttribute('data-field');
                var rowNumber = this.parentNode.getAttribute('data-row');
                var field = getField(rowNumber, fieldNumber); // and here
                field.innerHTML = 'X';
            });
        }
    }
});

/**
 * @param rowNumber
 * @param fieldNumber
 * @returns Element | null
 */
function getField(rowNumber, fieldNumber) {
    // we build our selector so we can say give us the the field with the fieldNumber within the row with the rowNumber
    var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
    var element = document.querySelector(fieldId);
    
    if (!element) { // if we cannot find it in the HTML we scream that we cannot find it.
        throw new Error('Cannot find the field ')
    }

    return element;
}
```

And now we want to be able to mark our fields by just passing in which row and field we want to mark and how

```javascript
/**
 * @param rowNumber
 * @param fieldNumber
 * @param symbol
 */
function markField(rowNumber, fieldNumber, symbol) {
    var field = getField(rowNumber, fieldNumber);
    field.innerHTML = symbol;
}
```

if we add this now to our code it will look as follows

```javascript
document.addEventListener('DOMContentLoaded', function() {
    var field = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];

    for (var rowNumber in field) {
        for (var fieldNumber in field[rowNumber]) {
            field = getField(rowNumber, fieldNumber); // we replaced it here

            field.querySelector(fieldId).addEventListener('click', function(e) {
                var fieldNumber = this.getAttribute('data-field');
                var rowNumber = this.parentNode.getAttribute('data-row');
                markField(fieldNumber, rowNumber);
            });
        }
    }
});

/**
 * @param rowNumber
 * @param fieldNumber
 * @returns Element | null
 */
function getField(rowNumber, fieldNumber) {
    // we build our selector so we can say give us the the field with the fieldNumber within the row with the rowNumber
    var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
    var element = document.querySelector(fieldId);
    
    if (!element) { // if we cannot find it in the HTML we scream that we cannot find it.
        throw new Error('Cannot find the field ')
    }

    return element;
}

/**
 * @param rowNumber
 * @param fieldNumber
 * @param symbol
 */
function markField(rowNumber, fieldNumber, symbol) {
    var field = getField(rowNumber, fieldNumber);
    field.innerHTML = symbol;
}
```

now we finally want to connect our internal array with our visual representation
```javascript
/**
 * @param rowNumber
 * @param fieldNumber
 * @param symbol
 */
function markField(rowNumber, fieldNumber, symbol) {
    board[rowNumber][fieldNumber] = symbol;
    var field = getField(rowNumber, fieldNumber);
    field.innerHTML = symbol;
}
```

which leads to 

```javascript
document.addEventListener('DOMContentLoaded', function() {
    var field = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];

    for (var rowNumber in field) {
        for (var fieldNumber in field[rowNumber]) {
            field = getField(rowNumber, fieldNumber); // we replaced it here

            field.querySelector(fieldId).addEventListener('click', function(e) {
                var fieldNumber = this.getAttribute('data-field');
                var rowNumber = this.parentNode.getAttribute('data-row');
                markField(fieldNumber, rowNumber);
            });
        }
    }
});

/**
 * @param rowNumber
 * @param fieldNumber
 * @returns Element | null
 */
function getField(rowNumber, fieldNumber) {
    // we build our selector so we can say give us the the field with the fieldNumber within the row with the rowNumber
    var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
    var element = document.querySelector(fieldId);
    
    if (!element) { // if we cannot find it in the HTML we scream that we cannot find it.
        throw new Error('Cannot find the field ')
    }

    return element;
}

/**
 * @param rowNumber
 * @param fieldNumber
 * @param symbol
 */
function markField(rowNumber, fieldNumber, symbol) {
    board[rowNumber][fieldNumber] = symbol;
    var field = getField(rowNumber, fieldNumber);
    field.innerHTML = symbol;
    
    console.log(board);
}
```

