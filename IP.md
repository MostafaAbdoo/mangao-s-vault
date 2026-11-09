# Lecture 1 & 2
*Spatial resolution* is a physical property of the imaging system rather than the digital file itself
Pixel count and spatial resolution are **not** equivalent. An image matrix of $160 \times 160$ pixels covering a $10\,\text{m} \times 10\,\text{m}$ area provides significantly finer spatial resolution than the exact same $160 \times 160$ matrix stretched over an $80\,\text{m} \times 80\,\text{m}$ physical area.
**Image Negatives**: s = IntensityMax - r // IntensityMax = L - 1
**Log Transform**: s = c log(l+ r) // When the input grey level values may have an extremely large range of values.
**Power-Law (Gamma)**: $s = cr^y$  // y < 1.0 -> make it brighter and Visca versa
**Thresholding, Contrast Stretching, Intensity Level Slicing, Bit-plane Slicing**
يمين فاتح, شمال غامق
![[Pasted image 20261108082641.png]]
![[Pasted image 20261108083433.png]]
# Lecture 3
*Convolution* is used to linearly filter a signal, for example to smooth a spike train.
*Correlation* is used to characterize the statistical dependencies between two signals.

in *Convolution* we flip the filter, both works using sum of products and save the result at the mid of the table
*Averaging Filter or Low Pass Filter*
![[Pasted image 20261108085232.png]]
it can be weighted too, median filter is better for noise
## Sharpening Filters
First Derivative (Gradient) $\frac{\partial f}{\partial x} = f(x+1) - f(x)$
2nd derivative (Laplacian) = $\frac{\partial^2 f}{\partial^2 x} = f(x+1) + f(x-1) - 2f(x)$ 
The 2nd derivative is more useful for image enhancement than the 1st derivative
$$\nabla^2 f = [f(x+1, y) + f(x-1, y) + f(x, y+1) + f(x, y-1)] - 4f(x, y)$$
![[Pasted image 20261108090018.png]]
First Derivative (Gradient):
Approximating the partial derivative in the x-direction == third row – first row
Approximating the partial derivative in the y-direction == third column – first column
Perwitt masks -> 1,1,1   Sobel Operators -> 1,2,1  and the other two columns one same values but negative and the other zeros
above was for edge detection
to obtain the sharpened image: $= 5f(x, y) - f(x+1, y) - f(x-1, y) - f(x, y+1) - f(x, y-1)$
>1st order derivatives generally produce thicker edges.
1st order derivatives have stronger response to grey level step(Edge Detection)
2nd order derivatives produce a double response at step changes in grey level.
2nd order derivatives have a stronger response to fine detail (Sharpening)e.g. thin lines.

# Lecture 4
## **Edge-based Approaches**
1st derivative (Gradient), 2nd derivative (Laplacian).
- Isolated Points -> Laplacian + threshold            
- Lines -> Laplacian + deal with double-line effect.
2nd derivative (Laplacian) = $\frac{\partial^2 f}{\partial^2 x} = f(x+1) + f(x-1) - 2f(x)$ 
*Lines*:
Masks for specific direction + threshold the absolute value
![[Pasted image 20261108034248.png]]
**Marr-Hildreth Edge Detector**:
The Laplacian is too sensitive to noise, so it is combined with a smoothing Gaussian filter 
The Laplacian of a Gaussian (LoG): $\nabla^2 G(x, y) = \left[\frac{x^2 + y^2 - 2\sigma^2}{\sigma^4}\right] e^{-\frac{x^2 + y^2}{2\sigma^2}}$ 
**Canny Edge Detector**:
A good approximation to the optimal step edge detector and is based on the 1st derivative of a Gaussian
$$\frac{d}{dx} e^{-\frac{x^2}{2\sigma^2}} = \frac{-x}{\sigma^2} e^{-\frac{x^2}{2\sigma^2}}$$
## **Thresholding**:
When intensity distribution of objects and background are sufficiently distinct, a single values can be used to segment, otherwise u can use multiple thresholding values
![[Pasted image 20261108041351.png]]
![[Pasted image 20261108040447.png]]
**Basic Global Thresholding**
Automatic procedure:
– Select initial 𝑻.
– Segment using 𝑻 , 𝑮𝟏 and 𝑮𝟐.
– Compute means 𝒎𝟏 and 𝒎𝟐.
– Compute new threshold 𝑻 = ½(𝒎𝟏 + 𝒎𝟐).
– Repeat until the difference between successive 𝑻 is < ∆𝑻.
*Requires*
- PDF of intensity levels of each class.
- Probability that each class occurs in an application
*Drawback*
- only works for bimodal histograms
- Noise -> solution -> smoothing
- Non-uniform illumination or reflectance -> solution -> Variable thresholding
**Optimum Global Thresholding (Otsu)** 
Otsu’s method selects the threshold by minimizing the within-class variance of the two groups so that it maximize the between-class variance.
*Drawbacks*
- Assumes the histogram is bimodal.
- Fails when the two classes are very unequal (i.e., have very different sizes).
**Variable thresholding**
Subdivide image into non-overlapping rectangles. Estimate a different threshold to segment each sub-image.
*Local Image Properties*: Threshold according to the mean and standard deviation of a neighborhood.
*Moving Average*: Scanning is carried out line by line in zigzag pattern to reduce illumination bias. Works well when the objects of interest are small (or thin) with respect to the image size, a condition satisfied in images of handwritten text.
## Hough Transform
![[Pasted image 20261108092154.png]]
![[Pasted image 20261108092209.png]]

# Lecture 5
## Region-based Segmentation
Partition image region 𝑹 into subregions such that:
a) $\bigcup_{i=1}^{n} R_i = R$
b) $R_i$ is a connected region.
c) $R_i \cap R_j = \phi$
d) $P(R_i) = True$ (P is a property)
e) $P(R_i \cup R_j) = False$
**Region Growing** (Like clustering)
- Group based a predefined criteria of growth.
- Start with a set of seed points.
- Append to each seed neighboring pixels that satisfy the criteria (have properties similar to the seed).
choice of seed: Without knowledge of the target, compute the histogram and choose the gray-level values corresponding to the strongest peaks.
**Region Merging**
(1) Form initial regions in the image.
(2) Build a regions adjacency graph (RAG).
(3) For each region do:
	(3.1) Consider its adjacent region and test to see if they are similar.
	(3.2) For regions that are similar (i.e., $P(R_i \cup R_j)=True$), merge them and modify the RAG.
(4) Repeat step 3 until no regions are merged.
**Region Splitting**
(1) If $P(R)=\text{False}$, split $R$ into four quadrants
(2) If $P$ is false on any quadrant, sub split
**Region Splitting and Merging**
- Subdivide image initially into a set of arbitrary disjoint regions.
- Then merge and/or split the regions to satisfy the segmentation conditions.
- 𝑃(𝑅𝑖) = TRUE if at least 80% of the pixels in 𝑅𝑖 have the property
## Motion detection
**Optical Flow**: Technique that measures how objects move between consecutive frames based on changes in pixel intensity.
**Background Subtraction**: Uses a reference background image for comparison purposes.
## Mathematical morphology
used in image analysis based on shape. Can be used to remove imperfections
Used to extract image components that are useful in the representation and description of regions shapes
**Bi-level versus Grey-scale Morphology**
- Binary morphological applied to bi-level images (foreground vs background pixels).
- Grey-scale operations are applied in the same manner but to grey-scale images with each pixel represented in the 3D integer space Z2 by vector (x,y,g)

Structuring Element – Hits, Fits and Miss
**Erosion**
$X ⊖ SE$
$g(x, y) = \begin{cases} 1 \text{ if } SE \text{ fits } X \\ 0 \text{ otherwise} \end{cases}$ 
- Erosion can split apart joined objects
- Erosion can strip away extrusions
- Erosion shrinks objects
**Dilation**
$X \oplus SE$
$g(x, y) = \begin{cases} 1 \text{ if } SE \text{ hits } X \\ 0 \text{ otherwise} \end{cases}$
- Dilation can repair breaks
- Dilation can repair intrusions
- Dilation enlarges objects!
**Opening**
erosion followed by a dilation $X \circ SE = (X \ominus SE) \oplus SE$
Opening smoothes the contour of objects, breaks narrow bridges, and eliminates thin protrusions.
**Closing**
dilation followed by an erosion $X \bullet SE = (X \oplus SE) \ominus SE$
Closing smoothes sections of contours as well, but fuses narrow breaks, eliminate small holes, and fill gaps.
![[Pasted image 20261108074825.png]]
