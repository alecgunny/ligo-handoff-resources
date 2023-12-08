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
I've written one or two that might prove helpful (requires you to have access to the FastML Slack):
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
Another suggestion I would make as far as reducing our technical debt would be to get rid of `hermes.quiver`'s abstract filesystem implementation and use a standard library like `fsspec` that does all of this stuff way better.

### Other stuff
I recently pushed some changes to aframe that involve using states with an actual batch dimension. It turns out `hermes` didn't support this, so I made a quick fix for it in [this PR](https://github.com/ML4GW/hermes/pull/48), which works in practice but could probably use an explicit test.
This change is necessary to making the aframe fix work, so might be worth doing before the other PR gets in, even though the change will just have to be re-implemented.

## aframe
### Paper readiness
- [MDC inference](https://github.com/ML4GW/aframe/pull/469)
    - Should be just about ready, but have not had a chance to run it through the MDC evaluation code
    - Instructions for how to generate data and evaluate are in the linked PR, as well as some more info as to what's inside here
    - I think I have plotting code somewhere that points to their DOI results, will try to find that
- Online benchmarking
    - In the [online-benchmarking.ipynb](./online-benchmarking.ipynb) notebook in this repo
    - Might take some massaging, and I don't have a specific environment for it, but it should be pretty clear to make work.
- Augmentation benchmarking
    - In the [augmentation-benchmarking.ipynb](./augmentation-benchmarking.ipynb) notebook in this repo

### aframev2 and BNS
I've begun an attempt to modernize aframe's infrastructure in a [v2 repo](https://github.com/ml4gw/aframev2).
There's lots of info about where things stand and how to run on the [README](https://github.com/ML4GW/aframev2/blob/main/README.md), but to give a quick rundown
- Training
    - What kind of model?
        - Supervised, time-domain (i.e. classic aframe)
            - Implemented and working
        - Supervised, frequency-domain (i.e. spectrograms for BNS)
            - Needs an `SupervisedFrequencyDomainDataset` with an `augment` method that just calls `super().augment(X)` then does `return spectrogram(X)`
            - Needs an `Architecture` base subclass for architectures that expect 4D inputs (batch, channel, frequency, time), as well as a `torchvision` ResNet that just inherits from this straightforwardly
            - Needs an `SupervisedFrequencyDomainAframe` `LightningModule` that inherits from `SupervisedAframe` and just specifies that the `arch` parameter should be a subclass of the `Architecture` defined above. These last two steps aren't strictly _necessary_, but they help you automatically register valid architectures to train this type of model on and list them when you call `--help` at the CLI.
        - Semi-supervised, time-domain (i.e. autoencoder)
            - Convolutional autoencoder model tested and working, but only implements correlation loss for now. See comments on the class about options about how to make this more complex, and notes in the [README](https://github.com/ML4GW/aframev2/blob/main/README.md) about possible future research directions as well as plotting tools that might help visualizing what the network is learning easier (which I think will be critical here).  
    - Where do you want to train?
        - On LDG
            - Implemented and working
        - On Nautilus
            - Not implemented, could probably use a `luigi.contrib.KubernetesTask` to run pretty straightforwardly
            - Trivial to run with a Kubernetes deploy yaml, which is probably a good exercise for folks anyway
- Hyperparameter tuning
    - Where do you want to run?
        - On LDG
            - This mostly works, though as I note in the README, I'm running into an issue with ray's Lightning wrappers that schedules one training job as a distributed job on all available workers, rather than splitting it up into `num_workers` training jobs with `gpus_per_worker` GPUs on each. This might be worth filing an issue with `ray`, but it's also entirely possible there's something I'm missing here
        - On Nautilus
            - Works if you already have a cluster spun up and you want to do a direct `apptainer run` (i.e. not using the `luigi` layer)
            - Have an implementation using Ethan's ray-kube library that can almost automate this at the `luigi` layer, see the README for more details.

### Various other odds and ends
Will update this with links and paths to various notebooks/analyses/useful snippets of code as I stumble upon them.
- [Offline benchmarking notebook](./offline-benchmarking.ipynb): code I use to read the CSV produced by the ServerMonitor and plot things like throughput, queue latency, etc.
- [Slightly cleaner rewrite of the ledger library](https://github.com/alecgunny/BBHNet/tree/luigi-infer/aframe/utils/aframe/ledger) I started in an old aframe branch that's meant to mimic more of the API of [pytables](https://www.pytables.org/), on which it could potentially be built eventually and which is supposed to be pretty fast for data loading.
That said, I spent some time playing with `pytables` over the summer and found that the API is not quite what we need, though I can't recall all of the exact details as to why.
I also didn't find it particularly fast for our dataloading use case (random sampling of waveforms), though with Deep's torch-based waveform generation maybe this is a moot point.

## DeepClean
This one is a bit of a jumble, but I'll try to make things as clear as possible.
If you want the TL;DR, just see the [README](https://github.com/alecgunny/deepclean-demo#readme) from the current project, in particular the [Where Things Stand section](https://github.com/alecgunny/deepclean-demo#where-things-stand).
For the full story:

My [first cut of rewriting DeepClean](https://github.com/ml4gw/deepclean) was structured a lot like aframe, and is similarly in desperate need of modernization.
I began this effort with a branch on my personal fork called [ml4gw-introduction](https://github.com/alecgunny/deepclean/tree/ml4gw-introduction), which attempted to begin introducing various `ml4gw` modules into the training and inference pipeline.

However, this work got interrupted when we decided to try to put together a production online pipeline, and this work continued in this branch in the [microservice project](https://github.com/alecgunny/deepclean/tree/ml4gw-introduction/projects/microservice).
The idea here was to split all of the various DeepClean tasks into various isolated "microservices" that communicated with each other over HTTP.
An overview of this setup can be found in [this document](https://docs.google.com/document/d/1bwpT6JBJwbfZplZZwxkRmTOERLh603Pus24SqnP79Xs/edit?usp=sharing) (also available in the Google Drive linked to above).

This setup _works_, but as I note in the issues section of that document, there's a problem where the cleaned data comes out gross.
Using the notebook I mention in that section, I was able to deduce that the issue is with how ONNX implements the transpose convolution operation, which is inconsistent with how Torch does during training (ONNX is the NN framework we use to convert our Torch model to a TensorRT runtime for accelerated inference).
Given this, and the fact that DeepClean's computational needs are really cheap, the microservice implemenation is going to be gross overkill.

### TAKEAWAY #1: DeepClean should drop inference-as-a-service and asynchronous microservice deployment

Given that, and the fact that I was in the process of modernizing aframe, I have [begun an overhaul of DeepClean](https://github.com/alecgunny/deepclean-demo) that uses more modern infrastructure and attempts to align the offline valiadtion process with the online deployment scenario to better estimate performance and encounter/solve issues earlier on. Issues like
- Using a narrower frequency band means you need to allow more future predictions to come in before you bandpass filter so that you can have more filter settle-in time
- DeepClean performance suffers near the edges of predictions, so you should just drop these predictions entirely rather than averaging over them. This induces a time-delay between your input and output that makes keeping track of prediction indices more complicated.
- Etc.

### TAKEAWAY #2: The online deployment code should be re-implemented in [the new repo](https://github.com/alecgunny/deepclean-demo) using many of the same tools that are in the [microservice project](https://github.com/alecgunny/deepclean/tree/ml4gw-introduction/projects/microservice), but using a torch model in-memory rather than IaaS

The microservice online code does a great job with the messy business of keeping track of time indices during online subtraction, and of monitoring cleaning quality to ensure we're never introducing artifacts.
If anything, this should simplify things.
The only major addition would be that you would now need to write code to detect and load new versions of the model, which is functionality Triton used to handle for you.

This should also simplify the training setup, which in the microservice code is running all the time and loading low-latency frames as they get written until it's accumulated enough data to train.
This is entirely unnecessary, and you can just leverage the offline training code by using standard data-access APIs (i.e. `TimeSeries.get`) and training offline.
This is trivial in the new framework, and can be done entirely from Python-land.
I would strongly suggest leveraging this functionality longer term, though since the microservice training code works currently I would also understand wanting to use it.
Just bear in mind that this means you'll have to use conda to manage the training code's environment, since it will need the gwf file reading libraries.
This will add some overhead in managing this environment (the new code lets a single `data` project handle all of this and write files to HDF5 archives for downstream projects, though obviously the online cleaning code will need these dependencies too).

This modernized infrastructure will overlap significantly with aframe's modernization, and attempts should be made early on to unify them under a shared framework that handles things like
- Deploying ray clusters on Kubernetes and launching tuning jobs
- Setting up S3 credentials and helping with reading/writing objects to cloud storage
- Setting up Weights & Biases settings in training containers
- Logging bokeh plots to Weights & Biases for more detailed experiment tracking
- Managing small syntax differences between Lightning's local CSVLogger and the WandBLogger

This shared framework will be entirely distinct from `ml4gw`, living at sort of the layer above it.

### TAKEAWAY #3: aframe has more functionality than deepclean right now. Rather than copy it over, make it more general

For more details on how the new code works, how to run it, and what it's missing, check out its [README](https://github.com/alecgunny/deepclean-demo#readme)
