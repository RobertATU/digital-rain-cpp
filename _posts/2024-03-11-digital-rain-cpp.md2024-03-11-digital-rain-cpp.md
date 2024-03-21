
## Introduction
In this report, I will detail the design, implementation and testing of a digital rainfall matrix project using modern C++ libraries. The aim of the project is to create an aesthetically pleasing visual effect resembling rainfall similar to what is seen in the Matrix movies within the visual studio terminal. 

In order to complete this project I had to explore different design decisions, algorithms, problem-solving strategies and the implementation of modern C++ features. The primary source of information of all these modern C++ features came from [cppreference.com](https://en.cppreference.com/w/)[1]

## Design & Test
At the core of this projects design is the Rain class which has a corresponding header file and cpp file. The class is responsible for managing the individual raindrops which are made up of characters stored in a vector. It features functions that are responsible for managing how the raindrops fall. This is elaborated on in the Algorithm section of this report. This algorithm to give the vector of characters an animated apperance is executed over and over for as many threads are created in the Raining() function until eventually the threads are joined at the end. The Raining() function was originally contained within the Rain class however since it creates multiple rain instances within it I created a seperate Cloud class to call it.

<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/RainHeader.png" width="400" height="300">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/RainClass.png" width="400" height="300">
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/Cloud.png" width="400" height="300">

Each of these instances are allowed to call their Fall() function asynchronously as a result of creating a vector of threads eac threads. 
When deciding how I would achieve this as asynchronous functionality I explored using both threads and futures. From my research I found that they where similiar in functionality. Futures can create threads when needed however it will not always do so, while using the threads library gives more explicit control over the creation of threads. From my testing with both using the task manager I found using threads better for resource usage. Memory was the resource effected by the increase of the number of threads and futures. Below are the results of creating large amounts of threads and then the same number of futures. As can be seen the threads seem to have less overhead as they use far less memory. This is why I decided to threads would suit the project better.

10,000 Threads:

<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/C++Threads10000.png" width="400" height="30">

10,000 Futures:

<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/C++Futures10000.png" width="400" height="20">


The Fall() function which is responsible for printing the characters use a mutex as without it each of the rain instances will interrupt each other resulting in unexpected results. The Rain class features multiple constructers and overloaded operators. It also features testing which is contained within the RainTest header and cpp files. In the main cpp file the tests are first called and displayed on the terminal for 3 seconds before the screen is cleared and the rainfall starts. To achieve this an instance of the cloud class is created and the Raining() function is called. To keep the rainfall continuously going the call to the Raining() function is contained within a While(1) loop.

<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/Main.png" width="400" height="300">

## Algorithm
To achieve the desired result for this project of simulating a visually pleasing matrix like rainfall effect multiple instances of the Rain class are created within a vector of threads in the Raining() function. Each of these instances represent an individual raindrop. Each of these drops call the Fall() function which animates the drop going down the screen by iteratively printing each element on a fixed x position and a y position which is incremented as the loop goes on. This gets followed by a blank space which clears the character at the top to give a falling effect. The GoToXY() function is used to manipulate and print in the specific position in the console. The x position of the instance which stays completley fixed is determined by a random number generator which uses time since epoch in its random engine to decide on an x position between 0 and 119 to asign to the instance. Below is an example of multipe instances of this algorithm operating simultaniously
<img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/Operation.png" width="400" height="300">

## Problem-solving
Throughout this projects I faced a number of issues. The first of which came from my initial design of creating an instance of the Rain object. Since the drop had to be continuously printing its own vector followed by blank spaces, only one drop at a time could be shown. To get around this i looked into asynchronus techniques, threads and futures. As more and more instances of Rain where created and in turn printing to the console using the GoToXY() function there was an increase in characters being printed in unexpected positions. This was because they were all interrupting each other and changing the x position before an instance could correctly print. I solved this issue by using a mutex just before and element is printed which meant that the other instances had to wait until the mutex was free before being able to use the locked function making it thread safe. I also had an issue when the rain drop would reach the bottom it would print along the x axis. This was caused by the variable I had set in the Fall() function for the screen height of the console, I had initially tested it with five character vectors and when I increased the it to ten I had to readjust the variable.


## Modern C++
This projects was designed with an abundance of modern C++ features and library to enchane functionality,efficency and maintainabilty. This features include:

- Object-Oriented design - The project being designed with object-oriented principles allowed for easy reusibility and maintainability of the Rain class and all its functionality

- Operator overloading -Using operator overloading on operators such as operator+ allows for manipulation of its functionality for the Rain class. In this project + is overloaded to join the two instances Drop vectors together 

- Testing - The RainTest.h and RainTest.cpp files contain test of the various constructors of the Rain class

- Random number generation using the <random> library - This utilizes a modern random number engine in order to provide better randomness and distribution for the Rain instances x position than that of legacy methods

  <img src="https://raw.githubusercontent.com/robertatu/digital-rain-cpp/main/doc/assets/random number.png" width="450" height="100">

- Concurency using threads - The implementation of threads allow multiple raindrops to fall simultaniously

- Concurency protection using mutexes - The use of mutexs keeps multiple instances from accessing the same resources at the same time therefore preventing unexpected results. In this project the mutex protects the GoToXY function, this ensures that the characters are printed in the correct positions

## Modern C++

[1] [Online]. cppreference.com. Available: [cppreference.com](https://en.cppreference.com/w/). Accessed: March 21, 2024.






