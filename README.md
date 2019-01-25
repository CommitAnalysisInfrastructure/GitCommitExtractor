# GitCommitExtractor
This [ComAnI](https://github.com/CommitAnalysisInfrastructure/ComAnI) plug-in realizes an extractor for extracting commits from Git repositories and providing them as input to a ComAnI analyzer. It supports the following extraction variants:

- **Full repository extraction**, which performs the extraction of all commits of a software repository. This requires the definition of the location of the target repository as part of the configuration file using the parameter `extraction.input`.
- **Partial repository extraction**, which performs the extraction of a predefined set of commits. Besides the location of the target repository, this requires the specification of an additional file, which contains a list of unique commit numbers, e.g.,  "b025911". Each line of this commit list file must contain exactly one commit number. Further, the author of the commit list file must ensure that the commit numbers specify commits of the target repository. The usage of a commit list file requires its definition in the configuration file as follows: `extraction.commit_list = <path>/<to>/commitlist-file`
- **Single commit extraction**, in which the content of a single commit can be passed on the command line as an input. Therefore, the infrastructure has to be executed using the `-i` option followed by the commit information, which is terminated by a last line containing the string “!q!”.

Depending on the extraction variant, this extractor executes the following Git commands:
- `git log --oneline`: Prints all commit numbers (SHAs)
- `git show -s --format=%ci <SHA>`: Prints the committer date for a particular commit; ; `<SHA>` will be replaced by a particular commit number, like "b025911"
- `git show -U100000 --no-renames <SHA>`: Prints the the entire commit information, the content of the changed files (100.000 lines of context including renamed files), and the changes to these files; `<SHA>` will be replaced by a particular commit number, like "b025911"

*Main class name:* `net.ssehub.comani.extraction.git.GitCommitExtractor`

*Support:*
- Operating system: all
- Version control system: “git”

For more information on how to use ComAnI and its plug-ins, we recommend reading the [ComAnI Guide](https://github.com/CommitAnalysisInfrastructure/ComAnI/blob/master/guide/ComAnI_Guide.pdf).

## Installation
Download the [GitCommitExtractor.jar](/release/GitCommitExtractor.jar) file from the release directory and save it to the ComAnI plug-ins directory on your machine. This directory is the one specified as `core.plugins_dir` in the configuration file of a particular ComAnI instance.

*Requirements:*
- The [ComAnI infrastructure](https://github.com/CommitAnalysisInfrastructure/ComAnI) has to be installed to execute this plug-in as the extractor of a particular ComAnI instance
-	[Git](https://git-scm.com/) has to be installed and globally executable

## Execution
This plug-in is not a standalone tool, but only executable as the extractor of a particular ComAnI instance. Therefore, it has to be defined in the configuration file via its fully qualified main class name as follows:

`extraction.extractor = net.ssehub.comani.extraction.git.GitCommitExtractor`

*Plug-in-specific configuration parameter(s):*

None

## License
This project is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0.html).

## Acknowledgments
This work is partially supported by the ITEA3 project [REVaMP²](http://www.revamp2-project.eu/), funded by the [BMBF (German Ministry of Research and Education)](https://www.bmbf.de/) under grant 01IS16042H.

A special thanks goes to the developers of [KernelHaven](https://github.com/KernelHaven/): Adam Krafczyk, Sascha El-Sharkawy, Moritz Fl\"oter, Alice Schwarz, Kevin Stahr, Johannes Ude, Manuel Nedde, Malek Boukhari, and Marvin Forstreuter. Their architecture and core concepts significantly inspired the development of this project. In particular, the mechanisms for file-based configuration of the infrastructure and the plug-ins as well as loading and executing individual plug-ins are adopted in this work.
