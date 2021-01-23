## Native downloads
BeAFix replication package can be downloaded from [here](https://doi.org/10.6084/m9.figshare.13626776.v1).

BeAFix [latest release](https://github.com/saiema/org.alloytools.alloy/releases/tag/2.3.8) can be downloaded from the [repository](https://github.com/saiema/org.alloytools.alloy).

## Installation

### Replication package, Docker version
* Pull the image *drstein/beafix:2.1.2* (available at [Docker Hub](https://hub.docker.com/r/drstein/beafix)) by executing docker `pull drstein/beafix:2.1.2` or by using Docker Desktop.

### Native version
* The replication package does not involve any specific install procedure. It runs on GNU/Linux and Mac OSX, and requires Java 8, standard Unix commands (find, awk, grep), a Bash shell, and Alloy Analyzer (the GUI version of BeAFix can be used, too). The selected solver must be Minisat, and the stack and memory have to be set to their maximum values.

### Running BeAFix natively
No installation steps are required (see above). The only dependency is Java 8, and standard Unix tools and shell. BeAFix is provided as a jar bundle containing all the required dependencies.


You can now continue with the steps described in [README.md](README.md) to reproduce the experiments.
