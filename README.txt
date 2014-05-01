Dandyland App
=============

This repository contains the source code for the Dandy Land Android app demo 

![alt tag](http://img0.etsystatic.com/004/0/5952378/il_fullxfull.352669016_64vr.jpg)
![alt tag](https://dl-web.dropbox.com/get/Screenshot_2014-04-30-19-09-48.png?_subject_uid=53377631&w=AADW-W6SJioLSwXkChDiC6pNLYe6KbH6bF8XtUq96UuMlw)


Bug Tracking
-------------

Please see the issues section to report any bugs or feature requests and to see the list of known issues.

https://github.com/dsamiam/Dandyland/issues	


      
Author: Darius D. Samah, 01-May-2014 (dsamah@luc.edu)


Summary:
--------
This game hardly even needs an explanation. Players choose a color and move along a multi-colored path towards the game-winning objective.
Movement is very simple. Draw a card and move to the next colored space that matches it. Some cards have 2 colored squares, which means you get to move twice. 
Some spaces have a licorice symbol and cause you to lose a turn, and to really mix things up, there is a special card for each of the special locations that causes you to teleport to that location.

This file summarizes what you will find in each of the files that make up the Dandy Land application.
	index.html        - This is the main application source file.
	


Prerequisites:
-------------- 	    -
1 download .Net Framework 4.0 and install it;              - compatible with source tree
2. Java SE Development Kit 8 Update 55



Setup:
------
1. Notepad++
2. Source Tree                                             -is a free Mercurial and Git Client for Windows and Mac that provides a graphical interface for your Hg and Git repositories
3. .NET Framework 4.5
4. PhoneGap account


After compiling your source code in notepad++, one will need to commit their code using source tree


Building
--------

The build requires Phonegap and the apk installer application to be installed in your development environment. 

PhoneGap is an open source implementation of open standards. That means developers and companies can use PhoneGap for mobile applications that are free, commercial, open source, or any combination of these



Steps to Create a Hello World Application in phonegap and apk installer
-------------------------------------------------------------

Step 1: First,Open the Notepad++ write the following code with in the html tags of notepad.

<html>
	<head>
		<script>
			/*GLOBAL VARIABLES*/
			var board=[];//array representing squares of gameboard
			var boardLength=30;//64;//int describing # squares on gameboard. Could be adjustable depending on alternate game rules.
			var boardNoAdjacentColoredSquareBool=true;
			
			var deck=[];//array representing cards in deck
			var deckLength=512;//int describing # cards in deck. Could be adjustable depending on alternate game rules.
			
			var players=[];//array representing players' positions
			var playersLength=4;//int describing # players in game.
			
			var playerNumberTurn=0;//int describing which players turn. Should be incremented after each player's turn and reset to 0 when greater than playersLength var.
			var currentCardIndex=0;//int describing the current card (index). Should be incremented after each turn (and reset to 0 when greater than deckLength var).
			
			var mustLandExactlyOnEndSquare=false;
			
			/*FUNCTIONS*/
			function initGame(){
				//INITIALIZE BOARD
				for(var i=0; i<boardLength;i++){//for-loop to set gameboard's squares. Arrays in JavaScript (and most programming languages) start at 0. This will fill the arrays with elements numbered 0 through 63 (boardLength-1).
					board[i]=
						Math.floor(//round the number to the next lowest integer. See http://www.w3schools.com/jsref/jsref_floor.asp
							Math.random()//generate a random number from 0 up to but not including 1. See http://www.w3schools.com/jsref/jsref_random.asp
							*
							6//multiply it by to get a random number from 0 up to but not including 6.
						);
				}
				
				//INITIALIZE DECK
				for(var i=0; i<deckLength;i++){//for-loop to set deck's card colors.
					deck[i]=
						Math.floor(//round the number to the next lowest integer. See http://www.w3schools.com/jsref/jsref_floor.asp
							Math.random()//generate a random number from 0 up to but not including 1. See http://www.w3schools.com/jsref/jsref_random.asp
							*
							6//multiply it by to get a random number from 0 up to but not including 6.
						);
				}
				
				//INITIALIZE PLAYER POSITIONS (AT 0)
				for(var i=0; i<playersLength;i++){
					players[i]=0;
				}
				
				drawGame();
			}
			function drawGame(){
				var gameString="";
				for(var i=0; i<board.length;i++){//for-loop to loop though gameboard's squares and draw them (and players) on board.
				
					if(i==0) gameString+="START -";//Mark first square with "START"
					else if(i==board.length-1) gameString+="END -";//Mark last (winning) square with "END"
					
					//DRAW GAMEBOARD SQUARES (COLORS)
					//gameString+="\t"+board[i];//draw square #
					gameString+="\t"+colorIndexToName(board[i]);//draw square color
					
					
					//DRAW PLAYER POSITIONS
					for(var j=0; j<players.length;j++){
						if(players[j]==i){//if player's position is same as current square of board. Draw player.
							//for(var k=0; k<j;k++)
								gameString+="\t";//indent to allow multiple players on a line
							gameString+="P"+(j+1);//use +1 to represent 1-4, not 0-3
						}
					}
					gameString+="\n";
				}
				
				document.getElementById("gameTextArea").value=gameString;
				//alert("Ready!")
			}
			function selectCard(){
				document.getElementById("btnDrawCard").disabled=true;
				
				document.getElementById("btnMove").disabled=false;
				document.getElementById("divCardDisplay").style.display="block";
				
				//document.getElementById("spanCardDisplay").innerHTML=deck[currentCardIndex];
				document.getElementById("spanCardDisplay").innerHTML=colorIndexToName(deck[currentCardIndex]);
				document.getElementById("spanCardDisplay").style.color=colorIndexToName(deck[currentCardIndex]);
			}
			function colorIndexToName(colorIdx){
				switch(colorIdx){
					case 0: return "Purple";
					case 1: return "Red";
					case 2: return "Yellow";
					case 3: return "Blue";
					case 4: return "Orange";
					case 5: return "Green";
					default: return "ERR";
				}
			}
			function movePlayer(){
				while(++players[playerNumberTurn]){
					if(board.length<=players[playerNumberTurn])return playerWins();
					else if(board[players[playerNumberTurn]]==deck[currentCardIndex])break;
				}
				
				if(++currentCardIndex>deckLength){//reset currentCardIndex back to 0 if there are no more cards.
					currentCardIndex=0;
					//you could reshuffle deck here too, but you'd have to take out any special cards.
				}
				if(++playerNumberTurn>(playersLength-1))playerNumberTurn=0;
				
				drawGame();
				
				document.getElementById("spanPlayerNumTurn").innerHTML=(playerNumberTurn+1);
				document.getElementById("btnDrawCard").disabled=false;
				document.getElementById("btnMove").disabled=true;
				document.getElementById("divCardDisplay").style.display="none";
			}
			function playerWins(){
				alert("Player "+(playerNumberTurn+1)+" Wins!");
				
				document.getElementById("gameTextArea").value="";
				document.getElementById("btnDrawCard").disabled=true;
				document.getElementById("btnMove").disabled=true;
				document.getElementById("divCardDisplay").style.display="none";
			}
		</script>
	</head>
	<body onload="initGame()">
		<!--GAME OPTIONS DISP-->
		<!--
		<div style="border solid red 2px;">
			# Human Players:<input id="txtNumPlayers" type="text" value="1"/>
			# AI Players:<input id="txtNumAIs" type="text" value="1"/>
			<input type="button" onclick="initGame()" value="Start Game"/>
		</div>
		-->
		
		<!--TURN DESC-->
		<strong>Player #<span id="spanPlayerNumTurn">1</span>'s Turn.</strong>
		<div>
			<input id="btnDrawCard" type="button" onclick="selectCard()" value="Draw Card"/>
		</div>
		<br/>
		
		<!--CARD CHOICE DISP-->
		<div id="divCardDisplay" style="display:none;"><!-- style="display:none;"-->
			You got a <span id="spanCardDisplay" style="color:red; font-weight:bold"></span>&nbsp;Card!
			<input id="btnMove" type="button" onclick="movePlayer()" value="Move!" disabled/>
		</div>
		<br/>
		<hr/>
		<br/>
		
		<!--GAME BOARD DISP-->
		<textarea id="gameTextArea" cols="100" rows="100"></textarea>
	</body>
</html>


Step 2: Next save the file as index.html

step 3: Open the Following link : Build the PhoneGap application (https://build.phonegap.com/) 

Step 4: Create a account by using Either AdobeID or GitHub)

Step 5: Enter the Login information.

Step 6: Click on add new apps

Step 7: Upload the index.html zip file  and click on create button

Step 8: Next,Successfully the apps are created for android

Step 9: Now the download the file and install in your device using the apk installer app and Test the application


Contributors
-------------

Thank you to all the contributors on this project. Your help is much appreciated.


Contributing
-------------

Please fork this repository and contribute back using pull requests.
Any contributions, large or small, major features, bug fixes, additional language translations, unit/integration tests are welcomed and appreciated but will be thoroughly reviewed and discussed.



License
-------
The MIT License (MIT)

Copyright (c) 2014 Darius Samah

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
