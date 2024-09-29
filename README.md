# Fusion-CAM-Workflow-Template-Framework

This Fusion Assembly acts as a framework for CAM programming, and configurable fixturing 

The file is intended to be used as described in this video lecture, and more broadly to work with the 
container method for setting up CAM programs. 
Strictly speaking the container method for programming is not needed, but it is a very strong compliment to this method. 

If you don't want to understand the background and theory behind the Workholding Library method, 
you can just skip to the usage instructions [here](#A-better-way!-working-with-ancesstor-files). 

# Core Concepts: 
    
## Simple Replacable Container 8:51
    
### Why simple joints don't work well
Replace doesn't work unless the two source components have a shared common reference to keep the joint well defined. 

A simple way to make this work is to use the component origin.
Since all components have this, when you replace a component the joint will maintain its integrity. 

What if you don't want to be stuck just using the component origin for joint snaping? 

### Construct a replacable joint anywhere on your part 10:16
1) Put the bodies you care about into a component. 

2) Add a new component at the same level of hte model higherachy, called `Joint Origin Container` (JOC). This will be an empty container. 

3) Create a new joint origin tied to the coordinate origin of JOC. This puts the joint origin inside this container. 

4) Createa a new joint, and pick the new joint origin as the snap reference for `Component 1`. Then pick any other point you like on one of the bodies as the other reference. This constrains the new joint origin to be fixed to the object. 

5) Use `Save As` to keep a common lineage of the Joint Origin Component 10:57


Once you have a couple of basic component files that share the same joint origin lineage, you can build an assemly that uses them to make replacable parts. 


# Buudling blocks for a replacable workholding library 11:57

## Workholding Assembly Hierarchy

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


## Building a Vise File 12:35

You can set up any kind of JOC structure you want in your vise files, 
but the suggested one here works for a huge variety of vises and fixturing and we recommend you start with it. 
Its important to note that all the vises you make need to start from this common file.

Once you pick your joint origin structure you will build all your vises off that root file using save-as. 
If you decide to change it later, you have to rebuild all your vise files off the new root. 

This configuration does support self centering vises with reversible jaws. The JawPosition1 and JawPosition2 JOC's are set up to support the two jaw locations, and configurations can be used to pick which one is active. 13:02

## Buidling a WCS File 13:30

The meaningful geometry is a simple cube with a centered hole that can be used to define the Z and X faces. 
Joint origins are added to define the WCS and its connection point to the zero point system. 

An actual gauging block may have more complex geometry. You can model that in for visualization purposes, but it won't be used for anything other than that. 
So we put that in the `Inconsequential Geometry` component. 

Note that there are not any JOC instances in this file. 
The joint origins are attached to the root of the model hierarchy. 

## Building a Palette File 13:54

This file is constructured in a very similar way to the vise file. 
Again, the joint origin structure used here must have a common lineage across all pallete type files and save-as is used to preserve that. 


# Mapping the joints in a Workholding Assembly File 14:34

# Download these files so you don't have to start from scratch! 16:31 

# What is a Workflow Template? 17:03

You might already be using one of these? You might also have heard it called the "Container method". 
Rob Lockwood presented the [original container method talk at a AU 20XX](https://www.autodesk.com/autodesk-university/class/Streamlining-CAM-Workflows-Templates-2019). 

Workflow templates, or container templates, are a powerful way to speed up your programming by associating toolpaths to the container so you can swap models in without having to re-click all the geometry. 

But as presented in the original talk, they have a key weakness -- a specific container template was tied to 
a single machine and workholding setup. 
If you wanted to change that out, you needed to basically start over and make a new container. 

We can integrate replacable workholding assemblies to fix this.
But first, lets just relive the pain of changing workholding or machine models using the old method. 


# The nightmare scenario we're all trying to get past when swapping workholding 18:32
This is an example of how hard it can be to swap an existing program to a new machine, new work holding, etc. 
This is the world we live in, if we don't take the time to build a replacable workholding library. 
It is not fun. 

# A better way! working with ancesstor files 22:13
## Understanding the Workholding Assembly File 23:29
## Build a Vise Type Container File 23:42
- Open the Vise ancesstor file, and right away use save-as to create a new file. Save-as is critical! 
- You are free to delete the vise geometry component. This leaves just the JOC instances. Don't delete those, they are the key to making this whole thing work. If you accidentally delete those, don't save anything. Start over from the ancestor file. 

You can never re-add a JOC once you've deleted it. Even if you give it the same name, internal information in Fusion has been lost. 

## Prep an existing vise geometry for use with the vise ancestor file 24:20 
- Open a vise geometry file. If there are existing joint origins in the source file, don't use them. Delete existing joint origins, so we can use the ones attached to our JOC in our Vise component file. 
- 25:10 Cleanup the vise model by removing any existing joints and motion links
- 25:15 Put the vise geometry into its own component
- 25:34 Make sure the origin of the container matches up with where you want the zero point to be
- 25:47 Add new motion joints to the vise. Make sure you use the origin of the container as the snap selection. This make sure the joint lives inside the component with the vise geometry. 
- 26:17 Add new motion link to couple the jaw motion together
- 26:25 Select the vise base and ground it to parent. 

After these steps we consider our Vise container fully rigged

## Copy the rigged vise into the new Vise Type Container file 26:32
- Copy the top level Vise container from the rigged file
- Go back into your Vise Type File and, right click on root component and `Paste new` 26:47
- Right click the newly pasted Vise container and select `Ground to parent`
- Create new joints between the vise geometry and the existing JOC origins 26:55





