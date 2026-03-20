---
title: 'Flexible, Automated Potentiostat/Galvanostat Control Software for Electrochemical Analysis'
tags:
    - Python
    - potentiostat
    - galvanostat
    - battery
    - supercapacitor
    - electrochemistry
    - electrochemical analysis
authors:
  - name: Alexander J. H. Bell
    orchid: 0009-0001-0654-5656
    affiliation: "1, 2"
  - name: James A. Dawson
    orchid: 0000-0002-3946-5337
    affiliation: "2"
  - name: Michael R. C. Hunt
    orchid: 0000-0002-3797-3768
    affiliation: "1"
affiliations:
  - index: 1
    name: Department of Physics, Durham University, Durham, DH1 3LE, United Kingdom
  - index: 2
    name: Chemistry -- School of Natural and Environmental Sciences, Newcastle University, Newcastle upon Tyne, NE1 7RU, United Kingdom
bibliography: paper.bib
---

# Summary
We present an open-source graphical user interface (GUI)-based Python program for running and automating electrochemical experiments supporting cyclic voltammetry (CV), linear sweep voltammetry (LSV), Galvanostatic charge/discharge cycling (GCD), chronoamperometry (CA), chronopotentiometry (CP), self-discharge (SD), and rate-testing. Designed to work with low-cost hardware, and specifically an open-source potentiostat/galvanostat design by @dobbelaere2017usb, it provides users with access to electrochemical research capabilities previously constrained to far more expensive apparatus. The program was developed with the following core objectives:

- Queuing and automation of experiments.
- Support for complex experiment workflows, *e.g.*, automated open-circuit potential (OCP) equilibration with the use of "OCP" as an input parameter.
- Accessibility to users with little experience performing electrochemical experiments *via* input parameter validation, GUI dialogue boxes, and comprehensive hover-over tooltips.
- Minimal memory usage by clearing data buffers and enabling of widget resizing *via* GUI software options menu to support a range of hardware resolution and memory configurations.

# Statement of need
Low-cost, "do-it-yourself" (DIY) potentiostats are increasingly widening in utility, with the `CheapStat` [@rowe2011cheapstat], `DStat` [@dryden2015dstat], and `HunStat` [@vamos2024hunstat] providing users the ability to conduct electrochemical experiments at a lower cost than with commercial hardware. However, these are constrained to potentiostatic experiments across limited ranges (\pm$1~\mathrm{V}$ for the `CheapStat` and `DStat`, and \pm$1.65~\mathrm{V}$ for the HunStat). This capability was greatly extended by the `USB Potentiostat/Galvanostat` design presented by @dobbelaere2017usb, with a \pm$8~\mathrm{V}$ potential range enabling the investigation of battery cells at much higher cell potentials. The addition of a galvanostatic mode, and the ability to accurately measure and apply current from nanoamps to \pm$25~\mathrm{mA}$, has significantly broadened the suite of electrochemical experiments accessible to low-cost DIY devices, with the lowest-cost commercial substitute around 20 times more expensive at \$1900 [@squidstatsolo]. The `MYSTAT`, a modification of the `USB Potentiostat/Galvanostat`, pushes this capability even further, achieving maximum potential and current ranges of \pm$12~\mathrm{V}$ and \pm$200~\mathrm{mA}$, respectively [@irving2021mystat].

As is typical for low-cost open source devices, the accompanying GUI software is limited, however, to a narrow range of experiments (CV, GCD, and rate-testing experiments for the `USB Potentiostat/Galvanostat` and the `MYSTAT`), single-experiment execution, and basic input parameters [@caux2022passstat;@kellner2015new;@rowe2011cheapstat;@dryden2015dstat;@vamos2024hunstat;@dobbelaere2017usb;@irving2021mystat]. Commercial potentiostat/galvanostat instruments, whilst broader in utility, lack open source and customisable software for experiment scheduling, data management, and advanced control. 

Our software bridges this gap and provides a flexible, extendable, and user-friendly GUI controller for potentiostat/galvanostat systems based on the @dobbelaere2017usb design, supporting complex experiment workflows and advanced control features. It equips users of the low-cost DIY `USB Potentiostat/Galvanostat` with functionality often confined to the proprietary software of far more expensive instruments, thereby significantly enhancing the accessibility and experimental capabilities for researchers within the low-cost DIY potentiostat/galvanostat ecosystem.

# Features
- Supports CV, LSV, GCD, CA, CP, SD, and rate-testing experiments.
- Experiment queueing using comma-separated variable lists as input parameters.
- Complex experiment workflows by supporting a range of parameter inputs such as potential limits of "1.0, OCP, 2.0, ..." or ramp rates of "5.0, STEP, 10.0, ...", *etc.*
- Potential and current limits to prevent unintended damage to the cell under testing, and equilibration parameters to advance experiments when equilibration conditions are satisfied.
- Alternating potential/current sequences for CA/CP experiments, allowing the user to provide alternating equilibration conditions within the experiment.
- Hover-over tooltips to guide users on supported parameters and their experimental implications, and parameter validity checks with detailed user feedback and correction suggestions.
- Automated OCP equilibration and/or fixed-delay between experiments.
- Automated calculation of 1C capacity for rate-testing using GCD analysis.
- Real-time data logging for individual experiments and experiment progress information logging to a summary file for all queued experiments.
- Progress bars displaying the remaining time, cycles, segments, *etc.*, until all experiments are complete.
- Real-time data visualisation options, including comparisons between experiments within the same queue.
- Post-experiment data visualisation.
- Compatible with Linux, Windows, and Mac operating systems and designed to run on light-weight hardware.
- Can be straightforwardly modified for compatibility with the `MYSTAT`.

# Software design
This software was designed to extend the experimental scope of low-cost potentiostat/galvanostat hardware whilst preserving usability for existing users. To ensure continuity for users familiar with using and modifying the basic program originally supplied by @dobbelaere2017usb, a key design decision was made to retain the core experiment execution workflow whilst allowing significant expansion. A new codebase was required to support the expanded suite of experiments, safety features (parameter validation, cutoff potential limits, *etc.*), automation, complex parameter sequencing, and user-friendly features (*e.g.*, progress bars and flexible plotting options). 

The software adopts a systematic, experiment-centric architecture: Each experiment of a given type follows a similar protocol (input validation, initialisation, equilibration, execution and data-logging, then termination), simplifying extension. Parameter validation and experiment queue construction is performed prior to execution, ensuring safer operation and reduced runtime failure for long automated runs. Data buffers are cleared between experiments, preserving only the minimal relevant data for the plotting options provided, improving compatibility with lightweight hardware. Termination functions allow experiments to stop gracefully and allow seamless running of future experiments. 

`PySide6` [@pyside6], utilising the `Qt6` framework, was chosen for the GUI architecture over legacy `Qt5` frameworks to ensure forward compatibility across Linux, Windows, and macOS systems, at the cost of dropping support for older Python/`Qt` environments. 

# Research impact statement
This software delivers a novel level of experimental control and automation for low-cost, open-source potentiostat/galvanostat systems, enabling experimental workflows that previously required proprietary commercial instruments. The automated OCP equilibration and 1C capacity calculation, alternating potential/current sequences (with alternating equilibration conditions), and experiment queueing, all reduce manual intervention substantially, and enable greater consistency when running experiments. 

An emphasis on parameter validation, explanations of incompatible parameters and parameter warnings via GUI dialog boxes, potential and current safety limits, and comprehensive hover-over tooltips significantly improves accessibility, particularly for non-specialist users. 

Near-term impact is supported by comprehensive documentation, standardised summary file formatting guided by FAIR principles [@wilkinson2016fair], and cross-platform support. The codebase is guided by `PEP 8` standards [@van2001pep], openly licensed, and organised to encourage community extension.  

Through lowering the financial and technical barriers to performing electrochemical measurements, this software is positioned to support research, teaching laboratories, and rapid prototyping within the growing electrochemistry community. 

# Operating principles
The software is designed to be entirely controlled *via* the GUI window built using `PySide6` [@pyside6], with the users constructing their experiments using the input fields and checkboxes in the corresponding experiment tab. A `CHECK` button for each tab executes an input parameter validation, guiding the user to invalid or incompatible inputs using dialogue boxes. A green `CHECK` button indicates to the user that their experiments are valid and ready to run. An example tab for rate-testing experiments with valid input parameters can be seen in \autoref{fig:rate_tab}. Universal parameters such as the OCP equilibration timescale, tolerance, and timeout threshold, together with more advanced experiment-specific features, are accessible through a software options menu.

The program sequentially executes the queued experiments, handles real-time logging of raw data to individual experiment data files, and writes experiment progress information to a human-readable summary file for all queued experiments. An example GUI seen when running cyclic voltammetry experiments, showing the real-time data visualisation options available to the user, is displayed in \autoref{fig:CV_GUI}.

All experiments are written in a similar code layout and described by detailed comments to facilitate users in modifying or adding experiments if required. The typical execution stack that handles the initialisation, running, and stopping of experiments is illustrated in \autoref{fig:flowchart}.

The summary file contains experiment metadata written in a format standardised across experiment types. Users may optionally write experiment notes within the GUI to be included at the top of the summary file to assist with post-experiment data contextualisation. An example summary file generated from the CV experiments running in \autoref{fig:CV_GUI} is displayed in \autoref{fig:summary_file}.

To improve software and experimental accessibility, hover-over tooltips are featured for all input parameter fields and checkboxes, guiding the user on their purpose and accepted parameters. An example tooltip is displayed in \autoref{fig:tooltip}.

![The rate-testing experiment tab showing user input fields and checkboxes, with a green `CHECK` button indicating that the experiment parameters are valid.\label{fig:rate_tab}](figures/rate_tab.png){width="70%"}

![An example GUI during a cyclic voltammetry experiment showing real-time data visualisation options, experiment progress information, and optional experiment notes to be written to the summary file.\label{fig:CV_GUI}](figures/CV_GUI.png){width="100%"}

![Flowchart of the typical execution stack for a set of experiments.\label{fig:flowchart}](figures/flowchart_mainloop.png){width="100%"}

![Sections of the summary file generated for the CV experiments seen running in \autoref{fig:CV_GUI}, with a base filename of `2025-11-24`.\label{fig:summary_file}](figures/summary_file.png){width="100%"}

![A tooltip for the current input when automatically calculating 1C for rate-testing experiments.\label{fig:tooltip}](figures/tooltip.png){width="70%"}

# AI usage disclosure
The OpenAI `o1` model was used as a final check to assist in catching edge cases for more complex experiment workflows and in the user input validation functions. All AI-generated suggestions were manually reviewed by human authors and, if judged appropriate, edited, integrated, and validated by human authors. 

# Acknowledgements
This work was supported by the UK Engineering and Physical Sciences Research Council and the Renewable Energy Northeast Universities (ReNU) CDT (EP/S023836/1).

# References
