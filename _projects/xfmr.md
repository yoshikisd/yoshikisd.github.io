---
layout: page
title: Enhancing the sensitivity of a hybrid GHz/x-ray spectrometer
description: A failed materials science project pivoted into a development effort aimed at resolving measurement sensitivity issues in a spectrometer tool key to the project's success.
img: assets/img/xfmr.png
importance: 1
category: Tool operation/development
related_publications: true
---

During my PhD studies I led a project at the Advanced Light Source (ALS) investigating magnetic oxide thin films using a hybrid x-ray/microwave spectroscopy technique called x-ray detected ferromagnetic resonance (XFMR). Early in the project, it became evident that the XFMR setup (at ALS Beamline 4.0.2) did not have sufficient sensitivity to measure the types of samples I wanted to study. Because of this issue, this materials-research effort transitioned into a tool-development effort **with the goal of improving the tool sensitivity, enabling it to measure a wider range of samples than before**.

Through experiments I designed with different detection hardware (photodiodes), along with troubleshooting and eliminating sources of microwave artifact signals, **I was able to improve the sensitivity of the tool by ~8x.**

Unfortunately, by the time I had completed this development effort, my time at the ALS had come to an end and I wasn't able to complete the science I originally set out to perform. Oh well.

A detailed summary of this work lives in the appendix of my PhD dissertation {% cite daynedissertation%}. Here, I attempt to give a short, "simple" explanation and will in the process omit some lower-level details.

### **What is XFMR?**
**XFMR** is an experimental technique that combines three spectroscopic methods: X-ray absorption spectroscopy (XAS), x-ray magnetic circular dichroism (XMCD), and ferromagnetic resonance (FMR). At an extremely high level...

**XAS** involves measuring how strongly a sample *absorbs* x-rays as you change the wavelength/photon energy of the incoming x-rays, which gives you information about the sample chemistry (as well as others as described below). One of the ways to measure an XAS signal of a thin film is to measure how strongly the underlying substrate of a thin film sample glows using a photodiode (this is called **photoluminescence yield**).

**XMCD** is an extension of XAS, where element-specific magnetic information about a sample is obtained by subtracting two XAS curves measured with right- and left-handed circularly polarized x-rays. *The intensity of the XMCD peak signal can be tied to how alligned the magnetic moments are with respect to the x-ray beam through a dot-product/cosine relationship*.

**FMR** is a method used to measure the sample-averaged magnetic properties of a sample by measuring the high-frequency (GHz) precessional motion of a sample's magnetization. FMR in our case involves applying a magnetic field to magnetize the sample, irradiating it with microwaves using a coplanar waveguide, and measuring how much microwave power was absorbed by the sample at different frequencies and field strengths.

**XFMR is specifically a method that involves driving magnetization precessions in a sample using FMR but studies those precessions with element-specificity by measuring time-dependent XAS-XMCD**.

<center><div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/xfmr_1.png" title="xfmr" width="800"%}
    </div>
</div></center>
<div class="caption">
    (a) Schematic of the XFMR sample setup, where the sample magnetization "M" undergoes precessional motion under an applied static magnetic field "H_static" and GHz-frequency microwave radiation "H_RF" applied with a coplanar wave guide "CPW". The time-dependent XAS-XMCD signal is measured by illuminating the sample with x-rays through a hole in the CPW and the photoluminescence signal is measured with a photodiode. (b) XAS-XMCD spectra measured on a Ni81Fe19 sample at different points during the magnetization precession in an XFMR experiment. The schematic on the far right indicates the "M" orientation (red arrow) associated with different XAS-XMCD spectra, where the XMCD peak intensity scale with the dot product of "M" with the x-ray beam propagation "k". (c) Time-dependent measurement ("delay scan") of the XMCD peak intensity at 707 eV (i.e., the vertical red bar in (b), where the numbered points correspond with the numbered spectra in (b). (a) was adapted from [1] (b,c) was adapted from [2].
</div>

<br><br>

### **Project description**
The ALS Beamline 4.0.2 is a soft x-ray tool that is capable of XAS-XMCD and XFMR measurements. These capabilities are provided by the use of dedicated "sample sticks" that have the appropriate detection hardware necessary for these measurements. Here, the "XFMR-stick" and "LY-stick" refer to sample sticks dedicated for XFMR measurements (static and time-resolved LY signals) and static LY XAS measurements, respectively.

The signal measured with the XFMR-stick are the static and time-dependent LY signal produced by the substrate that a thin film is deposited on top of. Different substrates have different LY efficiencies (i.e., conversion of x-ray flux into visible light). For this reason, high LY-efficiency substrates (such as MgO with ~0.1 efficiency [1]) are preferred for XFMR measurements. *While the selection of a suitable substrate should be partially guided by the XFMR experimental requirements (i.e., high LY efficiency), the substrate selection also depends on the desired thin film material to study; epitaxial growth demands substrates with similar crystal structures as the film material.*

In the case of my experiments, I was using La0.7Sr0.3MnO3 (LSMO) and La0.7Sr0.3FeO3 (LSFO) films on (LaAlO3)0.3(Sr2TaAlO6)0.7 (LSAT) substrates which have an efficiency ~0.004 [1]. *While sample pre-characterization with the LY-stick suggested measurements on the XFMR-stick would be feasible, XAS studies on the latter measured signals that were ~10x weaker than the former, and no XFMR signal could be detected from the magnetic samples.*

 **In order to expand the range of materials the XFMR setup can investigate (including the films I wanted to study), the LY-sensitivity of the XFMR-stick needed to be improved.**

<center><div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/xfmr_2.png" title="xfmr" width="600"%}
    </div>
</div></center>
<div class="caption">
    (a) XAS spectra (Fe L2,3) for a (001) MgO // 12 nm Ni80Fe20 / 3 nm Cu film collected using the LY-stick ("PD-LY") and XFMR-stick with several photodiode configurations ("PD-AXUV" the original configuration, "PD-A + Au mesh" the final optimized configuration, "PD-B" another photodiode that was tested). (b) Bar chart comparing the amplitude of the measured Fe L3 (~708 eV) signal acquired in (a).
</div>

To this end, I investigated and implemented methods to enhance the detection sensitivity of the XFMR tool at the ALS Beamline 4.0.2. As shown in the above figure, to bring the detection sensitivity of the XFMR-stick (PD-AXUV) up to par with the LY-stick (PD-LY), I upgraded the photodiode (PD) used in the former ("PD-A + Au mesh") to improve the LY sensitivity by ~10x.

There are several likely/known factors which prevented me from making the XFMR-stick as efficient as the LY-stick in detecting the LY signal:
1. The XFMR-stick has a ~100 micron pinhole in the coplanar waveguide that the x-rays need to pass through. Outside of this hole the x-rays (about several millimeters wide) get blocked. This is not an issue with the LY-stick.
2. The PD in the LY-stick is too big to fit in the XFMR-stick; we had to use different PDs.
3. **A 40 micron pitch gold mesh had to be placed between the sample and PD-A to eliminate an artifact signal that only appeared when the sample was irradiated with both x-rays and microwaves.** This was the trickiest part of the XFMR-stick upgrade which nearly halted the development effort; a whole discussion about root-cause analysis and testing is discussed in section A.1.4 in the appendix of my PhD dissertation {% cite daynedissertation%}.



### References
[1] C. Klewe et al, J. Synchrotron Radiat., 33(2), 12-19 (2020)<br>
[2] D. A. Arena, et al.,  Rev. Sci. Instrum., 80, 083903 (2009)<br>
[3] C. Piamonteze, et al., J. Synchrotron Radiat. 27, 1289 (2020).

