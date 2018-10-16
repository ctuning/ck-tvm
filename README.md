# Collective Knowledge repository for TVM and VTA

[![compatibility](https://github.com/ctuning/ck-guide-images/blob/master/ck-compatible.svg)](https://github.com/ctuning/ck)
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
See [CK getting started guide](https://github.com/ctuning/ck/wiki/First-Steps)
for more details about CK.

## Coordination of development

* [University of Washnigton](http://www.washington.edu)
* [cTuning Foundation](http://cTuning.org)
* [dividiti](http://dividiti.com)

## Minimal CK installation

The minimal installation requires:

* Python 2.7 or 3.3+ (limitation is mainly due to unitests)
* Git command line client.

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

See CK installation procedures for other operating systems [here](https://github.com/ctuning/ck#minimal-installation).

## CK workflow installation for TVM 

### Installing CPU version

```
$ ck pull repo:ck-tvm
$ ck install package --tags=lib,tvm,vcpu,vllvm
```

### Installing GPU (CUDA) version

```
$ ck pull repo:ck-tvm
$ ck install package --tags=lib,tvm,vcuda,vllvm
```

## Image classification via TVM

We provided a simple example to classify images using MXNet model and TVM. You can test it as follows:

```
$ ck pull repo:ck-mxnet
$ ck install package:mxnetmodel-mobilenet-1.0

$ ck run program:image-classification-tvm --cmd_key=classify_cpu
$ ck run program:image-classification-tvm --cmd_key=classify_gpu
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

## CK virtual environment for TVM/VTA

CK support lightweight virtual environment for all packages 
(automatically setting all necessary environment variables for 
different versions of different tools natively installed on a user machine).

You can start a virtual environment for a given TVM package as follows:
```
$ ck virtual env --tags=lib,tvm
> export | grep "CK_"
```

See [this blog post](https://dividiti.blogspot.com/2018/07/enabling-virtual-environment-for.html)
about CK virtual environment.

## Related Publications

```
@article{DBLP:journals/corr/abs-1802-04799,
  author    = {Tianqi Chen and
               Thierry Moreau and
               Ziheng Jiang and
               Haichen Shen and
               Eddie Q. Yan and
               Leyuan Wang and
               Yuwei Hu and
               Luis Ceze and
               Carlos Guestrin and
               Arvind Krishnamurthy},
  title     = {{TVM:} End-to-End Optimization Stack for Deep Learning},
  journal   = {CoRR},
  volume    = {abs/1802.04799},
  year      = {2018},
  url       = {http://arxiv.org/abs/1802.04799},
  archivePrefix = {arXiv},
  eprint    = {1802.04799},
  timestamp = {Thu, 01 Mar 2018 15:00:45 +0100},
  biburl    = {https://dblp.org/rec/bib/journals/corr/abs-1802-04799},
  bibsource = {dblp computer science bibliography, https://dblp.org}
}

@article{DBLP:journals/corr/abs-1801-08024,
  author    = {Grigori Fursin and
               Anton Lokhmotov and
               Dmitry Savenko and
               Eben Upton},
  title     = {A Collective Knowledge workflow for collaborative research into multi-objective
               autotuning and machine learning techniques},
  journal   = {CoRR},
  volume    = {abs/1801.08024},
  year      = {2018},
  url       = {http://arxiv.org/abs/1801.08024},
  archivePrefix = {arXiv},
  eprint    = {1801.08024},
  timestamp = {Fri, 02 Feb 2018 14:20:25 +0100},
  biburl    = {https://dblp.org/rec/bib/journals/corr/abs-1801-08024},
  bibsource = {dblp computer science bibliography, https://dblp.org}
}
```

## Feedback

* [CK community](https://github.com/ctuning/ck/wiki/Contacts).
* [TVM community](https://tvm.ai/community)
