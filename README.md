# Fusion-CAM-Workflow-Template-Framework

This Fusion Assembly acts as a framework for CAM programming, and configurable fixturing 

The file is intended to be used as described in this video lecture, and more broadly to work with the 
[container method](https://www.autodesk.com/autodesk-university/class/Streamlining-CAM-Workflows-Templates-2019#video) for setting up CAM programs. 
Strictly speaking the container method for programming is not needed, but it is a very strong compliment to this method. 

If you don't want to understand the background and theory behind the Workholding Library method, 
you can just skip to the usage instructions [here](#a-better-way-working-with-ancestor-files-2213). 



## Core Concepts: 
    
### Simple Replacable Container ***(time - 8:51)***
    
#### Why simple joints don't work well
Replace doesn't work unless the two source components have a shared common reference to keep the joint well defined. 

A simple way to make this work is to use the component origin.
Since all components have this, when you replace a component the joint will maintain its integrity. 

What if you don't want to be stuck just using the component origin for joint snaping? 

#### Construct a replacable joint anywhere on your part ***(time - 10:16)***
1) Put the bodies you care about into a component. 

2) Add a new component at the same level of hte model higherachy, called `Joint Origin Container` (JOC). This will be an empty container. 

3) Create a new joint origin tied to the coordinate origin of JOC. This puts the joint origin inside this container. 

4) Createa a new joint, and pick the new joint origin as the snap reference for `Component 1`. Then pick any other point you like on one of the bodies as the other reference. This constrains the new joint origin to be fixed to the object. 

5) Use `Save As` to keep a common lineage of the Joint Origin Component ***(time - 10:57)***


Once you have a couple of basic component files that share the same joint origin lineage, you can build an assemly that uses them to make replacable parts. 


## Buudling blocks for a replacable workholding library ***(time - 11:57)***

### Workholding Assembly Hierarchy

- Workholding Assembly File
    - Fixture Container 
        - Vise 
        - Pallet
    - WCS Artifact Container

- Vise Component 
    - StockAttachment (JOC)
    - ZeroPointAttachment (JOC)
    - JawPosition1 (JOC)
    - JawPosition2 (JOC)
    - SoftJawAttach1 (JOC)
    - SoftJawAttach2 (JOC)
    - Vise Geometry

- Pallet
    - WCS Attachment (JOC)
    - Zero Point Attachment (JOC)
    - Machine Model Attachment (JOC)
    - Pallet/Clamping unit Geometry

- WCS Artifact File
    - WCS (Joint Origin)
    - Zero Point Attachment (Joint Origin)
    - WCS Geometry Body (Body in root)
    - Incosequential Geometry (Component)


### Building a Vise File ***(time - 12:35)***

You can set up any kind of JOC structure you want in your vise files, 
but the suggested one here works for a huge variety of vises and fixturing and we recommend you start with it. 
One of the goals here is to make it so we can all share workholding type files and use them interchably. 
For that to happen, we all need to share the same common lineage of files, starting with the ancestor files here. 
Its important to note that all the vises you make need to start from this ancestor file.

Once you pick your joint origin structure you will build all your vises off that root file using save-as. 
If you decide to change it later, you have to rebuild all your vise files off the new root. 

This configuration supports both fixed-jaw vises and self centering vises with reversible jaws and a zero point system with a single zero point position. The JawPosition1 and JawPosition2 JOC's are set up to support the two jaw locations, and configurations can be used to pick which one is active.

A separate ancestor file is provided to support zero point systems with mulitple locations, that can support multiple vises. 

### Buidling a WCS File ***(time - 13:30)***

The meaningful geometry is a simple cube with a centered hole that can be used to define the Z and X faces. 
Joint origins are added to define the WCS and its connection point to the zero point system. 

An actual gauging block may have more complex geometry. You can model that in for visualization purposes, but it won't be used for anything other than that. 
So we put that in the `Inconsequential Geometry` component. 

Note that there are not any JOC instances in this file. 
The joint origins are attached to the root of the model hierarchy. 

### Building a Palette File ***(time - 13:54)***

This file is constructured in a very similar way to the vise file. 
Again, the joint origin structure used here must have a common lineage across all pallete type files and save-as is used to preserve that. 


## Mapping the joints in a Workholding Assembly File ***(time - 14:34)***

## Download these files so you don't have to start from scratch! ***(time - 16:31)***

## What is a Workflow Template? ***(time - 17:03)***

You might already be using one of these? You might also have heard it called the "Container method". 
Rob Lockwood presented the [original container method talk at a AU 20XX](https://www.autodesk.com/autodesk-university/class/Streamlining-CAM-Workflows-Templates-2019). 

Workflow templates, or container templates, are a powerful way to speed up your programming by associating toolpaths to the container so you can swap models in without having to re-click all the geometry. 

But as presented in the original talk, they have a key weakness -- a specific container template was tied to 
a single machine and workholding setup. 
If you wanted to change that out, you needed to basically start over and make a new container. 

We can integrate replacable workholding assemblies to fix this.
But first, lets just relive the pain of changing workholding or machine models using the old method. 


## The nightmare scenario we're all trying to get past when swapping workholding ***(time - 18:32)***
This is an example of how hard it can be to swap an existing program to a new machine, new work holding, etc. 
This is the world we live in, if we don't take the time to build a replacable workholding library. 
It is not fun. 

## A better way! working with ancestor files ***(time - 22:13)***
### Understanding the Workholding Assembly File ***(time - 23:29)***
### Build a Vise Type Container File ***(time - 23:42)***
- Open the Vise ancesstor file, and right away use save-as to create a new file. Save-as is critical! 
- You are free to delete the vise geometry component. This leaves just the JOC instances. Don't delete those, they are the key to making this whole thing work. If you accidentally delete those, don't save anything. Start over from the ancestor file. 

You can never re-add a JOC once you've deleted it. Even if you give it the same name, internal information in Fusion has been lost. 

### Prep an existing vise geometry for use with the vise ancestor file ***(time - 24:20)*** 
- Open a vise geometry file. If there are existing joint origins in the source file, don't use them. Delete existing joint origins, so we can use the ones attached to our JOC in our Vise component file. 
- ***(time - 25:10)*** Cleanup the vise model by removing any existing joints and motion links
- ***(time - 25:15)*** Put the vise geometry into its own component
- ***(time - 25:34)*** Make sure the origin of the container matches up with where you want the zero point to be
- ***(time - 25:47)*** Add new motion joints to the vise. Note that As-built joints only appear if you have `Capture Design History` turned on). Make sure you use the origin of the container as the snap selection. This makes sure the joint lives inside the component with the vise geometry. 
- ***(time - 26:17)*** Add new motion link to couple the jaw motion together
- ***(time - 26:25)*** Select the vise base and ground it to parent. 

After these steps we consider our Vise container fully rigged. 
If you'd like, you can save this file somewhere, but the file itself won't be used directly in any of your workholding assembly files. 
The only reason to save it is if you think you might want to come back and modify the rigging somehow, or perhaps change the vise geometry to simplify it. 


    Note: You may see a warning about how grounding to the parent will cause bodies to move. If you hit ok, and you see things move, that means that the origin of vise body component does not align with the origin of your root container. If you have this problem, you need to correct it. 

    First undo the grounding that you just did.

    You will need to use the move command to rotate the containers of the vise geometry so the vise body container aligns with the root container. In order to do this, make sure you have the `container` option selected in the move commands. 

    After you do this, you may see that the vise geometry itself is no longer aligned correctly. 
    To fix this, you suse a second move command but this time select the `bodies` option from the drop down. 
    Now you can move hte bodies without changing the origin of the container itself. 

    Once you have the vise body container's origin aligned with the root origin, and the vise bodies themselves in the right place, then you are ready to ground the vise body to the parent. If you did it right, you won't see any warnings and nothing will move on you. 


### Copy the rigged vise into the new Vise file from the github rep ***(time - 26:32)***
- Copy the top level Vise container from the rigged file
- ***(time - 26:47)*** Go back into your Vise Type File and, right click on root component and `Paste new` 
- Right click the newly pasted Vise container and select `Ground to parent`
- ***(time - 26:55)*** Create new joints between the vise geometry and the existing JOC origins 
- ***(time - 27:03)*** Select the `StockAttachment` joint inside the StockAttachment JOC. Hit J for `joint` 
- ***(time - 27:13)*** Drag the Z-drag handle up, then select the vise surface you want to align the bottom of the stock to 
- ***(time - 27:50)*** Create joints for the two jaw positions. You have to be careful about how you select the face so that the z-nomen is pointed along the sliding axis of the vise jaw. You also need to watch the sign of the z-nomen and using the flip to control it.  The joint should be set up so that when it is positioned in the +y position, the z-nomen points back toward the center of the vise. 

Don't forget to save this file. 
Rembmer that this is the new file you created by doing a save-as from the ancestor file. 


### Testing out your new vise file ***(time - 29:15)***
You can open the `Fixture Assembly` file provides. 
Save-as and create a new one, so you don't change your ancestor file. 
Then right-click on the `Vise v1` component and choose replace. 
Pick the new file you just saved a moment ago. 

In the video, we do this on a fully programmed part. 
But in the provided files that full `Workflow template template` file is already setup for configurations and has a link back to the Fixture assembly file (so the configurations you add to that file will carry through).
If you want to test out your work so far, you can first nake new version of the `Workflow template template` via save-as. 
Then you can break the link of the `Fixture container` group, and that will let you replace its children. 
When it asks, you only break the top level link, not all the children with it. 

### Extending your Vise type file to use configurations ***(time - 29:56)***

    Note: you don't have to use configurations.
    You can stop at creating the properly rigged fixture assembly files. 
    Then you would create a library of different vise files and fixture assembly files always starting with these provided ancestor files and using save-as. 
    This works, but creates a proliforation of different files you need to keep track of. 

If you'd like to avoid having many different vise and fixture assembly files, you can leverage configurations to keep all your vises in one file, all your fixtures in another file, and one last file for all your fixture assemblies then follow this method to use configurations.  

- ***(time - 31:26)*** Add second vise into the vise type file 
- ***(time - 31:46)*** Setting up configurations for the vise 
- ***(time - 32:05)*** Critical step of creating a Null configuration at the root, then other conditions for the different vises. 
- ***(time - 33:07)*** How the joints work with configurations  
- ***(time - 36:27)*** How the JOC framework supports fixed jaw vises

### Setting up configurations in your Fixture Assembly file instead ***(time - 36:54)***

This requires nesting configurations within configurations. 
So you are starting with a configured file for your vises, one for your clamping, and one for your WCS. 
The key thing to do here is to configure the `insert` in this file, so that as you turn the various configuartions on/off you are choosing which sub-configuration is active on the insert. 

- ***(time - 38:03)*** Important relationship between the clamping unit and the WCS, and how to link them with a theme table in the configuration
- ***(time - 40:21)*** Demonstration showing X or Y orientation of vise on a trunion

### Bringing a configured fixture assembly into a Workflow template template ***(time - 41:11)***
- ***(time - 41:35)*** Using configurations to manage the reversed jaw mode for self centering vises

### Overview of how to handle soft jaw modeling in this workflow ***(time - 48:12)***

### Summary of how to leverage the container method to speed up programming ***(time - 50:26)***

### Review of everything ***(time - 53:37)***




