---
title: "Geometry"
url: "/openstudio-openstudio-application-geometry/"
parent: "/openstudio-openstudio-application/"
aliases: ["/OpenStudio/OpenStudio_Application/Geometry/"]
categories: ["OpenStudio Application"]
lastmod: "2025-05-04T03:29:46Z"
---

The OpenStudio Application supports four file types for creating or import geometry for building energy models. The following is based on the OpenStudio Application description of the different kinds of files formats and processes used to create, import and modify building geometry.

## Overview
Choose a geometry type from the list above. FloorspaceJS allows you to create new geometry based on 2D floorplans. Alternatively, you can import existing geometry in gbXML, IDF, or OSM formats. Preview what your geometry will look like after translation to OSM format using the "Preview OSM" button. Once you are satisfied with your geometry, press the "Merge with Current OSM" button to merge your new geometry with your current OSM model. Your current OSM model will not be changed until "Merge with Current OSM" is pressed.
## FloorspaceJS
[FloorspaceJS](https://github.com/NREL/floorspace.js) is an open source software module that developers can leverage to produce building energy modeling UIs which include geometry creation. FloorspaceJS is meant to cover simple building geometry use cases only. More complex building geometry is best developed in a full featured CAD tool and exported to gbXML or other formats for building energy modeling. FloorspaceJS is implemented in JavaScript with minimal dependencies, allowing it to be integrated into a wide range of applications, including the OpenStudio Application.
## gbXML
[gbXML](https://gbxml.org/) is an industry supported XML schema for sharing building information between Building Energy Modeling (BEM) tools. Many BEM tools can author gbXML, a partial list is available online at http://gbxml.org/Software_Tools_that_Support_GreenBuildingXML_gbXML. A gbXML viewer developed by Ladybug Tools, https://github.com/ladybug-tools/spider-gbxml-tools/tree/develop/gbxml-viewer-basic, has been embedded in the OpenStudio Application to preview gbXML before it is translated to OSM. Ladybug Tools has also developed a more full-featured tool, including the ability to inspect and edit gbXML files, which is available at https://www.ladybug.tools/spider/gbxml-viewer.
## IDF
The EnergyPlus Input Data Format (IDF) is the file format read by the [EnergyPlus](https://energyplus.net/) simulation engine. Several tools are available which can author IDF, a partial list is available online at http://www.buildingenergysoftwaretools.com/. A custom viewer has been implemented which displays the contents of the IDF file in epJSON format. The EnergyPlus executable shipped with OpenStudio is used to convert IDF to epJSON format. For best results, please ensure that your IDF files are updated to the version of EnergyPlus used by this version of OpenStudio. IDF files can be updated to the latest version of EnergyPlus using the EP-Launch utility distributed with the full EnergyPlus installer (it is not included with OpenStudio).
## OSM
The OpenStudio Model (OSM) file format is the native file format used by [OpenStudio](https://openstudio.net/). OSM files authored in separate workflows may be imported and merged with the current OpenStudio Model.
## Merge OSM
The OpenStudio SDK includes translators from a variety of common Building Energy Modeling (BEM) formats to OSM. Geometry from these formats can be converted to OSM and used as a starting point for more detailed OpenStudio Model adjustments such as defining schedules, constructions, and HVAC. A new merge feature was recently added to the OpenStudio SDK. This feature supports iterative workflows where:
1. Initial OSM geometry is translated from an original format (e.g. gbXML, IDF, etc)
1. Non-geometry modifications (e.g. schedules, HVAC, etc) are made to the OSM
1. Updated geometry is available in the original format
The merge feature allows updated geometry to be merged with the current OSM while also preserving non-geometry modifications.

## Miscellaneous Guides
## Creating Space in FloorspaceJS
{{< wiki-video src="/media/os-app-floorspace-js-creating-space.mp4" poster="/media/os-app-floorspace-js-creating-space.png" title="OSApp FloorspaceJS Creating Space" >}}

## IFC to gbXML
Based off this [OSArch community forum post](https://community.osarch.org/discussion/comment/18046). It is possible to convert from IFC to gbXML. The tool [IFC-to-gbXML-converter](https://github.com/brunopostle/IFC-to-gbXML-converter) utilizes a Python script to convert from IFC to gbXML.

## Designing Curved Walls
Based on this [UnmetHours post](https://unmethours.com/question/11621/how-to-design-curve-walls/).

At the [2015 ASHRAE Energy Modeling Conference](https://www.ashrae.org/file%20library/conferences/conference%20resources/past%20ashrae%20conferences/2015-energy-modeling-conference-atlanta--ga--september-30-october-2--2015.pdf) in Atlanta, there was a discussion on result accuracy of an energy model depending on the number of planar surfaces in use. Specifically when creating a circle, it was found that 4 surfaces was too simplistic, but using 24 surfaces yielded similar results to 72 surfaces. However regardless of the number of surfaces, ensure that the zone volume and exterior surface area is matched accurately. 

In addition, for further analysis, it is possible to perform a rough sensitivity analysis by increasing the number of surfaces, for example from 8 to 12, then from 12 to 16, until the difference between the successive simulations is less than an acceptable amount.

## Adding Non-Rectangular Windows
Based on this [UnmetHours post](https://unmethours.com/question/9620/adding-non-rectangular-windows-to-an-openstudio-model/).

OpenStudio can only use rectangular or triangular windows, so if the user has a unusually shaped window, the shape of the window will need to be broken down into those shapes for the EnergyPlus simulation to run successfully.

## Modeling Columns and Beams
[EnergyPlus](/energyplus/), the simulation engine that OpenStudio uses, doesn't require the user to explicitly define 3D surfaces. Instead, 2D surfaces are assigned [constructions](https://bigladdersoftware.com/epx/docs/25-1/input-output-reference/group-surface-construction-elements.html#construction-000) which are made up of layers of [materials](https://bigladdersoftware.com/epx/docs/25-1/input-output-reference/group-surface-construction-elements.html#material-and-material-properties). The properties of these individual materials, including thickness, are what determines the energy performance of the building envelope.

It is not necessary to create beams and columns to simulate energy performance, all that's required is a 2D OpenStudio surface. Once the surface is created, the user can define the properties of the beam and column constructions.

An understanding of conduction heat transfer in order to properly define overall thermal properties of the beam/column ceiling, floor or wall construction (including air gaps, insulation, etc.).

To confirm and view thermal performance post-simulation you can look in the **Envelope Summary** section of the eplusbl.htm file. Additionally, you can use or write an EnergyPlus Measure for OpenStudio (see [Measures](/openstudio-openstudio-application-measures/) and NREL's [OpenStudio Measure Writer's Reference Guide](http://nrel.github.io/OpenStudio-user-documentation/reference/measure_writing_guide/)) to add additional output file formats and the change the unit conversion from SI to IP (see the OutputControl:Table:Style object in the Input-Output Reference PDF).

The *Building Envelope Thermal Bridging Guide* published by BC Hydro Smart, including a comprehensive list of material data, and construction assemblies which provide overall thermal performance characteristics that can be used in the OpenStudio model.

## Common Beam Materials
Based on this [UnmetHours post](https://www.archterms.com/what-is-a-beam-in-construction-everything-you-need-to-know/).

Beams can be constructed from several materials. The appropriate material depends on the structural requirements, building type, span, cost, construction methods, and architectural objectives.
Reinforced Concrete Beams

Reinforced concrete beams are extremely common in modern construction. Concrete is strong in compression but comparatively weak in tension. Steel reinforcement is therefore placed inside the concrete to resist tensile forces.

Reinforced concrete beams can be cast on site or manufactured as precast components. They are widely used in residential, commercial, industrial, and institutional buildings.
Steel Beams

Steel beams provide high strength relative to their weight and are particularly useful for long spans and heavily loaded structures. Common steel beam sections include I-beams, H-beams, channels, and box sections.

Steel beams are frequently used in commercial buildings, warehouses, industrial structures, bridges, and high-rise construction.
Timber Beams

Timber beams have been used in construction for thousands of years. Modern timber construction uses both solid-sawn timber and engineered wood products such as glulam and laminated veneer lumber.

Timber beams are common in residential buildings and can also be used for architectural exposed structures where the natural appearance of wood is desired.
Composite Beams

Composite beams combine two or more materials so that they work together structurally. A common example is a steel beam supporting a reinforced concrete floor slab. Properly designed connections allow the steel and concrete to share structural loads.

### Related OpenStudio Topics
- [Constructions](/openstudio-openstudio-application-constructions/)

### Related UnmetHours Posts
- [how to design the columns and beams?](https://unmethours.com/question/1304/how-to-design-the-columns-and-beams/)
- [How do I see what my overall u value is when I build constructions using materials in OpenStudio?](https://unmethours.com/question/747/how-do-i-see-what-my-overall-u-value-is-when-i-build-constructions-using-materials-in-openstudio/)
- [Why don't my construction assembly values show up in my simultation while they are clearly input into in my .idf](https://unmethours.com/question/1065/why-dont-my-construction-assembly-values-show-up-in-my-simultation-while-they-are-clearly-input-into-in-my-idf/)

### External Resources
- Building Envelope Thermal Bridging Guide ([2016 Lite Version](https://web.archive.org/web/20220901054651/https://www.bchydro.com/content/dam/BCHydro/customer-portal/documents/power-smart/builders-developers/building-envelope-thermal-bridging-guide-1.1.pdf), [2021 Version](https://web.archive.org/web/20250113221333/https://www.bchydro.com/content/dam/BCHydro/customer-portal/documents/power-smart/builders-developers/building-envelope-thermal-bridging-guide-v1-6.pdf#expand))
- [UnmetHours post](https://www.archterms.com/what-is-a-beam-in-construction-everything-you-need-to-know/).
