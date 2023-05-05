# javascript-point-and-shoot

Add standard boilerplate for canvas drawing(slightly different from your previous ones so look at video if you need reference)
Create a ravens variable and assign an empty array;
Create a Raven class with a constructor
Add a width and height property with fixed values of your choice for now to the constructor
x will be canvas.width which will allow Raven to 'fly' across the full width of the canvas
y will be a random value from 0 to canvas.height so the Raven object can spawn along at any point along the height of the canvas
For y, you should also offset the value by the height of the Raven object so they do not get clipped or 'fly' outside the canvas
Remember to put a parenthesis on the canvas.height and the offset
Add a directionX property which represents horizontal speed randomised between 3 and 8 as a starting point
Add a directionY property which represents vertical movement and randomise it between -2.5 and 2.5 (remember negative means up and positive means down for vertical, while for horizontal it means left and right respectively)
Add an update method after constructor and for now decrement x by directionX for now to move the Raven object created by update to the left
Add a draw method after update and use fillRect to draw out a simple rectangle first as a test
Create animate function after Raven class with a parameter of timestamp
As usual at a clearRect at the beginning to clear the previous frame when adding new frames continually
Add a requestAnimationFrame method on animate and call it at the end
Test out the Raven class by creating a temporary raven variable and assigning new Raven() which will have access to the properties and all the methods you have in Raven class
Use the methods in the constructor on raven in animate to test it out
You should now see a rectangular 'raven' moving horizontally to the left in your browser
To get the raven to move across the screen regardless of your pc specs, you would compare time lapses between frames and draw them according to certain timestamps
For starters create helper global variables timeToNextRaven set it to 0, ravenInterval set it 500(represents milliseconds), lastTime set it to 0 to hold value of timestamp from previous loop
Create deltaTime variable in animate and assign it timestamp minus the lastTime value
Assign lastTime to timestamp to save the value for comparison in the next loop
Increment timeToNextRaven by deltaTime
Add an if condition where if timeToNextRaven is more than ravenInterval, push a Raven class into ravens array, and set timeToNextRaven back to 0
console.log timeToNextRaven will show you NaN and further console.logging of the different variables will show that the first value called is undefined because animate() function does not have a value
To fix it, add a 0 to animate() in its argument
Now you should see the raven class added to the array every 500 milliseconds
Now remove the console.logs and add an array literal after the if condition, spreading the ravens array in it and calling the forEach method on it
For each object in the ravens array, call the update method
Do the same for the draw method
//TBC
