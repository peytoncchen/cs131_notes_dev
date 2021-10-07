---
title: Edge detection
keywords: Edge Detection Overview, Motivation, History of Edges, Image Gradients, Gradients in 1D, 2D discrete Gradients, Simple Edge Detector, Smoothing Kernels, Good Edge Detector Design, Sobel Edge Dectector, Canny Edge Detector, Hough Transform for Line Detection
order: 4 # Lecture number for 2020
---

Peyton Chen, Michelle Xing, Sotirios Papasotiriou, Edgar Roman, Jay Saleh, Alex Oseguera

# 4.1 Overview of Edge Detection

### What are edges? 

We can think of edges as significant local changes in an image. Edges often occur on the boundary between two different regions in an image, such as a region with shadowing and a region without shadowing. Regardless of where they appear though, edges are an incredibely important part of image analysis. 

### How do we represent edges?

Edges are represented through normal lines as well as lines that are seen in drawings, paintings and pictures. With these edges, we are provided a way to communicate with each other as well as express ourselves culturally. Additionally, edges convey meaning about scenes and objects that allow us to navigate the world. 

<img src="{{ site.baseurl }}/assets/images/img1.png" width="600">

### Edges and Biology

Through many years of research, science has discovered that edges are a fundamental component of biology called visual systems. Much of what we understand about human vision comes from our understanding of how the brain interprets edges. 

One of the most famous studies is one done by Hubel and Wiesel in the 1950's and 1960's. In Hubel and Wiesel's study, they sought to answer the question, using electrophysiology: What is the visual processing system like in mammals? In order to solve this question they studied cat brains which were the most similar to human brains from the visual processing point of view. They placed eletrodes on the primary visual cortex of the cats and measured what stimuli made the the neurons respond excitedly. What they found was that there were many cells in the primary visual cortex but found that the most important ones were the simple cells which responded to oriented edges. In fact, they discovered that the cats' visual processing would start with those simple structures of the visual world, in other words, edges. 

Hubel and Wiesel Experiment Image:

<img src="{{ site.baseurl }}/assets/images/img2.png" width="600">

However, not all edges are equal in the human visual system. In a research study done by Biederman, researchers divided a drawing made only of edges into equal parts, so that each part was roughly 50 percent of the edges and the sum of its parts would make up the original drawing. Human subjects were then asked to name the objects from only one of the parts deleted complimentary images.
It turns out the ability to name was very dependent on which half was shown to them

<img src="{{ site.baseurl }}/assets/images/img3.png" width="600">

In yet another study in neuroscience, people were placed in FMRI scanners and shown natural images of scenes. They would then capture the brain responses of the subjects from seeing these images and feed the response to an algorithm that would take the brain activity and guess what type of image the subjects were looking at. Then, they took those same images but instead generated the image with only edges (i.e removing color and other detail), showed them to the subjects and once again had the algorithm guess which image the subjects were looking at. They found that the algorithm was able to guess just as well using edge images as it was with fully detailed images, showing the brains affinity for edge detection.

<img src="{{ site.baseurl }}/assets/images/img4.png" width="600">

### What is the goal of edge detection? 

The goal of edge detection is to take images and identify sudden changes within them. The reason we want this is that, inuitively, much of semantic and shape information that we get from an image can be encoded in just the edges. Additionally, being able to represent image content with only edges might be more compact than using the raw pixels of an image which is desirable. 

### Why do we care about edges from the Computer Vision Perspective? 

With edges we can extract a lot of information about the world such as recognizing objects. Additionally, edges allow us to recover geometry and viewpoints in an image. 

### Origin of edges

Edges in images are normally produced by discontinuities found in images. Some of these discontinuities are:

Surface normal discontinuity:  
- This happens when two surfaces that have different normal vectors intersect. In this case we see two surfaces that are perpendicular to each other and therefore they form an edge

<img src="{{ site.baseurl }}/assets/images/img5.png" width="600">

Depth discontinuity:  
- In this case, one region in the image has a much different depth than the other 

<img src="{{ site.baseurl }}/assets/images/img6.png" width="600">

Surface Color discontinuity:  
- In this case we see that some color of the surface is different from other colors on the surface

<img src="{{ site.baseurl }}/assets/images/img7.png" width="600">

Illumination Discontinuity:  
- In this case there is a shadow which is caused by differences in illumination. 

<img src="{{ site.baseurl }}/assets/images/img8.png" width="600">


# 4.2 Image Gradient 
Image gradients will be a tool to help us compute edges in an image. 
### Derivatives in One Dimension 
The derivative of a function \\(f(x)\\) computes the rate of change of the function. 

\\[\frac{\partial f}{\partial x} = \lim_{\Delta x \to 0} \frac{f(x) - f(\Delta x - x)}{\Delta x} = f'(x)\\]

Images are not continious functions, they are discrete. Therefore the minimum value that \\(\Delta x\\) can take is 1. The best we can do is to approximate the derivative for a discrete function. To do that, instead of taking the limit we calculate finite differences along one dimension of the function. 

\\[\frac{\partial f}{\partial x} =  \frac{f(x) - f(x-1)}{1} = f'(x)\\]

\\[\frac{\partial f}{\partial x} = f(x) - f(x-1) = f'(x)\\]

Therefore, the derivative of a discrete function \\(f\\) can be approximated as a finite difference between two consecutive values of the function. 

### Types of Discrete Derivatives in One Dimension
There are different ways that we can approximate the derivative of a one dimensional function \(f\). 

- Backward 

  \\[\frac{\partial f}{\partial x} = f(x) - f(x-1) = f'(x)\\]
  
- Forward

  \\[\frac{\partial f}{\partial x} = f(x+1) - f(x) = f'(x)\\]

- Central 

  \\[\frac{\partial f}{\partial x} = f(x+1) - f(x-1) = f'(x)\\]

We can also implement the derivatives as filters with convolutions.  
- Backward 

  [ 0 1 -1]
- Forward

  [ 1 -1 0]

- Central 

  [ 1 0 -1]

To compute the backward, forward or central derivative we use the corresponding kernel and perform convolution with the function. 

### Discrete Derivatives in Two Dimensions 
Since images are in two dimensions, it is important to understand how to compute discrete derivatives for two dimensiononal functions.

The derivative of a two dimensional function \\(f(x,y)\\) , called gradient vector, computes the rates of change along the x-dimension and along the y-dimension. 
- Gradient vector
\\[\nabla f(x,y)=\left[\begin{array}{c}
\dfrac{\partial f(x,y)}{\partial x}
\dfrac{\partial f(x,y)}{\partial y}
\end{array}\right] =\left[\begin{array}{c} f_x
f_y\end{array}\right]\\]

Since the gradient is a vector, we can compute how strong the derivative is by computing the gradient magnitude.

- Gradient magnitude 

\\[\nabla f(x,y) = \sqrt{f_x^2 + f_y^2}\\]


We can also compute the gradient orientation that tells us the direction along which the gradient is strongest. 
- Gradient direction

\\[\theta = \tan^{-1}(\frac{df}{dy}/\frac{df}{dx})\\]

### Two Dimensional Derivative Filters 

To compute the two dimensional gradients we use filters to perform convolutions. 

Performing convolution with the following filter gives us an approximation of the derivative along the ***x-direction***. 

$$\frac{1}{3} {\begin{bmatrix} 1 & 0 & -1 \\
1 & 0 & -1 \\
1 & 0 & -1\end{bmatrix} }$$

Performing convolution with the following filter gives us an approximation of the derivative along the ***y-direction***. 

$$[\frac{1}{3}{\begin{bmatrix} 1 & 1 & 1 \\
0 & 0 & 0 \\
-1 & -1 & -1\end{bmatrix} } $$

An example that includes perfoming convolution with both pre-mentioned filters can be seen below. 
<img src="{{ site.baseurl }}/assets/images/img9.png" width="600">

# 4.3 A Simple Edge Detector

### Characterizing Edges

**(Definition)** edge: a place of rapid change in the image intensity function 

<img src="{{ site.baseurl }}/assets/images/img10.jpg" width="600">

- the edge direction is given by the direction of the gradient vector \\(\theta = \tan^{-1}(\frac{df}{dy}/\frac{df}{dx})\\) since it points in the direction of the most rapid increase in intensity 
- the edge strength is given by the gradient magnitude \\(\sqrt{(\frac{df}{dy})^2 + (\frac{df}{dx})^2}\\)

**(Example)** detecting edges in a real image

<img src="{{ site.baseurl }}/assets/images/img11.jpg" width="600">

### Effects of Noise 
**(Question)** What if we have a noisy image? 
<img src="{{ site.baseurl }}/assets/images/img12.jpg" width="600">

- problem: discrete gradient filters respond strongly to noise 
 - in the image below, see that it's hard to determine where the extremas of the derivative/gradient function are, thus hard to locate where the edge is 
<img src="{{ site.baseurl }}/assets/images/img13.jpg" width="600">

- solution: smoothing filters 
 - how? filters force pixels to look more like neighbors, removing noise 
  - example filters
    - mean smoothing [ 1 1 1]
    - Gaussian smoothing [ 1 2 1 ]

### Using Smoothing and Gradient Filters For Edge Detection 

As shown below, to find edges, look for peaks in \\(\frac{d}{dx}(f * g)\\)
1. get your image function f 
2. get the kernel of the smoothing filter: g 
3. apply g to f to obtain convolution f * g 
4. take derivative of resulting convolution \\(\frac{d}{dx}(f * g)\\)

Alternatively, we can use a special theorem to save one operation: 

**(Theorem)** Derivative Theorem of Convolution 
\\[\frac{d}{dx}(f * g) = f *\frac{d}{dx} g \\]

1. get your image function f 
2. get the differentiated version of the kernel of the smoothing filter
3. apply this new kernel to f 

<img src="{{ site.baseurl }}/assets/images/img14.png" width="600">

**(Question)** What does the derivative of a smoothing filter look like? 

<img src="{{ site.baseurl }}/assets/images/img15.jpg" width="600">

- N.B. tradeoff between smoothing and localization: larger Gaussian kernel size removes more noise, but blurs edges 

<img src="{{ site.baseurl }}/assets/images/img16.jpg" width="600">

#### Designing an Edge Detector 
- good edge detection: minimize false positives and false negatives
- good localization: edges detected must be as close as possible to true edges 
- single response: detector returns one point for each true edge point 

# 4.4 Sobel Transform

### Overview

The sobel transform uses two 3x3 kernels which are convolved with the original image to calculate approximations of the derivatives of the image. The two kernels are one for horizontal changes and one for vertical changes respectively. 

<img src="{{ site.baseurl }}/assets/images/img17.png" width="600">

The matrices themselves are created from a combination of gaussian smoothing matrices with differentiation matrices in order to create the final 3x3 matrix. This is done in order to provide the smoothing neccessary to deal with noise in images. 

<img src="{{ site.baseurl }}/assets/images/img18.png" width="600">

### Sobel Transform Problems

Although Sobel Transform is a simple filter to implement it does have issues. For one, it has poor localization which means that the detector responds multiple times for a single true edge. Additionally, performing thresholding favors certain directions over others. Specifically, the detector tends to miss diagonal edges more than horizontal or vertical edges. Finally, it is more susceptible to false negatives.

<img src="{{ site.baseurl }}/assets/images/img19.png" width="600">

# 4.5 Canny Edge Detector

<img src="{{ site.baseurl }}/assets/images/img20.png" width="600">

(Final Canny Edges)

## Overview

- The Canny Edge Detector is probably the most widely used edge detector in Computer Vision.

- Theoretical Model: The Canny Edge Detector assumes that edges are step functions that have been corrupted by additive Gaussian noise.

<img src="{{ site.baseurl }}/assets/images/img21.png" width="600">

- Canny has shown that the first derivative of the Gaussian closely approximates the operator that optimizes the product of signal-to-noise ratio and localization

## Implementation

- Supress Noise & Compute Magnitude and Direction

<img src="{{ site.baseurl }}/assets/images/img22.png" width="600">

<img src="{{ site.baseurl }}/assets/images/img23.png" width="600">

<img src="{{ site.baseurl }}/assets/images/img24.png" width="600">

<img src="{{ site.baseurl }}/assets/images/img25.png" width="600">

- Apply Non-Maximum Supression
  - As we can see above, we have an issue of too many pixels with a strong response. We can fix it by thinning multi-pixel wide "ridges" down to a single pixel width.
  - Intuition: Edge occurs where gradient reaches a maxima
  - So, we supress non-maximum gradient regardless of if it passes threshold
  - We do so by comparing the current pixel to its neighbors along the direction of the gradient, removing it if it is not the maximum. 

<img src="{{ site.baseurl }}/assets/images/img26.png" width="600">

<img src="{{ site.baseurl }}/assets/images/img27.png" width="600">

  <b>Note:</b> We will only remove non-maximum gradients along the direction of the gradient vector and not along the edge itself.

### What if the gradient orientation does not directly point at a neighboring pixel?

- The solution: Interpolation
- Example
  - Calculate the intensity value of <i>r</i> and <i>p</i> by using their neighboring pixels
  - Perform non-maximum supression between the gradient value at <i>q</i> with the interpolated values obtained at points <i>r</i> and <i>p</i>

<img src="{{ site.baseurl }}/assets/images/img28.png" width="600">

- Use Hysteresis & Connectivity Analysis to Detect Edges
  - In order to produce an edge map, we need to use a threshold to identify the final edge pixels; However, using a single threshold is suboptimal.

  <img src="{{ site.baseurl }}/assets/images/img29.png" width="600">

  - The solution: Hysteresis Thresholding
    - Avoid streaking near threshold value
    - Define two thresholds: Low & High
      - If less than Low, then not an edge
      - If greater than High, then strong edge
      - If between Low and High, then weak Edge
        - Consider its neighbors iteratively. Then, declare it an “edge pixel” if it is connected to a “strong edge pixel” directly or via other “weak edge pixels”
    <img src="{{ site.baseurl }}/assets/images/img30.png" width="600">

## Additional Notes

### Effect of \\(\sigma\\) (Gaussian kernel spread/size)

<img src="{{ site.baseurl }}/assets/images/img31.png" width="600">

- Large \\(\sigma\\) detects large scale feautures
- Small \\(\sigma\\) detects fine features

### Measuring Algorithm's Performance

- In order to quantify the algorithm's performance, we can compare how well the output of the algorithm matches the desired output for a given image. For example, we may compare the output to an edge map drawn by a human.

<img src="{{ site.baseurl }}/assets/images/img32.png" width="600">


# 4.6 Hough Transform for Line Detection

### Why Line Detection and Hough Transform vs. Edge Detection
There are many objects that can be characterized by the presence of straight lines. For example, we may want to do an analysis on 3-dimensional shapes like buildings, or you need to do quality control in a manufacturing components. You may also need to navigate sidewalks and need to find out where the boundaries of the sidewalk are.

<img src="{{ site.baseurl }}/assets/images/img33.png" width="600">

This is _**not**_ what edge detection does. Edge detection can give us edge maps that can tell us which pixels belong to which boundaries. But, it does not tell us anything about the shape of those boundaries or what shapes those pixels form. This is where Hough transform comes in.

### Hough Transform history and overview
Hough Transform was developed in 1962 (Hough 1962) and was first used to find lines in images a decade later (Duda 1972).

The primary goal with using Hough Transform is to find the location of lines in images. However, it should be noted that the Hough transform can also detect circles and other circles as longas we can write down a parametric equation for those structures.

Important things to note for Hough transform is that it can give quite robust detection under noise and partial occlusion. 


### Prior to Hough transform
Let's establish our input. Assume that we have performed edge detection, for example, by thresholding the gradient magnitude image. Therefore, we have some pixels that may partially describe the boundary of some objects in an edge map. This edge map is a binary image where edge pixels are one and non-edge pixels are zero.

<img src="{{ site.baseurl }}/assets/images/img34.png" width="600">

### Naïve Line Detection
One simple algorithm for line detection would be to:
- For every pair of edge pixels
  - Compute the line of the equation that passes through both points
  - Check if there are other pixels that satisfy this equation
  - In some cases, we can see that these are the only 2 points, then we discard this line. In other cases like the image below, we see that there are in fact a lot of other points that satisfy the equation and so you can declare that this is a true line in the image.

<img src="{{ site.baseurl }}/assets/images/img35.png" width="600">
  - Complexity is O(N<sup>2</sup>) for an image with N edge pixels which is quite expensive for high-resolution images. Of course, we can do better than this.


### Detecting lines using Hough transform
- Transform the edge points into a new space:
  - Consider an edge point of known coordinates (x<sub>i</sub>, y<sub>i</sub>)
    - There are many _potential_ lines passing through this point (x<sub>i</sub>, y<sub>i</sub>) and these lines all have the form y<sub>i</sub> = a*x<sub>i</sub> + b.
    - When looking at this equation, we note that for a known point (x<sub>i</sub>, y<sub>i</sub>), these are constants, while a and b can change. This means we will want to interpret this as giving rise to a new space where (a, b) are the variables. 
    - When considering different points (a, b), we can see that all of these points are colinear. They are colinear because they have to satisfy the same equation we had in the beginning y<sub>i</sub> = a*x<sub>i</sub> + b. 
    <img src="{{ site.baseurl }}/assets/images/img36.png" width="600">
    - As seen in the image above, we can make this equation more explicit by taking our original point (x<sub>i</sub>, y<sub>i</sub>) and transforming it into a line in the (a, b) space and the equation of this line will be b = -a*x<sub></sub>i + y<sub>i</sub>.
  - Adding another edge point (x<sub>2</sub>, y<sub>2</sub>) in our original space will cause another line to show up in the (a, b) space
    <img src="{{ site.baseurl }}/assets/images/img37.png" width="600">
    - This will cause both of these lines to intersect in the (a, b) space at a point (a', b')
    - Futhermore, this causes a line (y = a'*x + b') to a appear between (x<sub>i</sub>, y<sub>i</sub>) and (x<sub>2</sub>, y<sub>2</sub>)
    - Next, we would need to find the intersection points in the (a, b) space
by quantizing the lines
    <img src="{{ site.baseurl }}/assets/images/img38.png" width="600">
    - For every box in the quantized space, we add a one for he count of the number of lines that cross that space.
    - Each cell that have more counts than the number of threshold and use the paramters of that cell to make the correspoding (a, b) line

- Other Hough transformations
  - We can represent lines as polar coordiantes instead of linear spaces (-x*cosΘ + y*sinΘ = ρ)
    - A vertical line will have Θ = 90 and ρ equal to the intercept with the x-axis
    - A horizontal line will have Θ = 0 and ρ equal to the intercept with the y-axis

     
