# Collective Knowledge repository for MXNet

[![logo](https://github.com/ctuning/ck-guide-images/blob/master/logo-powered-by-ck.png)](https://github.com/ctuning/ck)
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

## Introduction

This repository provides high-level, portable and customizable Collective Knowledge workflows
for [TVM and VTA](http://tvm.ai).
It is a part of our long-term community initiative
to unify and automate AI, ML and systems R&D
using [Collective Knowledge Framework (CK)](http://cKnowledge.org),
and to collaboratively co-design efficient SW/HW stack for AI/ML
during open [ACM ReQuEST competitions](http://cKnowledge.org/request)
as described in the [ACM ReQuEST report](https://portalparts.acm.org/3230000/3229762/fm/frontmatter.pdf).
All benchmarking and optimization results are available 
in the [public CK repository](http://cKnowledge.org/repo).

## Coordination of development

* [University of Washnigton](http://www.washington.edu)
* [cTuning Foundation](http://cTuning.org)
* [dividiti](http://dividiti.com)

## Minimal CK installation

The minimal installation requires:

* Python 2.7 or 3.3+ (limitation is mainly due to unitests)
* Git command line client.

### Linux/MacOS

You can install latest CK via PIP (with sudo on Linux) as follows:

```
$ sudo pip install ck
```

You can also install CK in your local user space without sudo as follows:

```
$ git clone http://github.com/ctuning/ck
$ export PATH=$PWD/ck/bin:$PATH
$ export PYTHONPATH=$PWD/ck:$PYTHONPATH
```

### Windows

First you need to download and install a few dependencies from the following sites:

* Git: https://git-for-windows.github.io
* Minimal Python: https://www.python.org/downloads/windows

You can then install CK as follows:
```
 $ pip install ck
```

or


```
 $ git clone https://github.com/ctuning/ck.git ck-master
 $ set PATH={CURRENT PATH}\ck-master\bin;%PATH%
 $ set PYTHONPATH={CURRENT PATH}\ck-master;%PYTHONPATH%
```

## CK workflow installation for TVM 

### CPU

```
$ ck pull repo:ck-tvm
$ ck install package --tags=lib,tvm,vcpu
```

### GPU

```
$ ck pull repo:ck-tvm
$ ck install package --tags=lib,tvm,vcuda
```

## CK workflow for VTA (deep learning accelerator stack)

We provided CK workflows, packages and programs for [VTA (the Versatile Tensor Accelerator)](https://docs.tvm.ai/vta/index.html)
- an open, generic, and customizable deep learning accelerator with a complete TVM-based compiler stack.
It successfully participated in the [1st ACM ReQuEST tournament](http://cknowledge.org/request-cfp-asplos2018.html) 
(see [GitHub submission](https://github.com/ctuning/ck-request-asplos18-mobilenets-tvm-arm)
and the [associated paper in the ACM DL](https://dl.acm.org/citation.cfm?doid=3229762.3229764))
and we have eventually moved it to this common ck-tvm repository.

### VTA with a Pynq FPGA board

First, setup your Pynq board as described [here](https://docs.tvm.ai/vta/install.html#pynq-board-setup).
We suggest you to have a fast card of 16GB

Let us consider that the IP of your board is 192.168.2.99. 
Connect to this board using SSH and "xilinx" for both username and a password:
```
$ ssh xilinx@192.168.2.99
```

You can then install CK and pull CK repositories with TVM/VTA workflows and some data sets:
```
$ sudo pip install ck

$ ck pull repo:ck-tvm
```

You can try to start VTA server via CK:
```
$ ck run program:tvm-vta-pynq-server --sudo
```

Note that CK will attempt to automatically detect available compilers, Python and Pynq DMA library, 
build VTA with TVM run-time and start server on port 9091. CK may occasionally ask you to make 
a choice when more than one version of a required dependency is found - in most of the cases, you can just press Enter
to select the default one.

Now you can setup your host machine. We expect that you already have CK installed. 
Just pull the same repositories and run image classification example as follows
(note that you need to use --env.INIT_PYNQ only once to upload bitstream and reconfigure run-time):

```
$ ck pull repo:ck-tvm

$ ck run program:image-classification-vta-pynq --env.INIT_PYNQ
```

You can also specify a different host or port for your FPGA board as follows:
```
$ ck run program:image-classification-vta-pynq --env.INIT_PYNQ --env.CK_MACHINE_HOST=192.168.2.99 --env.CK_MACHINE_PORT=9091
```

CK will also attempt to detect required dependencies (such as LLVM compiler), install missing ones,
build TVM for FPGA and will run image classification example. You can then select some image
and obtain classification result.

If you encounter issues with this CK workflow, feel free to get in touch with the CK authors 
using mailing list of Slack channel [here](https://github.com/ctuning/ck/wiki/Contacts).

### VTA with a simulator

If you don't have an FPGA board, you can use an integrated simulator on your host machine.
You can do it as follows:
```
$ ck pull repo:ck-tvm

$ ck run program:image-classification-vta-sim
```

CK will attempt to build a TVM/VTA version with a simulator target, and will perform image classification using this simulator.

## Related Publications

```
@article{DBLP:journals/corr/ChenLLLWWXXZZ15,
  author    = {Tianqi Chen and Mu Li and Yutian Li and Min Lin and Naiyan Wang and Minjie Wang and Tianjun Xiao and Bing Xu and Chiyuan Zhang and Zheng Zhang},
  title     = {MXNet: {A} Flexible and Efficient Machine Learning Library for Heterogeneous Distributed Systems},
  journal   = {CoRR},
  volume    = {abs/1512.01274},
  year      = {2015},
  url       = {http://arxiv.org/abs/1512.01274},
  archivePrefix = {arXiv},
  eprint    = {1512.01274},
  timestamp = {Wed, 07 Jun 2017 14:40:48 +0200},
  biburl    = {http://dblp.org/rec/bib/journals/corr/ChenLLLWWXXZZ15},
  bibsource = {dblp computer science bibliography, http://dblp.org}
}

@inproceedings{ck-date16,
    title = {{Collective Knowledge}: towards {R\&D} sustainability},
    author = {Fursin, Grigori and Lokhmotov, Anton and Plowman, Ed},
    booktitle = {Proceedings of the Conference on Design, Automation and Test in Europe (DATE'16)},
    year = {2016},
    month = {March},
    url = {https://www.researchgate.net/publication/304010295_Collective_Knowledge_Towards_RD_Sustainability}
}

```

## Feedback

* [CK community](https://github.com/ctuning/ck/wiki/Contacts).
* [TVM community](https://tvm.ai/community)
