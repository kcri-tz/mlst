MLST
===================

Multi-Locus Sequence Typing


Documentation
=============

The MLST service contains one python script *mlst.py* which is the script of the lates
version of the MLST service. The method enables investigators to determine the ST based on WGS data.

## Content of the repository
1. mlst.py      - the program
2. test     	- test folder
3. README.md
4. Dockerfile   - dockerfile for building the mlst docker container


## Installation

Setting up MLST program
```bash
# Go to wanted location for mlst
cd /path/to/some/dir
# Clone and enter the mlst directory
git clone https://bitbucket.org/genomicepidemiology/mlst.git
cd mlst
```

Build Docker container
```bash
# Build container
docker build -t mlst .
# Run test
docker run --rm -it \
       --entrypoint=/test/test.sh mlst
```

#Download and install MLST database
```bash
# Go to the directory where you want to store the mlst database
cd /path/to/some/dir
# Clone database from git repository (develop branch)
git clone https://bitbucket.org/genomicepidemiology/mlst_db.git
cd mlst_db
MLST_DB=$(pwd)
# Install MLST database with executable kma_index program
python3 INSTALL.py kma_index
```

If kma_index has not bin install please install kma_index from the kma repository:
https://bitbucket.org/genomicepidemiology/kma

## Dependencies
In order to run the program without using docker, Python 3.5 (or newer) should be installed along with the following versions of the modules (or newer).

#### Modules
- cgecore 1.5.5
- tabulate 0.7.7

Modules can be installed using the following command. Here, the installation of the module cgecore is used as an example:
```bash
pip3 install cgecore
```
#### KMA and BLAST
Additionally KMA and BLAST version 2.8.1 or newer should be installed.
The newest versions of KMA and BLAST can be installed from here:
```url
https://bitbucket.org/genomicepidemiology/kma
```

```url
ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/
```

## Usage

The program can be invoked with the -h option to get help and more information of the service.
Run Docker container


```bash
# Run mlst container
docker run --rm -it \
       -v $MLST_DB:/database \
       -v $(pwd):/workdir \
       mlst -i [INPUTFILE] -o . -s [SPECIES] [-x] [-st]
```

When running the docker file you have to mount 2 directory:
 1. mlst_db (MLST database) downloaded from bitbucket
 2. An output/input folder from where the input file can be reached and an output files can be saved.
Here we mount the current working directory (using $pwd) and use this as the output directory,
the input file should be reachable from this directory as well.

-i INPUTFILE   input file (fasta or fastq) relative to pwd.

-s SPECIES 	species origin of input file.

-o OUTDIR	outpur directory relative to pwd.

-x 		extended output. Will create an extented output.

-st     save temp_folder. Will save the temporary folder.

## Web-server

A webserver implementing the methods is available at the [CGE website](http://www.genomicepidemiology.org/) and can be found here:
https://cge.cbs.dtu.dk/services/MLST/

Citation
=======

When using the method please cite:

Multilocus Sequence Typing of Total Genome Sequenced Bacteria.
Larsen MV, Cosentino S, Rasmussen S, Friis C, Hasman H, Marvig RL,
Jelsbak L, Sicheritz-Pont�n T, Ussery DW, Aarestrup FM and Lund O.
J. Clin. Micobiol. 2012. 50(4): 1355-1361.
PMID: 22238442         doi: 10.1128/JCM.06094-11
[Epub ahead of print]

References
=======

1. Camacho C, Coulouris G, Avagyan V, Ma N, Papadopoulos J, Bealer K, Madden TL. BLAST+: architecture and applications. BMC Bioinformatics 2009; 10:421.
2. Clausen PTLC, Aarestrup FM, Lund O. Rapid and precise alignment of raw reads against redundant databases with KMA. BMC Bioinformatics 2018; 19:307.

License
=======


Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
