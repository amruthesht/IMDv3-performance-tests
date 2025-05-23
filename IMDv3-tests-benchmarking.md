# IMDv3 performance tests and benchmarking

### Benchmarking

We benchmark the code by comparing it against the evident alternative for streaming viz. outputting data to files. To comprehensively study any effects of streaming on performance and quantitively verify its advantage over file I/O we run the following distinct scenarios:

1. vanilla code - `vanilla`
2. IMDv3 modified code - `imdv3`
3. IMDv3 modified code with File I/O (streaming set to off) - `fileio`
4. IMDV3 modified code with streaming - `streaming`

#### Choice of System

HEL in water

The input files for each MD engine can be found in the GitHub respoitory

#### Procedure for benchmarking

We start off by preparing the system using the CHARMM GUI Input generator. The system is prepared by putting together the HEL and adding water molecules around it. We use the starting structures provided for each of the 3 MD engines viz. GROMACS, LAMMPS, NAMD to run minimization and equilibration for about $0.25 ns$ followed by minimization and equilibration steps. Theses step are done for the vanilla code

The final production step is for $1 ns$ and executed for the 4 different cases discussed above.

#### MD engine optimization

We aim to do the benchmarking at computational resource settings that would be optimum for the system of interest. We limit ourselves to a single a100 GPU. We first test to figure out the exact number of CPUs that produces optimum performance for each MD engine. We then use the optimum settings from this step for eventual production and benchmarking.

#### Benchmarking data storage

Data stored as 2-D dataframe/array with each representing a separate data point i.e. run

Columns for the data frame are as follows

1. Date and Time Job submitted
2. Date and Time Job Ran
3. Date and Time Job Ended
4. Cluster Name
5. Node Name list
6. Number of GPUs requested
7. GPU type requested
8. Number of CPUs requested
9. CPU ID list
10. Type of Prod - N/A, vanilla, imdv3, imdv3+filei/o, imdv3+streaming
11. Purpose of Run - optimization, performance
12. Type of Run - equil, prod
13. Number of GPUs used
14. GPU type used
15. GPU ID used
16. Number of CPUs used
17. CPU IDs used
18. Frequency of *.out file I/O
19. Frequency of File I/O box dimensions - N/A if not outputting
20. Frequency of File I/O positions - N/A if not outputting
21. Frequency of File I/O velocities - N/A if not outputting
22. Frequency of File I/O forces - N/A if not outputting
23. Frequency of streaming I/O time - N/A if not streaming
24. Frequency of streaming I/O energies - N/A if not streaming
25. Frequency of streaming I/O box dimensions - N/A if not streaming
26. Frequency of streaming I/O positions - N/A if not streaming
27. Frequency of streaming I/O velocities - N/A if not streaming
28. Frequency of streaming I/O forces - N/A if not streaming
29. Positions wrapped - (yes/no - 1/0) N/A if not outputting
30. Integration Time Step (ps)
31. Number of Steps
32. Total time of run
33. Average performance (ns/day)
34. Standard deviation of performance (if provided)

The data for this dataframe can be extracted from log files i.e. `*.out` files inside the output folders. The directory structure for these is as follows: 

##### Directory structure for output

`output/type-of-prod/purpose-of-run/type-of-run/run-*`

e.g. `output/vanilla/optimization/prod/run-2`

NOTE: 

equil will only be run once inside `output/vanilla/performance/equil/run-1/`

The GitHub repository only stores data relevant for the analysis and post-processing i.e `*.out` and `*.log` files

#### Parameters of interest for becnhmarking

We vary the various output frequencies and streaming frequencies to study the effect of file I/O and streaming I/O on the speed and performance of the MD engine.

#### GitHub repository for benchmarking
https://github.com/amruthesht/IMDv3-performance-tests 