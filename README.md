Download Link: https://assignmentchef.com/product/solved-hlcv-exercise-1-image-filtering-and-object-identification
<br>
In this exercise you will first familiarise yourself with basic image filtering routines. In the second part, you will develop a simple image querying system which accepts a query image as input and then finds a set of similar images in the database. In order to compare images you will implement some simple histogram based distance functions and evaluate their performance in combination with different image representations.

<h1>Question 1: Image Filtering</h1>

<ol start="2">

 <li>Implement a method which computes the values of a 1-D Gaussian for a given variance <em>σ</em><sup>2</sup>. The method should also give a vector of values on which the Gaussian filter is defined: integer values on the interval [−3<em>σ,</em>3<em>σ</em>].</li>

</ol>

<em>.                                           </em>(1)

Open the file gauss module.py with your preferred editor and begin the script:

def gauss(sigma):

… … return G,x

<ol start="2">

 <li>Implement a 2D Gaussian filter in gauss module.py. The function should take an image as an input and return the result of the convolution of this image a with 2D Gaussian kernel of given variance <em>σ</em><sup>2</sup>. See Fig. 1 for an illustration of Gaussian filtering. You can take advantage of the convolve2d function from the scipy library if you don’t want to implement convolution yourself.</li>

</ol>

Open the file called gauss module.py with your preferred editor and begin the script:

def gaussianfilter(img,sigma):

… …

return outimg

<em>Hint: use the fact that the 2D Gaussian filter is separable to speed up computations.</em>

<ol>

 <li>Implement a function gaussdx for creating a Gaussian derivative filter in 1D according to the following equation</li>

</ol>

) (2) )         (3)

The effect of applying a filter can be studied by observing its so-called <em>impulse response</em>. For this, create a test image in which only the central pixel has a non-zero value:

(a)                                                                                                                (b)

Figure 1: (a) Original image (b) Image after applying a Gaussian filter with <em>σ </em>= 4<em>.</em>0.

imgImp = np.zeros((27,27)) imgImp[14,14] = 1.0

Now, create the following 1D filter kernels G and D.

sigma = 5.0 G = gauss(sigma)

D = gaussdx(sigma)

What happens when you apply the following filter combinations?

<ol>

 <li>first <em>G</em>, then <em>G<sup>T</sup></em>.</li>

 <li>first <em>G</em>, then <em>D<sup>T</sup></em>.</li>

 <li>first <em>D</em>, then <em>G<sup>T</sup></em>.</li>

 <li>first <em>G<sup>T</sup></em>, then <em>D</em>.</li>

 <li>first <em>D<sup>T</sup></em>, then <em>G</em>. where <em>G<sup>T </sup></em>refers to the transpose of vector <em>G</em>. Visualize the results and put them in your report.</li>

</ol>

<ol>

 <li>Use the functions gauss and gaussdx in order to implement a function gaussderiv that returns the 2D Gaussian derivatives of an input image in <em>x </em>and <em>y </em> Try the function on the three test images and comment on the output. Visualize the results and put them in your report.</li>

</ol>

<h1>Question 2: Image Representations, Histogram Distances</h1>

<ol>

 <li>Implement a function normalized histogram, which takes a gray-value image as input and returns a normalized histogram of pixel intensities. Compare you implementation with the built-in Python function histogram. Your histograms and the histograms computed with Python should be approximately the same, except for the overall scale, which will be different since numpy.histogram does not normalize its output.</li>

 <li>Implement other histogram types discussed during the tutorial (refer intro slides). Your implementationshould extend the code provided in the functions rgb histogram, rg histogram and dxdy histogram (in histogram module.py). Make sure that you are using the correct range of pixel values. For “RGB” the pixel intensities are in [0<em>,</em>255], for “rg” the values are normalized to be in [0<em>,</em>1]. For the derivatives histograms the values depend on the <em>σ</em><sup>2 </sup>of the Gaussian filter; with <em>σ </em>= 7<em>.</em>0 you can assume that the values are in the range [−32<em>,</em>32].</li>

 <li>Implement the histogram distance functions discussed during the tutorial (refer intro slides), by filling themissing code in the functions dist l2, dist intersect, and dist chi2 (in dist module.py).</li>

</ol>

<h1>Question 3: Object Identification</h1>

<ol>

 <li>Having implemented different distance functions and image histograms, we can now test how suitablethey are for retrieving images in a query-by-example scenario. Implement a function find best match (in match py), which takes a list of model images and a list of query images and for each query image returns the index of the closest model image. The function should take string parameters, which identify the distance function, the histogram function and the number of histogram bins. See the comments at the beginning of find best match for more details. Aside from the indices of the best matching images, your implementation should also return a matrix which contains the distances between all pairs of model and query images.</li>

</ol>

(a)                                                                           (b)

Figure 2: Query image (a) and the model images with color histograms similar to the query image (b).

Figure 3: Recall/precision curve evaluated on the provided set of model and query images.

<ol>

 <li>Implement a function show neighbors (in match module.py) which takes a list of model images and a list of query images and for each query image visualizes several model images which are the closest to the query image according to the specified distance metric. Use the function find best match in your implementation. See Fig 2 for an example output.</li>

 <li>Use the function find best match to compute the recognition rate for different combinations of distance and histogram functions. The recognition rate is given by the ratio between the number of correct matches and the total number of query images. Experiment with different functions and numbers of histogram bins and try to find the combination that works best. Submit the summary of your experiments as part of your solution.</li>

</ol>

<h1>Question 4: Performance Evaluation</h1>

<table>

 <tbody>

  <tr>

   <td width="295"></td>

  </tr>

  <tr>

   <td></td>

   <td></td>

  </tr>

 </tbody>

</table>

<ol>

 <li>a) Sometimes instead of returning the best match for a query image we would like to return all the model images with a distance to the query image below a certain threshold. It is, for example, the case when there are multiple images of the query object among the model images. In order to assess the system performance in such scenario we will use two quality measures: <em>precision </em>and <em>recall</em>. Denoting the threshold on the distance between the images by <em>τ </em>and using the following notation:</li>

</ol>

TP (True Positive) = number of correct matches among the images with distance <em>smaller </em>than <em>τ</em>,

FP (False Positive) = number of incorrect matches among the images with distance <em>smaller </em>than <em>τ</em>, TN (True Negative) = number of incorrect matches among the images with distance <em>larger </em>than <em>τ</em>,

FN (False Negative) = number of correct matches among the images with distance <em>larger </em>than <em>τ</em>, precision is given by

TP

<table width="620">

 <tbody>

  <tr>

   <td width="603">precision =  <em>, </em>TP + FPand recall is given by</td>

   <td width="17">(4)</td>

  </tr>

  <tr>

   <td width="603">TPrecall =  <em>.</em>TP + FN</td>

   <td width="17">(5)</td>

  </tr>

 </tbody>

</table>

For an ideal system there should exist a value of <em>τ </em>such that both precision and recall are equal to 1, which corresponds to obtaining all the correct images without any false matches. However, in reality both quantities will be somewhere in the range between 0 and 1 and the goal is to make both of them as high as possible.

Implement a function plot rpc defined in rpc module.py that you have to compute precision/recall pairs for a range of threshold values and then output the precision/recall curve (RPC), which gives a good summary of system performance at different levels of confidence. See Fig 3 for an example of RPC curve.

<ol>

 <li>b) Plot RPC curves for different histogram types, distances and number of bins. Submit a summary of your observations as part of your solution.</li>

</ol>