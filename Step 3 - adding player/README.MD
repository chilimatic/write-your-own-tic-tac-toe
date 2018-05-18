# Adding player

In this step we want to our players to the board.

Goals: 

 - having our X for player 1 and O for player 2 
 - forbid to paint two symbols into the same field
 - displaying which players turn it is
 - display the current round
 
## adding the players 
 
our players have two different symbols we will add these symbols to our code
 
So lets take a short detour was a symbol is, a symbol is something you can draw and you give it meaning.
What does it mean? if you take a piece of paper and you doodle a star on it, this is star is a symbol. 
 
you tell your friends it's a star and show your picture to them.
now you assigned meaning to it. Because your friends now know it is your star. 
 
so you can put your star on your stuff and everyone you told knows this is your stuff.  
 
 ```javascript
var player1Symbol = 'X';
var player2Symbol = '0'
``` 

now we added our symbols we can now see player 1 is "X" and player 2 is "0" so everytime we see this 
we know which player has drawn the X and which player has drawn the 0

as a next step we want to be able to switch between the X and the 0 everytime a player marks a field on our board.
So we need to bind/group them together so we can go back and forth between those two symbols.

The reason why we're doing this is because in our case it's easier to say "give me the symbol of player 1" which is an X 
than saying "this is X and give me the number of the player that uses X".


This has to do with how programing languages use arrays. an Array starts with 0 and can have a lot of entries. in our case 
we have two entries.
   

 ```javascript
var player1Symbol = 'X';
var player2Symbol = 'O';

var players = [player1Symbol, player2Symbol];
``` 

We now add the currentPlayer and we say take the first (the symbol on the position 0) in the array.

```javascript
var player1Symbol = 'X';
var player2Symbol = 'O';
var currentPlayer = 0;
var players = [player1Symbol, player2Symbol];
``` 

So we can now switch between the symbols by just checking if the our currentPlayer variable is 0 or 1.
   
This still might be confusing but hopefully with some code examples this will get easier to understand.

```javascript
/**
 * @returns {number}
 */
function switchPlayerSymbol() {
    if (currentPlayer === 0) {
        currentPlayer = 1;
    } else {
        currentPlayer = 0;
    }

    return currentPlayer;
}
```
So what is happening when we call switchPlayerSymbol ? 
we check 'what is our current player' and we than say change the player and give use the next player

```javascript
document.addEventListener('DOMContentLoaded', function() { 
    var board = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];
    
    var player1Symbol = 'X';
    var player2Symbol = 'O';
    var currentPlayer = 0;
    var players = [player1Symbol, player2Symbol];

    var field;
    for (var rowNumber in board) {
        for (var fieldNumber in board[rowNumber]) {
            field = getField(rowNumber, fieldNumber);
            field.addEventListener('click', function(e) {
                var fieldId = this.getAttribute('data-field');
                var rowId = this.parentNode.getAttribute('data-row');
                var nextPlayer = switchPlayerSymbol();
                
                markField(rowId, fieldId, players[nextPlayer]);
            });
        }
    }

    /**
     * @param rowNumber
     * @param fieldNumber
     * @returns Element | null
     */
    function getField(rowNumber, fieldNumber) {
        // we build our selector so we can say give us the the field with the fieldNumber within the row with the rowNumber
        var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
        var element = document.querySelector(fieldId);

        if (!element) {
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

    }
    
    /**
     * @returns {number}
     */
    function switchPlayerSymbol() {
        if (currentPlayer === 0) {
            currentPlayer = 1;
        } else {
            currentPlayer = 0;
        }
    
        return currentPlayer;
    }
});
```
so this is okay but we made a mistake, we always get the next player we want to use the current player.
so we need a function that returns the current player and at the end of the round we want to have the next player.

so we write our new function

```javascript
/**
 * @returns symbol
 */
function getCurrentPlayerSymbol() {
    return players[currentPlayer];
}
```
This functions just checks "what is our current player" and returns the symbol/sign of the player

```javascript
document.addEventListener('DOMContentLoaded', function() { 
    var board = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];
    
    var player1Symbol = 'X';
    var player2Symbol = 'O';
    var currentPlayer = 0;
    var players = [player1Symbol, player2Symbol];

    var field;
    for (var rowNumber in board) {
        for (var fieldNumber in board[rowNumber]) {
            field = getField(rowNumber, fieldNumber);
            field.addEventListener('click', function(e) {
                var fieldId = this.getAttribute('data-field');
                var rowId = this.parentNode.getAttribute('data-row');                
                markField(rowId, fieldId, getCurrentPlayerSymbol());
                switchPlayerSymbol();
            });
        }
    }

    /**
     * @param rowNumber
     * @param fieldNumber
     * @returns Element | null
     */
    function getField(rowNumber, fieldNumber) {
        // we build our selector so we can say give us the the field with the fieldNumber within the row with the rowNumber
        var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
        var element = document.querySelector(fieldId);

        if (!element) {
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

    }
    
    /**
     * @returns {number}
     */
    function switchPlayerSymbol() {
        if (currentPlayer === 0) {
            currentPlayer = 1;
        } else {
            currentPlayer = 0;
        }
    
        return currentPlayer;
    }
    
    /**
     * @returns symbol
     */
    function getCurrentPlayerSymbol() {
        return players[currentPlayer];
    }
});
```
So now we can switch between the players and we want to now display which players turn it is.

so we add some new HTML to our Display. 

```HTML
<div class="player">
    <ul>
        <li>current player: <span id="current-player"></span></li>
    </ul>
</div>
```

This is a list which shows us the current player the ```<span id="current-player"></span>``` is where we display the current 
player number;

we now once again need to connect our program with our display.

```javascript
function updatePlayerDisplay() {
    document.getElementById('current-player').innerHTML = currentPlayer;
}
```

so we can now call the updatePlayerDisplay function and it will show us which players turn it is.... but wait
since we're programming and we start counting at 0 not at 1 we actually show 'current player is 0' and the player two
would be shown as 'player 1' this is confusing and weird if you're not a programmer.
 
so we need to always add + 1 for the user

lets write a function for this.

```javascript
function getCurrentPlayerForDisplay() {
    return currentPlayer + 1;
}
```

so now we can call this function to always correct player number

```javascript
function updatePlayerDisplay() {
    document.getElementById('current-player').innerHTML = getCurrentPlayerForDisplay();
}

/**
 * @returns {number}
 */
function getCurrentPlayerForDisplay() {
    return currentPlayer + 1;
}
```

as maybe some of use realized we write a lot of small functions. That is because we want to use and change our programm
more easily. And reading currentPlayer + 1 is harder to understand than calling the function 'getCurrentPlayerForDisplay' 

so we want to have small parts we can read like short instructions in a book and we don't have to understand what is 
happening in the back but we can understand what it does.

We now add the updatePlayerDisplay function into our game.
```javascript
document.addEventListener('DOMContentLoaded', function() { 
    var board = [
        [null, null, null], // row 0
        [null, null, null], // row 1
        [null, null, null], // row 3
    ];
    
    var player1Symbol = 'X';
    var player2Symbol = 'O';
    var currentPlayer = 0;
    var players = [player1Symbol, player2Symbol];

    var field;
    for (var rowNumber in board) {
        for (var fieldNumber in board[rowNumber]) {
            field = getField(rowNumber, fieldNumber);
            field.addEventListener('click', function(e) {
                var fieldId = this.getAttribute('data-field');
                var rowId = this.parentNode.getAttribute('data-row');                
                markField(rowId, fieldId, getCurrentPlayerSymbol());
                switchPlayerSymbol();
                updatePlayerDisplay();
            });
        }
    }

    /**
     * @param rowNumber
     * @param fieldNumber
     * @returns Element | null
     */
    function getField(rowNumber, fieldNumber) {
        // we build our selector so we can say give us the the field with the fieldNumber within the row with the rowNumber
        var fieldId = 'div[data-row="' +rowNumber+ '"] > div[data-field="' +fieldNumber+ '"]';
        var element = document.querySelector(fieldId);

        if (!element) {
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

    }
    
    /**
     * @returns {number}
     */
    function switchPlayerSymbol() {
        if (currentPlayer === 0) {
            currentPlayer = 1;
        } else {
            currentPlayer = 0;
        }
    
        return currentPlayer;
    }
    
    /**
     * @returns symbol
     */
    function getCurrentPlayerSymbol() {
        return players[currentPlayer];
    }
    
    function updatePlayerDisplay() {
        document.getElementById('current-player').innerHTML = getCurrentPlayerForDisplay();
    }
    
    /**
     * @returns {number}
     */
    function getCurrentPlayerForDisplay() {
        return currentPlayer + 1;
    }
});
```

we now want to display the current round we're in so we add the html code
```javascript

```