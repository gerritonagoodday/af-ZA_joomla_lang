# Suite of Language Translation Utilities for Joomla

![Latest version available!](.images/announcement.png)

## Table of Contents

- [Welcome](#welcome)
  - [How do language strings work in Joomla CMS?](#how-do-language-strings-work-in-joomla-cms)
  - [Introduction to GIT](#introduction-to-git)
    - [Install Git](#install-git)
    - [Create an account for yourself on Github](#create-an-account-for-yourself-on-github)
    - [First Operations in GIT](#first-operations-in-git)
  - [Cool things you can do in GIT](#cool-things-you-can-do-in-git)
  - [How much work is involved in creating a language pack?](#how-much-work-is-involved-in-creating-a-language-pack)
  - [How much work will I continually need to invest to maintain a language pack?](#how-much-work-will-i-continually-need-to-invest-to-maintain-a-language-pack)
- [Instructions](#instructions)
  - [Naming conventions](#naming-conventions)
  - [How does the language pack build process work?](#how-does-the-language-pack-build-process-work)
    - [Step 0: Get you reference Joomla Core code base](#step-0-get-you-reference-joomla-core-code-base)
    - [Step 1: Select the relevant release](#step-1-select-the-relevant-release)
    - [Step 2: Check out code against a tag from the reference Joomla Code Base](#step-2-check-out-code-against-a-tag-from-the-reference-joomla-code-base)
    - [Step 3: Get your language pack repo](#step-3-get-your-language-pack-repo)
    - [Step 4: Create a branch in your local remote repo](#step-4-create-a-branch-in-your-local-remote-repo)
    - [Step 5: Set up the package configuration file](#step-5-set-up-the-package-configuration-file)
    - [Step 6: Produce a work file of what strings need to be translated](#step-6-produce-a-work-file-of-what-strings-need-to-be-translated)
    - [Step 7: Prepare to distribute the workload: Push the changes to the remote repo](#step-7-prepare-to-distribute-the-workload-push-the-changes-to-the-remote-repo)
    - [Step 8: Split the report out amoung the translation team members](#step-8-split-the-report-out-amoung-the-translation-team-members)
    - [Step 9: Team members do the translations and merge the resulting workfile back into the repo](#step-9-team-members-do-the-translations-and-merge-the-resulting-workfile-back-into-the-repo)
    - [Step 10: Run the work file script and commit and push all the files](#step-10-run-the-work-file-script-and-commit-and-push-all-the-files)
    - [Step 11: Test the language files for integrity](#step-11-test-the-language-files-for-integrity)
    - [Step 12: Build the package using the package build script](#step-12-build-the-package-using-the-package-build-script)
    - [Step 13: Test the language pack in Joomla](#step-13-test-the-language-pack-in-joomla)
    - [Step 14: Tag the translation in the local and then the remote repo](#step-14-tag-the-translation-in-the-local-and-then-the-remote-repo)
    - [Step 15: Release the translation package](#step-15-release-the-translation-package)
  - [What is this translation work file?](#what-is-this-translation-work-file)
  - [Special case: Create a new language pack](#special-case-create-a-new-language-pack)
  - [Tips when Translating](#tips-when-translating)
  - [Working with Google Translate](#working-with-google-translate)
- [Git Cheat Sheet](#git-cheat-sheet)
  - [Where on earth am I?](#where-on-earth-am-i)
  - [Renaming files](#renaming-files)
  - [Deleting files](#deleting-files)
  - [Post changes](#post-changes)
  - [Remote Repositories](#remote-repositories)
  - [Branches](#branches)
    - [A simple approach to use branches](#a-simple-approach-to-use-branches)
    - [List all branches in your local repo.](#list-all-branches-in-your-local-repo.)
    - [List all branches in your remote repo.](#list-all-branches-in-your-remote-repo.)
    - [Select an existig branch, i.e. checkout a branch:](#select-an-existig-branch,-i.e.-checkout-a-branch)
    - [Create a new branch off the currently-selected branch:](#create-a-new-branch-off-the-currently-selected-branch)
    - [Delete a branch from the local repo](#delete-a-branch-from-the-local-repo)
    - [Delete a branch from the remote repo](#delete-a-branch-from-the-remote-repo)
    - [Push a branch to your remote repo](#push-a-branch-to-your-remote-repo)
    - [Rename a branch](#rename-a-branch)
    - [Merge branches](#merge-branches)
  - [Dealing with Merge Conflicts](#dealing-with-merge-conflicts)
  - [Tagging](#tagging)
- [Further information](#further-information)
  - [Some useful reference material](#some-useful-reference-material)

_NOTE:_

>You can update this TOC as follows:
```awk
awk '/^#/ {gsub(/#/,"  ",$1); printf "%s- ", $1; $1=""; sub(/^ /,"");  printf "[%s]", $0; gsub(/\s+/,"-"); gsub(/[\!|\?\:]/,""); printf "(#%s)\n", tolower($0)}' README.md 
```

# Welcome

What is Joomla? It is a content management system. No more programming of discrete HTML and JS code to get the content out there for everyone to view and all the search engines to find. 
Find out more at [https://www.joomla.org](https://www.joomla.org/). Currently Joomla supports around 50 official spoken languages. There are also many other language-translations available that are run by teams that have not joined the Joomla CMS Translation corpus.

Every revision of Joomla requires some text to be translated from the origin language, English, into all these ca. 50 languages. Most translation teams have developed their own set of tools and methodologies to perform and managege these translations, and here is another set of such tools. The toolset differs from other tools in that it is totally command-line driven and uses the collaboration features of Git to spread the work load across team members. Optionally, it can also use the Google Translate cloud service (sometimes at a small cost), and it can also cross-reference similar previously-translated strings, or make use of your pre-built dictionary to help keep the translation consistent.

## How do language strings work in Joomla CMS?

Joomla CMS consists of a set of core "components", "modules" and "plug-ins". There is a sub-directory for each language's set of language files, named according to the language's ISO language code, in which a language file exists for each of the core components, modules and plug-ins. The content of the language file is structured as one string per text line of arbitrary length. Each line starts with a unique identifier, followed by a '=' and the actual string in double-inverted commas. 

For example, the British English (ISO code: en-GB) core language file language/en-GB/en-GB.ini contains:

```bash
...
JYES="Yes"
JNO="No"
...
```

Likewise, the corresponding German core language file for Germany (ISO code: de-DE) language/de-DE/de-DE.ini contains:

```bash
...
JYES="Ja"
JNO="Nein"
...
```

The quick answer to produce a translation pack, is to make a copy of the original (en-GB) language files, name them accordingly, translate the strings in them, and then slot them back into Joomla CMS in the correct places. Some cerebral work may be required. 

Note that we are only interested here in producing the translations of Joomla's core language files, not of any of the literally 1000's of third-party components, modules and plug-ins. These may well come with multiple translation files and may even include your language. If your language is not represented in this third-party product, why not produce a translation file for them while you are at it? 

## Introduction to GIT

A great introduction to using GIT is to watch this YouTube video: https://youtu.be/ulQA5tjJark by a very good presenter who deeply understands the subject 
and introduced and explains GIT very gently. Highly recommended!

### Install the Git client

By assumption, you are running on a Linux or a Mac, or at least have Linux for Windows installed on your Windows machine. You should also have the GIT application installed on your machine. If it is not installed yet, install GIT using your O/S's standard software installation tool, e.g. for Ubuntu Linux variants, do the following:

```bash
$ sudo apt install git
```

For Red Hat Linux variants, do this:

```bash
$ sudo yum install git
```

Create a working directory where all your GIT repositories are kept:

```bash
$ mkdir ~/git
$ cd ~/git
```

### Create an account for yourself on Github

Got to https://github.com and sign up for the free option. No need to part with any money! 
You need to do this if you want to be able to contribute back into a language pack, or want to manage your own language pack.  

* Configure yourself in Git

*(TO DO - needs updating to TFA)*

Set your user name, email address and password up if this is the only Git account that you are likely to use. Only set your password up like this if you are on your personal computer:

```bash
~/ $ git config --global user.name "yourusername"
~/ $ git config --global user.email "your@email"
~/ $ git config --global user.password "XXXXX"
```

* Check your configuration 

The line with password will be shown 'in the clear' if you have previously set it up, so be aware of who might be shoulder-surfing: 

```bash
~ $ git config --list
user.email=your@email
user.name=yourusername
user.password=XXXXX
...
```

You will occasionally be prompted to enter this information:

```bash
...
Username for 'https://github.com': XXXXX
Password for 'https://gerritonagoodday@github.com': XXXXX
```

### First operations in GIT

Let's look at the latest Joomla Release for example: You would first need to *clone* the entire repository (a.k.a. "repo") from [https://github.com/joomla/joomla-cms](#https://github.com/joomla/joomla-cms). This will bring down all the branches and also the complete history of all the file changes in the project over the course of project's lifetime. Follows the commands below. You will see that a new directory, ```joomla-cms```, will be created off the the ```git```-directory:

```bash
$ cd ~/git
~/git $ git clone https://github.com/joomla/joomla-cms
Cloning into 'joomla-cms'...
remote: Enumerating objects: 896503, done.
remote: Counting objects: 100% (524/524), done.
remote: Compressing objects: 100% (307/307), done.
remote: Total 896503 (delta 230), reused 398 (delta 183), pack-reused 895979
Receiving objects: 100% (896503/896503), 310.54 MiB | 2.51 MiB/s, done.
Resolving deltas: 100% (597396/597396), done.
```

If you have already previously cloned the repo, you do not need to do a *clone*, just do a *pull* from the code's root directory to get a complete refresh of the codebase:


```bash
$ cd ~/git/joomla-cms
~/git/joomla-cms $ git pull
remote: Enumerating objects: 619, done.
remote: Counting objects: 100% (619/619), done.
remote: Compressing objects: 100% (33/33), done.
remote: Total 943 (delta 571), reused 614 (delta 571), pack-reused 324
Receiving objects: 100% (943/943), 323.41 KiB | 1.54 MiB/s, done.
Resolving deltas: 100% (620/620), completed with 329 local objects.
```

And if you do anothe *pull*, it will simply tell you that no-one has checked in any new file changes since the last *pull*:


```
~/git/joomla-cms $ git pull
Already up-to-date.
```

### So, what do we have here?

In its simplest form, a GIT repository is chronological tree of branches, that have been tagged at arbitrary points in time as
markers that constitute code releases or other significant coding events in the code development life cycle.

Here is a friendly way to see what has arrived from the remote repository: 


```
~/git/joomla-cms $ git status
On branch 4.2-dev
Your branch is up-to-date with 'origin/4.2-dev'.

nothing to commit, working tree clean
```

You can assert that the default branch that we are in after this new pull, is ```4.2-dev```. 
Sometimes, more that one brach will be listed, and the currently-checked out branch will be indicated with a '*'. 

```
~/git/joomla-cms $ git branch
* 4.2-dev
```

You can see the code-event tags that have been allocated to the source tree over time, listed in alphabetical (*not chronological*) order. 
In this case, most of the tags indicate code release events:

```bash
~/git/joomla-cms $ git tag -n
...
...
4.2.0           Joomla! 4.2.0 Stable
4.2.0-alpha1    Joomla! 4.2.0 Alpha 1
4.2.0-alpha2    Joomla! 4.2.0 Alpha 2
4.2.0-alpha3    Joomla! 4.2.0 Alpha 3
4.2.0-beta1     Joomla! 4.2.0 Beta 1
4.2.0-beta2     Joomla! 4.2.0 Beta 2
4.2.0-beta3     Bump to version 4.2.0-beta3
4.2.0-rc1       Joomla! 4.2.0 Release Candidate 1
4.2.1           Joomla! 4.2.1 Stable
4.2.1-rc1       Joomla! 4.2.1 Release Candidate 1
4.2.1-rc2       Joomla! 4.2.1 Release Candidate 2
4.2.1-rc3       Joomla! 4.2.1 Release Candidate 3
4.2.2           Joomla! 4.2.2 Stable
4.2.3           Joomla! 4.2.3 Stable
4.2.3-rc1       Joomla! 4.2.3 Release Candidate 1
4.2.4           Joomla! 4.2.4 Stable
4.2.5           Joomla! 4.2.5 Stable
4.2.5-rc1       Joomla! 4.2.5 Release Candidate 1
4.2.5-rc1-sec   Joomla! 4.2.5 Release Candidate 1 with security patches
4.2.6           Joomla! 4.2.6 Stable
4.2.6-rc1       Joomla! 4.2.6 Release Candidate 1
4.2.7           Joomla! 4.2.7 Stable
4.2.7-rc1       Joomla! 4.2.7 Release Candidate 1
4.2.8           Joomla! 4.2.8 Stable
4.3.0-alpha1    Joomla! 4.3.0 Alpha 1
4.3.0-alpha2    Joomla! 4.3.0 Alpha 2
4.3.0-alpha3    Joomla! 4.3.0 Alpha 3
4.3.0-beta1     Joomla! 4.3.0 Beta 1
4.3.0-beta2     Joomla! 4.3.0 Beta 2
4.3.0-beta3     Joomla! 4.3.0 Beta 3
...
...
...
```

As of the time of writing, we will be working on producing a translation update for release ```4.2.8```. 
Since the Joomla project team is very diligent about tagging their releases, we can reliably get the desired Joomla release by checking out the *tag* (*not the  branch!*):
The convention at Joomla Dev HQ for release-based tags is to use the ```major.minor.point[-attribute]``` format for tags.
To get to the code that relates to release 4.2.8, you need to *checkout* the code that has been tagged as release 4.2.8:


```bash
~/git/joomla-cms $ git checkout tags/'4.2.8' 
Note: checking out 'tags/4.2.8'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 1547f8e760 Prepare 4.2.8 release
```

There! You now have all the code that is used for the 4.2.8 release of Joomla. You have also detached yourself from a branch-based reference 
and your code is now organized on a tag-based reference. The code for the other releases is hidden in the various .git directories and you 
can always do a checkout of the other releases if you need to take a peak in them. 

With the up-to-date code for this release in hand, it is now a matter of determining which new language strings need to be translated by 
comparing the existing language files in the language pack repository with those in the joomla-cms repository.

### Cool things you can do in GIT

* Create an alias to see the most recent forks, merges and commits:

Add the following alias to your ```~/.bashrc``` file, re-source your ```~/.bashrc``` file, and then run the alias:

Enter this using vi or nano in your ```~/.bashrc``` file:


```bash
alias gg="git log --graph --all --decorate --oneline"
```

Re-source your ```~/.bashrc``` file:


```bash
$ source ~/.bashrc
```

Now run the new ```gg``` command. It should liik something like this:


```bash
~/git/joomla-cms $ gg
* ddcb66cded (origin/4.3-dev) [4.3] Use CSRF only for the site''s domain (#39580)
* ce66f242a1 Add option to pass a release date to the bumb script (#39904)
* ba80d44d60 Remove dead code from plugin (#39530)
* 4b0a3d6a53 Fix version for now
* a9048d900c Add more information to CS fixer when failed (#39905)
*   000f357107 Merge branch '4.3-dev' of https://github.com/joomla/joomla-cms into 4.3-dev
|\  
| * 4d67edf23c correct folder in gitignore (#39890)
* | 408c0be2a4 Revert to dev
* | bfd0190632 (tag: 4.3.0-beta3) Joomla! 4.3.0 Beta 3
|/  
* 399a853709 [4.3] Update deleted files list in script.php for upcoming 4.3.0-beta3 (#39886)
* 9e68b74d48 Converts the captcha plugins to service providers (#39729)
* 1b74c76142 Converts the finder plugins to service providers (#39770)
*   90457c90e2 Merge pull request #39883 from sdwjoomla/upmerge-20230217-sdw-2
|\  
| * 9140ad087c Update SiteRouter.php
| * 2efb60f641 Merge branch '4.2-dev' into upmerge-20230217-sdw-2
|/| 
* | 85c43acbe5 [4.3] Media Manager error message (#39741)
* | 9d73496bb7 Converts local filesystem plugin to service providers (#39642)
* | 3a1114d736 [4.3] Fix the mobile footer of the source modal (#39873)
* |   76d21f9a31 Revert to dev
|\ \  
| * | 8a1a1816ec correct name of cypress.config in build/build.php (#39841)
| * | 3d0b247bfd add the missing lock component (#39833)
| * | 5fbdbab517 List views onclick (#39730)
| * | c28c8e3db3 [4.3] Admin modules and xtd-editor plugins (#39834)
| * | 34cfccdaa3 Skip side by side (#39828)
...
```
 - all in beautiful technicolour - hit Q to stop the scrolling!


* Avoid confusion: always know your current branch / tag!

You can avoid a lot of confusion and possible mishaps of getting code in branches mixed up by always displaying the currently-selected branch or tag on the command line. This is shown by default if you installed the Linux Git features on Windows. On the Linux terminal, you need to modify the PS1 variable in your local ```~/.bashrc``` file to show the current branch. Remeber to re-source your ```~/.bashrc``` file after such a change. For Debian-based Linux distros (around line 68), change the PS1 assignment to this:

For Debian and Ubuntu variants:

```bash
PS1="${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\] \[\033[01;34m\]\w \[\033[33;1m\]\$(git status 2>/dev/null | head -n1 | cut -d' ' -f3- | sed -e 's/\(at \)*\(.*\)/(\2) /')\[\033[01;34m\]\$\[\033[00m\] "
```

For all other Linuxes:

```bash
PS1="\[\e]0;\u@\h \w\a\]\[\033[01;32m\]\u@\h\[\033[00m\] \[\033[01;34m\]\w \[\033[33;1m\]$(git status 2>/dev/null | head -n1 | cut -d' ' -f3- | sed -e 's/\(at \)*\(.*\)/(\2) /')\[\033[01;34m\]$\[\033[00m\] "
```

This will now show this (assuming you have select branch 4.2.8):


```bash
gerrit@z2 ~/git/joomla-cms (4.2.8) $ 
```

## How much work is involved in creating a language pack?

Let's answer this question with a real-life example and re-usable code that you can run again any time you want to. We use the recently-cloned copy of Joomla CMS. If it was not recently cloned or has not recently been "pulled", then do a *pull* first.

```bash
~/git/joomla-cms $ git pull
Already up-to-date.
```

Use the following commands to determine the workload. The files that need to be translated from are all called ```en-GB.[name].ini``` and a few called ```en-GB.ini```:

* Number of files to translate: _408_

```bash
$ find  ~/git/joomla-cms -name "en-GB.*ini" | wc -l 
408
```

* Number of lines to translate in all these files: _11599_

```bash
$ find ~/git/joomla-cms -name "en-GB.*ini" -exec grep '[A-Z_0-9]*="' {} \; | wc -l
11599
```

* This is how many lines are unique: _9704_

```bash
$ find ~/git/joomla-cms -name "en-GB.*ini" -exec grep '[A-Z_0-9]*="' {} \; | sort -u | wc -l
9704
```

* Number of words to translate: _72284_

```bash
$ find ~/git/joomla-cms -name "en-GB.*ini" -exec grep '[A-Z_0-9]*="' {} \; | cut -d"=" -f2- | sed -e 's/"//g'  -e 's/%.//g'   -e 's/\s*%//' -e 's/<[^>]*>//g' -e 's/\\n/ /g' -e 's/-/ /g' -e 's/:/ /g' | tr [A-Z] [a-z] | tr ' ' '\n' | sort | grep -v "^$" | wc -l
72284
```

* Number of unique words to translate: _4997_

```bash
$ find ~/git/joomla-cms -name "en-GB.*ini" -exec grep '[A-Z_0-9]*="' {} \; | cut -d"=" -f2- | sed -e 's/"//g'  -e 's/%.//g'   -e 's/\s*%//' -e 's/<[^>]*>//g' -e 's/\\n/ /g' -e 's/-/ /g' -e 's/:/ /g' | tr [A-Z] [a-z] | tr ' ' '\n' | sort | grep -v "^$" | sort -u | wc -l
4997
```

Bear in mind that the context varies so a given English word such as 'file' can end up having to be translated into many different words.

* Top occurances of words

This is for interest only, and may give you some insight in how to speed the translation process up. The most frequently-occurring word in the English  source language is the indicative article, 'the', and can be discovered using this rediculously-long BASH one-liner:

```bash
find ~/git/joomla-cms -name "en-GB.*ini" -exec grep '[A-Z_0-9]*="' {} \; | cut -d"=" -f2- | sed -e 's/"//g'  -e 's/%.//g' -e 's/\.//g' -e 's/\s*%//' -e 's/<[^>]*>//g' -e 's/\\n/ /g' -e 's/-/ /g' -e 's/:/ /g' | tr [A-Z] [a-z] | tr ' ' '\n' | sort | grep -v "^$" | uniq -c | sort -nr | head -20
```
```bash
   3841 the
   2314 to
   1304 a
    964 for
    937 of
    917 this
    874 is
    864 in
    795 not
    704 you
    687 or
    654 and
    643 be
    559 will
    522 your
    470 user
    447 select
    418 if
    399 search
    397 language
```

The number of occurances of each word form a Zipfian distribution, which is what you would normally expect the case to be for word counts in a large corpus of any given spoken langauge. When you plot this data on a log-by-log graph, you get a straight-ish line:

```bash
$ find ~/git/joomla-cms -name "en-GB.*ini" -exec grep '[A-Z_0-9]*="' {} \; | cut -d"=" -f2- | sed -e 's/"//g' -e 's/%.//g' -e 's/\.//g' -e 's/\s*%//' -e 's/<[^>]*>//g' -e 's/\\n/ /g' -e 's/-/ /g' -e 's/:/ /g' | tr [A-Z] [a-z] | tr ' ' '\n' | sort | grep -v "^$" | uniq -c | sort -nr | head -300 | gnuplot -e "set terminal dumb; set logscale; set xrange [1:300]; plot '-' with lines notitle"
```
This gives us this graph:
```bash
                                                                               
  10000 +------------------------------------------------------------------+   
        |+                         +                          +           +|   
        |+                                                                +|   
        |+                                                                +|   
        |*****                                                             |   
        |+    **                                                          +|   
        |       *****                                                      |   
   1000 |-+          *************                                       +-|   
        |+                        *****                                   +|   
        |+                             *****                              +|   
        |+                                 *******                        +|   
        |+                                        *******                 +|   
        |                                                *****             |   
    100 |-+                                                   *****      +-|   
        |+                                                        ****    +|   
        |+                                                           *****+|   
        |+                                                                *|   
        |+                                                                +|   
        |+                                                                +|   
        |                          +                          +            |   
     10 +------------------------------------------------------------------+   
        1                          10                        100               
```
It is interesting to compare the graph of the translated package to that generated from the source language. They should be more or less the same straight line shape. So, here goes:

First we get all our words. Here are the top 20 of the af-ZA language pack:

```bash
$ find ~/git/af-ZA_joomla_lang/ -name "af-ZA.*ini" -exec grep '[A-Z_0-9]*="' {} \; | cut -d"=" -f2- | sed -e 's/"//g'  -e 's/%.//g' -e 's/\.//g' -e 's/\s*%//' -e 's/<[^>]*>//g' -e 's/\\n/ /g' -e 's/-/ /g' -e 's/:/ /g' | tr [A-Z] [a-z] | tr ' ' '\n' | sort | grep -v "^$" | uniq -c | sort -nr | head -20
```

```bash
   4224 die
   2664 nie
   1683 'n
   1247 is
   1118 in
   1102 van
    921 hierdie
    902 vir
    861 te
    858 vertoon
    800 word
    777 om
    703 jy
    661 sal
    625 kan
    598 en
    580 of
    570 wat
    564 jou
    536 met
```

Graphing the top 300 word occurances, we get:

```bash
find ~/git/af-ZA_joomla_lang/ -name "af-ZA.*ini" -exec grep '[A-Z_0-9]*="' {} \; | cut -d"=" -f2- | sed -e 's/"//g'  -e 's/%.//g' -e 's/\.//g' -e 's/\s*%//' -e 's/<[^>]*>//g' -e 's/\\n/ /g' -e 's/-/ /g' -e 's/:/ /g' | tr [A-Z] [a-z] | tr ' ' '\n' | sort | grep -v "^$" | uniq -c | sort -nr | head -300 | gnuplot -e "set terminal dumb; set logscale; set xrange [1:300]; plot '-' with lines notitle"
```

This gives us:

```bash
                                                                               
  10000 +------------------------------------------------------------------+   
        |+                         +                          +           +|   
        |+                                                                +|   
        |+                                                                +|   
        |*******                                                           |   
        |+      *****                                                     +|   
        |            ********                                              |   
   1000 |-+                  ********                                    +-|   
        |+                           *******                              +|   
        |+                                 ******                         +|   
        |+                                      ****                      +|   
        |+                                         *******                +|   
        |                                                ******            |   
    100 |-+                                                   *****      +-|   
        |+                                                        *****   +|   
        |+                                                            *****|   
        |+                                                                *|   
        |+                                                                +|   
        |+                                                                +|   
        |                          +                          +            |   
     10 +------------------------------------------------------------------+   
        1                          10                        100               
```

QED.

## How much work will I continually need to invest to maintain a language pack?

You do not need to deliver a translation for every Joomla CMS point release, but it would be nice if you could. Most point releases only require 5 to 10 strings to be translated. 
Some Joomla CMS point releases, however, introduce a significant number of functional changes, which means that there could be quite a few new strings that need to be translated. 
This can be anything up to a 150 new strings that need to be translated. 

# Instructions

The instructions are for any Joomla language pack that uses this tool-set for managing and building language packs for Joomla. Since the first langauae to use this pack was Afrikaans (a-ZA) and it is the first language in the alphabet, we use this for as an example thoughout. The next alphabetically-listed language is Amharic, and yes, this tool set was used to produce a language pack withon knowledge of the language and using the Google Translation Cloud Service.

## Naming conventions

The name of the language pack consists of the 2-letter ISO code of the language (lower-case), hyphenated with the 2-letter ISO code of the country (upper case). So for Afrikaans, which is only really spoken in South Africa (_ZA_), we have the language code _af-ZA_. For English we can have many variants for the countries that it is spoken in, e.g. _en-GB_, _en-US_, _en-NZ_, _en-ZA_, etc.

The name of the packaged file that is installed to Joomla is `[language-specifier]_joomla_lang_full_[version-details]`, where `[version-details]` consists of a series of numbers that follow the `semantic versioning` convention: `[major-revision].[minor-revision].[point-release]v[revision]`. The version number is given by the leader of the Joomla Language Development team, which will always coincide with the version of Joomla that it is aimed at, e.g. `3.9.5`. As leader of your own translation team, after everybidy's efforts have been merged to your working repo, you need to ensure that your repo is tagged as `3.9.5`, since the building process uses this tag value to create the actual package (more on branching and tagging later on). Furthermore, as a translation team leader, you are only allowed to increment the `[revision]` number (starting at 1), and then only every time that you need to publish a revision, say, when you discovered and corrected bug. 

For example, the language pack version would be `3.9.5v1`, and in the case of the `af-ZA` language, your language pack would be called `af-ZA_joomla_lang_full_3.9.5v1`. Since the actual file would be a ZIP file, the final file name would be hosted in Joomla Language Package repository would be called `af-ZA_joomla_lang_full_3.9.5v1.zip`. A subsequent, quick revision release package after having remodied an error would be called `af-ZA_joomla_lang_full_3.9.5v2.zip`. These version details are set up in the build configuration file, that will be described later on. 

## How does the language pack build process work?

The default English en-GB language pack that is bundled with the Joomla installation is used as a reference: The object of the creating or updating a language pack is to get _your_ language pack to contain exactly the same text strings that exist in the English language pack, albeit correctly translated into your specific language. Interestingly enough, the order of the strings is not important, as long as they are all present and carry the correct translations.

Needless to say, Joomla grows and changes all the time, resulting in new language strings getting added to each release. Every time that you execute this build process, it compares what you already have on your langauge pack against the latest English language pack, creates any new (but blank) files where necessary and gives you a work file that tell you what text strings need to be translated. You (and your team) then translate the missing text strings directly in the work file. The less often you produce langauge packs, the more text strings you are likely to have to translate each time. If you are starting from scratch, expect a huge amount of text strings to be translated. Once the translations are completed in the work file, you can then 'execute' the work file and run the build process to create your language pack. 

Here is a detailed step-wise explansion for this process:


### Step 0: Get you reference Joomla Core code base

You will need to have the Joomla source code of the release that you are creating a language pack for. The language pack build process (more on this later) unpacks the Joomla installation and uses the default English (en-GB) language strings as a reference, against which the a report is generated of missing language strings so that your language can be brought into alignment with the source reference. If the Joomla installation package has already been published as a .zip or a .tar.gz file, you can use this as your source reference when you run the build process.

If latest Joomla package has not been released yet but the source code is ready to be translated, you can use the source code out of the Joomla Git repository instead as a reference. Some additional preparation is required to get the latest Joomla source code from the Joomla Git repo, however, before running the build process.

If you previously created a local Joomla Git repo, just do a refresh of the repo with the `pull` command (ignore the last comments and instructions - these are meant for actual Joomla PHP developers). Also make sure that you are working with the correct Joomla release, which is marked as a tag, and not as a branch. In this example case, we will be producing an updated language pack for Joomla 4.2.8:


```bash
~/ $ cd ~/git/joomla-cms
~/git/joomla-cms $ git pull
etc...
~/git/joomla-cms $ git checkout tags/'4.2.8'
etc...
...
```

### Step 1: Select the relevant release

The releases in the Joomla Git repo are identified by their git __tags__, and not by their git __branches__. It is therefore important to check the code out that corresponds to the relevant tag. The release will be communicated to all Translation Teams and from this you can select the correct tag to use, e.g. from the Joomla Translation leader, you will get an instruction to create a new language pack for the new Joomla release `x.y.z`. You will need to look for the most recent tag that contains the `x.y.z`.

```email
From: Ilagnayeru Manickam <mig.joomla@gmail.com>
To: translations@lists.joomla.org
Subject: [Joomla Translation Team] Be Prepared for Joomla! x.x.x

Good Afternoon!

Be known that Joomla! x.x.x RC has been released (https://github.com/joomla/joomla-cms/releases/tag/x.x.x-rc).  Joomla! x.x.x stable version is expected to be released on dd/mm/yyyy ([https://github.com/joomla/joomla-cms/milestones).
...
...
Thanks.

- Ilagnayeru (MIG) Manickam
  Joomla! Translations Coordination Team
```   

### Step 2: Check out code against a tag from the reference Joomla Code Base

Now that you have pulled the latest Joomla source code repo in the previous step, list the available tags and select the relevant required tag, for instance `3.9.5`:

```bash
~/git/joomla-cms $ git tag -n | grep "^4.2.8"
4.2.8           Joomla! 4.2.8 Stable
|<----Tag---->| |<----Tag Comment---------------...
```

_NOTE:_ 
> Note the part that is a tag itself and the part that is a tag comment.
> Do not confuse Tags with Tag Comments, 
> Do not confuse Tag Comments with Code Commit comments!
> Do not confuse Tags with Branches!
> Do not confuse Branches with Tags!


Checking out a tag is similar to checking code against a branch, except that we need to explicity specify that this is a tag, or multiple tags, by using the `tags/`-specifier.

```bash
~/git/joomla-cms $ git checkout tags/'3.9.5-rc'
Checking out files: 100% (9506/9506), done.
Previous HEAD position was 6b8fd2b21f Tag Alpha 6
HEAD is now at 1547f8e760 Prepare 3.9.5 release
```

Check what we have:

```bash
~/git/joomla-cms $ git status
nothing to commit, working tree clean
HEAD detached at 3.9.15
```

We have now successfully checked out the required reference code from the Joomla Code Base. Since we are not going to develop any code off the Joomla main tree and are going to concentrate on developing the langauge pack only, we can leave this repo until the next time release.

### Step 3: Get your language pack repo

Get the latest version of your language pack repo. Note that this is a different repo to the Joomla Core code base repo in [https://github.com/joomla/joomla-cms](#https://github.com/joomla/joomla-cms). This one is, for example, at [https://github.com/gerritonagoodday/af-ZA_joomla_lang](#https://github.com/gerritonagoodday/af-ZA_joomla_lang). Do a clone:

```bash
$ cd ~git
~/git $ git clone https://github.com/gerritonagoodday/af-ZA_joomla_lang
...
~/git $ cd af-ZA_joomla_lang
```

Check if you are on the `master` branch - look for the `*`:

```bash
~/git/af-ZA_joomla_lang $ git branch
  3.9.3v1
  3.9.4v1
  3.9.4v2
* master
```

If not, check out the master branch first:

```bash
~/git/af-ZA_joomla_lang $ git checkout master
```

### Step 4: Create a branch in your local remote repo 

We need to branch our existing language pack, to a banch called "4.2.8", since a number of candidate translation files will be
prduced, that need to collected by translation team members, translated and then checked back in. 

Create a new branch in the remote repo, named according to the Joomla release that you are creating this package for and also your own release version (starting at 1), e.g. "3.9.5v1", pull the repo, and check-out the branch. This is your working area. 

```bash
~/git/af-ZA_joomla_lang $ git branch 4.2.8v1
~/git/af-ZA_joomla_lang $ git checkout 4.2.8v1
Switched to branch '4.2.8v1'
```

_NOTE:_
> It is very useful to stick to the x.y.zv[1..] naming convention throughout your process. 

### Step 5: Set up the package configuration file

In the `utilities/configuration.sh` file, set the following values. 
Unless you are creating a language pack for a new language, you only need to change the first variable, 
```export TRANSLATIONVERSION_XML="4.2.8.1"```. 
The comments in the rest of the file should be self-explanitary:

   ```bash
   # For your first release of version 3.9.5, say this:
   export TRANSLATIONVERSION_XML="4.2.8.1"
   # If your lanaguage is something other than af-ZA, change this:
   TARGETLINGO="af-ZA"
   # You may have called the repo for your language something else, 
   # although it helps to stick to this convention. Change this 
   # if your lanaguage is something other than af-ZA:
   GITREPONAME="af-ZA_joomla_lang"
   # Your langauge term for the word "Author"
   LOCAL_AUTHOR="Outeur"
   # Language name - in your own language and the English exonym (
   # Note: endonym is the local name for the language: 
   #           'Kiswahili' or 'Deutsch' or 'isiZulu'.
   #       exonym is what 'outsiders' use to refer to the language: 
   #           'Swahili' or 'German' or 'Zulu'.
   LINGONAME="Afrikaans (ZA)"
   LINGOEXONYM="Afrikaans"
   # This is the native name for the language and needs to be in the local script
   LINGOINDONYM="Afrikaans"
   TARGETCOUNTRY="South Africa"
   # Description of the langauge on one line.
   # This in your target language: "xxxxx (country xxx) translation for Joomla!"
   PACKAGE_HEADER='Afrikaanse Vertaling vir Joomla!'
   # Your langauge term for: "xxxxx language pack in the informal form of address", or something similar
   PACKAGE_DESC="Afrikaanse Taalpaket in die vertroulike aanspreeksvorm"
   # Local language terms:
   # Your langauge term for "Language"
   LOCAL_LANGUAGE="Taal"
   # "Schema"
   LOCAL_SCHEME="Skema"
   # Your langauge term for "Author"
   LOCAL_AUTHOR="Outeur"
   # Your langauge term for "Website"
   LOCAL_WEBSITE="Webwerf"
   # Your langauge term for "Revision"
   LOCAL_VERSION="Hersiening"
   # Date
   LOCAL_DATE="Datum"
   # "Please check the project website frequently for the most recent translation"
   LOCAL_INSTALL="Laat asb. weet indien daar enige tik-foute of grammaktia-foute is - hulle sal so spoedig moontlik reggemaak word!"
   # "All rights reserved" in your language, or use the English.
   LOCAL_ALLRIGHTS="Alle regte voorbehou"
   # Right To Left = 0 for most languages
   RTL=0
   # Locales by which this lnaguage is known
   # e.g. for German: de_DE.utf8, de_DE.UTF-8, de_DE, deu_DE, de, german, german-de, de, deu, germany
   LOCALE="af_ZA.uft8, af_ZA.UTF-8, af, af_ZA, afr_ZA, af-ZA, afrikaans, afrikaans-za, afr, south africa, suid-afrika"
   # First day of the week in the locale, mostly 1 = Sunday, sometimes 2 = Monday or 6=Saturday
   FIRSTDAY=1
   # First day of actual week, mostly 0 = Sunday (default)
   # Last day of actual week, mostly 6 = Saturday (default)
   #  - in which case, specify: "0,6"
   WEEKEND="0,6"
   # Calendar. Choose from "gregorian" (default), "persian", "japanese", "buddhist", "chinese", "indian", "islamic", "hebrew", "coptic", "ethiopic"
   CALENDAR="gregorian"

   # Name of package author or team
   AUTHORNAME="[your name]"
   # Email address of author or team
   AUTHOREMAIL="[your email address]"
   # Installation Configuration:
   #     A flag to display on successful completion of installation
   #     The Recommended size for the images is 256x256 pixels, PNG format and with background alpha-channeled.
   #     Find your flag in http://www.flags.net
   LINGOFLAG="http://www.flags.net/images/largeflags/SOAF0001.GIF"
   #     The website that hosts this translation team
   LINGOSITE="http://forge.joomla.org/gf/project/afrikaans_taal"
   ```

### Step 6: Produce a work file of what strings need to be translated

You, as team lead: Run the ```Chk4NewLanguageStrings``` utility:


```bash
~/git/af-ZA_joomla_lang $ cd utilities
~/git/af-ZA_joomla_lang/utilities $ ./Chk4NewLanguageStrings.sh -p=~/git/joomla-cms
[97] Making /home/gerrit/git/af-ZA_joomla_lang/utilities/../.build...
[2019.10.28 11:55:35][][INFO ][332] === BEGIN [PID 29222] Chk4NewLanguageStrings.sh ===
[2019.10.28 11:55:35][][INFO ][210] Checking configuration file
[2019.10.28 11:55:35][][INFO ][213] Reading configuration file configuration.sh
[2019.10.28 11:55:35][][INFO ][218] Checking sandbox directory is where this is launched from
[2019.10.28 11:55:36][af-ZA][INFO ][401] Checking / fixing target subversion directory layout
[2019.10.28 11:55:36][af-ZA][INFO ][288] Using unpacked installation from directory /home/gerrit/git/joomla-cms
[2019.10.28 11:55:36][af-ZA][INFO ][488] Comparing number of .ini files:
[2019.10.28 11:55:36][af-ZA][INFO ][495] - Number of en-GB files: 407
[2019.10.28 11:55:36][af-ZA][INFO ][496] - Number of af-ZA files: 407
[2019.10.28 11:55:36][af-ZA][INFO ][498] Files in the en-GB source translation that don''t yet exit in the af-ZA translation:
[2019.10.28 11:55:36][af-ZA][INFO ][503]  - Total 0 file(s)
[2019.10.28 11:55:36][af-ZA][INFO ][505] Files in the af-ZA target translation that don''t exit in the en-GB translation any more:
[2019.10.28 11:55:36][af-ZA][INFO ][510]  - Total 0 file(s)
[2019.10.28 11:55:36][af-ZA][INFO ][589] Determine changes in language strings that belong to the site section...
[2019.10.28 11:55:38][af-ZA][INFO ][615] === Summary of required work for the ''SITE'' section: === 
[2019.10.28 11:55:38][af-ZA][INFO ][619] Number of NEW Strings in en-GB source language not in af-ZA target language: 35
[2019.10.28 11:55:38][af-ZA][INFO ][620] Number of OLD Strings in af-ZA target language not in en-GB source language: 0
[2019.10.28 11:55:38][af-ZA][INFO ][621] Total number of af-ZA files that need to be modified: 3
[2019.10.28 11:55:39][af-ZA][INFO ][589] Determine changes in language strings that belong to the admin section...
[2019.10.28 11:55:45][af-ZA][INFO ][615] === Summary of required work for the ''ADMIN'' section: === 
[2019.10.28 11:55:45][af-ZA][INFO ][619] Number of NEW Strings in en-GB source language not in af-ZA target language: 17
[2019.10.28 11:55:45][af-ZA][INFO ][620] Number of OLD Strings in af-ZA target language not in en-GB source language: 5
[2019.10.28 11:55:45][af-ZA][INFO ][621] Total number of af-ZA files that need to be modified: 5
[2019.10.28 11:55:49][af-ZA][INFO ][589] Determine changes in language strings that belong to the install section...
[2019.10.28 11:55:49][af-ZA][INFO ][615] === Summary of required work for the ''INSTALL'' section: === 
[2019.10.28 11:55:49][af-ZA][INFO ][619] Number of NEW Strings in en-GB source language not in af-ZA target language: 1
[2019.10.28 11:55:49][af-ZA][INFO ][620] Number of OLD Strings in af-ZA target language not in en-GB source language: 0
[2019.10.28 11:55:49][af-ZA][INFO ][621] Total number of af-ZA files that need to be modified: 1
[2019.10.28 11:55:49][af-ZA][INFO ][877] ============= Next step: =====================================
[2019.10.28 11:55:49][af-ZA][INFO ][892]

Add this file to git:

  utilities $ cd ..
  $ git add .
  $ git commit -m \"ready to translate\"
  $ git -u origin push

Doing this by yourself?

  Do all the translations by editing the patch file.

Doing this as a team?

  If a team worked on this file, issue a pull-request to your team members
  and get them to check their merged work in, and then do a pull to get the 
  hopefully completed patch file. Check that all strings are translated.

As the project lead: 

Execute the completed patch file. 

Check the changes back into the repository with the commands:

  $ cd ~/git/af-ZA_joomla_lang
  $ git commit -m "Patched to next Joomla release"
  $ git push


[2019.10.28 11:55:49][af-ZA][INFO ][184] === END [PID 29222] on signal EXIT. Cleaning up ===
```

This will create the work file called `Workfile_[isolang-isocountry]-[x.x.x.x].sh`in the repo's root directory. This work file is actually a BASH-script with space left for where the translations need to be inserted. If it is just a point release, this work file may be only a few pages long. Do not execute this BASH-script yet. If there are any new files that need to be added to the package, they will be created, named accordingly, and added to the current branch in your local repo (you did check out the correct branch, right?) 

### Step 7: Prepare to distribute the workload: Push the changes to the remote repo

Do a commit and a push to get all this work to the remote repo, from which the other team members can access it with a `pull` operation to their own local repo. Any new files that had to be created for this new release will now also exist in the branch, albeit empty files that only contain headers:

```bash
utilities $ cd ..
$ git add .
$ git commit -m "ready for team to translate"
[3.9.5v1 6408b5c] ready for team to translate
 xxx files changed, xxx insertions(+), xxx deletions(-)
```

Now push this branch 3.9.5v1 that you have created locally, to the remote repo:

```bash
$ git push -u origin 3.9.5v1
Username for 'https://github.com': xxxx
Password for 'https://gerritonagoodday@github.com': xxxxx 
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (9/9), done.
Writing objects: 100% (9/9), 4.92 KiB | 839.00 KiB/s, done.
Total 9 (delta 6), reused 0 (delta 0)
remote: Resolving deltas: 100% (6/6), completed with 5 local objects.
remote: 
remote: Create a pull request for '3.9.5v1' on GitHub by visiting:
remote:      https://github.com/gerritonagoodday/af-ZA_joomla_lang/pull/new/3.9.5v1
remote: 
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
 * [new branch]      3.9.5v1 -> 3.9.5v1
Branch '3.9.5' set up to track remote branch '3.9.5' from 'origin'.
```

The `-u` parameter is important, and means that any future commits from this local branch will go to the corresponding remote branch, without having to specify this in future push operations.

You can check if the remote repo has this branch by listing the remote branches. Use the `-r` parameter to indicate that you are listing branches on the __remote__ repo:

```bash
~/git/af-ZA_joomla_lang $ git  branch -r
  origin/3.9.5v1
  origin/4.0
  origin/HEAD -> origin/master
  origin/master
```

At this stage, everything is prepared for a team of translators to get stuck in to translating the Workfile. 

### Step 8: Split the report out amoung the translation team members 

Team lead: Allocate chunks of the work file `Workfile_[isolang-isocountry]-[x.y.z.v].sh` by line numbers or "jobs" and agree who in the team will translate what chunk and who will review each chunk. Usefully, the sections in the file are divided up into "jobs", with each "job" corresponding to an affected file. There is no need to physically split the file, in fact, this would be counter-productive. Everyone must start their effort off by doing a clone or a pull, followed by a check-out of the relevant branch, e.g. "3.9.5v1" to get their copy of the (entire) work file:

Each of the team members will need to have performed Step 3 and Step 4, but here is a recap for team members:

```bash
~/git/ $ git clone [git URL]
~/git/ $ cd [git project]
```

or, if you previously obtined a copy of the repo:

```bash
$ cd ~/git/[git project]
~/git/[git project] $ git pull
```

Then check out the branch:

```bash
~/git/[git project] $ git checkout 3.9.5v1

```

Once the team member has done this, the translation effort can begin.

### Step 9: Team members do the translations and merge the resulting workfile back into the repo

Every translation team member: Translate and review your allocated chunk of the work file and then merge it back from the working branch into the repo's master branch when completed or when the project lead has issued a pull request. 

To merge the branch that you were working on into the 'master' branch, you need to go into the 'master' branch. Remember that you can only enter another branch if you have either committed all changes or have stashed any changes under keepsafe-name for later unstashing and use. If you don't do this, you will loose all your current changes. This may in rare cases be actually desired. So, assuming that your translation work is completed in your working branch, check it into your local repo (i.e. no `push` to the remote repo):

```bash
$ git add .
$ git commit -m "king_olav is complete"
```

No go into the master branch:

```bash
$ git checkout master
switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
```

Do a pull from the remote repo to collect all the work that other team members have already submitted, since 
you are about to merge your work into theirs (and the next team member's work will be merged into yours and your predecessors' work:

```bash
$ git pull
```

Now do the merge in your local repo:

```bash
$ git merge '3.9.5v1'
Updating ed0cb7b..e945c5e
Fast-forward
 WorkFile_af-ZA-3.9.5.1.sh  | 258 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
1 files changed, 944 insertions(+), 133 deletions(-)
```

Git is very good at resolving the changes to a single file made by multiple users. Your merge efforts will not be visible on the remote repo until you do a push of your master branch:

```bash
$ git push
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
   ed0cb7b..e945c5e  master -> master
```

_NOTE:_
>1. You need to 'be in the branch' that is being merged to. Use the `git checkout '[branch-name]'` for this.
>2. The code is automatically commited after a merge. No need to do a `git commit` after a successful merge.
>3. Sometimes, a merge conflict arrises and needs to be resolved through your manual intervention. See the next section.

### Step 10: Run the work file script and commit and push all the files



```bash
$ git merge '3.9.5v1'
Updating ed0cb7b..e945c5e
Fast-forward
 WorkFile_af-ZA-3.9.5.1.sh                              | 258 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 administrator/language/af-ZA/af-ZA.com_content.ini      |   1 +
 administrator/language/af-ZA/af-ZA.com_fields.ini       |  10 +--
 administrator/language/af-ZA/af-ZA.com_joomlaupdate.ini |   7 ++
 administrator/language/af-ZA/af-ZA.ini                  |   1 +
 administrator/language/af-ZA/af-ZA.lib_joomla.ini       |   3 +
 installation/language/af-ZA/af-ZA.ini                   |   1 +
 language/af-ZA/af-ZA.finder_cli.ini                     |   3 +
 language/af-ZA/af-ZA.ini                                |  29 ++++++++
 language/af-ZA/af-ZA.lib_joomla.ini                     |   3 +
 9 files changed, 949 insertions(+), 133 deletions(-)
 create mode 100755 WorkFile_af-ZA-3.9.5.1.sh
```

Your merge efforts will not be visible on the remote repo until you do a push of your master branch :

```bash
$ git push origin master
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
   ed0cb7b..e945c5e  master -> master
```


The team lead does a final pull of the work file, a final integrity check, and then executes the work file (which is a BASH shell script after all).
This can only be done by the team lead, since the work file will be based on the lead's particular work directory setup. 
This results in changes to many files and these all need to be added, committed and pushed to the remote repo. 


In the role of team lead, you do not need to do any merging as the translators will have done this when merging their efforts with that of the others. Run the workfile like this:

```bash
 ~/git/af-ZA_joomla_lang $ ./WorkFile_af-ZA-3.9.5.1.sh 
```

If there are no errors, then the work file script run successfully.
Check if the changes made correspond to the file referred to in the work file. You should see all the language file that were altered:

```bash
~/git/af-ZA_joomla_lang $ git status
On branch 3.9.5v1
Your branch is up-to-date with 'origin/3.9.5v1'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   WorkFile_af-ZA-3.9.5.1.sh
        modified:   administrator/language/af-ZA/af-ZA.com_content.ini
        modified:   administrator/language/af-ZA/af-ZA.com_fields.ini
        modified:   administrator/language/af-ZA/af-ZA.com_joomlaupdate.ini
        modified:   administrator/language/af-ZA/af-ZA.ini
        modified:   administrator/language/af-ZA/af-ZA.lib_joomla.ini
        modified:   installation/language/af-ZA/af-ZA.ini
        modified:   language/af-ZA/af-ZA.finder_cli.ini
        modified:   language/af-ZA/af-ZA.ini
        modified:   language/af-ZA/af-ZA.lib_joomla.ini

no changes added to commit (use "git add" and/or "git commit -a")
```

Now commit the changes and push the branch back to remote. Since you are by default still referring to the remote branch "3.9.5v1", there is no need to specify the '3.9.5v1'-bit again:
 

```bash
~/git/af-ZA_joomla_lang $ git add .
~/git/af-ZA_joomla_lang $ git commit -m "Translations applied to relevant language files"
~/git/af-ZA_joomla_lang $ git push
```

### Step 11: Test the language files for integrity

Team lead: Run the `Chk4BrokenLanguageStrings.sh` script, which will perform a series of integrity checks and produce a list of any found defects:

```bash
~/git/af-ZA_joomla_lang $ cd utilities 
~/git/af-ZA_joomla_lang/utilities $ ./Chk4BrokenLanguageStrings.sh
....
```

If any defects are found, they need to be fixed and this integrity test needs to be run again, until all errors are gone. 
Update the local and the remote branch again with any corrected changes. To recap, this s how:

```bash
~/git/af-ZA_joomla_lang/utilities $ cd ..
~/git/af-ZA_joomla_lang $ git add .
~/git/af-ZA_joomla_lang $ git commit -m "last changes"
[3.9.5 f7a644d] Translations applied to relevant language files
 10 files changed, 257 insertions(+), 75 deletions(-)
~/git/af-ZA_joomla_lang $ git push
```

### Step 12: Tag the translation in the local and then the remote repo

Remember that the tag name needs to complly to the format ```[major].[minor].[point]v[revision]```.

```bash
~/git/af-ZA_joomla_lang $ git tag '3.9.5v1' -m "Package built and tested"
```

It is not possible to tag the remote repo as such, but you can push the tag in the local repo to the remote repo.

```bash
~/git/af-ZA_joomla_lang $ git push origin  --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 178 bytes | 178.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
 * [new tag]         3.9.5v1 -> 3.9.5v1
```

### Step 13: Build the package using the package build script 

Team lead: Run the `MkLanguagePack.sh` script, which will produce a language pack file ready for you to install into Joomla.

```bash
~/git/af-ZA_joomla_lang $ cd utilities
~/git/af-ZA_joomla_lang/utilities $ ./MkLanguagePack.sh
...
```

### Step 14: Test the language pack in Joomla

Make sure that the language pack installs in Joomla with no errors. Turn on Joomla Debugging for Language, and see if there are any errors while running through the features that were affected by the new translation strings. Now would be a good time to tag the local repo with the release number and to push this to the remote repo too.


### Step 15: Release the translation package

The translation work for this release is now done. 

You are now ready to release the new langauge pack when the next version of Joomla is released. If your language is an officially-supported language in Joomla, will Joomla administrators automatically be notified once they have upgraded their version of Joomla and the new language pack version is available. It can be installed with a single click from the Administrator's panel. For this to happen, you need to publish your language pack to the project "Joomla!®3.x Accredited Translations" on http://forge.joomla.org/gf/project/jtranslation3_x/, under the section "Releases" for your language. Andto be able to do this, you need to sign up as an official translation team with Joomla Langiage at http://lists.joomla.org/mailman/listinfo/translations_lists.joomla.org. 


## What is this translation work file?

The work file is in fact a BASH script files and is a useful way of splitting the work between many team members.

When all the strings in it have been translated, the file is simply executed, which will distribute the translated strings to the correct files. The work file is executed like this:

```bash
./utilities/WorkFile_af-ZA-x.y.z.v.sh
```

The file consists of statements like these:

* To add a new string

Example:

```bash
echo "COM_FIELDS_FIELD_EDITABLE_IN_ADMIN=\"Administrator\""\
     >> /home/gerrit/git/af-ZA_joomla_lang/administrator/language/af-ZA/af-ZA.com_fields.ini
```

All you need to do is translate the string between the \"...\" markers. Don't touch the other escape-characters.

* To remove an old string

Example:

```bash
sed -e "/COM_FIELDS_FIELD_SHOW_ON_ADMIN\s*=/d" -i /home/gerrit/git/af-ZA_joomla_lang/administrator/language/af-ZA/af-ZA.com_fields.ini
```

No need to do anythnig with these, just make sure they don't get lost.

## Special case: Create a new language pack

This step is only required if you want to create a new Joomla language pack. Let's assume you want to create a language pack to Amharic / Ge'ez for Ethiopia ('am-ET'). You will only need the `utilities` directory from the 'af-ZA' project and will need to chancge a few values in the configuration.sh file, which is very well documented:

```bash
~/git $ mkdir am-ET_joomla_lang
~/git/am-ET_joomla_lang $ cp -r ../af-ZA_joomla_lang/utilities .
```

Set the configuraton values in `configuration.sh` and continue with the following steps. If you are looking for a quick prototype language pack with about 75% translation accuracy mixed in with utter nonsense (you have been warned), select the 'Google Translate' option below.

## Tips when Translating

__Things to remember:___
> All string identifiers are in upper case.
> Each strings is just one line - even if a long line wraps around your screen.
> Each string begins and ends with double inverted commas. Any embedded inverted commas in a string need to be escaped.

See the section __Top occurances of words__ on what the most frequently-occurring words are. You could potentially translate all these words in whole-word mode (__important!__) and perform a series of case-sensitive search-and-repleace across the entire work file before deseminating the file to team members. Experienced translators don't mind multiple jumbled-up languages. You could save a significant amount of typing like this. You can also try this technique with short phrases.

Have an agreed set of technical terms ready for your translation. Many technical terms are new and may not yet be established in your particular language. You could apply the search-and-replace techniqie here too. Either way, be consistent in the choice of terminology. Create a dictionary like the one in https://github.com/gerritonagoodday/af-ZA_joomla_lang/wiki/woordeboek-dictionary. 

Use HTML-codes for diacritic characters, e.g. &ecirc; = `&ecirc;`, &auml; = `&auml;`, etc...  to avoid any code mangling when some hapless soul opens and saves a language file with a rubbish editor. Learn the HTML-codes off by heart.

## Working with Google Translate

If you specificy the `-g` option when running `Chk4NewLanguageStrings.sh`, the program 
`google_translate.pl` will be used to invoke the Google Translate API. 
See [https://github.com/gerritonagoodday/google_translate](https://github.com/gerritonagoodday/google_translate)
on follow the instructions for setting yourself up as Google Cloud API user.
This API is not yet perfect and in particular can really mess things up where parts of a string have been marked as "notranslate",
with the aim that Google leaves things as is there. When things get messed up, the string "notranslate" ends up
in the translated text and other bit go wrong too, so this must be manually dealt with in the generated work file. 

If you are in a hurry and want to remove these failures, say, when building an incomplete new language package prototype,
you can remove these from the work files as follows:

```bash
sed '/notranslate/,+1 d' -i WorkFile_am-ET.sh
```

# Git Cheat Sheet

## Where on earth am I?

* The GO-TO Git command, ```git status```:

```bash
$ git status
On branch 3.9
Your branch is up-to-date with 'origin/3.9'.
```

* Show branch tree (in glorious technicolor). Look for the branch containing 'HEAD' - that is your currently-selected branch. 

```bash
$ git log --graph --all --decorate --oneline
* 25793d9 (origin/master, origin/HEAD) Update README.md
| * c9459b9 (origin/4.0, 4.0) new files
| * 4a60d7c 4.0 start
|/  
| * 7705d44 (HEAD -> 3.9, origin/3.9) 3.9
|/  
* 0fdd6dd (master) 
```

Make this command into an alias ```gg``` - see [Cool things you can do in GIT](#cool-things-you-can-do-in-git)

## Renaming files

If the file was already established in your from a previous staging operation (`git add`), you can simply rename the file in your IDE or in the command line: `mv old_filename new_filename`. You can also explicitly rename a file with the 'git move' command:

```bash
$ git mv old_filename new_filename
```

Confirm that was successful:

```bash
$ git status
   renamed: old_filename -> new_filename
```

_NOTE:_
>Remember to commit file name changes.


## Deleting files

If the file was already established in your from a previous staging operation (`git add`), you can't simply delete the file in your IDE or in the command line: `rm filename`, you must explicitly delete a file with the 'git rm' command:

```bash
$ git rm filename
```

If the file was also previously committed, you need to commit this deletion:

```bash
$ git commit -m "cleanup" filename
```

## Post your changes to the staging area

Your local Git repository can be thought of as these file storage areas:

* Your Working Directory
* The Staging area,a.k.a. the Index
* The local HEAD, which contains the most recently-committed changes
* The remote HEAD, which contains the most recently-committed changes from all contributors

We use the following processes to progress our work through these phases:

1. To *Post* all changes from your working directory to the staging area, save your working files first and then do this:

```bash
$ git add .
```

2. To *Commit* all staged changes to the current branch,use the *commit* command. Add a useful comment here to explain your working. If you are using an issue management system such as JIRA, start the comment with the JIRA Issue Number:

```bash
$ git commit -m "[PROJECT]-[ISSUE#] [Explanation of commit]"
[3.9 642042e] Update
 1 file changed, 184 insertions(+), 5 deletions(-)
```

3. *Push* these changes through to the remote repository's corresponding branch. If the branch does not yet exist on the remote repo because you have created a new branch and have now worked on it, this new branch will be created.

```bash
$ git push 
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 4.02 KiB | 187.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
   7705d44..642042e  3.9 -> 3.9
```

## Remote Repositories

The above `push` operation of the most recent changes to a remote repo introduces the concept of interacting with other source repositories. Assuming that you have been developing stuff in the absence of a version control system like Git and would like to add your collection of work into the root of a large project _somewhere else_, you can convert your work into a Git repo and then make it part of the remote. 

* Convert your non-versioned work into a repo

```bash
$ git init
Initialised empty Git repository in [my_project_directory]
```

* Add all the files in your project to your new (local) repo's staging area:

```bash
$ git add .
```

* Commit the staged files. This also makes them eligiable for pushing over to the remote repo in the next step:

```bash
$ git commit -m 'My project files'
```

The files are still not in the remote location. This is done in the next step.

* Join the remote repo (of which you know the remote git-file's URL) with your repo

First, check that no remote repo is already associated with this local repo:

```bash
$ git remote
$
```

Nothing. Now connect your local repo (notionally called 'origin') to the repo on the remote server (notionally called 'remote'):

```bash
$ git remote add origin https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
```

Check again if the _remote repo_ is associated with this _local repo_:

```bash
$ git remote
$ origin
```

Yes, it is, and it is called 'origin', which is an _alias for the remote repo_. You can see this in more detail with the _verbose_ version of this command:

```bash
$ git remote -v
origin  https://github.com/gerritonagoodday/af-ZA_joomla_lang.git (fetch)
origin  https://github.com/gerritonagoodday/af-ZA_joomla_lang.git (push)
```

* Finally, push the files from origin to master

```bash
$ git push origin master
```
If you have not set up your user credientials to the _remote repo_, you will be prompted to do so now.

## Branches

This section lists a bunch of recipes and cheats that relate to branches.

### A simple approach to use branches

By default, a Git repository has at least one branch, called 'master'.

Branches are created to develop specific feature sets in, or to create a specific release with feature sets in. Once the development of the branch has been completed, the code can optionally be 'tagged' with the release name, and the branch can be merged back into the master branch. All branches must eventually rendevouz to the master branch. When a branch has successfully been merged into the master (branch), the branch has effectively been removed. All that remains is the optional 'tag' of the code. 

Should a branch need to be revisited in order to, say, fix a minor problem for a point-release, a branch can be made from the previous release tag. The fix is made, added, commited and pushed to remote, a point-release package is built, and the branch tagged with the point-release name and is merged into the master again, and the point-release branch does not exist any more. Repeat this process as often as required.

### List all branches in your local repo. 

The one marked with a '*' is the currently-selected branch:

```bash
$ git branch
* 3.9
  4.0
  master
```

### List all branches in your remote repo. 

There is no concept of a "currently-selected branch" on the remote repo:

```bash
$ git branch -r
  origin/3.9.5v1
  origin/3.9.5v3
  origin/3.9.6v1
  origin/4.0
  origin/HEAD -> origin/master
  origin/master
```

### Select an existig branch, i.e. checkout a branch:

```bash
$ git checkout '3.9.5v1'
Switched to branch '3.9.5v1'
Your branch is up-to-date with 'origin/3.9.5v1'.
```

Also do this to intentionlly obliterate any code changes that you have made thus far on branch '3.9' and have not committed and pushed to remote, and to start afresh.

### Create a new branch off the currently-selected branch:

```bash
$ git branch [new_branch_name]
```

For example:

```$ git branch '3.9.5v1'```

Check the result - a new brnach was created but you are still on the last selected branch:

```bash
$ git branch
* 3.9.5v1
  4.0
  master
```

### Delete a branch from the local repo

Do this to remove a branch and all its work from your local repo:

```bash
$ git branch -d '3.9.5'
Deleted branch 3.9.5 (was 7705d44).
```

### Delete a branch from the remote repo

Do this to remove a branch and all its work from your remote repo:

```bash
$ git push origin -d '3.9.5'
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
 - [deleted]         3.9.5
```

### Push a branch to your remote repo

One way to make your work in your local branch visible to the public is to commit your branch and to push it to the remote repo. Chance are that the branch does not yet exist on the remote repo, so you will need to force the creation of the branch on the remote repo:

```bash
$ git push --set-upstream origin 3.9.5v1
...
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
 * [new branch]      3.9.5v1 -> 3.9.5v1
Branch '3.9.5v1' set up to track remote branch '3.9.5v1' from 'origin'.
```

_NOTE:_
>Remeber to add and commit your changes first before pushing to remote.

Once you have created the branch on the remote repo, you can push as per normal:

```bash
$ git push
Everything up-to-date
```

### Rename a branch

```bash
$ git branch -m [old_name] [new_name]
```

Make this name change effective on the remote repo by adding a new branch (because you can't rename a branch on remote unless you are actually on the git server):

```bash
$ git push origin [new_name]
```

### Merge branches

Merging code __from__ another branch into your currently-checked-out branch cleverly folds the files and lines of code in the files together, and also commits the changes at the same time. Of course, only commiteted code can be merged to somewhere else. 

To merge the branch that you were working on into the 'master' branch, you need to go into the 'master' branch. Remember that you can only enter another branch if you have either committed all changes or have stashed any changes under keepsafe-name for later unstashing and use. If you don't do this, you will loose all your current changes. This may in rare cases be actually desired.

```bash
$ git checkout master
switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
```

Now do the merge in your local repo:

```bash
$ git merge '3.9.5v1'
Updating ed0cb7b..e945c5e
Fast-forward
 WorkFile_af-ZA-3.9.12.1.sh                              | 258 +++++++++++++++++++++++
 administrator/language/af-ZA/af-ZA.com_content.ini      |   1 +
 administrator/language/af-ZA/af-ZA.com_fields.ini       |  10 +--
 administrator/language/af-ZA/af-ZA.com_joomlaupdate.ini |   7 ++
 administrator/language/af-ZA/af-ZA.ini                  |   1 +
 administrator/language/af-ZA/af-ZA.lib_joomla.ini       |   3 +
 installation/language/af-ZA/af-ZA.ini                   |   1 +
 language/af-ZA/af-ZA.finder_cli.ini                     |   3 +
 language/af-ZA/af-ZA.ini                                |  29 ++++++++
 language/af-ZA/af-ZA.lib_joomla.ini                     |   3 +
 utilities/configuration.sh                              |   4 +-
 10 files changed, 949 insertions(+), 133 deletions(-)
 create mode 100755 WorkFile_af-ZA-3.9.12.1.sh
```

_NOTE:_
>1. You need to 'be in the branch' that is being merged to. Use the `git checkout '[branch-name]'` for this.
>2. The code is automatically commited after a merge. No need to do a `git commit` after a successful merge.
>3. Sometimes, a merge conflict arrises and needs to be resolved through your manual intervention. See the next section.

Your merge efforts will not be visible on the remote repo until you do a push of your master branch :

```bash
$ git push origin master
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/gerritonagoodday/af-ZA_joomla_lang.git
   ed0cb7b..e945c5e  master -> master
```

When a branch has been merged to `master`, the branch will continue to exist. If you are ready to do so, you can delete the branch forever:

```bash
$ git branch -d '[branch-to-delete]'
```

## Dealing with Merge Conflicts

Git attempts to automatically merge code from branches. Where there is any doubt on how a merge of code should be done, it raises a _merge conflict_ and invokes a merge tool that the user can use to correctly resolve the conflict. Luckily, the merging operation is mostly a straight-forward process and does not require user intervention. 

Because it can get complicated to resolve conflicting code during a merge on the command line, a number of graphical tools are available to make the operation clearer. The tool of choice here is `meld`. It is totally awesome. If you do not already have it installed, install it like this:

```bash
$ sudo apt install meld
```
You also need to configure Git to automatically invoke `meld` when the need arrrises to resolve merge conflicts:

Set the graphical merginging tool up
```bash
$ git config merge.tool meld
```

Let's assume that you are merging branch '3.9.5' to branch 'master' and this happens:

...TODO...

Run the merge tool:

```bash
$ git mergetool
```

## Tagging

Branches (mostly) rendevouz back to the `master` branch eventually after performing the necessary commits and merges. You may well decide to delete the branch, as its contents is now in the master branch. You can mark the committed code base with a tag that indicates the code release Id, or anything else that is significant in the life cycle of the code base.

### Create a tag 

Simply _tag_ your code like this:

```bash
$ git tag '3.9.5v1'
```

What can be very helpful is to add a comment to your new tag:

```bash
$ git tag '3.9.5v1' -m 'First point release'
```

For this project, it is important to remember to do this only after you have committed your code. 

### List tags in a repository 

You can list all the tags and their comments in a repo (`-l` lists them without commants). In some cases, there can be many tags, so add a filter expression to limit the result:

```bash
$ git tag -n "3.9.5*" 
3.9.5           Joomla! 3.9.5
3.9.5-rc        Joomla! 3.9.5 Release Candidate
```

If you need to use a RegEx to for even finer filtering:

```bash
$ git tag -n | grep "^3\.9\.5"
3.9.5           Joomla! 3.9.5
3.9.5-rc        Joomla! 3.9.5 Release Candidate
```

Do not confuse *Tags* with *Tag Comments*, or with *Code Commit comments*!

### Checkout code against a tag

This is similar to checking code against a brnach, excepts that we need to explicity specify that this is a tag, or multiple tags, by using the `tags/`-specifier.

```bash
$ git checkout tags/'3.9.5'
Checking out files: 100% (9506/9506), done.
Previous HEAD position was 6b8fd2b21f Tag Alpha 6
HEAD is now at 1547f8e760 Prepare 3.9.5 release
```

Check what we have:

```bash
$ git status
HEAD detached at 3.9.5
```

Successfully checked out the right code. Should you want to develop and new code against the code that relates to this tag, it is good practice to create branch:

```bash
$ git branch '3.9.5-dev'            # branch code
$ git branch                        # check if branching was successful
* (HEAD detached at 3.9.5)          # yes it was:
  3.9.5-dev                         #  - here is the branch, but it is not the active one
  ...
$ git checkout '3.9.5-dev'          # Make this branch active
Switched to branch '3.9.5-dev'
$ git branch                        # Check if this branch is now active
* 3.9.5-dev                         # yes, successfully switched to.
  ...
```

## Escaping from Calamity / Groot Gemors

Here are a few methods that you can use to recover back to a known point 

### See what has not yet been staged or committed

The files must already be in the repo (added at least) since the last commit:

```bash
~/git $ git diff [filename]

```

Or to see all the file changes since the last commit:

```bash
~/git $ git diff

```

To see the difference between 2 commits, use the commit hashes when doing a `git log`:

```bash
~/git $ git diff [hash1] [hash2]

```

### Restoring to the last staging point

```bash
~/git $ git checkout [filename]

```

### Change the last commit message

```bash
~/git $ git --amend -m "new commit comment"

```

Only do this if you have not push your changes to the remote repo yet.

### Reset your local repo to a know commit point

First view the commits and select the hash of the commit that you want to resume to:

```bash
~/git $ git log
commit 2f59adbca8889d128d0676493babdc8c902923ca (HEAD -> master, origin/master, origin/HEAD)
Author: gerritonagoodday 
Date:   Thu Oct 31 20:11:26 2019 +0100

    [a comment]

commit 714c718f44178bb134cdaab9f8700ea02cb93a41
Author: gerritonagoodday 
Date:   Thu Oct 31 20:07:33 2019 +0100

    [another comment]

```

Now hard-reset to the desired point. All your changes from that point on will be lost (first 6 characteres will do):

```bash
~/git $ git reset --hard 2f59ad
```

Do a final check - you should be in a known state:

```bash
~/git $ git status
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working tree clean
```

The GIT Garbagae collection will completely remove all the deleted work after 30 days.

### Remove all untracked files

This will remove all files that have not been staged or committed yet:

```bash
~/git $ git clean -df
```


# Further information


## Some useful reference material

1. This is a good introduction to Git for a zero-knowledge start:

<a href="http://www.youtube.com/watch?feature=player_embedded&v=SWYqp7iY_Tc" target="_blank"><img src="http://img.youtube.com/vi/SWYqp7iY_Tc/0.jpg" 
alt="Git & GitHub Crash Course For Beginners" width="240" height="180" border="10" /></a>

2. Use GitKraken as a Git visualization tool. Download it from here: 

<a href="https://www.gitkraken.com/download" target="_blank"><img src="https://pbs.twimg.com/profile_images/714866842419011584/LRrR48qp_400x400.jpg" alt="GitKraken" width="240" height="180" border="10" /></a>


