# Requirements

The requirements for running the tool as well as the experiments performed in the paper are the following:
* **Docker**: for Docker versión of the replication package, either installing Docker Desktop for [Windows](https://docs.docker.com/docker-for-windows/install/) or [macOS](https://docs.docker.com/docker-for-mac/install/); or by installing [Docker Engine](https://docs.docker.com/engine/install/) (not available for Windows).
* **Java**: to run the tool natively (Command-Line and GUI version), *Java JDK 1.8* (if you don't have it installed, you can download it from [here](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html))
* **GNU Bash 4.4.20+**_(*)_: To run the script in the replication package.
  * We used some commands in ubuntu that they may work slightly different on MacOS: **timeout**, **grep**, and **sed**.
  
_(*) : Older versions might work._

The tool has been successfully tested with these requirements using GNU/Linux and Mac OSX. Our experiments were run with 16Gb of memory as maximum memory available for BeAFix.
