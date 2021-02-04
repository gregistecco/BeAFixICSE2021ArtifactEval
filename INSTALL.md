## Native downloads
BeAFix replication package can be downloaded from [here](https://doi.org/10.6084/m9.figshare.13626776.v1).

BeAFix [latest release](https://github.com/saiema/org.alloytools.alloy/releases/tag/2.3.8) can be downloaded from the [repository](https://github.com/saiema/org.alloytools.alloy).

## Installation

### Replication package, Docker version
* Pull the image *drstein/beafix:2.1.2* (available at [Docker Hub](https://hub.docker.com/r/drstein/beafix)) by executing docker `pull drstein/beafix:2.1.2` or by using Docker Desktop.

### Native version
* GNU/Linux or Mac OSX, and requires Java 8, standard Unix commands (find, awk, grep), a Bash shell, and Alloy Analyzer (the GUI version of BeAFix can be used, too). The selected solver must be Minisat, and the stack and memory have to be set to their maximum values.
* Set up BeAFixCLI-2.1.2 solver and memory options, see below.

Version 2.1.2 of BeAFix CLI requires minisat solver and memory related options to first be configured in either Alloy Analyzer or BeAFix-2.3.8.jar (GUI version).
The following steps must be executed before running BeAFixCLI-2.1.2.jar:

* Ejecute BeAFix-2.3.8.jar `java -jar BeAFix-2.3.8.jar`.
* Select to Options->Solver->MiniSat (only MiniSat, not the MiniSat with Unsat Core).
* Select to Options->Maximum memory->16384 Mb.
* Select to Options->Maximum stack->65536 K.
* Quit BeAFix.

_This is only required for BeAFixCLI-2.1.2.jar when running natively, if you are using the Docker version then these steps are not required._
_These steps can also be done in [Alloy Analyzer](https://github.com/AlloyTools/org.alloytools.alloy/releases/tag/v5.1.0)._

### Running BeAFix natively
No installation steps are required (see above). The only dependency is Java 8, and standard Unix tools and shell. BeAFix is provided as a jar bundle containing all the required dependencies.


You can now continue with the steps described in [README.md](README.md) to reproduce the experiments.
