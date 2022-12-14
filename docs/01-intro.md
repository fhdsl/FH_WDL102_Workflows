


# Introduction
This guide is an introduction to designing and testing WDL workflows for beginner and intermediate users who primarily develop and run their own workflows for their own research work.  




## What is WDL?
The Workflow Description Language (WDL) is a way to specify data processing workflows with a human-readable and -writeable syntax. WDL makes it straightforward to define analysis tasks, chain them together in workflows, and parallelize their execution. 

The language makes common patterns simple to express, while also admitting uncommon or complicated behavior; and strives to achieve portability not only across execution platforms, but also different types of users. Whether one is an analyst, a programmer, an operator of a production system, or any other sort of user, WDL should be accessible and understandable.

### OpenWDL

WDL was originally developed for genome analysis pipelines by the Broad Institute. As its community grew, both end users as well as other organizations using WDL for their own software, it became clear that there was a need to allow WDL to become a true community driven standard. The OpenWDL community has thus been formed to steward the WDL language specification and advocate its adoption.

There is ongoing work on WDL the specification, thus it has multiple versions.  Currently there are three versions to note:
- draft-2 - this version was the version that much of the Broad's public facing documentation and example workflows were written in. 
- 1.0 - this is a more recent version that is the most up to date version of the specification that Cromwell can interpret.  We use WDL 1.0 at the Hutch when we use Cromwell.
- 1.1 - this is an even more recent version that not all WDL engines support yet. 

We'll be using WDL 1.0 in this course but you can always check out the [openwdl repo](https://github.com/openwdl/wdl) for more information about tweaking these instructions for different versions of WDL.  


### Documentation about the WDL Spec

To begin learning how to write WDL itself, the best (and evolving) resource is the [WDL-Docs](https://wdl-docs.readthedocs.io/en/stable/) site being developed by the OpenWDL community.  There you'll find examples and guidance about the 1.0 WDL specification and using it to write workflows.  


### Tools to Edit WDL

[VSCode](https://code.visualstudio.com/) has multiple extensions for WDL, including "WDL DevTools" and  "WDL Syntax Highlighter" which can be very helpful for color-coding content and detecting issues while you're writing, before workflow validation.  

Also, there are a variety of extensions that can make working with the input files in json format much easier.  They can be used to detect errors, color code them, format them in ways more easy to view as a human.  Some examples include, `json`, `Prettify JSON`, or `JSON (Beautify JSON)`.  

##  What Is a WDL Engine?

A WDL engine is software that understands WDL and can interpret the instructions in order to execute the computational tasks involved in the workflow.  An example of a WDL engine, one used at the Fred Hutch, is Cromwell.  Cromwell is a software tool that can be used to coordinate and streamline the actual running of bioinformatic (or other analytic) workflows. It is the software that underlies the Broad Institute's [Terra platform](https://terra.bio/).  It's one of several tools and platforms that are able to run workflows written in the WDL language, including [miniWDL](https://miniwdl.readthedocs.io/en/latest/)from the Chan-Zuckerburg initiative, [DNANexus](https://www.dnanexus.com/), and other emerging tools.  

## Why use WDL for my research?

### Abstracting Storage and Computing Details
At many research institutions we have many resources for data storage, software installations and high performance computing available to researchers, which themselves are evolving.  Using a workflow manager that is configured specifically to the types of data storages available at an institution can provide the benefits of "abstracting" or hiding some degree of the logistics involved with actually accessing those data for use in an analysis from the user.  

For instance, beyond data storage in a local file system, researchers now may have the ability to store data in AWS S3 storage.  Accessing those data for the purposes of using them in workflows requires slightly different knowledge and mechanisms of retrieval (e.g. knowledge of how to use the AWS CLI).  By providing one configuration (or set of instructions for accessing data) for Cromwell that is tailored to what is available at the institution, any individual researcher can use Cromwell as an interface to their data, wherever they may be stored.  This intermediate tool then reduces the amount of time and effort on the part of the researcher in understanding the data storage flavor of the day, allowing them to focus more on their data and their analyses.

An even more valuable benefit exists for high performance computing resources.  When Cromwell is used as an intermediary, the backend configurations (or instructions for how to send jobs to HPC resources) can be defined once for all those resources currently available to Hutch users (such as our local SLURM cluster or AWS Batch for cloud computing).  This means that individual users do not have to become SLURM or AWS Batch experts in order to be able to run jobs on them.  They instead can focus on building their workflow and analyzing their results.  

### Reproducibility and Portability
Beyond the benefits of abstracting some of the institution-specific details from the analysis itself, this creates a convenient side effect.  The lack of need to tailor a workflow itself to the unique combination of data storage and computing resources, means that researchers focus their time on developing workflows using the workflow language WDL.  WDL is an open source workflow description language that one can use to describe the inputs, environments and outputs of individual tasks and how they are strung together for an analysis.  By creating workflows using tasks with these features defined, it makes these workflows far more reproducible (e.g. running a given workflow on any compute infrastructure will create results that are identical), and far more portable.  In academia where collaborations and career transitions mean potential loss of time spent tailoring analyses to various institutions' infrastructure is the name of the game, this benefit holds substantial value.  

### Software and Environments
One challenge associated with reproducibility and portability that is something often undervalued in many bioinformatic analyses, is the management of computing software and environments.  As projects evolve, they often begin in a testing phase with small datasets using readily available software or packages on a desktop computer or even potentially on a shared, remote computing resource. As they scale, it often is natural to simply do X, but Y times, all using the same computing environment of the user who originally developed the workflow.  

While this is common, it also creates headaches later. Using a workflow manager like Cromwell as a tool for deploying your analyses also means that you can begin your work directly by developing a WDL and running the tests via Cromwell. As you develop your workflow and want to scale it to an entire analysis, you can simply edit your workflow and Cromwell will keep track of all the tasks and their interrelations.

While workflows are being developed in the context of a WDL for running with Cromwell (or other WDL runners that are being developed), each task must be defined to occur in a specific environment.  That could be a user's environment on the local SLURM cluster, or that could be a clean environment with specific software packages loaded, or it could be a docker or singularity container that directly specifies the software and environment in which to run the task. 




