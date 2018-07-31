# Replication code for Moore et al., "Mimi-Page".

This repository holds all code required to replicate the results of
"Mimi-PAGE - An Open-Source Implementation of the PAGE09 Integrated
Assessment Model".

## Software requirements

You need to install [Julia](http://julialang.org/)
and [R](https://www.r-project.org/) to run the replication code.

## Preparing the software environment

On the Julia side of things you need to install a few packages.  These
consist of the packages required by Mimi, along with `RCall`, which is
used to access `R` for graphing.  You can use the following julia code
to do so:

````julia
Pkg.add("Mimi")
Pkg.add("Distributions")
Pkg.add("DataFrames")
Pkg.add("CSVFiles")
Pkg.add("RCall")
````

On the R side of things you also need to install a number packages,
which are all included in the `tidyverse` environment. From R code,
run the following:

````R
install.packages("tidyverse")
````

You also need to download the file ``Rallfun-v34.txt`` from
[here](https://dornsife.usc.edu/assets/sites/239/docs/Rallfun-v35.txt) and
place it in the ``src`` folder.

## Cloning the repository

This git repository uses a git submodule for Mimi-PAGE. To ensure the submodule gets properly downloaded, make sure to use the
git ``--recurse-submodules`` option when cloning the repository.
That is, you can clone the repository from the command-line with:

```sh
git clone --recurse-submodules https://github.com/anthofflab/mimi-page.jl.git
```

If you cloned the repository without that option, you can issue the
following two git commands to make sure the submodule is present on
your system: ``git submodule init``, followed by ``git submodule
update``.

## Running the replication script

To recreate all outputs for this paper, run the ``main.jl`` file in the folder ``src`` with this command:

````
julia src/main.jl
````

## Input files

### `data/excel-performance.csv`

The data in this file was generated by manually running the Excel version
of PAGE with the @RISK plugin. We started by opening the original PAGE Excel
file ``PAGE09 v1.7 default.xlsx`` in the 64 bit version of Microsoft
Excel 2016 on Windows 10. We used @Risk version 7.5.1 Build 146. The different
rows in the file correspond to runs where we modified two settings in the
``Simulation->Settings`` dialog: the ``Number of Iterations`` and the
``Multiple CPU Support`` setting. Each run was initiated by clicking the
``Start Simulation`` button. The elapsed time is the number @RISK reported
in the ``@RISK Simulating`` window.

The runs were performed on a system with an Intel Core i7-4770 @ 3.4GHz
CPU that had 16 GB of main memory.

### `data/PAGE09_mc_policya_100k.csv`

This contains the raw Monte Carlo output from the Excel version of PAGE09 for
each of four main outputs from the Excel PAGE '09 model: `td` (total
damages), `tac` (total adaptation costs), `tpc` (total preventative
costs), and `te` (total effect, combining these three).

### `src/mimi-page/output/mimipagemontecarlooutput.csv`

To generate the Monte Carlo comparison graph, you will need to run the
`montecarlo.jl` script in Mimi-PAGE.  This will generate a file
`src/mimi-page/output/mimipagemontecarlooutput.csv`, which includes the values for a
number of output metrics for each Monte Carlo run.

## Result tables

All result tables will be stored in CSV files in the folder
``results``. The following files will be created by running the main
script:

 - `deterministiccomparison.csv`: Reports the difference between
   Mimi-PAGE results for a set metrics and the corresponding metrics
   in PAGE '09, computed by `deterministiccomp.jl`, which is included
   in `main.jl`.
   
 - `performance.csv`: This combines the Excel performance metrics with
   Mimi-PAGE performance metrics generated when the `mccomp.jl` script
   is run, which is included in `main.jl`.

## Result plots

All results plots are saved in the folder ``results`` in various formats.
