# Requirements

The requirements for running the tool as well as the experiments performed in the paper are the following:
* **Docker** _(for docker version of our replication package)_: Either install Docker Desktop for [Windows](https://docs.docker.com/docker-for-windows/install/) or [macOS](https://docs.docker.com/docker-for-mac/install/); or [Docker Engine](https://docs.docker.com/engine/install/) (not available for Windows).
* **Java** _(for the native version of our replication package)_: to run the tool natively (Command-Line and GUI version), *Java JDK 1.8* (if you don't have it installed, you can download it from [here](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html))
* **GNU Bash 4.4.20+** _(for the native version of our replication package)_ _(*)_: To run the script in the replication package.
  * We used some Linux commands that may be used slightly differently on MacOS: **timeout**, **grep**, **awk** and **sed**.
  
_(*) : Tested with Bash 4.4.20. Older versions might work too._

The tool has been successfully tested with these requirements using GNU/Linux and Mac OSX. Our experiments were run with 16Gb of memory as maximum memory available for BeAFix.
