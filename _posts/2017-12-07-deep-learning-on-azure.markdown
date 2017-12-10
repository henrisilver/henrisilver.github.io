---
title: "The Quest to Run Deep Learning Experiments on Azure"
layout: post
date: 2017-12-07 19:27
tag:
- Azure
- Deep Learning
- Gene Expression Data
- Machine Learning
- Keras
- Theano
- GPU
- CUDA
category: blog
author: henriquesilveira
description: My quest to use Azure for running Deep Learning experiments implemented with Keras and Theano, using CUDA 
dashboardscreenshot: assets/images/azure-dashboard.png
---

Before graduating, I worked on an interesting and meaningful [project]({{ site.baseurl }}{% link _posts/2017-12-07-variations-deep-learning.markdown %}) that involved Deep Learning and gene expression datasets. I used different Deep Learning architectures to create experiments aiming to analyze gene expression data in the context of cancer classification. I wrote all the experiments in Python, using [Keras](https://keras.io) as the API to create neural networks which ran on top of [Theano](http://deeplearning.net/software/theano/).

At first, I naively thought of using my personal laptop to run the experiments and get all of the results. I have a late 2013 Macbook Pro with an Intel i5 processor. However, in spite of usually having a small number of examples in each dataset, gene expression data is complex since datasets generally comprehend thousands of features. Some of the datasets I used had over 20000 features (and around 500 examples). Therefore, the first batch of experiments could not even be completed since my computer had run out of memory.

Then I realized I should run my experiments making use of GPUs, since Keras and Theano have support for that. It would accelerate the running time of the experiments considerably as many of the math operations involved in the training of neural networks can be parallelized. I took a Surface Book with a dGPU I have at home ([it has a GPU from Nvidia](https://www.gizmodo.com.au/2015/10/microsoft-surface-books-secret-nvidia-gpu-what-is-it/), but it is not clear which specific model of GPU was used) and installed the necessary tools:

* [Visual Studio 2017](https://www.visualstudio.com/vs/), which provides necessary compilers
* [CUDA](https://developer.nvidia.com/cuda-toolkit), version 8
* [cuDNN](https://developer.nvidia.com/cudnn) v5, compatible with CUDA 8
* Python, Keras and Theano

After some iterations of intalling and uninstalling everything (it is important to install Visual Studio first, then CUDA and then cuDNN) and following some good resources I found on the web (please find below a list on additional resources related to Deep Learning on the Surface Book), I was finally able to start running the experiments. At first, things seemed to be working and I got really excited. The training progress of the networks was being much faster thanks to the GPU. And when I thought everything would work, I was surprised with "Out of Memory" exceptions and errors.After I stopped for a moment and thought about it, I realized I should not be surprised; the GPU being used had around 1GB of memory, less than my MacBook's memory. So it would never succeed to run the experiments.

After all of that I realized I would need some robust infrastructure to be able to run the experiments. I thought of using a big cluster available at my university and that can be used for scientific computing. However, I was disappointed to find out that the creation of new user accounts was suspended due to an improvement process the cluster was undergoing. So I was left with two options: I could either buy and assemble a powerful personal computer, with a capable GPU and enough memory; or I could use services such as Azure and AWS to run the experiments in the cloud.

Since I had credit at AWS due to GitHub Education's Student Developer Pack, I decided to try AWS first. However, I soon found out that I would not be able to use machines with GPUs: as a new user, I had to request a limit increase to be able to use more powerful machines, since only the basic ones were available to me as a free-tier user. But the request was rejected as AWS told me my account had minimal usage and they would need me to first utilize my current instance capacity before accessing other machine configurations.

So I moved to Azure and it turned out to be the best decision I took. At first, I imagined that I would have to pay for the service. However, it was a nice surprice to find that I would also receive some credit on Azure: new users get US$ 200 to spend on their first month. I redeemed the credit and then looked for documentation on how to set up a development environment on Azure. I found a couple of good resources, which are listed at the end of this post.

One interesting machine configuration available on Azure is Standard NC6: it has got 6 Intel Xeon vCPUs, 1 Tesla K80 Nvidia GPU with 16GB of memory and 56 GB of RAM. I decided to use that configuration, which seemed good enough for my needs. In order to do that, I had to update my account: a Pay-As-You-Go subscription was needed to use Standard NC6 machines. Even though I registered a credit card on Azure, I would still be able to use the US$ 200 credit before being charged on the credit card.

When I was about to set up the virtual machine, I received an email from Azure telling me that the account was suspended. That was very frustrating, since I had spent many days trying to get the experiments running and a different issue would pop up every time. The reason for the suspension was not clear. I decided to open a ticket with Azure Customer Service and I was very happy with the response I got. I was treated very well by the Azure Customer Service team, which solved the issue in a couple of hours even though it was a Saturday.

Finally, it was time to get the experiments running. My subscription had been reactivated and I was hopeful there would not be any more issues. I provisioned the virtual machine using a Deep Learning specific Ubuntu image availabel in the Azure market place. It was interesting as it included all the tools: CUDA, cuDNN, Python, Theano... and many more. After a couple of minutes, the virtual machine was ready and I could login using SSH via the command line (which I ended up using, transfering files using scp). There was also the option to install a GUI client called X2Go to remotely access the virtual machine.

Luckily, the experiments ran successfully that time and after a couple of days I was able to collect all results. Yes, only after a couple of days, since even on a poweful machine each batch of experiments was taking hours to complete. Impressive!

Something important to have in mind is that services like AWS and Azure charge users per hour. Even if your virtual machine is idle, but still on, you are getting charged. Therefore, it is important to stop the virtual machine after you are done with it. On Azure, if the status of your infrastructure is "Stopped (De-allocated)", you can rest assured, since there will be no charges.

Below is a picture of the dashboard on Azure for the virtual machine I used - it includes some pretty cool information.

[![Dashboard Screenshot]({{ site.url }}/{{ page.dashboardscreenshot }})]({{ site.url }}/{{ page.dashboardscreenshot }})

---

Additional resources on provisioning Deep Learning VMs on Azure:
* [Introduction to the Deep Learning Virtual Machine](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/deep-learning-dsvm-overview)
* [Data Science Virtual Machine for Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [Provision a Deep Learning Virtual Machine on Azure](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/provision-deep-learning-dsvm)
* [Azure GPU Tensorflow Step-by-Step Setup](https://blogs.msdn.microsoft.com/uk_faculty_connection/2017/03/27/azure-gpu-tensorflow-step-by-step-setup/)
* [If you’re not using your Infrastructure turn if off! Saving costs on Azure](https://blogs.msdn.microsoft.com/uk_faculty_connection/2017/03/31/if-your-not-using-your-infrastructure-turn-if-off-saving-costs-on-azure-2/)

Additional resources related to Deep Learning on the Surface Book:
* [TensorFlow with the Surface Book](http://www.apnorton.com/blog/2017/01/04/Machine-Learning-with-the-Surface-Book/)
* [Using TensorFlow in Windows with a GPU](http://www.heatonresearch.com/2017/01/01/tensorflow-windows-gpu.html)
* [Up and running with CUDA on Microsoft Surface Book](https://medium.com/@ryanga/up-and-running-with-cuda-on-microsoft-surface-book-3ec2d25f096b)

<!-- 
## Summary:

You can pick as item to see how to apply in markdown.

#### Especial Elements
- [Evidence](#evidence)
- [Side-by-Side](#side-by-side)
- [Star](#star)
- [Especial Breaker](#especial-breaker)
- [Spoiler](#spoiler)

#### External Elements
- [Gist](#gist)
- [Codepen](#codepen)
- [Slideshare](#slideshare)
- [Videos](#videos)

---

## Evidence

You can try the evidence!

<span class="evidence">Paragraphs can be written like so. A paragraph is the basic block of Markdown. A paragraph is what text will turn into when there is no reason it should become anything else.</span>

{% highlight html %}
<span class="evidence">Paragraphs can be written like so. A paragraph is the basic block of Markdown. A paragraph is what text will turn into when there is no reason it should become anything else.</span>
{% endhighlight %}

---

## Side-by-side

Like the [Medium](https://medium.com/) component.

**Image** on the left and **Text** on the right:

{% highlight html %}
<div class="side-by-side">
    <div class="toleft">
        <img class="image" src="{{ site.url }}/{{ site.picture }}" alt="Alt Text">
        <figcaption class="caption">Photo by John Doe</figcaption>
    </div>

    <div class="toright">
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
    </div>
</div>
{% endhighlight %}

<div class="side-by-side">
    <div class="toleft">
        <img class="image" src="{{ site.url }}/{{ site.picture }}" alt="Alt Text">
        <figcaption class="caption">Photo by John Doe</figcaption>
    </div>

    <div class="toright">
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
    </div>
</div>

**Text** on the left and **Image** on the right:

{% highlight html %}
<div class="side-by-side">
    <div class="toleft">
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
    </div>

    <div class="toright">
        <img class="image" src="{{ site.url }}/{{ site.picture }}" alt="Alt Text">
        <figcaption class="caption">Photo by John Doe</figcaption>
    </div>
</div>
{% endhighlight %}

<div class="side-by-side">
    <div class="toleft">
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
    </div>

    <div class="toright">
        <img class="image" src="{{ site.url }}/{{ site.picture }}" alt="Alt Text">
        <figcaption class="caption">Photo by John Doe</figcaption>
    </div>
</div>

---

## Star

You can give evidence to a post. Just add the tag to the markdown file.

{% highlight raw %}
star: true
{% endhighlight %}

---

## Especial Breaker

You can add a especial *hr* to your text.

{% highlight html %}
<div class="breaker"></div>
{% endhighlight %}

<div class="breaker"></div>

---

## Spoiler

You can add an especial hidden content that appears on hover.

{% highlight html %}
<div class="spoiler"><p>your content</p></div>
{% endhighlight %}

<div class="spoiler"><p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p></div>

---

## Gist

You can add Gists from github.

{% highlight raw %}
{ % gist sergiokopplin/91ff4220480727b47224245ee2e9c291 % }
{% endhighlight %}

{% gist sergiokopplin/91ff4220480727b47224245ee2e9c291 %}

---

## Codepen

You can add Pens from Codepen.

{% highlight html %}
<p data-height="268" data-theme-id="0" data-slug-hash="gfdDu" data-default-tab="result" data-user="chriscoyier" class='codepen'>
    See the Pen <a href='http://codepen.io/chriscoyier/pen/gfdDu/'>Crappy Recreation of the Book Cover of *The Flame Alphabet*</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.
</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
{% endhighlight %}

<p data-height="268" data-theme-id="0" data-slug-hash="gfdDu" data-default-tab="result" data-user="chriscoyier" class='codepen'>See the Pen <a href='http://codepen.io/chriscoyier/pen/gfdDu/'>Crappy Recreation of the Book Cover of *The Flame Alphabet*</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

---

## Slideshare

Add your presentations here!

{% highlight html %}
<iframe src="//www.slideshare.net/slideshow/embed_code/key/hqDhSJoWkrHe7l" width="560" height="310" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
{% endhighlight %}

<iframe src="//www.slideshare.net/slideshow/embed_code/key/hqDhSJoWkrHe7l" width="560" height="310" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>

---

## Videos

Do you want some videos? Youtube, Vimeo or Vevo? Copy the embed code and paste on your post!

**Example**

{% highlight html %}
<iframe width="560" height="310" src="https://www.youtube.com/embed/r7XhWUDj-Ts" frameborder="0" allowfullscreen></iframe>
{% endhighlight %}

<iframe width="560" height="310" src="https://www.youtube.com/embed/r7XhWUDj-Ts" frameborder="0" allowfullscreen></iframe>

[1]: http://daringfireball.net/projects/markdown/
[2]: http://www.fileformat.info/info/unicode/char/2163/index.htm
[3]: http://www.markitdown.net/
[4]: http://daringfireball.net/projects/markdown/basics
[5]: http://daringfireball.net/projects/markdown/syntax
[6]: http://kune.fr/wp-content/uploads/2013/10/ghost-blog.jpg
 -->