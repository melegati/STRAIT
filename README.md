# STRAIT
A tool for software reliability analysis using GitHub repositories, developed as part of my [**Bachelor's thesis**](https://is.muni.cz/th/a2htp/) and extended in [**Master's thesis**](TODO). It was proposed in paper http://dx.doi.org/10.1109/MSR.2019.00025 on *2021 IEEE/ACM 18th International Conference on Mining Software Repositories (MSR)*


The tool allows automated reliability analysis of the projects hosted on GitHub using the selected Software Reliability Growth Models (SRGMs). The input data for the model will be collected from the issue tracker of the GitHub projects.

## Requirements

* Java, version 8 or above: [https://www.java.com/en/](https://www.java.com/en/)
* R Project, version 3.5.0 or above: [https://cloud.r-project.org/](https://cloud.r-project.org/)
* Apache Derby DB, version 10.14.2.0 or above: [https://db.apache.org/derby](https://db.apache.org/derby/papers/DerbyTut/install_software.html#derby)
* A personal access token to the GitHub api. More information here: [https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

## Installation

1. In the R console, install the rJava packages: 
    * install.packages("rJava")
    * install.packages("nls2")
    * install.packages("broom")
2. Set the environment variables for R Project:
    * Set R_HOME to the R installation directory (this value could be found by running `R.home()` in R)
    * Add R_HOME\library\rJava\jri to the path
    * In some cases, the tool is not able to find the JRI, then you will need to add the following when calling the tool: `-Djava.library.path=$R_HOME/library/rJava/jri`. For example: ```java -Djava.library.path=$R_HOME/library/rJava/jri -jar strait.jar [OPTIONS]```. In Windows, use %R_HOME% instead.
3. Define properties in *git_hub_authentication_file.properties*
    * Copy the file *git_hub_authentication_file.properties* from the folder `DataProvider/src/main/resources` to the same folder of the `STRAIT.jar`
    * Fill the token property with your personal access token to the GitHub api
4. Make sure Apache Derby client server is running or run:
    * In Windows, - *startNetworkServer.bat*
    * In Linux or Mac OS, *startNetworkServer*

## Usage

The tool can be executed from command-line by running:

```java -jar strait.jar [OPTIONS]```

An overview of command-line options is in Table - options. The tool also prints a
list of all options if no argument is provided. The help can be accessed by running:

```java -jar strait.jar --help```

A simple execution of the tool to evaluate the testify
project hosted at Github may look like:

```java -jar strait.jar -url https://github.com/stretchr/testify -ns testify -e -fde -fc -fdu -ft 2018-01-01T00:00:00 2021-01-01T00:00:00```

Main options:

* When you first run the tool on a repository, you must use the *-url* option specifying the location of the project and the option *-ns* specifying the name of the snapshot for storing the gathered issues. 

* After the first run, data regarding that repository is stored in the databae and the *-sn* could used to specify the
snapshot name instead of the *-url* - for the analysis. 

* The option *-e* starts the execution of the SRGM analysis. No specific models are selected, so all of the available SRGMs will be applied. 

* Some options could be added to filter the issues that will be considered in the analysis. The *-fde* will filter only defects from issue reports. With the option *-fc*, closed issues are only considered. The *-fdu* option filters out duplicated issues. Furthermore, with *-ft* it limits the time period for which issue reports will be considered.

# Table - options
| Short option | Long option | Arguments |
| :---: | :---: | :---: |
| -h | --help | - |
| -url | - | [Repository URL] |
| -asl | --allSnapshotsList | - |
| -sn | --snapshotName | - |
| -cf | - |  [Path to config file]|
| -sl | --snapshotsList | - |
| -s | --save | [Data format] |
| -e | --evaluate | [Output file name] |
| -p | --predict | [Number of time units for prediction] |
| -ns | --newSnapshot | [Name of the new snapshot] |
| -fl | --filterLabel | [Label names] |
| -fc | --filterClosed | - |
| -ft | --filterTime | [From] [To] |
| -fde | --filterDefects | - |
| -fdu | --filterDuplications | - |
| -ms | --models | [Models] |
| -pt | --periodOfTesting | [Testing period time unit] |
| -tb | --timBetweenIssues | [Time unit for TBF] |
| -gm | --graphMultiple | - |
| -so | --solver | [Solver] |
| -out | - | [Output type] |
