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
