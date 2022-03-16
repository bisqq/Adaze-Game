# Adaze

I had this idea to make a maze game based on music. So the player would input a song of there choice, which then I would used to implement into an audio visualizer. To prevent anyone from cheating I made the exact cubes that were playing the frequencies rise as the player got closer to it. Although the plugin in Unreal Engine to this day is still in beta, I have looked around and seen some people package the game which works fine. So based on that I will be coming back to work on this project.

https://user-images.githubusercontent.com/29154540/158494663-492dd5f9-b0d6-4bdd-9d37-f71e014c1b51.mp4

The video below demonstrates the wall in action(test), I used a softer song compared to a hard hitting to demonstrate that it doesn't matter what song you use, you still can't cheat😆.

https://user-images.githubusercontent.com/29154540/158516133-e3123c30-c464-452a-9b7f-e6d2ca493f1e.mp4

## Mobile Version
In the mobile version I worked on implementing a gyroscope so you would have to move your phone in order to look around which completely eliminates the look(left) joystick. This small trick makes it just more immersive.

![](https://user-images.githubusercontent.com/29154540/158496664-3d57be78-b735-43d3-baa8-30f8872ff7f2.png)

## Audio Visualizer
The way this visualizer works is we have an `Audio Controller` that handles the audio input. We first create a function called `Set Indexes` to set the frequency and amplitude index into an array. 

![](https://user-images.githubusercontent.com/29154540/158520578-c988251d-d598-4445-9ece-48bfd89c7464.png)
![](https://user-images.githubusercontent.com/29154540/158520631-e98efd5f-37c7-49ef-a34a-97fed29f49cb.png)

After that we create another function called `Calculate Equalizer`, this takes each equalizer objects from the array and transform it.

![Screenshot 2022-03-15 221738](https://user-images.githubusercontent.com/29154540/158521647-e4a41c22-07f0-438d-805b-d35c06c40869.png)
![Screenshot 2022-03-15 221751](https://user-images.githubusercontent.com/29154540/158521651-c0edda11-6f0d-4858-8a84-9f7ab5e66f04.png)

The final function called `Sort Equalizer` sorts the `Equalizer Object Array` by checking the `Actor Tag`.

![](https://user-images.githubusercontent.com/29154540/158521495-4f0009ea-173d-4537-b122-1bc210be5f0f.png)

In the main graph on `Event Tick` we call the built in function `Calculate Frequency Spectrum` we input the sound and start the time. This outputs a `float` array where we then input `for each` spectrum of the `Equalizer Object Array`, we set some max and min height for the `Frequency Spectrum Array` and output it to `Set Actor Scale 3D`. 

![](https://user-images.githubusercontent.com/29154540/158522795-fcc67f4c-0460-4086-8f74-275c8327bd48.png)
![](https://user-images.githubusercontent.com/29154540/158522801-885ae75a-54c5-45e2-97fd-6a4e0e042ca8.png)

On Event BeginPlay we grab all instances of the `BP_Audio` Class and insert it into the `Equalizer Object Array`. The Class just has a cube with a collision box on it.

![](https://user-images.githubusercontent.com/29154540/158522691-9f2c5448-d5d8-499b-9f59-427e58e94fb8.png)

## Rising Wall
We create a collection with a `vector4`. Now in the material editor we call that collection as a parameter and mask out only the `RBG`, this is where we take in position. We then call in the `Actor Position` and set some parameters. Now we input all those into a `SphereMask`. This mask is used to caclulate if the player is in the radius, and if they are we adjust the `World Position Offset` in the output node.

![](https://user-images.githubusercontent.com/29154540/158523983-779c0350-f6f1-4338-8b95-5124998ae1be.png)

The emissive colour is pretty basic we just lerp between 2 colours using the `Time` and a sine node. We then plug that into the `emissive` output.

![](https://user-images.githubusercontent.com/29154540/158524100-dcf02d29-a537-44da-a366-34746f41cd8c.png)
![](https://user-images.githubusercontent.com/29154540/158524234-3b8a6e21-8932-4b2e-8104-5dc6a28267c5.png)

One last thing I might inlcude is the tron effect or a mix of the fill and tron.

![](https://user-images.githubusercontent.com/29154540/158524496-0791ea88-d2cb-41d0-a361-52453d2b1ede.png)
![](https://user-images.githubusercontent.com/29154540/158524713-aabd356d-d731-466f-8fab-8893b2874432.png)


