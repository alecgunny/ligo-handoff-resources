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

## aframe
The [original `aframe` repo](https:
