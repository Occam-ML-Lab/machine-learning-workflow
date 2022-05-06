# Machine Learning Workflows
Machine learning often requires you to run long-running scripts. It's not convenient to do this on your laptop. At some point, everyone will want to switch to a set-up where you **write code on your local machine**, but **run experiments on a remote computer**. This document is intended to help you set this up. Overall, this is a laborious process, and you will need to develop a lot of skills around managing computers. There is no way around this. You will have to google around **loads** to build these skills independently.

This guide is not complete since nobody has time to write this stuff in a huge amount of detail, and because details change all the time. However, hopefully it will point you in the right direction. It is assumed that you know how to use a terminal, some shell scripting, and git. [MIT's _Missing Semester of Your CS Education_](https://missing.csail.mit.edu) course is a good place to start to learn more about this.

Some guides specifically for Imperial students:
- [Alex Spies's DoC Environment Setup](https://hackmd.io/@afspies/Bkd7Zq60K)
- [Nuric's great guide *specially* for Imperial students can be found here](https://www.doc.ic.ac.uk/~nuric/teaching/remote-working-for-imperial-computing-students.html).
- [ImperialMScAIUtils: Guides and utilities](https://github.com/harrygcoppock/ImperialMScAIUtils), written for the MSc in AI students, but useful for everyone.

## Overall Workflow
- Write code and debug on small test cases on your local machine.
- Copy/sync code to your remote machine.
  - The low-tech solution is to use the tool `scp` ([guide](https://www.imperial.ac.uk/computing/people/csg/guides/file-storage/scp/)).
  - The better solution is to have your coding IDE automatically sync your code. [Visual Studio Code](https://code.visualstudio.com) and [PyCharm](https://www.jetbrains.com/pycharm/) can both do this after setting it up.
- `ssh` ([guide](https://www.imperial.ac.uk/computing/people/csg/guides/remote-access/ssh/)) into an Imperial machine, and run your script.
  - Make sure you don't run directly in the log-in shell. If you sever the ssh connection from your laptop, it kills the process! Running inside `tmux` solves this ([guide](https://missing.csail.mit.edu/2020/command-line/)).
- Your script should write results to a file, while it's running.
  - Scripts can be killed (e.g. if somebody resets the computer it's running on).
  - You should make sure that your code writes all the results you need to a file, ideally while it's running.
  - Once in a while, you can copy the results back to your local machine, and have a separate script make plots. This is a great way to check up on experiments, even while your main script is still running!
  - You also allow your scripts to be **restartable**: If your script does get killed, you could reload intermediate results, and pick up where you left off.
  - You should write your experiment outputs to `/vol/bitbucket/` ([guide](https://www.imperial.ac.uk/computing/csg/guides/file-storage/quota/)).
  - Network file storage (NFS) is super useful! It's accessible from any computer!

## DoC Machines
DoC has lots of machines that you can log into. These are located in the main computer lab. It may be a good idea to go to the computer lab phsycially, and sit next to the machine that you log into through your laptop. This way, you can see how your remote commands affect the machine while you're also logged into it physically!

See the [list of DoC lab workstations](https://www.imperial.ac.uk/computing/csg/facilities/lab/workstations/).
- Most machines have several cores. This can be super useful. Remember, lots of CPUs can do a lot in parallel!
- Some machines have GPUs. I think you can ask CSG to have priority or even sole access to a GPU machine, if you really need it for your project.

## Parallel Experiments
If you need to run many similar experiments in parallel (e.g. grid-search over different parameter values), you can consider using cluster functionality. CSG provides a great [guide](https://www.imperial.ac.uk/computing/people/csg/services/hpc/), and seems to provide two interfaces:
- [Condor](https://www.imperial.ac.uk/computing/people/csg/services/hpc/condor/) for CPU-only tasks. This can give you access to hundreds of cores!
- [Slurm](https://www.imperial.ac.uk/computing/people/csg/guides/hpcomputing/gpucluster/) for GPU-enabled tasks.

I find [jug](https://jug.readthedocs.io/en/latest/) a great tool for running many similar experiments in parallel. You can make this work nicely with cluster managers like Condor/Slurm, because it only needs an NFS to coordinate running jobs.

If you need more resources, you could even look at the [Research Computing Service](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/), but an academic may need to grant you access to this.

**Please contribute more**.

## Coding IDE & Remote Sync
How to set up VScode or PyCharm to sync your code to a remote machine. **Please contribute**.

## Python Environment
It's usually a good idea to use [anaconda](https://www.anaconda.com) as your Python environment. It's super easy to set up and lives in a single folder. So if you mess up your environment, you can just delete it, and start from a clean slate again. I installed my anaconda setup in `/vol/bitbucket`, so my Python setup is uniform across all network machines.
