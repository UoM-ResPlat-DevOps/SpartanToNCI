-- *Slide* --
# Part Zero: Goals for today
* Part 1: Understanding NCI and Application Process (6)
* Part 2: Understanding The Raijin Environment and Resources (8)
* Part 3: Understanding Basic PBSPro Job Submission at NCI (5)
* Part 4: Understanding Advanced PBSPro Job Submission at NCI (0)
-- *Slide End* --

-- *Slide* --
### Part Zero: Documentation and Help
* This presentation: 
* Wiki : `opus.nci.org.au`
* Helpdesk : `help.nci.org.au`
* Email : `help@nci.org.au`
-- *Slide End* --

-- *Slide* --
### Part 1: NCI and Raijin
* NCI is Australia's peak facility. Main HPC system in Raijin; 2.6 PetaFLOP peak compute performance. 24th on Top500 list when built (121st, Nov 2016)
* Fujitsu Primergy Cluster, Lenovo NeXtScale System, 
* 4472 compute nodes (Sandy Bridge and Broadwell), 6 login nodes, 5 data movers. Memory ranges from 32-128 GB, GPUnodes
* Also includes virtual laboratories, data distribution services and high performance (150GB/s r/w) Lustre file system.
-- *Slide End* --

-- *Slide* --
### Part 1: NCI and Tenjin
* Also has cloud Infrastructure - Tenjin - a private cloud built on NeCTAR hardware.
* Designed for services complementary to the HPC system for exporting processed data sets and on-demand computation.
-- *Slide End* --

-- *Slide* --
### Part 1: Access to NCI
* Determined by competititive National Computational Merit Allocation Scheme (NCMAS); includes NCI (Raijin), Pawsey Centre (Magnus), Monash (MASSIVE), and UQ (FlashLite).
* Partner Shares (Government agencies, medical research centres, and universities). Each NCI partner distrubutes its share their discretion. ANU has its on merit scheme.
-- *Slide End* --

-- *Slide* --
## Part 1: NCMAS
* NCMAS 2018 information and application forms are available online at: `https://ncmas.nci.org.au` 
* All applicants are **strongly** encouraged to read the Information and FAQ pages on the NCMAS website in full before starting their applications. 
* Email support for applicants is available at `ncmas@nci.org.au`.
-- *Slide End* -

-- *Slide* --
### Part 1: Easier Access
* NCI Startup Scheme. Open to all researchers, but lead CI must be post-doc or higher.  
* Facility Startup Schemes. Much easier entry, small allocations. Handy to establish track record.
* Partner Scheme Manager determines allocations and tracks resource usage.	
* Note that projects can have allocations from different schemes. Partner scheme and NCMAS allocations can be mixed.	
-- *Slide End* --

-- *Slide* --
### Part 1: Registration Process
* Start at `https://my.nci.org.au/mancini` and follow the prompts. You need to use your institutional e-mail address. You will be asked for a project code.
* The Lead Chief Investigator (CI) of the project will be e-mailed for approval. Once approved, a username will be generated and e-mailed to you. 
* If you wish to join another project (subject to approval) go to `http://my.nci.org.au/mancini/project/projectID/join`
* All usage of compute systems is accounted against projects.
-- *Slide End* --

-- *Slide* --
### Part 2: Login Process
* The hostname for Raijin is `raijin.nci.org.au`. As with similar systems logins are via SSH (built in Linux, Mac OS X).
* The command `ssh username@raijin.nci.org.au` will put the user one of the login nodes; use -Y (or -X) for X-Windows forwarding.
* Consider using an SSH config and/or passwordless SSH (although password is used on a number of NCI web logins...).
* On Raijin user processes are limited to 2GB, and CPU time to 30 minutes.
-- *Slide End* --

-- *Slide* --
### Part 2: File Transfers
* File transfers can be carried out through `scp`, `sftp`, `rsync` etc.
* For large quantities of data use the the dedicated data-mover nodes, `r-dm.nci.org.au` 
-- *Slide End* --

-- *Slide* --
### Part 2: Data Storage 
* Home directory mount `/home` is 2GB, with backup. Working data mount is `/short`, 72GB per user, no backup.  
* Project files `/project` is backed up, for important files shared amongst groups.
* Large NCI-global filesystems mounted at `/g/data1,2,3`, negotiable size. 
* To query Lustre for current usage `lquota`. For project accounting and PBS `nci_account`. See also `short_files_report`, `gdata[1-3]_files_report`.
-- *Slide End* --

### Part 2: MDSS Archive
* There is also the MDSS archiving system available by negotiation with dual-copy backup.
* Massdata storage is a tape library; not a mounted file system, not for read/write. Tar recommended for lots of small files. 
* Use `mdss ls` to list files, `mdss get` and `mdss put` copy files, 

-- *Slide* --
### Part 2: Projects
* Your default project is set in the `.rashrc` file. The current working project can be changed with the `switchproj ProjID`
* The status of projects allocations can be determined by `nci_account [ -P ProjectID ] [ -p 2016.q4 ] [ -v ]`, where -P is project, -p is the time period, and -v is verbode detail.
* Limits per job, per user, and per project for specific queues can be determined with `nf_limits -P project -n ncpus -q queue` (e.g., `nf_limits -P qr12 -n 20 -q copyq`)
-- *Slide End* --

-- *Slide* --
### Part 2: NCI Data Research Repostory
* The NCI National Research Data Collection is Australia’s largest collection of research data, encompassing more than 10 PB of nationally and internationally significant datasets. See: `http://nci.org.au/services/nci-national-research-data-collection/`. 
* The data catalogues are also made available through geonetwork `https://geonetwork.nci.org.au`
-- *Slide End* --

-- *Slide* --
### Part 2: NCI Data Research Repostory
*  Datasets are stored on the NCI global filesystems. These are called `/g/data1`, `/g/data2`, `/g/data3`, `/g/data1a` etc. The data stored on these filesystems are available to NCI’s raijin, the VDI, and data services. (e.g., `cd /g/data/rr3/publications`)
* Range of formats: NetCDF/HDF5, GeoTIFF, GRIB etc. Licensed with CCBY4.0, CCBY-NC-ND, CCBY-NC-SA, ECMWF, others. Is controlled and can have embargo periods etc.
-- *Slide End* --

-- *Slide* --
### Part 2: Modules
* Approximately 1200 software applications and versions installed on Raijin (`module avail`). Uses tcl modules system. Environment modules are not preserved between login and compute nodes.
* Usual commands available: `module help`, `module avail`, `module load`, `module unload`, `module purge` etc. Use of *specific* versions is strongly encouraged.
* Include the application desired for use (`module load application/version`) in job scripts.
-- *Slide End* --

-- *Slide* --
# Part 3: Scheduler and Queues
* Raijin uses PBS Professional v13 as the scheduler and resource manager.
* Many queues; `express`, `normal`, `copyq`, `hugemem`, `gpu`, `gpupascal`, `knl`, `normalbw`, `expressbw`, `normalsb`, `megamem`. Different queues have different costs to the project and default walltime.
* Listing of current jobs available through `qstat`; alternatives are `qstat -a`, `nqstat`, and `nqstat_anu`. The latter two list the number of jobs in each queue, `nqstat` updates every 30 seconds, and `nqstat_anu` updates instantaneously.
-- *Slide End* --

-- *Slide* --
### Part 3: Sample PBSPro Job Script
```
#!/bin/bash   
#PBS -l walltime=01:00:00    
#PBS -l mem=1 GB    
#PBS -l jobfs=1 GB   
#PBS -l ncpus=4   
#PBS -q expressbw   
#PBS -P ProjectID
module load application/version
command data   
```
-- *Slide End* --

-- *Slide* --
### Part 3: Other Common Directives
#PBS –l ngpus=yy	Specifies the number of gpus, ngpus has to be 3 x ncpus
#PBS –l jobfs=xxxMB	Specifies the disk requirement
#PBS –l software=xxx,yyy	Specifies all the licensed software
#PBS –l wd	Starts the job from the directory it was submitted
#PBS –W depend=after:xxx	Sets dependencies between this and other jobs.
-- *Slide End* --

-- *Slide* --
### Part 3: Job Control Commands
* To submit a job use `qsub JobName`. Job status can be determined by `qstat JobID`, `qstat -s JobID` or `qstat -u Username`, or `qdel JobID` to delete a job. To review a job's details use `qstat -f JobID`.
* A job's queue can be changed with `qmove Newqueue JobID`, 
* Standard output and error streams are collected by PBSPro and saved in `<Jobname>.o<Jobid>` for standard output and `<Jobname>.e<Jobid>` for standard error.
* Interactive jobs can be launched with the `-I` option and the resource requirements specified at launch. e.g., `qsub -I -l ncpus=2,mem=1G,walltime=00:15:00 -q expressbw`
-- *Slide End* --

-- *Slide* --
### Part 3: Local Disk for I/O
* If your job writes to `/short` too often, export to `jobfs` (a node-local disk), using the `-l jobfs=xxgb`
* Inside a job, the path to jobfs is in the PBS_JOBFS environment variable.
-- *Slide End* --

-- *Slide* --
### Part 3: Job Priority at NCI
* Priority at NCI = ncpus/walltime
* Job history can be checked at `http://usersupport.nci.org.au`. Check CPU efficiency, used vs requested walltime, memory.
-- *Slide End* --










- *Slide* --


NCI’s Virtual Desktop Infrastructure (VDI) is an interactive environment for data
visualisation and analysis in the cloud. The VDI substantially simplifies complicated
analysis workflows that span multiple NCI systems by bringing together key data
collections, analysis software, and compute systems in a familiar interface which users
can work with as though on their own computer, without the need to wait on interactive
queue jobs.

The VDI supports a number of user communities and operates on NCI’s private partner
cloud Tenjin. Access is enabled on a per-project basis at the request of the lead
CI. Those who do not currently have access can put in a request through
help@nci.org.au or help.nci.org.au to be activated and their data made available in the
VDI.


Programming and analysis
e.g., MATLAB*, Python, iPython/Jupyter Notebooks, R, IDL*, and various compilers
Earth and climate science tools and libraries
e.g., UV-CDAT, Ferret, NCO, CDO, Ncview
Geospatial tools and libraries
e.g., QGIS, GDAL, GRASS
Bioinformatics tools
e.g., IGV
Data format libraries
e.g., NetCDF, HDF5
Workflow tracking
e.g., VisTrails (vistrails.org)
Remote job submission to HPC batch processing queues
VDI User Guide: https://opus.nci.org.au/display/Help/VDI+User+Guide

Best to submit jobs which are close to memory and walltime to improve efficiency of scheduler.

- Time	in memory access? Or Flop? Vec?	
- Context Switch	
- Oversubscribe?	
- Sometimes less than 100% give you faster result	

Allinea offers MPI Profiling, I/O Profiling, Tracing, and Hardware Perf
https://en.wikipedia.org/wiki/Allinea_DDT (Commercial product)

DDt is debugging
Profiling is called map
And has performance report

Message	passing	time	is	NOT	usually	the	problem	
•  Main	problem	is	having	processes	wai;ng	for	other	processes	(blocking	MPI	calls)	
•  Imbalance	cannot	be	eliminated,	but	could	be	minimised	
•  Have	a	smaller	number	of	processes	with	less	work	than	the	majority,	not	more.	


Independent I/O works for maximum contiguous access.	

Collective I/O better for non­‐contiguous accesses	


Network Common Data Form (NetCDF) & Hierarchical Data Format (HDF)
(from Unidata: http://www.unidata.ucar.edu/software/netcdf/docs/faq.html#whatisit)



=================================

Day 4

Intel blurb
- CPUs, Omnnipath, memory storage, compilers and tools

Parallel studio XE
- C++, C, Fortran, Python, Java, OpenMP, MPI, TB
- Composer and Cluster editions


2017
- 2nd gen Intel Phi and AVX-512

- SSE and AVX OpenMP explicit vectorisation in OpenMP

- Intel compilers have better performance in floating point and interger for C and Fortran (especially Fortran)
- Intel python is better and faster (free?)
- Intel MKL for linear algebra (blas, lapack, pardiso), FFTs, vector math etc

- Intel DDR/Optance SSD, 3D NAND SSDs

http://software.intel.com/moderncode

-01 optimise for speed (no code size increase)
-02 default, optimise for speed 
-03 high level loop optimiseation caution. Good for applications that have loops that do many floating point calculations or process large datasets.

[Note that some poorly suited applications, 03 is actually slower than 02]

-g  symbols for debugging
-ipo Multifile interprocedural optinmisation
-fast optimise for speed inclided -ipo, -O2 etc.
-qoomp OpenMP support
-parallel Automatic parallesiation (caution) looks for paralleslistaion in code, inserts OpenMP pragmas.


Inprocedural optimisations, two stage process. 


Knights Landing designed for problems that thread and vectorisation. Very good at it. Requires lots of work. If problem not vectorisation or threads use Xeon processords (Ivy Bridge, Haswell, Broadwell)


CPU + GPU = GPGPU Coprocessing (GPUs have thousands of core per processor)

Whilst GPU is computing, the CPU can do other things.

CPU optimised for low-latency access to cache and control logic, whereas GPU for data parallel computation

If the problem can be broken down into blocks, GPUs can improve things - quickly!

