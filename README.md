# Purpose

The purpose of this repository is to provide high level instructions for a class
leader to set up a modern cpp IDE on stuudent computers that is both free and 
runs on Windows.

# What's with the name?

Windows is terrible. Proprietary software is worse.  This project
should be viewed through the lens of 'harm reduction programs.' The 
best thing by far is to implore students to get off windows like 
they were getting off of Heroin.

However most Computer Science students (girls as well as boys) come to
us after a decade of gaming. Can I play Halo if I switch to Ubuntu? Is there
a version of Grand Theft Auto or Call of Duty for Debian?  Explaining the 
virtues of Free Software has not worked for me.  I even showed students
Richard Stallman's excellent (and brief) TedX talk available at the url below:

<https://www.youtube.com/watch?v=Ag1AKIl_2GM>

The reactions I received were simmilar to exposing them to a Bernie
Sander's rally. "That is a cool old dude. Am I going to change my life? No."

# What's wrong with other alternatives?

## The professional solution

Visual Studio costs $800.  Although one can use the AStyle plugin to
write styled code. On our lab computers this was never installed, let
alone configured.

## What students use

To save money, many students use Bloodshed's Dev-C++.  This has not been
maintained for 10 years and there are more than 340 known unfixed bugs. See
the url below for more detail.

<http://clicktobegin.net/programming/why-you-shouldnt-use-dev-c/>

# Clean Needles

If students will not leave proprietary software alone. I still want
them to learn posix style command line driven development. Here is my
zero cost and Free Solution.  These are not self study directions for
students. This requires using elisp to load packages and configure
packages and python to run cpplint.  This is directed at the grad
student adjunct or instructor teaching them.

# Emacs

The hacker's editor.  You can use if for 30 years. It is not really and editor.
It is a REPL.  The best way for windows users to get it is via:

<http://vgoulet.act.ulaval.ca/en/emacs/windows/>

If you are a VIM afficianado, you can use Evil mode.

# Google C++ Style Guide

This has been operationalized in a python package (library) called
cpplint that is freely available.  Now Google's style guide is
somewhat controversial in the C++ community.  See
<https://news.ycombinator.com/item?id=7900238> for a critique of the
guide ane the video <https://www.youtube.com/watch?v=NOCElcMcFik> for
its philosophy.  What is not in doubt is that successful projects
adhere to a style.  If you check out only one of these references
watch, Greg Hartman-Kroah's "I Don't Want Your Code",
<https://www.youtube.com/watch?v=fMeH7wqOwXA>.

# Semantic Auto-Complete

We install auto-complete with semantic parsing for a back end. We also make 
auto-complete aware of our compiler include files.

# Caveat

These instructions do not cover all possible scenarios.  Emacs
packages change version numbers and will need to be tweaked.  Teachers
should not expect children who are taking their first programming
course to configure a system that requires two separate languages
besides C++, elisp and python.  If they can do this they should be 
taking a higher level course. 

# Benefit

Students learn to be comortable on the command line and can add
familiarity with many open source tools incrementally such as gdb and
valgrind.

# Installation

First install modified Emacs for windows from:

<http://vgoulet.act.ulaval.ca/en/emacs/windows/>

Select all of the defaults.  It has a one click installer.  You will
need this to modify the Windows PATH variable.  The PATH is list of
directories where the operating system searches for a program.

Second install Cygwin. Cygwin can be confusing. It only installs a base package 
on default and there is no repo management system. Instead, to additional
packages, repeat the installation process with following difference.  When you
come to the screen below search for the new program and left-click **skip**
to download the binary of that program.  For our base install you should
find g++, make, cmake and gdb.

![img](images/cygwin_select.png "Click on skip to add the package")

Third, to make linux utitities available from any path, you should
understand where Cygwin is installed.  It is installed in C:/ not
C:/Program Files. The binary versions of programs are in C:/Cygwin/bin
which is what you need to add to the path.  You can not permanenty
modify the PATH variable from cmd shell in windows.  You need to use the GUI.
You can use version 3 of the Powershell but not every student will have that
and the syntax is awkward.

Instead go to Control Panel which exists in every version of Windows since NT.
Find or select Advanced System Settings and Select Environmental Variables. If 
given a choice you want System variable not for just the user.  Find the PATH
and copy the variable value to a file in emacs. I use path<sub>scratch</sub>. Make a copy 
and add the path to cygwin at the end.  Here is my modified windows path.

### old path

    %SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\

### New path

    %SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;c:\cygwin\bin

If you open a new cmd shell on windows 

And here is the screen where you can change the environmental variable
in Windows. In the next three images we display the Advanced System Variables,
the Environmental Variables and finally the path selection.  The path will normally
extend far beyond the borders.  My advice, is do not try to attempt to edit the
path there but rather copy it to Emacs where adding the path to C:/Cygwin/bin is 
easy.

![img](images/TheEnvVarScreen.png "The advanced system settings")

Click on environmental variables and the next screen appears:

![img](images/TheEnvVarScreenSelection.png "Click on skip to add the package")

You will have to scroll in the system variables to find the PATH variable.

![img](images/thePathVariable.PNG "Click on skip to add the package")

Here are the tricks of Emacs and Windows CUA to successfully copy and edit the path.

-   Find path<sub>scratch</sub> in the usual way C-x C-f path<sub>scratch</sub>.
-   This brings up a blank file
-   Go to the Edit System Variable
    -   Press C-a to select all of the variable value
    -   Press C-c to copy the value to the windows clipboard
-   Put the cursor back in Emacs in the path<sub>scratch</sub> file.  C-y called
    yank to paste the variable back in Emacs.
-   Notice that each place the path searches is separated by a semi-colon
-   Make sure to put a semi-colon before new path.
-   Add the C:/Cygwin/bin to the path in Emacs
-   Click Ok on all of the winows.
-   Close the old command window and open a new one.
-   Linux utilities such as 'ls' now work in the command shell from any directory

# Install python

We need python to use cpplint which will give us our styling.  We also
have to install setup tools, pip and virtualenv.  First go to the main
python site:

<https://www.python.org/downloads/windows/>

As of this writing the latest release in the 2.7.x series is 2.7.10.
Select that release for windows. Depending on whether or not you have
a 64 bit machine. For my Windows 10 Virtual machine, I only have to search
system properties.  See information below:

![img](images/WindowsInfo.png "My system is 32 bit use the x86 installer")

Click on the appropriate python:

![img](images/windowsPython.png "I select the last. You might select the 2nd to last")

This will install python in C:/Python27. We now need to go back to the
Environmental variables and add c:/Python27 to the path in the same
way as before.  

## Test the python installation

Open a new cmd window.  Type python. You should be taken into the
python cmd interpreter. Type quit() and continue.

![img](images/testPython.png "Test python")

Run a cmd shell as administrator. Navigate to the python27 directory
make a directory for scripts. Make a scripts directory if it does not
exist.  See the screenshot below:

![img](images/makeScripts.png "Make Python Scripts folder")

you can then download the following two files:

<https://bootstrap.pypa.io/ez_setup.py>
<https://bootstrap.pypa.io/get-pip.py>

Copy these files to C:/Python27/Scripts.  See the screen shot below:

![img](images/copyPyScripts.png "Copy the scripts")

You can then run them in the way shown below:

    python ez_setup.py
    python get-pip.py

It is generally good practice to set up a virtual environment in
Python. We then set up a virtual environment.