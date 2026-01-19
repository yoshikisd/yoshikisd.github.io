---
layout: page
title: Various projects on a pulsed laser deposition tool
description: A DUV-laser-based physical vapor deposition tool that I took care of during grad shool, performing both maintenance, development, and making epitaxial film growth recipes.
img: assets/img/pld1.png
importance: 2
category: Tool operation/development
related_publications: true
---
<center><div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pld1.png" title="pld1" width="400"%}
    </div>
</div></center>
<div class="caption">
    Pretty plasma plume (white and blue ellipse/dome) produced by a pulsed DUV laser shot on a target material surface (bottom half of the image). The plasma plume deposits the target material on the substrate (sitting upside down on the cylindrical column, top half of the image).
</div>

During my PhD studies I used and took ownership over a high vacuum, DUV excimer laser deposition tool that performed pulsed laser deposition (PLD). Here I include the various projects I worked on in relation to this tool.

### **What is PLD?**
PLD is a laser-based physical vapor deposition technique used for the epitaxial growth of heterostructures with complex compositions and well-controlled interfaces. With optimized growth parameters, PLD offers the ability to perform stoichiometric transfer of elements from a target material to a substrate surface, growing an epitaxial film whose thickness can be controlled with atomic level precision.

### **Projects**
#### **Optimizing thin film deposition process for collaborative research**
The ability for PLD to perform stoichiometric elemental transfer makes it particularly suited to perform epitaxial deposition of magnetic complex oxides (such as La<sub>0.7</sub>Sr<sub>0.3</sub>MnO<sub>3</sub>) whose magnetic and electronic properties are sensitive to changes in its stoichiometry. With PLD, you can use a target material with the exact same composition as the film you desire.

That said, the processing parameters of the tool (laser fluence, number of laser shots, substrate temperature, oxygen flow rate) need to be optimized to grow **epitaxial films with the desired thickness (~10-100 nm), structural quality (coherently-strained single crystal), and magnetic quality (hysteresis and M-vs-T curves consistent with the literature)**. To achieve this for the La<sub>0.7</sub>Sr<sub>0.3</sub>MnO<sub>3</sub> and La<sub>0.7</sub>Sr<sub>0.3</sub>FeO<sub>3</sub> films I grew for collaborators, I designed experiments to systematically optimize the processing parameters, where each step in the optimization was informed by data gathered from sample characterization methods including XRD/XRR, VSM, and AFM.

My scientific collaborators used these films I made and did some neat scientific work investigating new magnetic and transport physics in these material systems, leading to 8 publications as of Jan. 2025:
{% cite LSMO_barrier_2021%}{% cite PhysRevB.107.054415%}{% cite PhysRevB.108.174434%}{% cite doi:10.1073/pnas.2317944121%}{% cite doi:10.1021/acs.nanolett.4c02697%}{% cite 10.1063/5.0239675%}{% cite ghazikhanian2025electricallyinducedferromagnetismirradiated%}{% cite tang2025tunablemagnonphononcavitystructural%}

#### **Doubling accessible laser fluence range**
The laser fluence (energy density) is an important parameter that influences the quality of the grown film. If the fluence is too low, then the target material will not be properly ablated, hampering stoichiometric transfer of material. Too high of an energy may induce particle emission from the target and/or create defects due to bombardment from high kinetic energy species.

For a given PLD tool, the fluence needs to be optimized on a per-material basis owing to their different absorption behaviors. For the tool I used during graduate school (from NBM Design with a Coherent COMPex laser), **a laser fluence of ~1 J/cm<sup>2</sup> was accessible** and served well for growing materials such as La<sub>0.7</sub>Sr<sub>0.3</sub>MnO<sub>3</sub>. However, other materials, such as LaFeO<sub>3</sub>, may require higher fluences (i.e., ~2 J/cm<sup>2</sup>).

In order to expand the range of fluences accessible to the tool, I designed and introduced a translatable focusing lens that enables users to reproducibly change the laser beam focus on the target. By allowing users to focus the beam size down to ~1/2 of the original beam size, **the tool can now access fluences up to 2 J/cm<sup>2</sup>, enabling the deposition of new materials such as LaFeO<sub>3</sub>**.