---
title: Visualization of 3D data
author: Bruno C. Vellutini
date: today
date-format: long
created: 23 December 2025
modified: today
toc: true
toc-depth: 3
bibliography: references.bib
lang: en
format: html
link-citations: true
colorlinks: true
citecolor: Maroon
urlcolor: MidnightBlue
---

## Summary {#sec-summary}

This tutorial will show different approaches to visualize 3D datasets in Fiji.

## Requirements {#sec-requirements}

- Fiji/ImageJ
- 3Dscript plugin

## Setup {#sec-setup}

### Download datasets

- Go to...

### Download Fiji and 3Dscript

- Go to https://fiji.sc
- Choose Distribution: Stable
- Click the big download button
- Copy fiji-stable-linux64-jdk.zip to working directory and unzip it

![](media/01-fiji-unzip.png)

- Open the new directory fiji-stable-linux64-jdk/Fiji.app/
- Double-click on fiji-linux-x64 launcher
- Fiji will open

![](media/02-fiji-open.png)

- Click on Help > Update...

![](media/03-fiji-update.png)

- The updater will run and open open and say if Fiji is up-to-date
- Click Manage Update Sites

![](media/04-fiji-manage.png)

- A window will open with a list of plugins available to install in Fiji

![](media/05-fiji-plugins.png)

- Find BigStitcher in the list and click on the checkbox
- Click Apply and Close 

![](media/06-fiji-bigstitcher.png)

- Then Apply Changes

![](media/07-fiji-changes.png)

- Wait… until the downloads are finished. Then, click OK

![](media/08-fiji-ok.png)

- Restart Fiji (close window and double-click the launcher)
- Check if BigStitcher is installed under Plugins > BigStitcher

![](media/09-fiji-ready.png)

- You are ready!

## Open dataset {#sec-open-dataset}

- We will work with an MRI dataset included in Fiji
- To open it go to File > Open Samples > T1 Head (16-bits)
- Increase the zoom to 200%

## Orthogonal Views {#sec-orthogonal-views}

- Orthogonal views is a tool that shows the optical sections through XZ and YZ dimensions.
- It is a great and quick way get a sense of the tridimensionality of your dataset
- Click on Image > Stacks > Orthogonal Views (or press ctrl+shift+H)
- The XZ and YZ panels will open next to your XY stack

![](media/19-stack-orthogonal.png)

- The yellow lines are synchronized between the panels
- Click in different parts of the head to inspect the position under different angles

![](media/20-stack-explore.png)

- This is a great way to start understanding your data in 3D

## Volume Viewer {#sec-volume-viewer}

- Volume viewer is a powerful built-in plugin for volume visualization [@ref]
- To activate it go to Plugins > Volume Viewer
- It has different modes of visualization: Slice, Slice & Borders, Max Projection, Projection, and Volume
- The interface is interactive and intuitive to use
- You can activate the volume viewer to render the sample surface in 3D
- There are also rendering options for the surface

## 3D Viewer {#sec-threed-viewer}

- The 3D Viewer is another plugin built-in in Fiji
- It is a simple plugin for 3D rendering 
- You can also interact with the rendered volume
- But the options for configuration are more limited
- But you can, for example, generate animations
- Go to Plugins > 3D Viewer

## 3Dscript {#sec-threedscript}

- 3Dscript is a GPU-accelerated plugin to generate renderings and animations of 3D data [@ref]
- It supports stacks with multiple channels and timelapses, has several options to control the rendering appearance, allows for transformations and cropping, and can generate animations using natural language
- The latter is specially useful to have precise control over the animation
- To start select the stack you want to open and go to Plugins > 3Dscript > Interactive 
- Two new windows will open
- 3D Animation with the initial rendering of the data and Interactive Raycaster with all the control parameters

- The Contrast section shows a histogram of pixel intensities of the image as a reference
- For each channel (you can pick the channel to edit in the dropdown menu) you can set the minimum, gamma, and maximum values for the intensity and alpha (transparency)
- The weight controls the opacity of the channel (0=invisible, 100=visible)
- Rendering algorithm has three options: independent transparency (default), combined transparency, and maximum intensity projection. This affects mostly multichannel images. The default is usually good for most use cases

### Adjusting the channels

- Adjusting the intensity and alpha values of the channels is the most impactful way to improve the rendering of your 3D animation
- By default, 3Dscript loads these values from the original stack
- But often, they need to be tweaked for rendering in 3D
- Intensity is the standard levels also used in the contrast tools. The minimum defines which pixel value in the image corresponds to black and the maximum defines which value corresponds to white
- The head dataset loads with min=3 and max=521
- Change the min to 250. You will see that the darkest parts of the rendering will become even darker and no longer visible. We are losing real information from the data; we do not want that. Set the min to 0, for now
- Change the max to 250. The brightest parts of the rendering will become all white. It is so bright that we can no longer resolve details of the surface. We are losing information and also do not want that. Set the max to 500
- Note that when you change a intensity value, the min/max black line in the histogram moves. You can also grab the line and move the line manually to change the values
- The blue line represents the alpha values. In 3D rendering a pixel has a transparency value linked to its intensity. The alpha min defines the value for full transparency and the max the value for full opacity
- Let’s set the alpha min to 250. This will make darker pixels more transparent and information gets lost. Set it to 0
- By default 3Dscript sets the gamma value of alpha to 2.0. Let’s return it back to 1.0 to have a linear adjustments. Note that this improves the visualization of the head’s surface
- Now set the alpha max to 250. The surface will become even more solid because we are defining that pixels that have a value above 250 will be fully opaque. To compare, set alpha max to 5000. You will notice that the sample will become more transparent and even the brain inside the skull will be visible. Set it 500
- Generally, setting the intensity and alpha to the same values is a good sane start for optimizing the rendering

### Orienting the sample

- Click on the Transformation show menu to reveal the rotation, translation, and scaling controls
- The 3D Animation window is interactive
- Click on the head and move it around. It’s a great way to see your sample from all the different angles
- Note that the values in the Transformation panel get updated every time you move interactively
- You can also change the precise values you want to position the sample
- Press Reset and change the Rotation Y to 180 to see the right side of the head
- Now change Scale to 5 and Translation X to -500 to focus the rendering on the nose
- Press Reset to return to the original position

### Cropping the bounding box

- Another useful option is the ability to crop the bounding box to show the inside of the sample
- You can do it in the XYZ directions or in the near/far axis defined from the user point of view
- Set the Z range minimum value to 60 (you can also drag the slider). This will slice through the sample in the Z axis. Now rotate to see the cropped sample from other angles
- Set the Y range min to 125 and rotate around
- Reset the position and cropping parameters
- Finally set the Near/far min to 0 and move the sample around to see the dynamic reslice of the sample with this cropping parameter
- Reset transformations and cropping parameters

### Bookmarking a view

- 3Dscript allows you to bookmark a view for later inspection
- Simply click on the green icon to add the current Contrast, Transformation, and Cropping parameters to the bookmark

### Output parameters

- This panel defines the dimensions of the output animation. It is the original stack dimensions by default
- You can also define if the bounding box and scale bar will be visible (enabled by default)

### Animation scripting

- Now that we learned the basics of setting up the rendering and view parameters, we can start generating animations of the data
- Click on the Animation show section and the button Start text-based animation editor
- This will open a special editor window for writing the animation script
- The editor has an strong autocomplete, which means you only need to type one letter at a time to be able to write the exact text needed
- Let’s start with the simplest animation, rotating the head 360 degrees
- To do this we need to define the number of frames that the animation will have and what will happen during these frames
- We can start with defining an animation with ten frames
- Frame counting begins from 0. That means that 0 to 9 has 10 frames.
- Type `f` and the editor will autocomplete `From frame <frame>`
- Type `0` and space and the autocomplete `From frame 0 to frame <frame>`
- Type `9` and space and the autocomplete will show a dropdown menu with several options (rotate by, translate, zoom by a factor of, reset transformation, and change)
- Choose `rotate by` and press tab to autocomplete `From frame 0 to frame 9 rotate by <degrees>`
- Type 360 and space `From frame 0 to frame 9 rotate by 360 degrees` and a dropdown will show the options horizontally, vertically, and around
- Choose horizontally and on the next menu choose (none)
- We have our first animation script, it’s just this single sentence
- Press Run
- A new window will show up with an image stack of 10 frames containing the generated animation
- Press play or \ and watch the head turn 360 degrees in these 10 frames
- Note that you didn’t need to define how many degrees the head needs to turn in each frame. You just state that you needed the head to turn 360 in these 10 frames and 3Dscript deals with it
- Now, the animation is a bit jumpy, not smooth. To make it smoother we can simply add more frames to it
- Change 9 to 35 so that the animation will have 36 frames in total, each frame will rotate by 1 degree. It is smoother. The rotation is very linear
- We can also accelerate/decelerate animation transition using the easing options
- Type ease at the end of the sentence and Run
- Note how the head slowly accelerates the rotation and then slowly decelerates towards the end
- All the other options manually set in the Raycaster window will be applied

- Let’s add a couple more commands
- After the 360 degree rotation we want to show the inside of the head
- TO do that we can using the cropping parameters to change the position of the bounding box
- Write below the first line

```
From frame 0 to frame 35 rotate by 360 degrees horizontally
From frame 36 to frame 71 change channel 1 bounding box min z to 60
From frame 72 to frame 99 change channel 1 bounding box min z to 0
```

- This is saying to set the z min to 60 (roughly half way through the sample) for about 30 frames and then going back to zero in the next 30 frames

- We can also issue multiple commands to happen simultaneously. We can define a rotation and cropping

```
From frame 0 to frame 71:
- rotate by 270 degrees horizontally 
- change channel 1 bounding box max z to 60
```

- Now cropping is happening together with the rotation

### Set initial conditions

- Note that if you simply re-run the above command, the head will start already cropped
- That’s because 3Dscript takes the current parameters as initial conditions for the animation
- Since the previous animation changed the bounding box without changing it back, the value remains set
- To prevent this issue, we can set the initial conditions of the animation (highly recommended)
- This is also needed when you want to start in a position different from the default position of the stack
- You can do so using the `At frame 0:` construction

```
At frame 0:
- rotate by 90 degrees around (0, 1, 0) 
- change channel 1 bounding box z to (0, 129)

From frame 0 to frame 71:
- rotate by 150 degrees horizontally 
- change channel 1 bounding box max z to 60
- zoom by a factor of 2
```


- We can also change scale bar and bounding box options to tweak the animation

```
At frame 0:
- rotate by 90 degrees around (0, 1, 0) 
- change channel 1 bounding box z to (0, 129)
- change bounding box visibility to off
- change scalebar length to 50
- change scalebar width to 10
- change scalebar offset to 20

From frame 0 to frame 71:
- rotate by 180 degrees horizontally 
- change channel 1 bounding box max z to 60
- zoom by a factor of 2

From frame 72 to frame 100:
- change channel 1 bounding box max z to 129
- zoom by a factor of 0.5 
```

- Save the animation text to file to have it as a record and for later re-use or improvement

### Channels timepoints

- This concludes our single channel, single timepoint head example
- Let’s try now a sample with two channels and timepoints
- Close all the 3Dscript windows (including the editor)
- The dataset named `btd-gap-stg_3_z3_t53s_E3_4x.tif` has 2 channels, 30 slices, 200 timepoints, is 2.3GB
- This would be ok to open in most computers, but note that this is a 4x downsampled dataset. The original is 37GB which would be impossible to fit in the memory of most laptops
- The good news is that 3Dscript works very well without needing to load the entire stack in memory. We can simply load it as a virtual stack in Fiji. That’s really, really great.
- To open the dataset as virtual stack go to File > Import > TIFF Virtual Stack...
- Or, a secret Fiji trick, drag and drop the >> arrows at the edge of the Fiji window to open as virtual stack
- Inspect with orthogonal views
- Return to 100% before opening the 3Dscript
- Now open 3Dscript again and use the + to increase the size of the 3D animation window
- Note that the rendered image is similar to the original stack, at the same timepoint
- Rotate the sample interactively to see the other side, the surface of the embryo
- To reorient the sample like this, reset the transformation and change Rotation Y to 180 

- This time, before we start optimizing the rendering, we will generate a basic animation across timepoints to have an overview of how the sample changes over time
- On the Editor set the initial conditions to timepoint 1 and 180 degrees rotation, and generate a 10-frame animation from timepoint 1 to 200 (the last). Then press Run

```
At frame 0:
- change timepoint to 1
- rotate by 180 degrees horizontally

From frame 0 to frame 9 change timepoint to 200
```

- 3Dscript will generate the 10-frame animation and set the 3D Animation window to the last timepoint
- Unfortunately, there’s no way to set the timepoint for the 3D Animation window simply for interacting (it always show the last timepoint set for the animation script)
- From the animation we can observe three things that we want to improve.
- 1. the signal from channel 1 is overexposed at the last timepoint. The intensity of this channel changes over time. The signal becomes so strong at the last timepoints that they become overexpose. We will fix that
- 2. The signal from channel 2 is not so bright. We want to increase the contrast.
- 3. The sample is tilted upwards (right side is pointing up). We want to make the sample completely horizontal, parallel to the bounding box

- Adjust the intensity and alpha max values of channel 1 from 600 to 1500 
- Note how the details along the bright stripe over the embryo are more visible
- Add this information to the script’s initial state and press Run to generate the updated animation

```
At frame 0:
- change timepoint to 1
- rotate by 180 degrees horizontally
- change channel 1 min intensity to 200
- change channel 1 max intensity to 1500
- change channel 1 min alpha to 200
- change channel 1 max alpha to 1500

From frame 0 to frame 9 change timepoint to 200
```

- Now we will improve the other channel. Change the Contrast to Channel 2
- Change the intensity and alpha max of channel 2 to 3000
- Note how the gray signal is brighter and the sample surface looks more solid and less porous. That’s because we changed the alpha max value to make all the pixels above 3000 to have 100% opacity
- But also note that Channel 1 became a little overexposed as well
- The intensity level is ok as it is going nicely from purple to yellow, so I don’t want to change it. But what I can do is make these pixels more transparent so that they are not so bright
- Setting the alpha max to 3000 improves the situation
- Let’s add these updates to the script and Run

```
At frame 0:
- change timepoint to 1
- rotate by 180 degrees horizontally
- change channel 1 min intensity to 200
- change channel 1 max intensity to 1500
- change channel 1 min alpha to 200
- change channel 1 max alpha to 3000
- change channel 2 min intensity to 150
- change channel 2 max intensity to 3000
- change channel 2 min alpha to 150
- change channel 2 max alpha to 3000

From frame 0 to frame 9 change timepoint to 200
```

- Great, the animation is looking nice now
- The last detail is to adjust the sample orientation around the Z axis
- Change the Transformation Rotation Z to -4
- The sample will become parallel to the window
- To make this permanent we will add the following line to the script’s initial stage

```
- rotate by 4 degrees around (0, 0, 1)
```

- Great. The core editing is done. We can now change some general parameters
- We want to hide the bounding box and make the scale bar more visible

```
At frame 0:
- change timepoint to 1
- rotate by 180 degrees horizontally
- rotate by 4 degrees around (0, 0, 1)
- change channel 1 min intensity to 200
- change channel 1 max intensity to 1500
- change channel 1 min alpha to 200
- change channel 1 max alpha to 3000
- change channel 2 min intensity to 150
- change channel 2 max intensity to 3000
- change channel 2 min alpha to 150
- change channel 2 max alpha to 3000
- change bounding box visibility to off
- change scalebar length to 50
- change scalebar width to 5
- change scalebar offset to 10

From frame 0 to frame 9 change timepoint to 200
```

- We can now finish off by increasing the number of frames of the animation to make it smoother
- The maximum number of frames is 200 as we have 200 timepoints. If you add more frames then you’ll get duplicated frames and a lagging animation
- Try first increasing to 100 frames by changing 9 to 99:

```
From frame 0 to frame 99 change timepoint to 200
```

- Note that it takes a bit longer to generate the animation, but it is fast
- Try now setting the to 199 frames

```
From frame 0 to frame 199 change timepoint to 200
```

- Each frame is now a timepoint and the animation is as smooth as it can be given the original data

- We are done with this animation, let’s save it
- Always save the animation as tif first. Simply press ctrl+s or go to File > Save or File > Save As > Tiff...
- I normally add a `3D` prefix to the filename

- Then we want to save it as a video
- File > Save As > AVI...
- Change the Compression to None (otherwise your image quality will be degraded) and choose the frame rate of the video. For this 15fps works fine
- This will create an uncompressed `.avi` file which you can play on your’s system video player
- However, for presentation and other usages, it is good practice to compress the video into a mp4 container using a high-quality compression parameter to reduce file size without affecting image quality. A good software for doing this is Handbrake


- What if you want to see the channel 1 signal all along the movie
- How can you dynamically change the max intensity/alpha?


## Define dataset {#sec-define-dataset}

- Before we begin, we need to define a multiview dataset and resave the dataset
- Defining multiview dataset will create an XML file where all the dataset metadata and information and data from the registration process will be stored
- Go to Plugins > BigStitcher > General > Define Multi-View Dataset

![](media/21-dataset-define.png)

- A window named Choose method to define dataset will open
- On the Define Dataset using field choose Zeiss Lightsheet Z.1 Dataset Loader (Bioformats) from the dropdown menu, since our testing dataset is from Zeiss Lightsheet Z.1, then press OK

![](media/22-dataset-zeiss.png)

- Click browse in the next window, select the CZI file, and press OK

![](media/23-dataset-browse.png)

- BigStitcher will read the metadata of the CZI and show a dialog with the details
- Check that five angles are present, that there are two channels and that the XYZ resolution matches the expected values (see above)
- Then press OK

![](media/24-dataset-metadata.png)

- If you look into the working directory, an XML file named `dataset` will have appeared there 

![](media/25-dataset-xml.png)

- You can open this file in a text editor to see the information stored there
- Right-click the file, select Open With... and choose Text Editor
- The file has the file name, the image dimensions, the XYZ resolution, etc

![](media/26-dataset-contents.png)

- Note that the XML only stores metadata from the dataset and not the actual image data

## Resave dataset {#sec-resave-dataset}

- Next, we need to convert the actual image, which is still stored in the CZI, to a format that allow us to open and visualize this heavy dataset in an efficient and lightweight manner
- For that, we will resave the data into HDF5 format
- Go to Plugins > BigStitcher > I/O > Resave as HDF5 (local)

![](media/27-resave-start.png)

- The new window Select dataset for Resaving as HDF5 will automatically load the last used XML file, in this case, our `dataset.xml`
- You can choose whether you want to convert every angle, all channels, all timepoints, or only a subset of those
- We want it all, press OK

![](media/28-resave-all.png)

- Another window will appear with some resaving options 
- Leave the options as is, but make sure that the Export path is pointing to the `dataset.xml` file (click on Browse and, if the file is not selected, navigate and select `dataset.xml`)
- Press OK and wait...

![](media/29-resave-file.png)

- Resaving this dataset takes about 3 min. But consider that larger datasets will take significantly longer (with several timepoints, for example)
- The Log window will show that it’s done

![](media/30-resave-done.png)

- Note that another XML file named `dataset.xml~1` and a new HDF5 file named `dataset.h5` were created
- Every time the dataset file is saved, BigStitcher creates a backup copy. `dataset.xml~1` was the original dataset.xml which was renamed after the resaving
- If you inspect the new dataset.xml in a Text Editor you will see that it now points to the dataset.h5 file

![](media/31-resave-xml.png)

- Whenever we refere to the multiview dataset we are referring to the XML/HDF5 pair

## Visualize dataset {#sec-visualize-dataset}

- We can finally open the main BigStitcher application

### Start BigStitcher

- Go to Plugins > BigStitcher > BigStitcher

![](media/32-bigstitcher-start.png)

- The last dataset.xml file will be automatically loaded in the select dataset window, click OK

![](media/33-bigstitcher-latest.png)

- This will open two windows, the BigDataViewer and the Multiview Explorer

![](media/34-bigstitcher-windows.png)

- The Multiview Explorer shows a table with the individual views of the dataset. We have 5 views, each with 2 channels. Therefore, we have in total 10 views.
- Clicking in a row will show the data in the BigDataViewer. You can select multiple rows freely. You can also sort the table by channel or angle for example
- Select the five views from Channel 561 (channel 2)

![](media/35-bigstitcher-bright.png)

- The image is too bright, we need to adjust the contrast
- For that, go to Settings > Brightness & Color
- A new window will open

![](media/36-bigstitcher-contrast.png)

- Change the max value of channel 2 to 2500

![](media/37-bigstitcher-adjusted.png)

- Now we can visualize the dataset in more detail

### Learn BigDataViewer

- It is important to familiarize yourself with the BigDataViewer commands and shortcuts 
- BigDataViewer is very intuitive to use but a quick look at the Help is important to not get lost
- Some of the most important commands are as follow:
- Shift+X, Shift+Y, Shift+Z: That’s your compass. If you get lost, pressing one of these shortcuts will get you back to the original XY, YZ, ZX orientation
- In this scope the rotation axis is Y
- Therefore, pressing shift+y will show you the separate angles

![](media/38-bigstitcher-shifty.png)

- Hold left mouse button to rotate the data around the pointer
- Hold right mouse button to drag the view
- Ctrl+shift+scroll+up or down will zoom in/out fast (don’t use shift for normal speed)
- Tip, select the Explorer window with the five views selected and press C. This will autocolor the views which is great for visualization
- Tip, select the BigDataViewer and press i to activate interpolation for better visualization

::: {layout-ncol=2}

![](media/39-bigstitcher-nearest.png)

![](media/40-bigstitcher-interpolation.png)

:::

- Take some time to explore the data, the different views, zoom in and out, find the beads, and get familiar with the BigDataViewer
- Then finish with the 5-views oriented as in the image

![](media/41-bigstitcher-ready.png)

## Detect points {#sec-detect-points}

- We can now start the first processing step of the pipeline, detecting points of interest to be used for the registration
- The Multiview Explorer is always the starting point
- Select the five views of channel 561 (if not selected)
- In this case, we want only the views of channel 2 to be selected since it is in this channel that the beads are visible
- Then, then right click the rows with the mouse
- A long menu will appear. This is the main menu of BigStitcher. Anything that we select will only be applied to the selected views.
- After right-clicking, find the section Processing and select Detect Interest Points...

![](media/42-detect-menu.png)

- In this first window we can choose the type of detection (Default: Difference of Gaussian) and a label to describe these interest points
- One dataset can have multiple sets of interest points
- We will keep the default options and press OK

![](media/43-detect-points.png)

- We can set different parameters for the Difference of Gaussian approach
- What is important to us at this point is to make sure that Interactive... is set for Interest point specification and that you change Downsample XY from Match Z Resolution (less downsampling) to 2x. The latter is not essential for all datasets but it works better for this dataset which has relatively high anisotropy (lower Z resolution compared to XY). Keep Downsample Z as 1x.
- Press OK

![](media/44-detect-details.png)

- In case, we can select which view we will open the interactive window and if we will load the entire stack. Simply press OK to load the entire first view

![](media/45-detect-views.png)

- A stack will open with a rectangular ROI placed at the top left corner
- The ROI shows a live view of detected points for the current parameters Sigma and Threshold present in the other window

![](media/46-detect-interactive.png)

- Sigma is the size (radius) of the point and Threshold is the intensity-based cut value to discard low-quality detections
- The default is to Find DoG maxima (red) or, in other words, bright spots. You can also find dark spots surrounded by bright areas (useful for some samples)
- This is a normal ImageJ window, so you can zoom, adjust contrast, and if you lose the ROI you can simply draw a new rectangular ROI
- You can drag the ROI around by clicking inside it and holding/dragging it 

![](media/47-detect-zoom.png)

- What we want to do now is to adjust the Sigma and Threshold so that most of the beads outside the embryo are properly detect with the least of spurious detections
- The best way to begin is to zoom in into a bead outside the embryo and check if the circle size is matching well the bead. Move the Sigma slider, aiming for a circle slightly larger than the bead
- Remember to also move through the Z slices of the stack to see how the detection behaves
- If the size looks good, now increase the Threshold until real beads begin to not be detect. Once you reach this point roll the slider back until most beads are detected
- Note that, no matter how much you tweak these parameters, there’ll always be many detections in the sample tissues. These non-bead detections will not have a strong influence on the registration given that you have enough detected real beads
- Once satisfied, press Done

![](media/48-detect-start.png)

- Bead detection takes some time
- Once DONE, go to the Multiview Explorer window and press Save

![](media/49-detect-done.png)

- This is important because the detections are initially saved in memory and will be only written to disk after pressing Save (detections are saved in a directory named interestpoints.n5)
- You can notice that the column #InterestPoints in the Multiview Explorer now shows 1 for the five views of channel 561 (but not for channel 488)

![](media/50-detect-increment.png)

## Register views {#sec-register-views}

- Now we can try to register these views using the detected interest points
- With the 5-views selected, right-click and run Register using Interest Points...

![](media/51-register-menu.png)

- We can change different registration parameters like the algorithm to be used or the specific set of previously interest points
- We want to go with the default Fast descriptor-based (rotation invariant) Registration algorithm as it works well for bead-based registration with Z.1 datasets
- Since our views are very further apart with almost no overlap, we want to change the option Registration in between views to Compare all views against each other
- Leave the other options as is making sure we are using the Interest points labeled as `beads`
- Click OK

![](media/52-register-type.png)

- A novel window will open with several other parameters to tweak. Please refer to the BigStitcher documentation for the specific function of these
- For us, it is important to note two. The option Fix views set to Fix first view means that all other views will be mapped to the first angle. And the Transformation model set to Affine means that the data will be transformed non-rigidly to fit the individual views. This is important since different portions of the stack might have a certain degree of distortion from the objective lenses and an affine transformation helps to fit the views better together
- The other parameters we will only need to change if our registration fails
- Press OK

![](media/53-register-affine.png)

- This small window with Regularization Parameters can be kept as is (Rigid and 0.10). Press OK
- Same for the interest point grouping options. Press OK

::: {layout-ncol=2}

![](media/54-register-regularization.png)

![](media/55-register-grouping.png)

::: 

- The registration will begin and be over in a few seconds. Don’t blink or you will miss it! If successful, you will see that the individual views will now have moved over (registered) the first view and they are all overlapping in the BigDataViewer window

::: {layout-ncol=2}

![](media/56-register-before.png)

![](media/57-register-after.png)

::: 

- Now that the views are registered, explore the dataset to verify that the registration worked well. The best way to do this is visually
- One of the first things that you can do is to press shift+y and zoom in into a bead close to the embryo’s surface

![](media/58-register-check.png)

- You will see the point spread function of one bead in each individual view forming a star with generally four views (as the fifth view is too further away).
- If the sample is registered well, the center of the point spread functions of the different views should match in the middle of the star

![](media/59-register-bead.png)

- Another thing that you can do is to find a structure you know well in the sample and check that the tissues are actually registered. It can happen that the beads are nicely registered, but the tissues themselves are a bit off
- One way to do this is to select only two contiguous views and check them closely
- When done, make sure to Save the project again

![](media/60-register-save.png)

- After saving, the #Registrations column should now show the number 3 for the selected views (if not deselect and select them again to update the counter)

![](media/61-register-increment.png)

## Set bounding box {#sec-set-bounding}

- Our dataset is registered, but before fusing the views it is important to set a bounding box around the sample. This reduces the final dimensions and file size of the fused data.
- For that, right-click and select Define Bounding Box... 

![](media/62-bounding-menu.png)

- We want to define it interactively, so leave the Bounding Box option as isotropic
- You can give the bounding box a custom name and define different bounding boxes for different purposes, but the default name is good enough for this tutorial. Click OK

![](media/63-bounding-interactive.png)

- Two windows will open: BigDataViewer with the sample and some purple shade and a bounding box window full of sliders

![](media/64-bounding-box.png)

- For defining the bounding box I follow a specific procedure, always in the same order, to avoid inadvertently leaving out a part of your sample when fusing
- First, press shift+x to orient the sample on XY

![](media/65-bounding-shiftx.png)

- Go through the sample (Z) top to bottom to get a sense of the entire volume and stop back at the middle

::: {layout-ncol=3}

![](media/66-bounding-top.png)

![](media/67-bounding-mid.png)

![](media/68-bounding-bottom.png)

:::

- Move the x min slider to the right to cut out the region on the left of the sample (the dashed line is the reference edge). Get close to the sample, but leave a gap

![](media/69-bounding-xmin.png)

- Once the placed x min, go again top to bottom through Z to make sure nothing was cut out
- Now do the same of x max to cut out the region on the right side of the sample

![](media/70-bounding-xmax.png)

- Next we want to cut a bit from the top and bottom regions
- Move the slider y min to cut from the top and y max to cut from the bottom. Remember to go through Z to make sure it is not cutting the tip off the embryo (it happens)

::: {layout-ncol=2}

![](media/71-bounding-ymin.png)

![](media/72-bounding-ymax.png)

:::

- Finally, press shift+y to cut out the exceeding portions in the Z direction

![](media/73-bounding-shifty.png)

- Use z min to cut from the top (in this orientation), always going through Y to check!
- Then adjust z max to cut from the bottom (in this orientation), also going through Y.

::: {layout-ncol=2}

![](media/74-bounding-zmin.png)

![](media/75-bounding-zmax.png)

:::

- When done, press OK in the bounding box window

![](media/76-bounding-ok.png)

- The dimensions of the interactively defined bounding box and the estimated sized of the fused image will appear.
- Press OK and Save

![](media/77-bounding-dimensions.png)

## Fuse dataset (one channel) {#sec-fuse-single}

- Finally, let’s begin the fusing of registered views
- Select the five registered views in the Multiview Explorer, right-click and press Image Fusion...

![](media/78-fuse-menu.png)

- The window with fusion options will appear

![](media/79-fuse-options.png)

- Our “My Bounding Box” is automatically selected for the Bounding Box
- We can choose to downsample the fused image. I highly recommend downsampling 2x, 4x, or even 8x, depending on the dataset, if you are fusing a dataset for the first time.
- Fusing without downsampling (1x) can take a really long time for large samples (many hours), so it is good practice to downsample the fused image to make sure the fusing parameters are good for your dataset before fusing the whole thing.
- But for this tutorial, you can leave at 1x
- For Interpolation leave Linear interpolation as it gives better outputs
- Fusion type is an important parameter to choose wisely
- The simplest is Avg, which averages the signal of every view per pixel. This is quick but it is combining the good contrast of one view with the blurred side of another view and the resulting contrast will be suboptimal.
- Avg, Blending is the same as Avg, but it blends smoothly the edges of the different views giving a slightly better fusing than simple Avg
- Avg, Blending & Content Based improves the other two options by adding a step that checks and keeps only the best information for each coordinate (keep good contrast, discard blurred information). This option gives the best results. However, it is also the one that requires more memory and takes longer to finish (much longer)
- Therefore, I would start with 2x or 4x downsample using Avg, Blending before trying less downsampling and the content based fusion
- For the Pixel type I often use 16-bit, but it depends on what is your goal with the fused image. If it is only to have a volume visualization, 8-bit might be enough. If further processing and analysis is expected, definitely go for 16-bit or, in special cases, 32-bit.
- BigStitcher also has an option for using the interest points information during the fusion step to obtain better results (Non-Rigid fusion). This is for advanced users and I have not tried it enough to have an opinion about it.
- Generally, we want to have one fused image per timepoint per channel
- And I always Save as (compressed) TIFF stacks for the Fused image option. Choosing Display using ImageJ can be dangerous as the fused image will be large and your computer can run out of memory and crash. Writing to disk is safer.
- After pressing OK there’ll be another window to define the min/max levels, but they are automatically detected. Press OK

![](media/80-fuse-minmax.png)

- The output directory will be the same where the dataset.xml is. You can add a Filename addition to distinguish different types of fusion and downsampling (useful when doing it multiple times)
- Press OK and fusion will start

![](media/81-fuse-filename.png)

- Once done, drag and drop the fused dataset named `avg_blend_1x_fused_tp_0_ch_1` in Fiji to open it and adjust the contrast with the Brightness/Contrast tool to see the data

::: {layout-ncol=2}

![](media/82-fuse-open.png)

![](media/83-fuse-contrast.png)

:::

- Inspect the fusion result, checking for artifacts. If your sample has a membrane staining, for example, check for doubled membranes
- Note that this is an isotropic dataset

![](media/84-fuse-isotropic.png)

## Duplicate transformation {#sec-duplicate-transformation}

- Now that we have successfully registered and fused the views of one channel, we can simply apply the series of transformations to the other channel without the need to detect interest points or register the channel independently

![](media/85-duplicate-unregistered.png)

- You can do so using the tool Duplicate Transformations from BigStitcher
- First, close the Multiview Explorer and the Select dataset window that pops-up
- Then go to Plugins > BigStitcher > General > Tools > Duplicate Transformations

![](media/86-duplicate-open.png)

- Select the option One channel to other channels

![](media/87-duplicate-channels.png)

- A Select dataset window will open with the last dataset.xml already opened. Press OK

![](media/88-duplicate-dataset.png)

- Now choose the source channel. Remember that we registered the Channel 561 (channel 2).
- The Target channel(s) is All Channels (all the other channels except for the source one).
- The last option, Duplicate which transformations is important. Generally, Replace all transformations work for most cases. However, I often prefer to use Add last transformation only. This will take the last transformation from the source channel and apply to the target channel.
- Note, however, that for this to work, the source channel can only be one transformation ahead of the target. If for instance, we ran two subsequent transformations for the source channel, then applying only the last would not duplicate all the transformations. Always check the #Registrations in the Multiview Explorer.

![](media/89-duplicate-options.png)

- Once you press OK, the transformations will be applied in the XML file. It’s quick.
- Now open BigStitcher again and check if the 5 views of the other channel are registered (they should)

![](media/90-duplicate-registered.png)

## Fuse dataset (all channels) {#sec-fuse-all}

- We have now both channels registered, but only one fused
- To fuse both channels select all the views in the Multiview Explorer

![](media/91-fuse-all.png)

- Then set the desired parameters for fusion (optimized previously) and run the fusion again as described above
- This time there will be two files as output: avg_blend_1x_fused_tp_0_ch_0.tif and avg_blend_1x_fused_tp_0_ch_1.tif

![](media/92-fuse-outputs.png)

- Open both files in Fiji and adjust their contrast

![](media/93-fuse-contrast.png)

- Then go to Image > Color > Merge Channels...

![](media/94-fuse-merge.png)

- Select ch_0 for C1 and ch_1 for C2 and press OK

![](media/95-fuse-merged.png)

- A red-green 2-channel stack will open
- As red-green isn’t good, use the LUT tool to update the colors to green for C1 and magenta for C2.

::: {layout-ncol=2}

![](media/96-fuse-redgreen.png)

![](media/97-fuse-greenmagenta.png)

:::

- We can even compare this fused dataset with one single view of the original dataset.
- Drag and drop the CZI file, select the first view only to import, and put the stacks side-by-side for a comparison slice by slice

![](media/98-fuse-versus.png)

## References

::: {#refs}
:::
