---
layout: page
title: ASIWizard
description: An image analysis automation software for analyzing "Artificial Spin Ice" nanomagnetic arrays, written in the form of a MATLAB GUI wizard.
img: assets/img/asiwizard_finalinspection.png
importance: 1
category: Software development
related_publications: false
---
<center>🚧 <b>This page is under construction. It's going to take a while to write some stuff</b> 🚧</center>
<br>
This page contains an overview to the backend design of ASIWizard, an image analysis automation software which I developed during my time at the University of California Davis. Documentation to download, install, and operate ASIWizard <a href="https://github.com/yoshikisd/ASIWizard">can be found here.</a>.

In this "blog" I discuss core features rather than be a comprehensive overview of the software. As a consequence, there will be some inconsistencies between the actual ASIWizard pipeline shown in the GitHub and what will be presented here.

### **What are Artificial Spin Ices?**
In "short":
1. ASIs are extended arrays (square, hexagonal, etc) of stadium-shaped nanomagnets fabricated with lithographic methods (e.g., electron beam lithography).
2. Each nanomagnet behaves as a bar magnet, where it's magnetization is represented by an arrow pointing towards the "North" pole end.
3. The nanomagnets cannot move, but their magnetizations can flip 180 degrees.
4. The nanomagnets always want to experience attracting interactions; point their "North" poles to the neighboring nanomagnets "South" poles.
5. Physicists are interested in ASIs because the arrays in which the nanomagnets are placed in alters how the nanomagnets interact with each other and gives rise to different magnetic ordering behavior throughout the entire ASI array (>10x larger than the individual nanomagnet).
6. Physicists often study ASIs using microscopes that can image magnetization in a material. The grayscale contrast in these magnetic microscopy images give information about what direction the magnetizations are pointing within each of these nanomagnets.

For more details on ASIs and magnetic microscopy, see XXX.
<br><br>

### **Image analysis automation with ASIWizard?**
<center><div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_xmcdpeem.png" title="xmcd_peem_image" width="400"%}
    </div>
</div></center>
<div class="caption">
    An example magnetic microscopy image, acquired with x-ray photoemission electron microscopy. The black and white pills/stadiums are the nanomagnets whose magnetizations point towards the left (black) or right (white).
</div>

Each ASI can be composed of ~1000 individual nanomagnets. Analyzing magnetic microscopy images of ASIs requires, at a minimum, identifying and labeling the magnetization of **each individual nanomagnet** and establishing spatial relationships between neighboring nanomagnets to characterize local interactions and global magnetic ordering. Many studies of ASIs involve both the (1) systematic variation of experimental parameters (temperatures, geometry, etc) and (2) the accumulation of statistics through re-imaging the same array or imaging duplicate arrays.

**Thus, the number of individual nanomagnets in the dataset can grow to quantities that become unfeasible to manually inspect within a reasonable time frame. It is therefore highly desireable to automate the nanomagnet detection and assignment process to expedite data analysis and inform future design of experiments.**

("Fun" note: There were >100 such images I had to look at throughout my PhD. The folds in my brain probably look like these arrays now.)
<br><br>

**ASIWizard is designed to accelerate the analysis of these images by performing 3 tasks:**
1. Automated nanomagnet/vertex detection - Automate the detection and magnetization assignment of each individual nanomagnet.
2. Automated lattice coordinate assignment - Automate the assignment of lattice coordinates to each nanomagnet and vertex (space where the nanomagnet tips point towards)
3. Characterize the ASI - using quantities calculated from (1) and (2)

ASIWizard was created from a collection of MATLAB functions I wrote to perform tasks (1-3), wrapped under a GUI. It had occurred to me after having several students help me with this data analysis that a GUI-based analysis software could be easier for first-time users to employ (rather than the spaghetti code I wrote at that time).

<center><div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_finalinspection.png" title="final_inspection" width="500"%}
    </div>
</div></center>
<div class="caption">
    Behold ASIWizard, in all its MATLAB-paywall-blocked glory.
</div>

Despite the jab I made about MATLAB, the GitHub repo does contain a compiled version which can be installed as an executable with MATLAB Runtime. So you don't need a copy of MATLAB to use this!

The GUI is written in a wizard-format to help guide users through steps related to image pre-processing and data manipulation for quality control of the analysis output. The bottom half of the UI shows the ASI image modified at different steps of the analysis process depending on the steps being performed. On the top left corner of the UI are two tabs: The "Vertex detection" tab which handles **task (1)** and the "Image processing" tab which handles **tasks (2-3)**. See <a href="https://github.com/yoshikisd/ASIWizard">the GitHub repo's README</a> to see what the data processing flow looks like with the GUI.

#### **Task 1. Automated nanomagnet/vertex detection**
The key feature of ASIWizard is an automated feature detection scheme based on <a href="https://en.wikipedia.org/wiki/Earth_mover%27s_distance">the Earth Mover's Distance (EMD)</a>, which was brought to my attention by Prof. Jeremy Mason. The EMD lets us quantify how dissimilar two grayscale images look from each other by (in some sense) treating them as piles of dirt, where the pixel intensity is related to how tall the dirt is at that pixel position. The dissimilarity is quantified by calculating what is the minimum amount of work (amount of dirt times the distance to move it by) needed to transform one pile of dirt (image A) into the other (image B).

Feature detection with the EMD is performed with 3 main steps:
1. Loading a topography image of the ASI into the software (this image needs to be pre-registered with the magnetic image).
2. Selecting an ROI that encloses a feature that we want to detect throughout the whole image; this gives us the reference image. **ASIWizard is designed around detecting the array vertices first and subsequently deducing nanomagnet position from the vertex locations. (explanation given at the end)** 
3. Calculate the EMD between the reference image and a similarly-sized sliding window that scans over the whole image. **This produces a EMD map where the lowest values (valleys) correspond with the most likely vertex locations**

<center><div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_emdtest.png" title="xmcd_peem_image" width="800"%}
    </div>
</div></center>
<div class="caption">
    EMD calculations performed between a reference image and several framed captured by a sliding window. Notice that the more dissimilar the two sets of images become, the larger the EMD quantity becomes as well.
</div>

I chose to design ASIWizard around vertex detection since vertices (1) are more prominent (and thus easier) features to detect and (2) contain more information about the nature of the lattice than the individual nanomagnets do. The latter point makes it easier to implement a scheme for mapping the detected vertices and nanomagnets to an ideal square/hexagonal lattice for coordinate assignment.

Within ASIWizard, **steps 1 and 2** look like the following:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_importim_1.png" title="first import"%}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_importim_2.png" title="flatfield"%}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_importim_3.png" title="select roi" %}
    </div>
</div>
<div class="caption">
    Left: First, import the topography image of the ASI nanomagnet array. Middle: Then perform a flatfield correction (helps with the detection process). Right: Finally, select an ROI representative of the vertex we want to detect throughout the image.
</div>

Then, with a bit of CPU parallelization with **ParFor** loops, **step 3** look like the following:

<center><div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_importim_4.png" title="multicpu" width="500"%}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/asiwizard_importim_6.png" title="output" width="500"%}
    </div>
</div></center>
<div class="caption">
    Left: Using multiple CPU cores take a bit of time to spin up in MATLAB. Right: After the EMD is calculated, ASIWizard shows the topography image overlain with clusters of red dots showing where the EMD falls below a certain threshold EMD value. Note that the red dots predominantly reside at the center of each vertex.
</div>

The output of this processing task is saved as a .mat file to be reloaded for the "Image processing" tab. Currently, vertices have been located, labeled with a number, and have been stored as entries in a **vd.vertex** (vd = vertex detect) attribute. However, we still havent determined the nanomagnet positions yet.



#### **Task 2. Automated lattice coordinate assignment**
 The problem now is how do we automatically make the software figure out what lattice coordinates each vertex should get assigned. Moreover, at this point, we still haven't identified/calculated the positions of each nanomagnet yet.