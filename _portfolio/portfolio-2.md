---
title: "Mapping Electrocatalyst Activity across >6000 Conditions Simultaneously"
excerpt: "Short description of portfolio item number 2 <br/>
<img src='/images/Spatial Map Images/Figure; Pt on Carbon.png' width=500>"
collection: portfolio
---






## Motivation: 

In 2019 my Ph.D lab fabricated well-ordered electrode arrays used for chemical imaging experiments. The design allowed for >6000 electrodes (!) to be monitored simultaneously, while using light as the output. These devices were a dream come true for our lab – given that their development took many years.

One exciting application was to use the array as an electrocatalyst screening platform. Researchers have long been searching for new materials with better electrocatalytic properties to improve fuel cells, and other industrial processes. While other researchers typically screen new materials one by one, our platform allows thousands of new materials to be screened in a single experiment! 

*Yet, an unforeseen challenge was in how to process the data. Each catalytic screening experiment produced a single video file with electrode responses represented as light intensity over time. Previous group members working on smaller systems had relied on manual video analysis, using a GUI to select each region of interest (ROI). But here there are >6000 ROI's  – clearly demanding an automated solution! I therefore designed a data pipeline that extracts each channel's information and creates a simple visualization to quickly interpret catalytic results. 







## Outline: 

The goal is to map the catalytic activities onto an image containing all electrode positions. This way, we can visually identify which electrode is more active than others. 





**Process:** 

1.	Create a blank template of each electrode position.

2.	Extract light intensity data for each location. 

3.	Clean each channel's data and map catalytic activity onto the electrode template.





## 1.  Create a blank template of each electrode position 


We first needed a blank template containing all electrode positions. I create the template using a short script written in ImageJ (a common software for handling video data). 

The script takes advantage of ambient light in the room to illuminate the background such that averaging ~200 frames results in a clear image of all electrode positions. A single representative frame is shown in Figure #, while the result of the averaged frames is shown in Figure #. 




<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/Spatial Map Images/New Figure 1; Raw Frame, Avg 1-200.jpg" alt="Single Raw Frame and Average of Frames 1-200" width="600">
</p>

<p align="center">

Figure 1: Single Raw Frame (Left), Average of Frames 1-200 (Right)

</p>






The image is then inverted (Figure #) to make each electrode white on a black background, and thresholded (Figure #) to convert the grayscale color scheme to binary (black/white. The electrode positions are labeled as 1's, and the background is labeled as 0's. 



<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/Spatial Map Images/Figure 3; Inverted.jpg" alt="Inverted Frame" width="300">
</p>

<p align="center">

Figure 3: Inverted Frame

</p>


<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/Spatial Map Images/Figure 4; Threshold.jpg" alt="Threshold Frame" width="300">
</p>

<p align="center">

Figure 4: Inverted Frame

</p>





Selecting the appropriate thresholding algorithm is critical. A "local threshold" gives better uniformity, yet it is still important to fine-tune the parameters to produce a clean result. More details are provided in the corresponding Jupyter notebook.



## 2. Extract intensity data for each position. 

ImageJ has a powerful function called "analyze particles", which can be used to extract the intensity data from each ROI across all frames. Each dataset is saved with a unique label identifying the position from which it came.



## 3.  Clean each channel's data and map catalytic activity onto the electrode template.

We next import both the electrode template and the corresponding data into Python for further analysis. 

We label the electrode positions in the template with the same values used in ImageJ. This allows us to match the intensity datasets to their original position. 

The labeled values are visualized below with a sequential colormap. 


<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/Spatial Map Images/Figure 5; ROI Labeled Image.png" alt="ROI Labeled Image" width="500">
</p>

<p align="center">

Figure 5: ROI Labeled Image

</p>



Next, we analyze each trace, and identify the frame in which the light turns on. This value is converted to potential (the metric used for electrocatalysis), and subsequently mapped to its respective position on the template. 


<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/Spatial Map Images/Figure 6; Pt on Carbon.png" alt="Pt on Carbon" width="500">
</p>

<p align="center">

Figure 6: Mapping Electrocatalyst Activities

</p>




A rainbow colormap is used to represent the catalytic activities. Here, red electrodes are better catalysts than blue. 


Lastly, I plot the KDE summarizing these values, for a more quantitative interpretation. 

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/Spatial Map Images/Figure 7; KDE of Pt on Carbon.png" alt="KDE Summary" width="600">
</p>

<p align="center">

Figure 7: KDE Summary

</p>




## Conclusion:

The described pipeline is incredibly powerful for visualizing results from large electrode arrays. 

It produces simple visualizations from data that would otherwise be quite challenging to interpret manually. 

I anticipate this pipeline being recycled for future array-based projects, long after I leave the lab.  



