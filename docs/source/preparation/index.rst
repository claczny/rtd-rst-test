Preparation
===========
In order to maximize the time for teaching and learning on multi-omic analyses, we would ask you to prepare yourself using the materials below.
These include mainly fundamental aspects, such as relevant publications and computational skills including setting up your laptop, connecting to the `uni.lu HPC <https://hpc.uni.lu/>`_, and running test applications.
Please try these out as early as possible and let us know should you encounter issues so we can try to help in debugging them.

Publications
------------
You can find a copy for your personal use in the `publications owncloud folder of the course <https://owncloud.lcsb.uni.lu/s/OrtKd15mdiZIXRj>`_.
The access password was or will soon be sent to you.
Please note that these should **not** be distributed, unless the respective licenses allow so.

Metagenomics
^^^^^^^^^^^^
- `Sharon, Banfield, 2013, Science <https://www.ncbi.nlm.nih.gov/pubmed/24288324>`_
- `Sieber, 2018, Nature Microbiology <https://www.ncbi.nlm.nih.gov/pubmed/29807988>`_
- `Brown, 2016, Nature Biotechnology <https://www.ncbi.nlm.nih.gov/pubmed/27819664>`_

Metatranscriptomics
^^^^^^^^^^^^^^^^^^^

Metaproteomics
^^^^^^^^^^^^^^

Metametabolomics
^^^^^^^^^^^^^^^^

Microbial ecology and networks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- `Fromont, 2019, Molecular Ecology <https://www.ncbi.nlm.nih.gov/pubmed/30714238>`_
- `Roettjers and Faust, 2018, FEMS Microbiology Reviews <https://www.ncbi.nlm.nih.gov/pubmed/30085090>`_

The archaeal domain
^^^^^^^^^^^^^^^^^^^
- `Zaremba, 2017, Nature <https://www.ncbi.nlm.nih.gov/pubmed/28077874>`_
- `Spang, 2017, Science Review <https://www.ncbi.nlm.nih.gov/pubmed/28798101>`_
- `Williams, 2017, PNAS <https://www.ncbi.nlm.nih.gov/pubmed/28533395>`_
- `Castelle and Banfield, 2018, Cell <https://www.ncbi.nlm.nih.gov/pubmed/29522741>`_

Biomolecular extraction
^^^^^^^^^^^^^^^^^^^^^^^
- `Roume, 2013, ISME J <https://www.ncbi.nlm.nih.gov/pubmed/22763648>`_
- `Muller, 2014, Nature Communications <https://www.ncbi.nlm.nih.gov/pubmed/25424998>`_

Single-cell analyses
^^^^^^^^^^^^^^^^^^^^

Compute
-------

If possible, we strongly encourage using a Linux-based or macOS-based laptop, simply because most tools used in this course are command line-based.
Please do not worry if this does not represent a feasible solution for you.
Most of the computations will be performed on the uni.lu HPC remotely.

In case local computations are necessary and should not be compatible with your laptop, we will ask you to team-up in small groups.

Accessing the HPC
^^^^^^^^^^^^^^^^^

You have or will receive your uni.lu HPC login credentials from us.
These consist of a user ID and a key-pair (public and private key).
Please use these credentials and follow the instructions on the `uni.lu HPC Access website <https://hpc.uni.lu/users/docs/access.html>`_) for `Linux <https://hpc.uni.lu/users/docs/access/access_linux.html>`_, `macOS <https://hpc.uni.lu/users/docs/access/access_linux.html>`_, and `Windows <https://hpc.uni.lu/users/docs/access/access_windows.html>`_.

N.B. The port on which the SSH servers are listening is not the default one (i.e. 22) but **8022**, so please make sure to adjust this in your configuration.

To log-in using ``ssh`` under Linux or macOS, use::

    ssh <YOUR-ID>@access-iris.uni.lu -p 8022 # N.B. Replace <YOUR-ID> with your individual ID

You will then reach the access node of the UL HPC.
This particular node is meant as an **intermediary** point from which you reserve your actual resources, i.e., compute nodes.

N.B. **NEVER** run compute intensive commands on the *access* node but reserve a compute node for this.

Reserving resources
"""""""""""""""""""

Detailed descriptions on how to reserve compute resources on the UL HPC can be found `here <https://hpc.uni.lu/users/docs/slurm.html>`_.

Below, we will list a few examples of the most common operations

Interactive sessions
""""""""""""""""""""
An interactive session behaves the same way as a regular session, i.e., on the access node, but provides *dedicated* resources to run computational tasks.
The simplest reservation is::

  srun --pty bash -i

This will result in reservation of a single core for 1 hour.

N.B. If your reservation times out, the job will be killed automatically and there is *no way* to extend this.
This means that you should consider upfront how long you estimate your session to run.

To reserve an interactive session for a maximum of 4 hours, please use::

    srun -t 04:00:00 --pty bash -i

To include a total of 4 cores on a **single** node, please use::

    srun -N 1 -c 4 -t 04:00:00 --pty bash -i

N.B. Make sure that your command *always* includes ``-N 1``.
Otherwise, your job will be distributed over multiple nodes which only works under specific circumstances (e.g., using `MPI <https://en.wikipedia.org/wiki/Message_Passing_Interface>`_), which are not covered in this course.

Batch sessions
""""""""""""""

Interactive sessions are great for running short-lived tasks and, as the name suggests, performing interactive operations.
This means that one typically performs the development in an interactive session but keeps the actual execution for batch sessions.

Batch sessions are also longer lived than interactive sessions and have a maximum runtime of 5 days (``05-00:00:00``).

For batch sessions, you should have a stand-alone script ready that you pass along the command call.
Assuming your stand-alone script is called ``my_script.sh``, you can submit it to the UL HPC via:
```
sbatch -N 1 -c 10 -t 01-02:00:00 my_script.sh
```
Using this call, the script will have 10 cores (``-c 10``) available on a single node (``-N 1``) for 1 day 2 hours (``-t 01-02:00:00``).

In the above call, the resource configuration was specified on the command line directly.
Alternatively, you can specify the resources in the script (``my_script.sh``) itself.
While this reduces the chance of error by directly including the resource reservations into the script itself, it is less flexible as one has to edit the file first.
If you would like to specify the resources directly in the script, this is how the first lines of the script would have to look like::

  #!/bin/bash -l
  #SBATCH -N 1
  #SBATCH -c 10
  #SBATCH -t=01-02:00:00
  #SBATCH -p batch
  #SBATCH --qos=qos-batch

  <WHATEVER_YOUR_SCRIPT_SHOULD DO>


The lines::

  #SBATCH -p batch
  #SBATCH --qos=qos-batch

specify to which "queue" this job should be allocated. ``slurm`` offers great flexibility here, which will however not be necessary to consider during this course. We will use either interactive sessions or batch sessions with the above configurations.
