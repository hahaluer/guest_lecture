# Applied Numerical Computing

## Introduction to OSU HPCC
- What is HPC?
  - It is **not** a single computer that magically makes problems run faster.
  - It **is** many computers working together over a high-speed network, as a team, to solve problems too big for a PC.
    - Only certain types of problems will run quicker on a supercomputer.
    - Our resources are often helpful for all kinds of problems because you don't have to tie up your laptop with a long-running problem.
- HPC provides computing for problems that are too big or run too slow on you personal computer.
- HPC is there for experiments that are too big, too small, too fast, too slow, too dangerous, or too costly to study in the real world.
  - The U.S. Government uses HPC for nuclear testing.
  - Proctor and Gamble uses HPC to create better shampoo.
  - Here at OSU, HPC is used for climate modeling, fluid dynamic simulations, physical chemistry, cryptography research, bioinformatics, and more!
- Fun facts about OSU HPCC
- Quick intro of the people!
- Overview of the equipment: Pistol Pete II, Cowboy, and Tiger.
- Other regional and national efforts we are involved in...
  - Give a *quick* overview of OneOCII and XSEDE and give them some school pride.

## Navigating in a Terminal
- Today we are going to practice submitting a Python job on Cowboy.
- To log into Cowboy, we are going to use SSH, which is a secure way to login within a terminal-based environment.
  - Use links on the New-User Tutorial to help them set up the software and log in with temporary accounts.
  - Windows users use PUTTY (See setup on slide)
  - Mac and Linux users use Terminal (Hard Drive >> Applications >> Utilities >> Terminal Icon)

```bash
username@cowboy.hpc.okstate.edu
```

> It looks like nothing is going in when you enter your password. Don't worry! It's being entered.

- There are three important directories on Cowboy: `/home`, `/scratch`, and `/opt`.
- `/home` has a 25 GB limit. This is where you keep your scripts and project files.
- `/scratch` is unlimited, *but shared*. It serves as a temporary staging ground for large data sets.
  - `/scratch` should only be used while your job is running.
  - Users may be asked to clear `/scratch` files they are no longer using.
- `/opt` contains shared files and programs that are available to all users.
  - The files for today's exercise are stored in `/opt`.

> None of these file systems are backed up. Make sure you always have a local copy of your data!

- Who am I?

```bash
whoami
```

- Where am I?

```bash
pwd
```

- Let's change locations into our `/scratch` directory.

```
cd /scratch/username
pwd
```

- Let's go home.

```bash
cd
pwd
```

- `/` is the *root* at the beginning and a divider elsewhere.

- Let's make a new directory within our home folder.

```bash
makedir sample
```

- To see folders and files within our home folder use `ls`, which *lists* the contents of whatever file you give it.

```bash
ls /home/username/
```

> Tab auto-complete is your friend!

- How many of you want to type the full path of a file each time you want to do something with it?

- Most commands see the world from the perspective of whatever the current working directory is.

```bash
ls
```

- Explain the difference between *absolute paths* and *relative paths* using address example.
  - Any path that starts with the root is an *absolute path*. Other paths are *relative*.
  - Most of the time we can use relative paths unless we want to access something from another part of the file system.

- Let's do just that and copy today's example files to our local directory using the *copy* command.

```bash
cp /opt/example_submission_scripts/python/ .
```

> Don't forget tab auto-complete!

- We get an error because copy is designed to only copy one thing at a time.

- Introduce flags.

- We need the recursive flag to copy will copy the directory and everything inside it.

> Use the up arrow to scroll though recent commands.

```bash
cp /opt/example_submission_scripts/python/ .
ls
```

- Explain the `.` shortcut.

- Since we don't need our first directory anymore, let's delete it using the *remove* command.

```bash
rm sample
```

- Looks like we ran into the same problem before. We need to use the *recursive* flag with remove since it is designed to remove single files.

> Warning: `rm` is permanent! There is no way to recover files once you delete them!

- Let's also add the *interactive* flag for some security.

```bash
rm -r -i
ls
```

- Let's have a look at the example files.

```bash
cd python
ls
```

- There are three files: `elephant.pbs`, `elephant.py`, `README.md`.

- We will talk about each file in a moment, but first two more shortcuts to help us with navigating.

- To go up a directory, use `..`.

```bash
cd ..
pwd
```

- To go back to the previous directory (like the back button on your browser), use `-`.

```bash
cd -
```

- Explain the Cowboy Workflow slide and the scheduler using the example of seating at a restaurant.

## Submitting a Job

- It's good practice to start with the `README.md` file.

- To view the contents of a file, use `cat`.

```bash
cat README.md
```

- The next file is `elephant.py`. We will not discuss the details in this script, but feel free to take a look. It draws a picture of an elephant using imaginary numbers.

```bash
cat elephant.py
```

> If you wanted to run this script on your personal computer in a Linux terminal, you would use the command `python elephant.py`. We'll see this come up later.

- The last file, `elephant.pbs`, is our submission script. This file contains all the information the scheduler needs to run our job.

```bash
cat elephant.pbs
```
### Submission Script

|        Command             |                                           Meaning                                                   |
|       ---------            |                                          ---------                                                  |
| `#PBS`                     | `#` indicates a comment (human eyes only). `#PBS` are special comments intended for the scheduler.  |
| `#PBS -q                   | Tells the scheduler which queue to use (explain the queue slide).                                   |
| `#PBS -l nodes=1:ppn=1`    | Requests the number of nodes and cores used for the job.                                            |
| `#PBS -l walltime=10:00`   | How long the job will run before the system stops is.                                               |
| `#PBS -j oe`               | Makes sure status updates and errors are written to the same output file. Please don't change this. |
| `cd PBS_O_WORKDIR`         | Tells the scheduler to move into your project directory so it can find your project's files.        |
| `module load python/3.5.0` | Load the appropriate software to run your job.                                                      |
| `python elephant.py`       | Runs the script.                                                                                    |

- Let's submit our job to the scheduler!

```bash
qsub elephant.pbs
```

- To check on the status of your job, use the following command (it takes thirty seconds for the scheduler to update):

```bash
showq | grep username
```

> `grep` is a tool for searching text. `showq` by itself shows the status of every job in the queue. We use `grep` to filter the results for our status.

- Every job writes an output file with the naming convention [submission script].o[job number]. This file contains a status report of the job. It will have all the screen output that you would have seen if you ran the job directly from the terminal. If something ever goes wrong with a job, this is the first place you want to look to start resolving the problem.

```bash
ls
cat elephant.pbs.o962
```

- This particular script also created a picture file of an elephant. We can't view pictures from a terminal, so you need to transfer the picture to your local computer to see what it looks like.

- Don't have the students do a file transfer, but demonstrate it yourself.

- Congratulations! You have officially used a supercomputer.

> You can even have Cowboy email you when your job is done so you don't have to wait while it's in the queue. (`#PBS -m abe -M <email address>`)

- Check out our New User Tutorial for more information.
