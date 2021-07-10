---
title: "Quantifying Interparticle Spacing"
excerpt: "Measuring the distances between 'direct' neighbors of randomly positioned particles.   <br/> 
<img src='/images/interparticle_images/Figure 1; Darkfield Image of NPs png.png' width=500>"
collection: portfolio
---




## Motivation:

My Ph.D lab is well-known for making nanoelectrode arrays used for chemical imaging. A fellow co-worker created a new design where single nanoparticle electrodes are dispersed in a polymer film. Here the particle size is easily varied from 10 - 500 nm. However, the spatial resolution of future imaging measurements is determined by particle spacing – which is effectively random for this process! We therefore wanted to quantify the average spacing between a single NP and its neighbors.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/interparticle_images/Figure 1; Darkfield Image of NPs png.png" alt="Darkfield Image of NPs" width="500" height="1000">
</p>

<p align="center">

Figure 1: Darkfield Image of Nanoparticles

</p>


Figure 1 shows an example microscope image of the array, where each white dot is a separate nanoparticle. My advisor initially suggested that my co-worker should _manually_ measure distances (one by one) using a ruler.  But there are hundreds of particles per image, and dozens of different conditions to quantify! I quickly volunteered to programmatically automate the process, saving my co-worker from countless hours of tedium, and arguably boosting the measured accuracy.

&nbsp;

## Outline:



The goal is to quantify the inter-particle spacing for directly neighboring particles.




**Process:**

 1. Obtain (x,y) coordinates for each individual particle.

 2. For every point, identify its direct neighbors.

 3. Measure the Euclidean distance for neighboring particles.


&nbsp;

## 1. Identifying Coordinates for Each Particle:


Coordinate extraction was relatively simple. I used an algorithm called "Laplacian of the Gaussian" (LOG), which is a spatial filter that finds bright regions of an image with a dark background. The Scikit-Image implementation extracts the coordinates of each point, and their approximate size.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/interparticle_images/Figure 2; LOG Applied to Data.png" alt="Laplacian of Gaussian" width="800" >
</p>

<p align="center">

Figure 2: Nanoparticle Coordinates Identified using LOG

</p>


Figure 2 shows the LOG method applied to our nanoparticle image. The original image is displayed on the left, and the yellow circles show the identified points on the right. The large circles demonstrate that closely packed particles could not be resolved using this method and are instead identified as a single unit. Considering that there are less than 20 large circles and over 1000 particles total, I concluded that this was acceptable error.



## 2. Determining Direct Neighbors from a List of Coordinates:

Direct neighbors are particles that are immediately adjacent on any side.  For example, Figure 3 shows a central particle in red, and its direct neighbors labeled in yellow. Notice that direct neighbors are not the closest n points! If particles are positioned behind others, they are considered non-adjacent – for example to the bottom left of the red dot. Therefore, simply selecting the nearest _n_ neighbors based on distance would not be an accurate method!

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/interparticle_images/Figure 3 Cropped.jpg" alt="Example of Direct Neighbors"
width=220> 
</p>


<p align="center">

Figure 3: Example of 'Direct' Neighbors

</p>


After relentless searching, I found a method called "Delaunay Tessellation", which draws triangles between all points while maximizing the vertex angles.  Figure 4 below shows the result of the tessellation on the points previously identified.

<p align="center">

<img src="{{ site.url }}{{ site.baseurl }}/images/interparticle_images/New Figure 4.png" alt="Delaunay Tessellation"
width=600>

</p>


<p align="center">

Figure 4: Visualization of Delaunay Tessellation 

</p>


The SciPy implementation of Delaunay Tessellation outputs all connected vertices, and with a little logic, we can easily identify the points that are direct neighbors! Figure 5 below shows 2 more examples demonstrating the effectiveness of this method.

<p align="center">

<img src="{{ site.url }}{{ site.baseurl }}/images/interparticle_images/New Figure 5.png" alt="Example of Neighbor ID 2" width=800>
</p>

<p align="center">

Figure 5: Two Examples of a Central Nanoparticle (Red) with its Direct Neighbors (Yellow)  

</p>


## 3. Measuring the Distance Between Neighbors:

The last step is to find the Euclidean distance between each neighboring point, where only the distances from the yellow to red dots are considered. Figure 6 displays the results in an annotated histogram.

<p align="center">

<img src="{{ site.url }}{{ site.baseurl }}/images/interparticle_images/Figure 6; Annotated KDE.png" alt="Annotated KDE"
width=700>

</p>

<p align="center">

Figure 6: Summarized Results for Total Interparticle Spacing

</p>


&nbsp;

## Conclusion:

Overall, this workflow serves as a quick and reliable method to characterize object spacing in an image. The runtime for the example shown here was less than 30 seconds, though increases by O(N<sup>2</sup>), where N is the # of particles present.

Needless to say, my co-worker was overjoyed when I showed him the results – as it saved him _countless hours_ of tedium if he had performed manual measurements!


---


**For a walkthrough of the code, please view the following hosted Jupyter [Notebook](https://mybinder.org/v2/gh/PeterDefnet/Measurement-of-Interparticle-Distance/main?filepath=Notebooks/Interparticle%20Distance%20of%20Direct%20Neighbors.ipynb), or view it on [Github](https://github.com/PeterDefnet/Measurement-of-Interparticle-Distance/blob/main/Notebooks/Interparticle%20Distance%20of%20Direct%20Neighbors.ipynb).**

