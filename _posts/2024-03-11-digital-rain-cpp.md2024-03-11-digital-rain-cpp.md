---
layout: post
title: A Project in Modern C++
tags: cpp coding project
categories: demo
---


## Introduction
In this report, I will detail the design, implemtation and testing of a digital rainfall matrix project using modern C++ libraries. The aim of the project is to create an asthetically pleasing visual effect resembling rainfall similiar to what is seen in the Matrix movies within the visual studio terminal. In order to complete this project I had to explore different design decisions, algorithems, problem-solving strategies and the implementation of modern C++ features.
## Design & Test
At the core of this projects design is the Rain class which has a corrosponding header file and cpp file. The class is responsible for managing the individual raindrops which are made up of characters stored in a vector. It features functions that are responsible for managing how the raindrops fall. This is done by printing the vector using a for loop. The x position of the drop is fixed while the y postion in incremented by one. At the same time a blank space is printed at the top charachter to erase it from the terminal. This is done over and over for as many threads are created in the Raining() function. Each of these raindrops are called and allowed to run asyncronusly as a result of using threads. The Fall() function which is responsible for printing the charachters use a mutex as with it each of the rain objects will interupt eachother resulting in unexpected results.
The Rain class features multiple constructers and overloaded operators. It also features testing which is contained within the RainTest header and cpp files. In the main cpp file the tests are first called and displayed on the terminal for 3 seconds before the screen is cleared and the rainfall starts. To keep the rainfall continously going the Raining function is contained within a While(1) loop.
## Algorithm
To achieve the desired result for this project of simulating a visually pleasing matrix like rainfall effect multiple instances of the Rain class is created within a vector of threads in the Raining function. Each of these instances represent an individual raindrop. Each of these drops call the Fall() function which animates the drop going down the screen by iterativly printing each element on a fixed x position and a y position which is incremented as the loop goes on. This gets followed by a blank space which clears the charchter at the top to give a fallin effect. The GoToXY() fuction is used to manipulate and print in the specific position in the console.
## Problem-solving
As more and more instances of Rain where created and in turn printing to the console using the GoToXY() function there was an increase in characters being printed in unexpected positions. This was because they were all interupting each other and changing the x position before an instance could correctly print. I solved this issue by using a mutex just before and element is printed which meant that the other instances had to wait until the mutex was free before being able to use the locked function making it thread safe. 
## Modern C++


This is a paragraph. Add an empty line to start a new paragraph.

Font can be *Italic* or **Bold**.

Code can be highlighted with `backticks`.

Hyperlinks look like this: [GitHub Help](https://help.github.com/).

A bullet list:

- vectors
- algorithms
- iterators

You can add an impage that has been uploaded to the repository in a /docs/assets/images folder.

<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/code.png" width="400" height="300">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/code2.png" width="400" height="300">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/code3.png" width="400" height="300">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/Operation.png" width="400" height="300">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/RainHeader.png" width="400" height="300">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/C++Futures10000.png" width="400" height="20">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/C++Threads10000.png" width="400" height="30">



