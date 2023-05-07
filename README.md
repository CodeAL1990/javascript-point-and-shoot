# javascript-point-and-shoot

Add standard boilerplate for canvas drawing with a rainbow background(slightly different from your previous ones so look at video if you need reference)
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
Remove the temporary raven variable and create helper global variables timeToNextRaven set it to 0, ravenInterval set it 500(represents milliseconds), lastTime set it to 0 to hold value of timestamp from previous loop
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
Currently, you are pushing new Raven objects into the ravens array endlessly which will cause performance issues as time goes on
You will need to remove objects as the raven objects disappear from the canvas
Add a property in the constructor called markedForDeletion and set it to false
Add an if condition in update where if x is less than 0 minus the width of the raven sprite, markedForDeletion becomes true (meaning if the position of the sprite is completely beyond the canvas, delete it)
Use the filter method on the ravens array after the forEach methods in animate where if object's markForDeletion property is false, return true and add them, delete the rest(double negative technique)
Now that you have set up how your canvas works, it's time to remove the rectangular boxes and replace them with your desired sprites
Add image property in constructor and assign new Image() method, linking the source image after it
Just like in the sprite animation and movement type projects, create spriteWidth and height properties and assign their correct dimensions
Add a drawImage method in draw on your ctx with the first five parameters with image, x, y, width and height from the constructor
Change fillRect from draw to strokeRect to see that your image is being squeezed to fit the size of your hardcoded width and height
The parameters you used above in drawImage states where you want your image to be in the destination (dx, dy, dw, dh)
To crop out the sprites individually, you want to use the source parameters (sx, sy, sw, sh) to carve them out
sx and sy should be both 0 to indicate the top leftmost sprite which is the first sprite in your sprite sheet
Notice that your sprites are stretched on the boxes because width and height is being hardcoded and the sprite is stretched to fix them
Move spriteWidth and height to the top of the constructor so subsequent properties can use them in their calculations
Replace the values of width and height to the spriteWidth and height respectively and multiply them by 0.5(remember division takes more computation power and multiplication is more performance friendly) to decrease their aspect ratio
To randomise the sizes of the ravens, add a property called sizeModifier and randomise it from 0.4 to 1
Remember to put the property above width and height so they can use the property in their calculations
Add a property called frame and set it to 0 (used for calculating positions of sprite along the sprite sheet)
Add a property called maxframe and set it to 4(used for reset checks)
Add an if condition in update where if frame is more than maxFrame, set it back to 0, else increment frame
To move along the sprite sheet and apply the different sprite pose at each specific frame, replace the 0 value of sx in drawImage to the frame multiplied by the spriteWidth
We want to achieve identical frame rates across different pc strengths so we will pass deltaTime in the update method in animate and also the original update method you created in Raven class
You will now add helper properties to achieve the above
Add a property called timeSinceFlap and set it to 0 in constructor
Add a property called flapInterval and set it to 100
Increment timeSinceFlap in update by deltaTime after markedForDeletion if condition
Add an if condition where if timeSinceFlap is more than flapInterval, invoke the if condition of frame more than maxFrame you have written before (cut and paste it into the if condition you just created)
Also, set timeSinceFlap back to 0 after the frame condition is ran
The above if condition will make sure that only when timeSinceFlap reaches 100 in deltaTime which is calculated differently in each computer due to different computer power, invoke the frame condition. This will allow all computers to achieve identical frame rates
The flapInterval is the bottleneck in this update method as higher values means slower intervals which is better for slower computers and vice versa
You can have the ravens flap at different speed by replacing the hardcoded 100 into a randomised number between any number depending on your computation power to another number
Currently, ravens are flying in a straight line which makes your point and shoot game very easy
You can make use of directionY property in the update method and increment y by directionY to move them vertically up or down as well
We want the ravens to bounce of the top and bottom canvas to the left where it 'ends' instead of disappearing upwards or downwards of the canvas
To achieve this, add an if condition where if y is less than 0 (up) or y is more than canvas.height minus this.height(down), set directionY to its opposite value of directionY multiply by negative 1
Now we will create score variable to calculate score
Initialize score variable and set it to 0
Create a function called drawScore and apply white to its text with fillStyle method and write a basic score text to display score with fillText method using score variable, and 50,75 as the coordinates in the parameters
Call drawScore before the forEach loop in animate because you want the score to be behind the raven objects you are creating
The font is very small currently so use the global font method on ctx (ctx.font) and set it to '50px Impact' for now
To create a shadow on the font you can add a second fillStyle and a second fillText methods in drawScore at coordinates 55 and 80. Set the first fillStyle to black and second to white.
Create a click event listener for window and pass the event argument to the function
For now, console.log the event's coordinates of xy
We will now try to create collision detection on where you click in the canvas on the different colors you have set up in the canvas
Initialize a detectPixelColor variable and use the built in getImageData method on ctx with e.x, e.y, 1, 1 as arguments
You will get error on local server(if you open it with your computer as host) due to virus protection purposes. If you open with live server extension(with vscode), you will get the imageData
For now, we will create a second canvas just for hitbox collision
Create a second canvas on top of your original canvas in html with an id of collisionCanvas
Comment out the rainbow background to get more visual clarity
Set up the new canvas just like your original canvas in js (canvas, ctx, width and height)
Check with your css file by giving it a background color to see if it appears in your browser
Add a property called randomColors and assign an array of 3 array items randomised between 0 to 255 for all 3 items, wrapping each of them in Math.floor to get whole numbers only
Add color property to constructor and concatenate all three array items above with rgb colors (rgb(array0, array1, array2)). Video uses concatenation with normal "" while i used template literals ``
Change strokeRect to fillRect in draw and add a fillstyle method on canvas and assign color property to it
You should now see different background colors for each sprite
Currently all raven objects are pushed with the draw method in animate regardless of sizes and we want to make sure big ravens go front small ravens behind
To achieve this, use the sort method on ravens array after push and pass a,b as arguments to the function
Return the width of a argument minus the width of b argument to get ascending order (vice versa for descending)
We will now try to prevent the error when using local server due to cross 'contamination' of data
Use the collisionCtx you have created before and replace the ctx of fillStyle and fillRect in draw
Just like ctx in animate, you need to clear the previous drawn frames of collisionCtx in animate as well
getImageData is scanning canvas for pixel data
Currently you are scanning the main canvas(canvas1) for pixel data, scan collisionCtx instead
Using inspect in the browser, you can see the data property when you click on the ImageData object
To capture this data property, create a constant variable called pc in the event listener and assign the property of this data to it
For now, we will check for collision between cursor and the raven objects, using rgb values
Do a loop on the ravens array using forEach and for each object, run the if condition where the array item on each object.randomColors match the color of their corresponding data property(i.e r to r, g to g, b to b), there is collision and so set markedForDeletion of the object to true
Also, increment score variable to show that user score a point
Set opacity of collision canvas in css to 0 to remove the border around it and uncomment the rainbow background to reinstate it
You should be able to click the raven or abit around the raven and make them disappear and score should increase for each click on the raven
Now we want to add sound effects when clicking on the ravens to give a feeling of 'shooting' the ravens, and to add an after effect of 'eliminating' the ravens
Create an explosions variable and assign an empty array
Create an Explosions class with x,y and size arguments because it will be dependent on the position and dimensions of the raven object
Add a property called image to the class an assign new Image() to it, sourcing the after effect image of your choice when clicking on the ravens
Add the properties of the sprite's width and height that you have sourced
x, y, and size will be assigned to itself(the arguments in this case) because the values will be extracted from outside the class
Add frame property and set it to 0
Add sound property and assign new Audio() to it, sourcing the sound effect you want when clicking on the ravens
Add property timeSinceLastFrame and set it to 0
Add property frameInterval and set it to 200
Add the update method on this class just like you did for Raven class and pass the deltaTime argument on it similarly
Increment timeSinceLastFrame by deltaTime in update
Add an if condition if timeSinceLastFrame is more than frameInterval, increment frame
Add an if condition at the start of update when frame becomes 0 is true, use the built in play method on sound
Now add a draw method with drawImage on the main canvas, not the collision canvas
Use the same parameters as Ravens(not technically the same as you are using it for the boom sprite sheet) EXCEPT for x, y, width and height(dx, dy, dw, dh) since you are using the object values from outside the class
x will still be x, same for y but dw and dh will be assigned to the size property you created in the constructor, which is linked to the argument in the class
Now we will need to trigger this explosion sfx somewhere in the event listener
Since we only want it to happen when a raven is 'eliminated', you should place it in the if condition for the rgb detection
Use the push method on the explosions array and pass new Explosion() to it
Remember we are using the values of x,y and width from the Raven class, so for the arguments in this push method in the forEach loop, pass the x,y and width to it
To update the explosions array, spread the explosions array in the array literals in animate function
Just like the ravens array, filter the explosions array the same way
Add markedForDeletion property since you do not have it in Explosion class and set it to false
Add an additional if condition in the timeSinceLastFrame if condition where after incrementing the frame, if frame is more than 5(the total boom sprite length), markedForDeletion is set to true
Currently when you play the game, if you change your frameInterval to a higher value like 500, notice that the explosions is slower but the ravens still move fast which makes it jarring since the explosions and ravens seem 'disconnected'
To fix this, set timeSinceLastFrame back to 0 after each frame increment in the update method in Explosion class
The alignment of the explosion with the raven seems abit disjointed(explosion places itself a little bit below the raven)
To fix this, offset sy of Explosion in drawImage by the size property divided by 4(or whatever number you deem visually aligned)
Now we shall create a game over scenario
Create gameOver global variable and set it to false
Add an if condition in Raven class under update where if x is less than 0 offsetted by the width of the raven(if raven passes through the canvas completely), gameOver will be set to true
To invoke the gameOver scenario, add an if condition in animate where if gameOver is false, requestAnimationFrame will run. If not, requestAnimationFrame will not run
Your game should stop now once a raven crosses the canvas completely
Now we will show a game over message once gameOver is true
Create a drawGameOver function
Add fillStyle of black to the canvas
Use the fillText method on the canvas and show the message game over message and concatenate the score to dispaly them. Dividing the width and the height of the canvas in the width and height arguments will showcase the message in the middle of the screen
To call the above function, add it as an else condition in the gameOver condition
The displayed message is not exactly centered as it is using the top and left side of the canvas to push the message by half of each to approximately the center
To fix this, add a textAlign built in method on the canvas and set it to center
To create a shadow like your score, add a second set of fillStyle with a contrasting color and offset the second set of fillText after the division of canvas.width and height respectively
\*\*Optional experimentation with array and class to solidify knowledge learned when creating the Raven and Explosion class
Create particles variable and give it an empty array
Create Particle class and pass the x, y, size, and color arguments to it
Add x, y, size, and color property and assign all of them to themselves
Add radius and maxRadius properties as the particles will be circular
radius will be a randomised number of size divided by 10
maxRadius will be a random number between 35 and 55
Add the markedForDeletion property and set it to false
Add a speedX property and give it a random number from 0.5 to 1.5 as the particle will be drifting to the right as the ravens move to the left
Create update method and in it, increment x by speedX
Increment radius by 0.2
If radius is more than maxRadius, markedForDeletion is set to true
Create a draw method and pass the beginPath method to the main canvas
Use the fillStyle method on the canvas and assign it the color property
Use the arc method on the canvas and pass the xy position, the radius, the start angle of 0 and an end angle of Math.PI multiplied by 2(which is basically a circle)
Use the fill method on the canvas
The particle class is complete and since you want the particles to follow the ravens as they move to the left, you want to invoke the condition in the raven class where the raven is 'moving'
In the double if condition, everytime the frame is incremented, push new Particle with the x,y,width, and color arguments of the Raven class to the particles array
Just like the other classes, spread the particles array in the array literals in animate
Do the same for filter
Move size in Particle class to the top for other properties to use it in their calculations
Add size divided by 2 to x property
Add size divided by 3 to y property
This will make particle appear behind the ravens
The order in which you spread also decides how you want javascript to layer your animations
Spreading particles at the start of the array literals before the other 2 classes will make it appear first then the rest
Change the increment of radius in update to higher values to get bigger particles and vice versa
Creating particles for all ravens will cause performance issues,
we will now create a property to randomise the particle sizes and what percentage of ravens will have it
Add a hasTrail property in Raven class and assign it a boolean condition where whenever the random number is more than 0.5(from 0 to 1), hasTrail is true(which means half the ravens will have a trail of particles)
Move the particles.push into a new if condition where if hasTrail is true, invoke the push method
We now want the particles to slowly disappear at the end of the trail
Add a globalAlpha property to canvas in the Particle class under draw method
Assign 1 minus the radius divided by max radius
When the radius is the same size as the max radius due to the incrementing radius in update, 1 minus 1 will cause globalAlpha to become 0 which is means opacity is 0 (1 is fully opaque)
Once the above is applied, globalAlpha will be applied to both your ravens and particles
To only target the particles, wrap the save and restore methods in the draw function at the start and the end respectively
save method will create a snapshot of canvas global settings while calling restore will revert the canvas to what they were before
\\Im using linux the particles aren't appearing will test it in windows to see if particles appear
\\Its the same for windows i will need to re-watch the video in the particles section
\\Using VM for linux so the rainbow background actually makes me lag so i disabled it
