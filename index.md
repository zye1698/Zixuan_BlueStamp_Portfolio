# MIMIC (Motion Imitated Manipulation via Intelligent Camera)
<!---Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails! -->

I built a robotic arm that follows a user’s hand movements using a depth camera, inverse kinematics, and a Raspberry Pi. The project combines mechanical assembly, servo electronics, networking, computer vision, and a live PyBullet simulation. The biggest challenges were generating enough power and torque, preventing delayed movement commands, and making the RealSense camera and high-current servos operate reliably.

<!--- You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site 
``` -->


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Zixuan Y | Henry M. Gunn High School | Electrical Engineering | Incoming Sophomore

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

<!---**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE -->

For my final milestone, I added camera-based gesture control to the physical arm. An Intel RealSense depth camera captures color and depth images, and MediaPipe identifies 21 joints on my hand. The program measures the wrist movement in three dimensions and maps it to the arm’s X, Y, and Z directions. Palm rotation controls the wrist, while the distance between my thumb and index finger opens or closes the claw. Making a fist pauses tracking so the arm holds its current target. The camera runs on my Mac while the robot controller runs on a Raspberry Pi. They communicate through an authenticated TCP connection over an SSH tunnel. Each camera packet has a sequence number, and the Pi keeps only the newest packet so delayed hand movements do not build up in a queue. IKPy converts each requested XYZ position into shoulder, arm1, arm2, and arm3 angles. The Pi then applies joint calibration and sends the resulting targets to a PCA9685 servo driver. To visualize this, I added a live PyBullet simulation beside the camera footage. The simulation displays the angles commanded by the Pi, making it easier to identify reversed joints and calibration errors before relying only on the physical mechanism. The digital arm is color-coded so each segment is easy to identify. Through this project, I learned about depth cameras, computer vision, inverse kinematics, URDF robot models, PWM servo control, joint calibration, motion smoothing, electrical power distribution, computer aided designs, 3d printing, and soldering. In the future, I would like to add a camera onto the arm itself so it would be able to grab an object and move it somewhere else without human input. 



# Second Milestone

<!---**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

-->
<iframe width="560" height="315" src="https://www.youtube.com/embed/fRJQXy2gdK8?si=11t69dIF9wAHzhi0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


<!--For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone -->

For my second milestone, I programmed inverse kinematics control using IKPy. I converted the arm’s CAD assembly into a URDF model which contains all the joints I set in Onshape. IKPy uses this model to calculate the joint angles needed for the end of the arm to reach a requested X, Y, and Z position. The main challenge during this milestone was getting enough torque from the motors to lift the arm. MG995 servos did not have enough torque to lift the joints which faced the most load. To fix this, I replaced 3 servos in high leverage positions with 2 DS3235SG servos and 1 MG996R Servo. My original 9V and 15V battery setups could not provide enough sustained current, so their voltage dropped when several high-torque servos moved under load. I replaced the battery setup with a regulated wall power supply that could deliver the required current at a safe servo voltage. Direct GPIO control caused the servos to jitter due to low current and timing instability. Upgrading to a PCA9685 16-channel driver fixed this by providing a dedicated high-current power rail and stable hardware PWM signals. This gave the motors much more reliable torque and prevented many of the stalls and resets I had been seeing.


# First Milestone
<!---
**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**
-->

<iframe width="560" height="315" src="https://www.youtube.com/embed/Pkb9nWDbD5U?si=JTCi9905RD_vcnpL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<!--- For your first milestone, describe what your project is and how you plan to build it. You can include:
- An explanation about the different components of your project and how they will all integrate together
- Technical progress you've made so far
- Challenges you're facing and solving in your future milestones
- What your plan is to complete your project -->

For my first milestone, I assembled the robotic arm and created the electrical system needed to control its six servos. I designed all the parts for my robot on Onshape which is an online CAD software. I then printed these parts using a 3d printer. I connected 6 MG995 servos to my raspberry pi using jumper wires. I then tested each motor independently to determine its channel, safe angle range, center offset, and direction. This was important because repeatedly commanding an incorrectly calibrated or mechanically blocked servo could cause twitching, excessive current draw, or damage. After verifying individual movement, I programmed a collapsed starting position that gave the joints enough range for future inverse kinematics control.


# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
