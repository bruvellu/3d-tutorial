---
title: Visualization of 3D data in Fiji
author: Bruno C. Vellutini
date: today
date-format: long
created: 23 December 2025
modified: today
toc: true
toc-depth: 4
bibliography: references.bib
lang: en
format: html
lightbox: true
link-citations: true
colorlinks: true
citecolor: Maroon
urlcolor: MidnightBlue
---

## Summary {#sec-summary}

This tutorial shows different approaches to visualize 3D microscopy data in Fiji [@Schindelin2012-di]. It provides a quick introduction to some of the tools bundled in Fiji, such as Orthogonal Views, Volume Viewer, 3D Viewer [@Schmid2010-ep], and BigDataViewer [@Pietzsch2015-md], and a more in-depth explanation of the plugin 3Dscript [@Schmid2019-bm].

## Requirements {#sec-requirements}

- Fiji <https://fiji.sc>
- 3Dscript <https://bene51.github.io/3Dscript/>

## Setup {#sec-setup}

### Install Fiji

- Go to <https://fiji.sc>, choose `Distribution: Stable`, and click the download button
- Copy the downloaded archive to your working directory and unzip it
- Open the `Fiji.app` directory and double-click on the launcher
- The main window of Fiji will open

### Install 3Dscript

- Click on `Help` > `Update...` and wait
- Click on `Manage Update Sites`
- A list of plugins will open
- Search for `3Dscript` and click on the checkbox
- Click `Apply and Close` and then `Apply Changes`
- Wait until the downloads are finished. Then, click `OK`
- Restart Fiji (close it and double-click the launcher)
- Check if `3Dscript` is installed under `Plugins` > `3Dscript`
- You are ready!

### Download 3D datasets

- `T1 Head (16-bits)` dataset is included in Fiji (no need to download in advance)
- `btd-gap-stg_3_z3_t53s_E3_4x.tif` dataset downloadable from Zenodo

## Open dataset {#sec-open-dataset}

We will begin by visualizing an MRI dataset of a human head that is bundled in Fiji.

- Go to `File` > `Open Samples` > `T1 Head (16-bits)`

::: {layout-ncol=2}

![](media/01-sample-open.png)

![](media/02-open-head.png)

:::

- Increase the zoom to 200% for better visualization

![](media/03-head-zoomed.png)

## Orthogonal Views {#sec-orthogonal-views}

Orthogonal Views is a tool that shows the optical sections through the orthogonal planes of XY: XZ and YZ. It is an easy and quick way to get a sense of the tridimensionality of your dataset. Whenever I’m opening a dataset for the first time I use Orthogonal Views. To activate it:

- Click on `Image` > `Stacks` > `Orthogonal Views` (or press ctrl+shift+H)
- The XZ and YZ panels will open next to your XY stack

![](media/04-ortho-views.png)

- The yellow lines are synchronized between the panels
- Click around the different parts of the head to inspect the same position under different angles

::: {layout-ncol=2}

![](media/05-ortho-first.png)

![](media/06-ortho-second.png)

:::

Orthogonal Views is a great way to start understanding your 3D data.

## Volume Viewer {#sec-volume-viewer}

Volume Viewer is a more powerful plugin for 3D visualization as it supports slicing, projections, and rendering. The interface is interactive and intuitive to use. To open it:

- Go to `Plugins` > `Volume Viewer`. The main interface will open in the `Slice` mode.

![](media/07-volume-viewer.png)

- Click around and move the sample to see optical sections from different angles.
- Then, activate the `Volume` mode to render the sample’s surface in 3D and explore it as well, playing with the different rendering parameters

::: {layout-ncol=2}

![](media/08-volume-render.png)

![](media/09-volume-options.png)

:::

Volume Viewer also provides a way to take snapshots of the current view.

## BigDataViewer {#sec-bigdata-viewer}

BigDataViewer [@Pietzsch2015-md] is one of the most important tools for visualizing large, multidimensional datasets. It provides a simple and intuitive interface and shortcuts to swiftly navigate through your sample even in a regular laptop. This is possible because of the underlying file format used by the BigDataViewer: the XML/HDF5 combo. Therefore, before opening the plugin, we must convert our dataset.

- Go to `Plugins` > `BigDataViewer` > `Export Current Image as XML/HDF5`

![](media/10-bdv-export.png)

A dialog with export options will open.

- Under `Export path`, click on `Browse` to select the output directory for `t1-head.xml`

![](media/11-bdv-path.png)

The export process will start. Since this is a small dataset, it’ll be fast. But, for large datasets, this can take hours.

When done, you will find two new files in your working directory: `t1-head.xml` and `t1-head.h5`

![](media/12-bdv-files.png)

The XML file stores metadata information about the image. The HDF5 file stores actual image data. These two files will always be in a pair. To open the XML/HDF5:

- Go to `Plugins` > `BigDataViewer` > `Open XML/HDF5` and select the `t1-head.xml`

![](media/13-bdv-open.png)

The BigDataViewer interface will open showing an optical section of the head sample.

![](media/14-bdv-interface.png)

Getting familiar with the BigDataViewer is an essential skill for navigating large 3D datasets. It’ll also be important for the multiview registration pipeline. So, take the time to learn the basic commands and shortcuts. It is nicely intuitive. The [BigDataViewer’s page](https://imagej.net/plugins/bdv/) on the [ImageJ Docs](https://imagej.net) has the official documentation and we can also go to `Help` > `Show Help` for an up-to-date overview.

Some of the movements to try:

- `left+click+drag`: turn the sample around the mouse pointer at any arbitrary angle.
- `right+click+drag`: move the sample in the XY plane (of the view).
- `scroll`: move through the Z plane (of the view). Use shift+scroll to move fast.
- `ctrl+shift+scroll`: zoom in or out.

::: {layout-ncol=4}

![left-click](media/15-left-drag.png)

![right-click](media/16-right-drag.png)

![scroll](media/17-shift-scroll.png)

![zoom](media/18-ctrl-shift.png)

:::

But, most importantly, are the commands to put your sample back to its original orientation or along any of the original dimension axes:

- `shift+z`: orient the sample on the XY plane.
- `shift+x`: orient the sample on the ZY plane.
- `shift+y`: orient the sample on the ZX plane.

::: {layout-ncol=3}

![XY](media/19-bdv-xy.png)

![ZY](media/20-bdv-zy.png)

![ZX](media/21-bdv-zx.png)

:::

Finally, a visual tip. The default interpolation between image slices is `nearest-neighbors`. Press `i` to activate the `tri-linear` interpolation to obtain a much smoother (and improved) data visualization.

::: {layout-ncol=2}

![nearest-neighbors](media/22-bdv-nearest.png)

![tri-linear](media/23-bdv-trilinear.png)

:::

## 3D Project {#sec-threed-project}

This is a simple tool to quickly generate a 3D animation (e.g., 360-degree rotation) from an image stack. There are basic parameters for adjusting the rendering, like projection method and opacity, and for controlling the animation. There’s only a bit of [documentation](https://imagej.net/imaging/z-functions#3d-project). To try:

- Go to `Image` > `Stacks` > `3D Project...` and click `OK` to generate a basic animation

![](media/24-3d-project.png)

As noticeable above, 3D Project doesn’t do so well with our MRI dataset. However, it works OK for fluorescent microscopy images, so I encourage you to try with other datasets in the future.

## 3D Viewer {#sec-threed-viewer}

The 3D Viewer [@Schmid2010-ep] is a 3D visualization plugin bundled in Fiji. It has been the default 3D rendering engine for many years and provides a good starting point for visualizing and interacting with 3D images. The interface provides some rendering and animation options, but it is possible to create more advanced visualizations and animations with code. For more details, please consult the [documentation](https://imagej.net/plugins/3d-viewer/).

Here, we’ll only open our dataset with 3D Viewer for visualization.

- Go to `Plugins` > `3D Viewer`

![](media/25-3dviewer-open.png)

An import dialog will open. In addition, to the image field itself, pay attention to the `Resampling factor` parameter. The default is 2, which means 2x downsampling of the original stack. Always downsample at least 2x because 3D Viewer will crash when trying to open large datasets.

- Click `OK` on the options dialog and when asked about converting to 8-bit

::: {layout-ncol=2}

![](media/26-3dviewer-options.png)

![](media/27-3dviewer-convert.png)

:::

The main interface will open.

- Explore the sample interactively.

::: {layout-ncol=2}

![](media/28-3dviewer-interface.png)

![](media/29-3dviewer-interactive.png)

:::

## BigVolumeBrowser {#sec-bigvolume-browser}

BigVolumeBrowser is a Fiji plugin to render and interact with 3D data. It’s a fork of the unreleased BigVolumeViewer (a BigDataViewer cousin). The project is being actively developed and seems to have a [good documentation](https://github.com/UU-cellbiology/bigvolumebrowser/wiki) already. It’s a good candidate for some testing and for keeping an eye in the future. However, we’ll not cover it in this tutorial as it’s simply too recent. 

## 3Dscript {#sec-threedscript}

3Dscript is a GPU-accelerated Fiji plugin to generate animations of 3D/4D data [@Schmid2019-bm]. It supports stacks with multiple channels and timepoints, has several options to control the rendering appearance, allows custom transformations and cropping of the data, and generates animations using natural language. The latter is specially useful to have precise control over the animation. Once properly installed, 3Dscript is incredibly fast to generate the animations and doesn’t require a lot of RAM as the datasets can be opened as virtual stacks (more about this below).

To get start, make sure the head dataset is still open.

- Go to `Plugins` > `3Dscript` > `Interactive Animation`

![](media/30-3dscript-open.png)

Two new windows will open: `3D Animation` with the initial rendering of the data and `Interactive Raycaster` with all the fields to control the rendering parameters.

![](media/31-3dscript-windows.png)

### Contrast

The Contrast section shows a histogram of pixel intensities of the image for each channel, which we can choose using the dropdown menu. We can set the minimum, gamma, and maximum values for the intensity and alpha (transparency) properties of each pixel. The weight option controls the general opacity of the channel (0=invisible, 100=visible). There’s also more advanced options like lighting and rendering algorithm which we’ll simply use the default states as it is usually good for most of the use cases.

Adjusting the intensity and alpha values is the most impactful way to improve the 3D rendering. With the intensity setting we can define which pixel value in the image corresponds to total black (minimum) and which corresponds to total white. It’s the same as in the standard `Brightness & Contrast` tool. By default, 3Dscript will load these values from the original stack. In this case, it loaded min=3 and max=521. Let’s change these values to see how it impacts the 3D rendering.

- Change the intensity minimum to 250

You will see that the darkest parts of the rendering will become even darker and no longer visible. We are losing real information from the data; we do not want that. Set the min to 0, for now
- Change the max to 250. The brightest parts of the rendering will become all white. It is so bright that we can no longer resolve details of the surface. We are losing information and also do not want that. Set the max to 500
- Note that when you change a intensity value, the min/max black line in the histogram moves. You can also grab the line and move the line manually to change the values
- The blue line represents the alpha values. In 3D rendering a pixel has a transparency value linked to its intensity. The alpha min defines the value for full transparency and the max the value for full opacity
- Let’s set the alpha min to 250. This will make darker pixels more transparent and information gets lost. Set it to 0
- By default 3Dscript sets the gamma value of alpha to 2.0. Let’s return it back to 1.0 to have a linear adjustments. Note that this improves the visualization of the head’s surface
- Now set the alpha max to 250. The surface will become even more solid because we are defining that pixels that have a value above 250 will be fully opaque. To compare, set alpha max to 5000. You will notice that the sample will become more transparent and even the brain inside the skull will be visible. Set it 500
- Generally, setting the intensity and alpha to the same values is a good sane start for optimizing the rendering

#### Orient sample

- Click on the Transformation show menu to reveal the rotation, translation, and scaling controls
- The 3D Animation window is interactive
- Click on the head and move it around. It’s a great way to see your sample from all the different angles
- Note that the values in the Transformation panel get updated every time you move interactively
- You can also change the precise values you want to position the sample
- Press Reset and change the Rotation Y to 180 to see the right side of the head
- Now change Scale to 5 and Translation X to -500 to focus the rendering on the nose
- Press Reset to return to the original position

#### Crop bounding box

- Another useful option is the ability to crop the bounding box to show the inside of the sample
- You can do it in the XYZ directions or in the near/far axis defined from the user point of view
- Set the Z range minimum value to 60 (you can also drag the slider). This will slice through the sample in the Z axis. Now rotate to see the cropped sample from other angles
- Set the Y range min to 125 and rotate around
- Reset the position and cropping parameters
- Finally set the Near/far min to 0 and move the sample around to see the dynamic reslice of the sample with this cropping parameter
- Reset transformations and cropping parameters

#### Bookmark view

- 3Dscript allows you to bookmark a view for later inspection
- Simply click on the green icon to add the current Contrast, Transformation, and Cropping parameters to the bookmark

#### Set output options

- This panel defines the dimensions of the output animation. It is the original stack dimensions by default
- You can also define if the bounding box and scale bar will be visible (enabled by default)

### Animation

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

#### Set initial conditions

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

::: {layout-ncol=2}

![](media/56-register-before.png)

![](media/57-register-after.png)

::: 

#### Channels timepoints

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


## References

::: {#refs}
:::
