# Fusion-CAM-Workflow-Template-Framework

This Fusion Assembly acts as a framework for CAM programming, and configurable fixturing 

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


# Overview of how to integrate a Workholding Assembly File into a Workflow Template 14:34

# Get started with the sample Workholding Assembly File 16:31 


