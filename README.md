# mentat_on_hpc
A how-to guide for running mentat on the HPC.

# Overview

Mentat requires the GUI, even if running in background mode `-bg`. There are solutions to fake a display, but this does not work with mentat. As stated in the documentation 
> Note that the DISPLAY environment variable must point to a valid display.

The way around this is to use an interactive session that points to your computer's display. This does mean that you have to keep the ssh session open for the entire time that the interactive session is running.

# How to set this up?

1. [MobaXTerm](https://mobaxterm.mobatek.net/download-home-edition.html) is the easiest way that I have found. So have that installed.
2. When in MobaXterm, use the "+ Start local terminal option".
3. Next you need to start a ssh session with X-Forwarding, this is done with the command below. Note, it will ask for your password.
> `ssh -X username@hpc1.sun.ac.za`
4. If all worked, you should now be in your home directory on the HPC. This will look the same as if you had done a normal ssh.
5. Now we need to enter an interactive session on one of the nodes. This is done using the command: (You can set the specifications to suit your job.)
> `qsub -I -X -V -l walltime=01:00:00 -l select=1:ncpus=4:mem=8GB`
6. If done correctly you will be greeted with the following two lines (obviously the job number will be different):
> `qsub: waiting for job 282033.hpc1.hpc to start`

> `qsub: job 282033.hpc1.hpc ready`
7. You are now in an interactive session on a visualisation node on the HPC. You will be in your home directory and have access to all your files as per normal.
8. To load mentat type:
> `module load app/mentat` This will produce no output if done correctly. 
9. To check if it worked you can type:
> `which mentat` If it points you to a directory with mentat it worked.
10. Now you can run your .proc file. using 
> `mentat -bg Filename.proc`.
11. You will probably be met with the following warning, I don't know what this means, but it still works :)
> `QStandardPaths: XDG_RUNTIME_DIR points to non-existing path '/run/user/YourUserName', please create it with 0700 permissions.`
12. If the above does not work an additional thing to try can be to install [Xming X Server for Windows](https://sourceforge.net/projects/xming), and then make sure it is running before you open mobaXterm, then follow the same process.

# Using the python API in mentat
If you need to interface with mentat using py_post or py_mentat on the hpc there are a couple of additional steps to follow to get this working. The following steps assumes that the mentat module has already been loaded. If not, see point 8 above.
1. First you need to load python:
> `module load python/3.8.1`
2. Then export the py_post and py_mentat library locations to `LD_LIBRARY_PATH` and `PYTHONPATH` using:
> `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/apps2/MSC/mentat2021.4/shlib/linux64`

> `export PYTHONPATH=$PYTHONPATH:/home/apps2/MSC/mentat2021.4/shlib/linux64`

You should now be able to run python and import py_menat and py_post. You can check this by doing the following in the interactive session:

1. Type `python3` - This will show a `>>>` on the line below.
2. Now type `import py_mentat` - If it did not give an error message it worked and the library has been imported and can be used.

# Additional Nodes
1. I have not tried to run a bash script in the interactive session, but I am sure it should work. Remember you won't qsub the bash script since you are already within the qsub.
2. If you find some other quirks and workarounds, please add them to this page.
