
**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images/detected_solidYellowLeft_multicolor.jpg "Detected lines"

---

### Reflection

###1. After thinking the task over I concluded that the solution lies somewhere around detecting proper lines on the image.
Of course the key algorithm here becomes Canny edges detection and Hough-lines transform. We learned it for greyscale images only, hence comes the
first step:

1. convert to greyscale. It was pretty easy, I just had to copy-paste some code from previous quizzes and adapt it.

2. Blur the image. As there are lots of artifacts that we're not going to deal with on the image, we had to suppress that
noise using Gaussian Blur. The parameters were copied from previous quizzes, but required some experimenting when the
pipeline was ready. Not a big deal.

3. Canny edge detection. John F. Canny did the job for us. Works awesome. Bunch of data in the output.

4. Next step is expected to be Hough-lines transform but here comes the creative part. Lines should be properly selected.
And to get rid of useless information it was a good place to introduce a viewport.

    I selected a trapeze centered with the camera and edges following the perspective of the straight road going forward.
    Actually I dislike this approach, as the road could turn sharply, car could go off road (to park for example) and in
    this case the whoel algorithm would miserably fail. However for the sake of the two videos in the task it worked..


5. Hough-lines transform. Finally get the lines.

6. Overlay the lines (being able to tweak color and thickness (used it for debugging) on the initial image.

![Left and right linei groups][image2]

That's all! One frame is set!

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 



###2. Identify potential shortcomings with your current pipeline


Well, up until the extra task I thought I was doing ok.
But when the roud bent a little more and there was a gap in flat surface and road markup I understood how brittle the
algorithm is. I know for US this is kind of exception but in third world countries with moderate climate
roads primarily consist of such gaps... So such approach will not work at all. As mentioned before car could slide off
the lane, turn on a crossing, a huge white truck could pass by.

###3. Suggest possible improvements to your pipeline

If we're are talking about guiding some semi-robotic vehicles on near-ideal roads with consistent markings what I would
think of next is some kind of memory. Currently there is no interconnection between adjacent frames whereas environment
cannot change significantly in 1/25 of a second. So gaps, pedestrian crossings could be handled by remembering kind of
"hey we were on a lane last three frames, right?". So there are required additional calculations that would implement a
gradient-like approach.

And of course, line separation and filtering should become dynamic. For example, actual lines detected from a previous
frame could influence the viewport to search for lanes on the next frame and so on...

But I find it so much beyond this project, because I can't imagine using this approach for real-life cars. A much more
robust technique is required, that would allow to detect objects (known and unknown) and their orientation/position
in 3 dimensions relative to the car