---
layout: post
title: "Installing anvi'o"
excerpt: "Instructions to install the v2 branch of the platform."
modified: 2019-05-14
tags: []
categories: [anvio]
redirect_from:
 - /2015/05/01/installation/
 - /install-anvio/
comments: true
image:
  feature: https://github.com/merenlab/anvio/raw/master/anvio/data/interactive/images/logo.png
---

{% include _toc.html %}

{% include _project-anvio-version.html %}

This article explains basic steps of installing anvi'o using rather conventional methods both for end users and current of future developers.

We do recommend you to install anvi'o on your system, but if you just want to run it without any installation, you may find the article on [trying anvi'o in docker]({% post_url anvio/2015-08-22-docker-image-for-anvio %}) more suitable.

Please consider opening an <a href="https://github.com/meren/anvio/issues">issue</a> for technical problems, or join us on Slack if you need help:

{% include _join-anvio-slack.html %}

## Installation with conda (painless method suggested for everyone)

This is a very simple and effective way to install anvi'o on your system along with most of its dependencies.

{:.notice}
<div id="windowsreminder" style="min-height: 50px;" markdown="1">
<img src="/images/windows10.png" style="float:left; width:50px; border: None; display: block; margin-right: 5px;">Although these installation instructions primarily target and rigorously tested for Linux and Mac OSX, you will be able to follow them if you are using Microsoft Windows **if and only if you first install the [Linux Subsystem for Windows](https://docs.microsoft.com/en-us/windows/wsl/install-win10)**. Our users have reported success stories with Ubuntu on WSL.
</div>

**For this to work, you need [miniconda](https://docs.conda.io/en/latest/miniconda.html) to be installed on your system.** If you are not sure whether it is installed or not, open a terminal (hopefully an [iTerm](https://www.iterm2.com/), if you are using Mac) and type `conda`. You should see an output like this instead of a 'command not found' error (your version might be different):

```bash
$ conda --version
conda 4.8.3
```

If you don't have conda installed, then you should first install it through their [installation page](https://docs.conda.io/en/latest/miniconda.html). Once you have confirmed you have conda installed, run this command to make sure you are up-to-date:

``` bash
conda update conda
```

Good. Next, installing anvi'o.

{:.notice}
It is a good idea to make sure you are not already in a conda environment before you run the following steps. Just to be clear, you can indeed install anvi'o in an existing conda environment, but if things go wrong, we kindly ask you to refer to meditation for help, rather than [anvi'o community resources]({% post_url anvio/2019-10-07-getting-help %} since there is no way we can help you if you are installing anvi'o in a different conda environment :) If you want to see what environments do you have on your computer and whether you already are in one of them in your current terminal by running `conda env list`. **If all these are too much for you and all you want to do is to move on with the installation**, simply do this: open a new terminal, and run `conda deactivate`, and continue with the rest of the text.

**Please choose only one of the two following subsections.**

### Installation for Mac OSX users

Resolving dependencies (especially on Mac systems) can take a very long time for conda (which is a [known problem](https://github.com/conda/conda/issues/7239)), hence, here we will use a serious shortcut to generate an environment for anvi'o `v{% include _project-anvio-version-number.html %}`:

The solution here works very well for Mac OSX and **reduces installation time to minutes** that was first suggested by [Blake Sanders](https://micro.uchicago.edu/program/students/blake-sanders), a graduate student at the Committee on Microbiology, University of Chicago. If you follow that solution precisely, you can continue with the next line as if nothing happened.

If you don't want to do this, or if this somehow is not working for you, you can click on "*A more conventional way to install stable anvi'o through conda*" section down below and follow the recipe there instead. If things failed here it would be extremely helpful if you could let us know which operating system you were using with its version, and what error did you get so we can perhaps improve it.

``` bash
# first get a copy of the following file (if you get a "command not found"
# error for wget, you can either install it first, or manually download the
# file from that URL:
wget http://merenlab.org/files/anvio-conda-environments/anvio-environment-6.2.yml

# just to make sure there is not a v6.2 environment already: 
conda env remove --name anvio-6.2

# create a new v6.2 environment using the file you just downloaded:
conda env create -f anvio-environment-6.2.yml

# activate that environment
conda activate anvio-6.2
```

*If you are here, jump to "Checking your installation" section without running the commands described under "For Linux / Windows users".*

### Installation for Linux / Windows users

This is the conventional way of running conda to install stable anvi'o through conda. Even though the title says it is for Linux and Windows users, it will also work for Mac OSX users (but for them the installation will likely take forever, hence the alternative above).

First you will need to create a conda environment for anvi'o v{% include _project-anvio-version-number.html %} (it is essential to create it with Python 3.6 as shown below):

``` bash
conda create -y --name anvio-6.2 python=3.6
```

Once it is ready, activate your new environment:

``` bash
conda activate anvio-6.2
```

And finally install anvi'o in it:

``` bash
conda install -y -c conda-forge -c bioconda anvio==6.2
```

That's all!

### Checking your installation

If you are here, it means you managed to install anvi'o either through the conventional way or through the YAML file. We don't care. We're just happy for you. Now, please run this command in your terminal for a final check:

```
conda list | grep anvio-minimal | grep 6.2 | awk '{print $3}'
```

If the output of this is `py_1`, you are golden :) But if it says `py_0`, it means there is one more thing you have to do:

```
conda remove -y --force anvio-minimal
conda install -y -c bioconda -c conda-forge anvio-minimal==6.2=py_1
```

Once this is done, you should test anvi'o quickly to make sure everything is in order:

``` bash
anvi-self-test --suite mini
```

If at the end of this your browser automatically loaded the test run results, you are golden.

### Last words

{:.notice}
**Your browser didn't pop-up?**. If all tests seemed to run perfectly but the browser didn't pop-up, it may mean that Python on your system is unable to find your default browser. Not a biggie (but see the note on Chrome down bellow and try the Python command shown there). Paste the address `localhost:8080` to your browser to see the interactive display. It may also mean that you are on a server system where there is no graphical interface for the browser to show up. That is fine, too. Read this article to learn how you can forward displays from servers to your laptop: [working with remote displays]({% post_url anvio/2018-03-07-working-with-remote-interative %}).


{:.notice}
<div id="windowsreminder" style="min-height: 50px;" markdown="1">
<img src="/images/windows10.png" style="float:left; width:50px; border: None; display: block; margin-right: 5px;">Windows users may be greeted by a "This site can't be reached" error even when their browsers show up. Copy-paste on your address bar `localhost:8080` and press ENTER to see if it fixes it. Sorry. You are the one who brought a Microsoft Windows to an open-source fight.
</div>


{:.warning}
**IMPORTANT NOTE**: You may need to activate the anvi'o conda environment every time you open a new terminal window. Depending on your conda setup, you will either need to run `source activate anvio-6.2` or `conda activate anvio-6.2` (this assumes you named your conda environment for anvio `anvio-6.2` as per the commands above using the `--name` flag --if not, please replace `anvio-6.2` with whatever you have used to name your environment). You can always list your conda environments by typing `conda env list`.

<div class="extra-info" markdown="1">

<span class="extra-info-header">A note on Chrome</span>

Currently, the [Chrome Web Browser](https://www.google.com/chrome/browser/desktop/) has the most efficient SVG engine among all browsers we tested. For instance, Safari can run the anvi'o interactive interface, however it takes orders of magnitude more time and memory compared to Chrome. Firefox, on the other hand, doesn't even bother drawing anything at all. Long story short, the anvi'o interactive interface __will not perform optimally__ with anything but Chrome. So you need Chrome. Moreover, if Chrome is not your default browser, every time interactive interface pops up, you will need to copy-paste the address bar into a Chrome window. You can learn what is your default browser by running this command in your terminal:

``` bash
python -c 'import webbrowser as w; w.open_new("http://")'
```

</div>

If you are here, you are done. Congratulations, and thank you! You are now free to go.


## Following the active codebase (you're a wizard, arry)

If you follow these instructions you can follow the `master` branch of anvi'o, which is where we add new features and bug fixes in between stable releases. Following the `master` branch as prescribed here will not prevent you to also have a stable anvi'o version on the same computer in parallel.

{:.warning}
This section is not meant to be followed by those who would define themselves as *end users* in a conventional sense. It means if you would consider yourself someone who feels more comfortable with *stability*, *calmness*, and *predictibility* rather than *advanture* and *suprise*, or if you think feeling the occasional urge to ask your computer *WHAT NOW YOU STUPID CALCULATOR?* is not your cup of tea, you should stick with the installation recipe described in the previous section. Because if you just found yourself in this section and do not know what is `git` or `master`, you may be about to take upon more work than you anticipate (and while Meren totally thinks you should do it anyway because you have the world in your grip and all these unknown is yours to conquer, those who know how to cultivate happiness and productivity at the some time will suggest you to don't do it and move on with your day).

Following the active codebase will come with various advantages, but you must also consider the fact that it can be less stable than official releases. Nevertheless, here we go for those of you who live life at the edge.

{:.notice}
Special thanks go to [Jarrod Scott](https://twitter.com/metacrobe) who tested this recipe out and suggested changes. If you something doesn't work here anymore please fix it, and share your solution with us.

Because resolving dependencies can take a very long time for conda on Mac OSX, here we will have a serious shortcut to generate a `master` environment for anvi'o quickly. Use the following recipe only if you are setting up anvi'o on a Mac OSX system. Otherwise, jump to the next one.

### Conda setup for Mac OSX users

``` bash
# first get a copy of the following file (if you get a "command not found"
# error for wget, you can either install it first, or manually download the
# file from that URL:
wget http://merenlab.org/files/anvio-conda-environments/anvio-environment-master.yml

# just to make sure there is not a master environment already: 
conda env remove --name anvio-master

# create a new environment using the file you just downloaded:
conda env create -f anvio-environment-master.yml

# activate that environment
conda activate anvio-master
```

If everything worked here, you can jump to the "setting up the local copy of the anvi'o codebase" section.

### Conda setup for Linux/Windows users

Create a new conda environment, and activate it:

``` bash
conda deactivate
conda create -y --name anvio-master python=3.6
conda activate anvio-master
```

If this all worked, these are the outputs you should see:

``` bash
(anvio-master) meren ~ $ python --version
Python 3.6.7

(anvio-master) meren ~ $ which python
/Users/meren/miniconda3/envs/anvio-master/bin/python
```

Good? Good. Then, install the following dependencies in this conda environment:

``` bash
conda install -y -c conda-forge -c bioconda -c defaults \
                 prodigal \
                 mcl \
                 muscle \
                 hmmer \
                 diamond==0.9.14 \
                 blast \
                 megahit \
                 bowtie2 \
                 bwa \
                 samtools \
                 centrifuge \
                 trimal \
                 iqtree \
                 fastani \
                 fasttree \
                 r-base \
                 r-stringi \
                 r-tidyverse \
                 r-magrittr \
                 r-optparse \
                 bioconductor-qvalue \
                 trnascan-se
```

Once this is done, you can continue with the next step of acquiring and setting up the anvi'o codebase on your computer.

### Setting up the local copy of the anvi'o codebase

If you are here, it means you have a conda environment with everything except anvi'o itself. We will make sure this environment _has_ anvi'o by getting a copy of the anvi'o codebase from GitHub.

Here I will suggest `~/github/` as the base directory to keep the code, but you can change if you want to something else (in which case you must remember to apply that change all the following commands, of course):

``` bash
# setup the code directory and get the anvi'o codebase
mkdir -p ~/github && cd ~/github/
git clone --recursive https://github.com/meren/anvio.git
```

Now it is time to install the Python dependencies of anvi'o:

``` bash
cd ~/github/anvio/
pip install -r requirements.txt
```

Now we have the codebase and we have the conda environment, but they don't know about each other. Now we will setup your conda environment in such a way, every time you activate it, you will get the very latest updates from the main anvi'o repository. While you are still in anvi'o environment, copy-paste these lines into your terminal:

``` bash
cat <<EOF >${CONDA_PREFIX}/etc/conda/activate.d/anvio.sh
# creating an activation script for the the anvi'o master conda environment
# so (1) Python knows where to find anvi'o libraries, (2) BASH knows
# where to find its programs, and (3) every time the environment is activated
# it downloads the latest code from the master repository
export PYTHONPATH=\$PYTHONPATH:~/github/anvio/
export PATH=\$PATH:~/github/anvio/bin:~/github/anvio/sandbox
echo -e "\033[1;34mUpdating from the master repository in GitHub \033[0;31m(press CTRL+C to cancel)\033[0m ..."
cd ~/github/anvio && git pull && cd -
EOF
```

Finally we define an alias, `anvi-activate-master`, so you can use it to initiate everything like a pro:

```
echo -e "\n# >>> ANVI'O STUFF >>>" >> ~/.bash_profile
echo 'alias anvi-activate-master="conda activate anvio-master"' >> ~/.bash_profile
echo "# <<< ANVI'O STUFF <<<" >> ~/.bash_profile
source ~/.bash_profile
```

At this stage if you run `anvi-activate-master`, you should see similar outputs to these:

```
$ anvi-self-test -v

Anvi'o version ...............................: esther (v6.2-master)
Profile DB version ...........................: 31
Contigs DB version ...........................: 14
Pan DB version ...............................: 13
Genome data storage version ..................: 6
Auxiliary data storage version ...............: 2
Structure DB version .........................: 1

$ which anvi-self-test

/Users/meren/github/anvio/bin/anvi-self-test
```

If that is the case, you're golden.


<details markdown="1"><summary>Show/hide playing with the code</summary>

Now you can go to the anvi'o codebase,

```
cd ~/github/anvio
```

Make change,

``` bash
# as you probably know the following line is good for Mac
# computers but will fail in Linux systems unless you remove
# the part that goes like -i '' :)
sed -i '' 's/esther/ESTHER/g' anvio/__init__.py
```

See it in the git log:

``` diff
$ git diff

diff --git a/anvio/__init__.py b/anvio/__init__.py
index 1ceca28a..75c91a73 100644
--- a/anvio/__init__.py
+++ b/anvio/__init__.py
@@ -13,7 +13,7 @@ import platform
 import pkg_resources

 anvio_version = '6.2-master'
-anvio_codename = 'esther'
+anvio_codename = 'ESTHER'

 DEBUG = '--debug' in sys.argv
 FORCE = '--force' in sys.argv
```

But also see in action:

```
$ anvi-self-test -v

Anvi'o version ...............................: ESTHER (v6.2-master)
Profile DB version ...........................: 31
Contigs DB version ...........................: 14
Pan DB version ...............................: 13
Genome data storage version ..................: 6
Auxiliary data storage version ...............: 2
Structure DB version .........................: 1
```

If you want to see what is going on in the codebase lately install `tig`,

``` bash
pip install tig
```

And run it:

``` bash
cd ~/github/anvio/
tig
```
</details>

So, this is the end of setting up the active anvi'o codebase on your computer so you can follow our daily additions to the code before they appear in stable releases, and use anvi'o exactly the way we use on our comptuers for our science on your own risk. Please note that given this setup so far, every time you open a terminal you will have to first activate conda, and then the Python environment:

```
conda activate anvio-master
anvi-activate-master
```

You can always use `~/.bashrc` or `~/.bash_profile` files to add aliases to make these steps easier for yourself, or remove them when you are tired.

<details markdown="1"><summary>Show/hide Meren's BASH profile setup</summary>

This is all personal taste and they may need to change from computer to computer, but I adeed the following lines at the end of my `~/.bash_profile` to easily switch between different versions of anvi'o on my Mac system:

{:.notice}
If you are using Anaconda rather than miniconda, or you are using Linux and not Mac, you will have to find corresponding paths for lines that start with `/Users` down below :)

``` bash
init_anvio_stable () {
    {
        deactivate && conda deactivate
    } &> /dev/null

    export PATH="/Users/$USER/miniconda3/bin:$PATH"
    . /Users/$USER/miniconda3/etc/profile.d/conda.sh
    conda activate anvio-6.2
    export PS1="\[\e[0m\e[47m\e[1;30m\] :: anvi'o v6.2 :: \[\e[0m\e[0m \[\e[1;32m\]\]\w\[\e[m\] \[\e[1;31m\]>>>\[\e[m\] \[\e[0m\]"
}


init_anvio_master () {
    {
        deactivate && conda deactivate
    } &> /dev/null

    export PATH="/Users/$USER/miniconda3/bin:$PATH"
    . /Users/$USER/miniconda3/etc/profile.d/conda.sh
    conda activate anvio-master
    export PS1="\[\e[0m\e[40m\e[1;30m\] :: anvi'o v6 master :: \[\e[0m\e[0m \[\e[1;34m\]\]\w\[\e[m\] \[\e[1;31m\]>>>\[\e[m\] \[\e[0m\]"
}

alias as=init_anvio_stable
alias am=init_anvio_master
```

With this setup, in a new terminal window I can type `as` or `am` to run the stable or master version of anvi'o, or to switch from one to the other:

```
meren ~ $ anvi-self-test -v
-bash: anvi-self-test: command not found

meren ~ $ as

:: anvi'o v6.2 :: ~ >>>

:: anvi'o v6.2 :: ~ >>> anvi-self-test -v
Anvi'o version ...............................: esther (v6.2)
Profile DB version ...........................: 31
Contigs DB version ...........................: 14
Pan DB version ...............................: 13
Genome data storage version ..................: 6
Auxiliary data storage version ...............: 2
Structure DB version .........................: 1

:: anvi'o v6.2 :: ~ >>> am

:: anvi'o v6 master :: ~ >>>

:: anvi'o v6 master :: ~ >>> anvi-self-test -v
Anvi'o version ...............................: esther (v6-master)
Profile DB version ...........................: 31
Contigs DB version ...........................: 14
Pan DB version ...............................: 13
Genome data storage version ..................: 6
Auxiliary data storage version ...............: 2
Structure DB version .........................: 1
```


**But please note** that both aliases run `deactivate` and `conda deactivate` first, and they may not work for you especially if you have a fancy setup. I'd be very happy to improve these shortcuts.

</details>


## Other installation (with varying levels of pain)

If you are an end user we really suggest you to follow the installation instructions for conda. But then it is your computer, nothing here is as scary as it looks, and you can do it.

We suggest you to use `virtualenv` to start a Python 3.6 environment, and install anvi'o in it. Don't use `pip` as the anvi'o package stored at PyPI is lacking some files due to size limitations. Instead, visit the following link, go to the bottom of the page, download the file `anvio-X.tar.gz` and work with that file:

[https://github.com/merenlab/anvio/releases/latest](https://github.com/merenlab/anvio/releases/latest)

The best way to see what additional software you will need running on your computer for anvi'o to be happy is to take a look at the contents of [this file](https://github.com/merenlab/anvio/blob/master/conda-recipe/anvio/meta.yaml) (which is a conda build recipe, but it will give you the idea (ignore anvio-minimal, you basically have taken care of it by installing anvi'o, and focus on the rest)).

## Running the "Mini Test"

You can make anvi'o test itself very quickly anytime by running,

``` bash
anvi-self-test --suite mini
```

It is absolutely normal to see 'warning' messages. In most cases anvi'o is talkative, and would like to keep you informed. **You should read those warning messages carefully, but in most cases they will not require action.**

Upon the successful completion of all the tests, your browser *should* popup and take you to the interactive interface. When you click that 'Draw' button whenever you see one.

When anvi'o is done drawing the test data, you should see something like this:

{:.notice}
This is one of the older version of [the anvi'o interactive interface]({% post_url anvio/2016-02-27-the-anvio-interactive-interface %}), and it shall stay here so we remember where we came from.

<div class="centerimg">
<a href="{{ site.url }}/images/anvio/misc/mini-test-screenshot.png"><img src="{{ site.url }}/images/anvio/misc/mini-test-screenshot.png" width="50%" /></a>
</div>

All fine? Perfect! Now you have a running anvi'o!

It is time to go through some anvi'o tutorials (see the pull-down menu at the top of this page), or take a look at [all the other posts on the platform]({{ site.url }}/software/anvio).
