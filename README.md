# Mbed Chess Game
An embedded chess game device with 1-player and 2-player functionality.

Team Members: Logan Starr, Mert Us, and Matthew Hannay

## Overview 
In this project we built a chess game using an mbed LPC1768 Microcontroller. The game includes a 2-player mode, as well as a 1-player mode in which the AI opponent is implemented using the Minimax Algorithm. Players have the ability to control the game using a joystick and a mobile phone app connected via bluetooth. The device is beginner-friendly, with the possible moves highlighted on the screen when a piece is selected, and a red LED to indicate if a user attempts an illegal move. The game can be reset using a button or the bluetooth app, and the mode can be changed using an on-board switch. The game mode change takes effect after the game is reset. An LCD Screen is used to display the board and a speaker plays tunes when the game ends.

## Components List
* LPC1768 Mbed With Mbed OS 2
* uLCD-144-G2
* Adafruit Bluefruit BLE UART
* SparkFun Mono Audio Amp Breakout
* PCB Mount Speaker
* 5-Way Tactile Switch Breakout
* 8 Position DIP Switch
* Push Button
* Red LED
* 10k Ohm Resistor
* 5V Breadboard Power Supply

## Project Demo
[<img src="./images/chess_proj_thumbnail.png" width="600">](https://www.youtube.com/watch?v=JMscDs1BIZE&t=2s)

## Schematic
<img src="./images/chess_proj_schematic.png" width="600">

## Software Design
### Classes
* __Nav_Switch__: Reads inputs from the 5-way tactile switch. Owned by ECE 4180 instructor Dr. James O. Hamblen.
* __BoardState__: Stores the state of the chess board and implement game moves such as moving and capturing pieces. The game board is implemented as an array of 64 characters. The method for calculating the utility of the board state for the AI opponent is in this class.
* __Gameboard__: Implemented as a wrapper around the BoardState class with the addition of rendering graphics for the LCD display and the game cursor. The Gameboard class includes sprites for images of pieces nad methods for selecting/unselecting squares during the game.

<img src="./images/chess_proj_initial_board.png" width="600">

### State Machine
The game process is implemented as a state machine with five states:
* __whiteSelecting__: This is the initial state of the game. Once the player selects a square with a white piece, possible moves by the piece are highlighted and the game advances to __whitePickedUp__. If the player tries to select a square without a white piece the red LED flashes to indicate an illegal move.
* __whitePickedUp__: In this state, player decides what square to move the selected piece to. In order to unselect, player can select a square other than the possible moves by that piece, and the game will go back to __whiteSelecting__ state. Once the move is made, if the game is in 2-player mode, game advances to __blackSelecting__, else if the game is in 1-player mode it advances to __blackAI__. The game is implemented such that in 1-player mode, the player always controls white pieces.
* __blackSelecting__: The __blackSelecting__ state works the same as the __whiteSelecting__ state, but the player selects black pieces and advances the game to __blackPickedUp__.
* __blackPickedUp__: The same as the __whitePickedUp__ state, and advances back to __whiteSelecting__ once the move is finalized.
* __blackAI__: The program runs the Minimax Algorithm to decide the best move and once it makes the move, changes state to __whiteSelecting__.

<img src="./images/chess_proj_state_machine.png" width="600">
