# **Finding Lane Lines on the Road** 

## Writeup 
---

[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_avg.jpg "Grayscale"
[image2]: ./test_images_output/solidYellowCurve_avg.jpg "Grayscale"
[image3]: ./test_images_output/challange.jpg "Grayscale" 

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. At first I used 6 steps, however, thanks to the challange I added a step improves the detection by ignoring edges that are not white nor yellow. First, I converted replaced all yellow pixels in the image to white, then that I have the pixels that interest me in white, I filter all other colors having a black and white image. Becuase this last image is in the RGB format I then convert it into grayscale. Then, in the fourth step, I smooth the image applying a Gaussian Noise kerne with cv2.GaussianBlur function. Later I apply Canny edge detector. After applying the edge detector, I define a polygonal region of interest and obtain an image that is masked by it. Finally, with the help of the Hough transform I find lines in the image and based on these and the use of the draw_lines() function the lane lines are detected and annotated on the input video. So, to summaryse, the steps taken where:

1. Replace yellow pixels for white ones. 
2. Filter the image to keep only white pixels.
3. Convert RGB image into grayscale. 
4. Smooth the image. 
5. Use canny edge detection.
6. Define a region of interest and mask the image accordlingly. 
7. Detect and get lines with Hough transform function. 
8. Use draw_lines() function to annotate the video. 


In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows. Worth noticing that I copyied the function and changed its name to separate_lines(). First, I divided the lines obtained with the Hough transform function cv2.HoughLinesP() into right and left ones based on their slope. Positive slope means that they are right lane lines and negative, left lane lines. Then I averaged the lines (starting and end point pairs) of each groups (right and left) and with the result I obtained the parameters of the line equation: slope and bias. With these parameters I was able to extrapolate the lane lines from a desired initial y0 coordinate to a final one yf. In order to stop the flicker resulting from only applying the previous step to get line annotations I used a moving average of the slope and bias to later extrapolate the line taking into account the information frome the 20 previous frames. Once the extrapolated lines are calculated they are drawn in the image. So, to summaryse, the steps taken to change the draw_lines() function where:

1. Divide right and left lines based on their slopes. 
2. Average the starting and end points of each line. 
3. Get a average slope and bias for each left and right lanes.
4. Get the moving average of the slope and bias from the last 20 frames. 
5. Extrapolate the lines with the slope and bias and the intersection with desired initial and final y coordinates. 

A sample of the result for each of the videos are shown bellow.  

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image3]
![alt text](./test_images_output/challange.jpg = 100x200)

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
