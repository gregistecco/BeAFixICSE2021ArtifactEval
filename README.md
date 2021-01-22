# BeAFix (submission for Artifact Evaluation at ICSE 2021)

### BeAFix: Bounded Exhaustive Search of Alloy Specification Repairs

*Simón Emmanuel Gutiérrez Brida, Germán Regis, Guolong Zheng, Hamid Bagheri, Thanhvu Nguyen, Nazareno Aguirre and Marcelo Frias*

## Artifact Description

BeAFix is an automated repair technique for faulty models written in Alloy, a declarative language based on first-order relational logic. BeAFix is backed with a novel strategy for bounded exhaustive, yet scalable, exploration of the spaces of fix candidates and a formally rigorous, sound pruning of such spaces. Moreover, different from the state-of-the-art in Alloy automated repair, that relies on the availability of unit tests, BeAFix does not require tests and can work with assertions that are naturally used in declarative languages. 

BeAFix provides two ways of usage: 
* A *Command-Line* process usually used to perform batch reparation process or to use it as complement tool.
* A *Graphical User Interface* as an extension of Alloy Analyzer, allowing the use of the tool in an interactive way. 

## Getting and Installing BeAFix
BeAFix can be obtaing by downloading the correponding jar files (Command-Line or GUI Interfaces), or by means of docker which also contains all the replication package scripts and study cases. Detailed information about the installation can be found in [INSTALL.md](INSTALL.md).

## Using BeAFix

### Command-Line Interface

To run the CLI version you need the corresponding jar file (the one that has CLI in the name) and execute `java -jar <BeAFixCLI jar> --help` to see how to run the tool.

### Graphical User Interface

The GUI version of BeAFix is an extension of the Alloy Analyzer. The following image shows the features that BeAFix provides.

![](Images/BeAFixScreenshotAll.png)

In order to try to repair a faulty alloy specification the user needs to:
1. Mark the suspicious expressions (can be more than one). A marked expression has the form `{#m#([vars]) expression}` where vars is a comma separated list of variables related to the marked expression, this is used by the variabilization pruning technique. As an example we can consider the following marked expressions:

   * `{#m#() some x : T | P[x]}` where the whole quantifier expression is marked for mutation.

   * `some x : T | {#m#(x) P[x]}` where only a part of an expression is marked for mutation.

2. At the **Execute** menu, the **Repair** option should be shown and must be selected.

3. During the reparation process, at the right side panel, BeAFix shows information about the current state, namely: mutated expressions (original and mutated) with its corresponding [mutation operator](APPENDIX.md), and a the result of evaluating that candidate.

4. Finally, a detailed report of the process is shown, including, if found, the fixed specification file.
   * _The repaired candidate is shown with the mutations used, the repaired specification may have some issues since the candidate writter is very simple_

5. Aditionally, at the **Option** menu, the user can set a particular configuration for BeAFix:

   * **variabilization**: Enables/disables variabilization pruning technique (default is false).
   * **maxdepth**: How many mutations are allowed per marked expression (default is 2).
   * **timeout**: Time budget (in minutes) for the repair process (default is 0, unlimited).
   * **Use expression type**: If variabilization will use expressions exact types for constructed relations or more general ones (default and suggested is false).


## Reproducing the paper experitments

### Benchmarks
This replication package contains the benchmarks used in the paper. These can all be found in the benchmarks folder.

* **A4F-1B**: 1936 models from the Alloy4Fun benchmark , each having one bug.

* **A4F-MB**: 273 models derived from the Alloy4Fun benchmark , each having multiple bugs.

* **ARepair-1B**: 16 models from the ARepair benchmark , each having one bug.

* **ARepair-MB**: 15 models from the ARepair benchmark , each having multiple bugs.

* **depth1granularity**: the 15 models have the faulty expression marked at the "line" level.

* **depth2granularity**: 13 of the 15 models with the faulty expression marked with one more level inside of the original expression. For example, if the expression marked at line level was c in (a + b), and the fault is in b then the marked expression would be a + b.

* **depth3granularity**: 5 of the 13 models with the faulty expression marked with one more level.

* **depth4granularity**: 3 of the 5 models with the faulty expression marked with one more level.

_granularity didn't have a great impact on the reparability_

### Running the experiments

BeAFix is in active developement, for that reason, paper's experiments must be replicated using the same version we used at that moment.

To run the docker, the command is `docker run -it drstein/beafix:2.1.2`. Inside the folder containing the replication package contents, the following command must be executed:

`./runBeAFix.sh BeAFixCLI-2.1.2.jar <path to study case> <depth> <pruning> <timeout> <results folder>.`

Where **path to study case** is the full path to an *.als* file inside the benchmarks folder; **depth** is how many mutations to apply to each buggy expression; **pruning** *enables (1)* or *disables (0)* the use of pruning techniques; **timeout** (in minutes) states how much time is allowed to find a fix, the script adds 5 extra minutes to forcefully terminate BeAFix if it didn't finished by itself in the allotted timeout; and **results folder** is where results will be stored (if the folder doesn't exist it will be created, but if it exists it should be empty).

On our experiments we used depth 2 to 5 (only running cases with an increased depth for cases that were not repaired), and we used 60 minutes as timeout.

### Output information details

After running an experiment, the results folder defined when calling the BeAFix script will contain the following:

* `<model's name>.summary` a sequence of values separated with `;` that starts with the model's name, followed by:
  * **TO from script** if BeAFix didn't end in the allotted time and was finished by the script. In this case no other value will be present in the summary.
  * **FAILED WITH CODE** followed by an exit code, if an error occurred. In this case no other value will be present in the summary.
  * **repaired (true/false)** stating if BeAFix was able to find a fix or not. 
  * **time (ms)** the running time of BeAFix.
  * **spuriosity (real/spurious)** a check only done when having a model with oracles that will only be used to validate a fix (none of our experiments use this) this will always be real for this replication package.
  * **the repair** as the modifications done by BeAFix to repair the model (separated by the symbol £).
  * **repair verification** this last value was only used as a debug helper and should be ignored.
* `<model's name>_repair.als` a repaired model (if a repair was found), this model can sometimes have some issues because the candidate writer was only used as a debug helper. This file can sometimes stay in the same path as the original model.
* `<model's name>_repair.verification` a side product of the last verification process.
* `<model's name>_summaryLog.log` if BeAFix ended with an error code, this file contains the last 300 lines of some log files generated by BeAFix, this log files are deleted by the `cleanFiles` function in `runBeAFix.sh`, you can avoid this cleaning by commenting the `cleanFiles "$scase"` line.

#### Running the experiments per cases
To run whole cases, all Alloy4Fun 2 bugs classroom cases for example, the following command can be used:

`find benchmarks/A4F-MB | awk '/Classroom/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} <depth> <pruning> <timeout> <results folder>`

We recommend using meaningful and separate results folder for each run, depth, and pruning configuration, we give a list of commands for each case and pruning configuration using depth 2:

**Alloy4Fun 1 bug cases**

`find benchmarks/A4F-1B | awk '/\.als/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} <depth> <pruning> <timeout> <results folder>`

**Alloy4Fun 2 bugs cases**

*Classroom*:

`find benchmarks/A4F-MB | awk '/Classroom/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 CLASSROOM-2B-NOPRUNING-DEPTH2`

`find benchmarks/A4F-MB | awk '/Classroom/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 1 60 CLASSROOM-2B-PRUNING-DEPTH2`

*CV*

`find benchmarks/A4F-MB | awk '/CV_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 CV-2B-NOPRUNING-DEPTH2`

`find benchmarks/A4F-MB | awk '/CV_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 1 60 CV-2B-PRUNING-DEPTH2`

*Graphs*

`find benchmarks/A4F-MB | awk '/Graphs_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 GRAPHS-2B-NOPRUNING-DEPTH2`

`find benchmarks/A4F-MB | awk '/Graphs_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 1 60 GRAPHS-2B-PRUNING-DEPTH2`

*LTS*

`find benchmarks/A4F-MB | awk '/LTS_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 LTS-2B-NOPRUNING-DEPTH2`

`find benchmarks/A4F-MB | awk '/LTS_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 1 60 LTS-2B-PRUNING-DEPTH2`

*Production*

`find benchmarks/A4F-MB | awk '/Production_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 PRODUCTION-2B-NOPRUNING-DEPTH2`

`find benchmarks/A4F-MB | awk '/Production_/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 1 60 PRODUCTION-2B-PRUNING-DEPTH2`

*Trash*

`find benchmarks/A4F-MB | awk '/Trash/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 TRASH-2B-NOPRUNING-DEPTH2`

`find benchmarks/A4F-MB | awk '/Trash/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 1 60 TRASH-2B-PRUNING-DEPTH2`

**ARepair 1 bug**

`find benchmarks/ARepair-1B | awk '/\.als/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 AREPAIR-1B-DEPTH2`

**ARepair multiple bugs**
`find benchmarks/ARepair-MB/depth1granularity | awk '/\.als/' | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar {} 2 0 60 AREPAIR-2B-GRAN1-DEPTH2`

#### Merging summaries and running next depth experiments

Assuming a results folder (called results-depth2 for simplicity), Graphs 2 bugs cases, and pruning:

* Merging : `find results-depth2 | awk '/\.summary$/' | xargs -I {} cat {} >> results.summary`
* Obtaining not repaired cases: `cat results.summary | grep -v '^.*; true' | sed 's|;.*$||g' >> notRepaired`
* Running next depth: `cat notRepaired | xargs -I {} ./runBeAFix.sh BeAFixCLI-2.1.2.jar benchmarks/A4F-MB/{}.als 3 1 60 GRAPHS-2B-PRUNING-DEPTH3`
