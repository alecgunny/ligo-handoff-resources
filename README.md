# Handing off LIGO Projects
## Talks and Docs
I've thrown most anything that could be considered remotely useful into [this Google drive](https://drive.google.com/drive/folders/1S9b1JlFAjUmav0ERiKTp7FuYLT5vN7Fq?usp=sharing).
There's probably quite a bit of duplicate material, and certainly some unrefined material as well, but to highlight some of the more noteworthy things:
- [Georgia Tech CRA Seminar](https://docs.google.com/presentation/d/1CQG62Z0QeVOqJc4Jep8_I3Tn_N5ZLEaiGROeGCwB6II/edit?usp=drive_link) on building an ML ecosystem for GW astronomy
- [APS 2023 talk](https://docs.google.com/presentation/d/1Itkx4BStQ_L2dvnoQCpXwgOMlXBuSBELJApFCxardqs/edit?usp=drive_link) on `ml4gw` and `hermes`
- [FlexScience 2022 talk](https://docs.google.com/presentation/d/1qpL6IV4_2c5HfUyfoQ9bEnv3T37dJmOMXtdBpGgbIYM/edit?usp=drive_link) on applying these libraries to a production DeepClean
- [The orgininal IaaS paper presentation](https://docs.google.com/presentation/d/1lorE_GwMP-4QoqhBQWkt43Klzkw-PnUck0A6JF-amPQ/edit?usp=drive_link). Mostly a historical artifact at this point.

This folder also contains folders for a lot of diagrams I've made over the years on diagrams.net, where you can open them, create copies and edit as you please.

There are a couple talks I've done which were implemented as Jupyter notebook slideshows, and those are less easily shareable on a Google Drive.
However, you can either clone their GitHub repos and open the corresponding slideshow HTML document, or you can view them on nbviewer.org
- [ADASS 2023](https://nbviewer.org/github/alecgunny/adass-2023-ml4gw-demo/blob/presentation/train.slides.html#/) demo on using `ml4gw` and `hermes` ([GitHub repo](https://github.com/alecgunny/adass-2023-ml4gw-demo/))
- [Initial talk on use of IaaS in GW astronomy](https://nbviewer.org/github/fastmachinelearning/gw-iaas/blob/20.11/projects/slideshow/presentation.slides.html#/), given at MIT in 2020. ([GitHub repo](https://github.com/fastmachinelearning/gw-iaas/tree/20.11/projects/slideshow))

### Canvases on FastML Slack
Slack has a tool called Canvases for building pretty, Markdown-esque docs (it's basically a ripoff of Notion for those who are familiar).
I've written one or two that might prove helpful:
- [Review of aframe analyses](https://fastml.slack.com/docs/T6RPRPG92/F05NP48K04R)
- [Links to papers](https://fastml.slack.com/docs/T6RPRPG92/F05PVH651S6) about sensitivity analysis. Forms the foundation for how we evaluate `aframe`.
- [Initial sketch of data access overview document for new comers](https://fastml.slack.com/docs/T6RPRPG92/F0605DUA4J3). Folks should consider continuing to fill this out with whatever tidbits they've found helpful.

### Notion channl
Speaking of Notion, we briefly tried it for recording aframe (then BBHNet) meeting notes.
We found it generally useful, but filled up our quota on the free tier pretty quickly, and switched to recording notes on our GitHub wiki.
The notes for those meetings can still be found [here](https://lavish-trampoline-4ed.notion.site/ad72236bd7e54ddab3dd4c5b741c7152?v=8115b2f66016480e861a769e4140de79&pvs=4), and should be publicly accessible to all.

## Accounts
- **GitHub** ML4GW organization - Ethan and Will are now the main admins of this account
- **Weights & Biases** ml4gw organization- Ethan and Will should be owners on here now
    - Our projects are polluted with a bunch of testing runs I've done, folks should feel free to delete those more or less with impugnity
- **Pypi** ml4gw account - this is managed by the email address `ml4gw@ligo.mit.edu`
    - Either Ethan or Will should contact the devops group about taking over this account
    - Contact me when you have, and we can get on a Zoom session to turn over the 2FA
    - I believe Pypi has organization functionality, so it might make sense to set that up and see if we can transfer the `ml4gw` project to belong to that org rather than any one user to avoid this going forward
- **Dockerhub** ml4gw organization - haven't looked at this yet, but if/when it becomes something you need let me know and we can transfer it
- **Nautilus** - I'm only the admin on the `bbhnet` account, so presuming you want to move things over to `aframe` I think it just makes sense for Ethan or Will to ask for admin privileges and start that
    - The one catch here is that I've create an `s3://aframe` bucket in the west s3 region. If you have difficulties reading/writing, I can take a look at the privileges there

## `ml4gw`
In the last couple weeks, I've opened up a flurry of `ml4gw` PRs implementing lots of functionality I've had floating around over the last few months.
This includes
- [Support for HDF5 dataloading for a single archive with multiple datasets](https://github.com/ML4GW/ml4gw/pull/77)
- [A `torch`-based implementation of the chi-squared veto](https://github.com/ML4GW/ml4gw/pull/79)
- [Generalizing some of our 1D-resnet work from aframe](https://github.com/ML4GW/ml4gw/pull/82)
- [A general-purpose framework for implementing time-domain autoencoder architectures](https://github.com/ML4GW/ml4gw/pull/84)

Most of these work in principle, with the exception of the chi-squared implementation which needs some iteration on its scaling.
However, they're largely missing testing and I'm sure could use some checks for edge cases that haven't been considered.
These could be good PRs for newcomers to finish off to get familiar with the framework and some of our dev/testing practices.

## `hermes`
This is one thing I wish I had made time for a while ago, but it largely worked for our use cases so there wasn't much motivation to modernize it.
Basically, I've begun a [huge PR](https://github.com/ML4GW/hermes/pull/47) that attempts to do several things:
- Consolidate all the various `hermes.x` libraries into a single `hermes` library that can be installed via pip and pushed to Pypi
- Remove the `hermes.cloudbreak` and `hermes.stillwater` libraries which are largely unused nowadays (the one useful bit from `stillwater`, `ServerMonitor`, is kept)
- Remove "special" snapshotter and online average stateful models, which now live in `ml4gw`
- Allow for specification of certain model inputs/outputs as `states` to support arbitrary stateful models

This last bit is the least complete and unfortunately the most complicated, mostly because the logic in the `Exporter` class is a bit convoluted to support all the different backends we want to support (and requires some familiarity with all of them).
It might be worth deprecating e.g. keras `SavedModel` support for the time being to simplify this.
Probably the biggest issue is that it was written before almost anything else I did in this space, and so is probably not as well-designed as it might be if I started it again knowing everything I know now.
If things look really daunting, it might be worth trying to implement some of the functionality from scratch and seeing if A) you can come up with something better, or B) if the choices I made actually make sense given some constraint I no longer remember.

The other thing that makes this tricky is that Triton configs are built on protobufs, which are inherently non-Pythonic and require learning some special syntax (hopefully this is something LLMs can be helpful with these days).
The biggest piece of advice I can give you is that setting elements on protobufs will often not work and your best bet is to construct a fresh `ModelConfig` with just the properties you want to update set, and then calling `.MergeFrom(new_config)` on the config you want to update.

Ethan has also brought to my attention a library built by NVIDIA that attempts to make some similar simplifications called [pytriton](http://github.com/triton-inference-server/pytriton).
I know nothing about it, but if it can remove some of your development burden, it's definitely something that's worth taking some time to play with.

### Other stuff
I recently pushed some changes to aframe that involve using states with an actual batch dimension. It turns out `hermes` didn't support this, so I made a quick fix for it in [this PR](https://github.com/ML4GW/hermes/pull/48), which works in practice but could probably use an explicit test.
This change is necessary to making the aframe fix work, so might be worth doing before the other PR gets in, even though the change will just have to be re-implemented.

## aframe
The [original `aframe` repo](https:
