
[RAiSD: software to detect positive selection based on multiple signatures of a selective sweep and SNP vectors](https://www.nature.com/articles/s42003-018-0085-8)
===============================================

Authors: Nikolaos Alachiotis (n.alachiotis@gmail.com) and Pavlos Pavlidis (pavlidisp@gmail.com)

First release: 9/6/2017 

Last update: 31/12/2018       

Version: 1.8

About
-----

RAiSD (Raised Accuracy in Sweep Detection) is a stand-alone software implementation of the μ statistic for selective sweep detection. Unlike existing implementations, including our previously released tools (SweeD and OmegaPlus), RAiSD scans whole-genome SNP data based on a composite evaluation scheme that captures multiple sweep signatures at once. 

The main article ([PDF](https://www.nature.com/articles/s42003-018-0085-8.pdf)) describing RAiSD and the μ statistic is published in Communications Biology:

    1. RAiSD detects positive selection based on multiple signatures of a selective sweep and SNP vectors   
       https://www.nature.com/articles/s42003-018-0085-8   

Related publications:

2. Accelerated Inference of Positive Selection on Whole Genomes   
https://ieeexplore.ieee.org/abstract/document/8533493  
[PDF](http://kalman.mee.tcd.ie/fpl2018/content/pdfs/FPL2018-43iDzVTplcpussvbfIaaHz/2IXjcULtzLwXxGVD0BRuki/4etDSBv306dBHUVjNtmSyT.pdf)



Download and Compile
--------------------

The following commands can be used to download and compile the source code. 

    $ mkdir RAiSD
    $ cd RAiSD
    $ wget https://github.com/alachins/raisd/archive/master.zip
    $ unzip master.zip
    $ cd raisd-master
    $ make
    
The executable is placed in the path RAiSD/raisd-master/bin/release. A link to the executable is placed in the installation folder, i.e., raisd-master.

Test Run
--------

To verify that RAiSD is installed correctly, a test run can be done with the following commands. These commands are going to download a dataset (one of the many that we used for evaluation purposes) and execute RAiSD to process 1,000 simulated sets of SNPs (genomic region size = 100000 bp, weak bottleneck, selective sweep at the center of the region).
    
    $ wget 139.91.162.50/raisd_data/d1.tar.gz
    $ tar -xvzf d1.tar.gz
    $ ./RAiSD -n test_run -I d1/msselection1.out -L 100000
    
Upon completion, two output file are generated: RAiSD_Info.test_run and RAiSD_Report.test_run. 
    
In-tool Help
------------

RAiSD outputs a quick-reference help message that provides a short description for each of the supported command-line flags with the following command. 

    $ RAiSD -h
    
The quick-reference help message generated by the current RAiSD release is the following.

    This is RAiSD version 1.8, released in December 2018.

     RAiSD
         -n STRING
         -I STRING
        [-L INTEGER]
        [-h]
        [-v]
        [-f]
        [-s]
        [-t]
        [-p]
        [-S STRING]
        [-T INTEGER]
        [-d INTEGER]
        [-k FLOATING-POINT]
        [-l FLOATING-POINT]
        [-m FLOATING-POINT]
        [-b]
        [-a INTEGER]
        [-M 0|1|2|3]
        [-O]
        [-R]
        [-P]
        [-y INTEGER]
        [-D]

     -n	Provides a unique run ID that is used to name the output files, i.e., the info file and the report(s).
     -I	Provides the path to the input file, which can be either in ms or in vcf format.
     -L	Provides the size of the region in basepairs for ms files. If known, it can be used for vcf as well, leading to faster processing.
     -h	Prints this help message.
     -v     Prints version information.
     -f	Overwrites existing run files under the same run ID.
     -s	Generates a separate report file per set.
     -t	Removes the set separator symbol from the report(s).
     -p	Generates the output file RAiSD_Samples.STRING, where STRING is the run ID, comprising a list of samples in the input file (supported only with VCF).
     -S	Provides the path to the list of samples to be processed (supported only with VCF).
     -T	Provides the selection target (in basepairs) and calculates the average distance (over all datasets in the input file) between the selection target and the reported locations.
     -d	Provides a maximum distance (in base pairs, from the selection target) to calculate success rate in terms of reported locations in the proximity of the target of selection (provided via -T).
     -k	Provides the false positive rate (e.g., 0.05) to report the corresponding reported score after sorting the reported locations for all the datasets in the input file.
     -l	Provides the threshold score, reported by a previous run using a false positive rate (e.g., 0.05, via -k) to report the true positive rate.
     -m	Provides the threshold value for excluding SNPs with minor allele frequency < threshold (0.0-1.0).
     -b     Indicates that the input file is in mbs format.
     -a     Provides a seed for the random number generator.
     -M     Indicates the missing-data handling strategy (0: discards SNP (default), 1: imputes N per SNP, 2: represents N through a mask, 3: ignores allele pairs with N).
     -O     Shows progress on the display device (at snp set granularity).
     -R     Includes additional information (window start and end, and the mu-statistic factors for variation, SFS, and LD) in the report file.
     -P     Generates four plots (for the three mu-statistic factors and the final score) in one PDF file per set of SNPs in the input file using Rscript (activates -s, -t, and -R).
     -y     Provides the ploidy (integer value), use to correctly represent missing data.
     -D     Generates a site report, e.g., total, discarded, imputed etc
    
Input File Formats
----------------------

The current RAiSD release can process SNP data in Hudson's ms or VCF (Variant Call Format) file formats. The d1 test dataset, previously used for the test run, is in Hudson's ms format. For the VCF format, refer to the respective Wikipedia entry (https://en.wikipedia.org/wiki/Variant_Call_Format).

    RAiSD version 1.7 (or later) can also parse and process [ANGSD](http://www.popgen.dk/angsd/index.php/ANGSD) VCF files.  
    RAiSD version 1.8 (or later) can also parse compressed VCF files (including ANGSD-generated VCF files) in [GZ](https://www.gnu.org/software/gzip/) file format. 

Output Files
------------

RAiSD generates two output files, the RAiSD_Info and the RAiSD_Report, with the run name (provided via "-n") as file extension. 

RAiSD_Info.test_run

The first 20 lines of the RAiSD_Info.test_run file are shown below.

    RAiSD, Raised Accuracy in Sweep Detection
    Copyright (C) 2017, and GNU GPL'd, by Nikolaos Alachiotis and Pavlos Pavlidis
    Contact n.alachiotis/pavlidisp at gmail.com

    Command: ./RAiSD -n test_run -I d1/msselection1.out -L 100000 -f 
    Samples: 20
    Region:  100000 bp
    Format:  ms

    A pattern structure of 65536 patterns (max. capacity) and approx. 1 MB memory footprint has been created.

    0: Set 0 | sites 6300 | snps 6300 | region 100000 - Var 53090 8.400e-04 | SFS 12005 1.000e+00 | LD 38385 1.333e+00 | MuStat 49905 3.908e-04
    1: Set 1 | sites 6296 | snps 6296 | region 100000 - Var 49665 1.970e-03 | SFS 3745 1.000e+00 | LD 46945 2.000e+00 | MuStat 49685 8.320e-04
    2: Set 2 | sites 6109 | snps 6109 | region 100000 - Var 50835 1.890e-03 | SFS 42100 1.000e+00 | LD 89390 1.500e+00 | MuStat 50770 6.825e-04
    3: Set 3 | sites 6052 | snps 6052 | region 100000 - Var 49590 1.880e-03 | SFS 5545 1.000e+00 | LD 22460 1.500e+00 | MuStat 50150 1.102e-03
    4: Set 4 | sites 6184 | snps 6184 | region 100000 - Var 51920 3.020e-03 | SFS 26445 1.000e+00 | LD 55080 1.333e+00 | MuStat 52400 1.180e-03
    5: Set 5 | sites 5519 | snps 5519 | region 100000 - Var 49310 1.660e-03 | SFS 5475 1.000e+00 | LD 42490 1.333e+00 | MuStat 47165 8.925e-04
    6: Set 6 | sites 6052 | snps 6052 | region 100000 - Var 49640 1.700e-03 | SFS 9090 1.000e+00 | LD 61435 1.250e+00 | MuStat 48080 6.200e-04
    7: Set 7 | sites 6274 | snps 6274 | region 100000 - Var 50480 2.100e-03 | SFS 18910 1.000e+00 | LD 36695 1.333e+00 | MuStat 50490 8.505e-04

The RAiSD_Info file provides execution- and dataset-related information (command line, number of samples, region size, dataset format), as well as a result line per SNP set in the input file. Each per-set result line provides the following information:
a) set index, b) number of sites, c) number of SNPs, d) region size, e) best-score location and the respective score for each of the factors that form the μ statistic, denoted as VAR, SFS, and LD, and f) best-score location and the respective score for the μ statistic (MuStat).

The RAiSD_Info file additionally reports total execution time, memory footprint, and statistics about the total number of SNP sets in the input file (total, processed, skipped), as shown in the last 6 lines of the file.

    Sets (total):     1000
    Sets (processed): 1000
    Sets (skipped):   0

    Total execution time 32.03070 seconds
    Total memory footprint 1109 kbytes
    
RAiSD_Report.test_run

The first 20 lines of the RAiSD_Report.test_run file are shown below (using -R to include additional information in the report file).

    // 0
    430	20	840	1.640e-04	2.800e-01	2.580e+00	1.185e-04
    445	20	870	1.700e-04	2.600e-01	2.500e+00	1.105e-04
    450	20	880	1.720e-04	2.600e-01	2.077e+00	9.288e-05
    455	20	890	1.740e-04	2.800e-01	2.210e+00	1.077e-04
    460	20	900	1.760e-04	2.800e-01	2.071e+00	1.020e-04
    480	30	930	1.800e-04	2.800e-01	2.097e+00	1.057e-04
    490	30	950	1.840e-04	2.600e-01	1.667e+00	7.973e-05
    500	40	960	1.840e-04	2.600e-01	1.113e+00	5.324e-05
    515	50	980	1.860e-04	2.800e-01	8.798e-01	4.582e-05
    550	110	990	1.760e-04	2.800e-01	9.087e-01	4.478e-05
    565	120	1010	1.780e-04	2.800e-01	9.688e-01	4.828e-05
    585	150	1020	1.740e-04	2.800e-01	1.308e+00	6.371e-05
    595	170	1020	1.700e-04	2.800e-01	1.687e+00	8.031e-05
    600	170	1030	1.720e-04	2.800e-01	1.595e+00	7.683e-05
    605	180	1030	1.700e-04	3.000e-01	1.352e+00	6.897e-05
    625	190	1060	1.740e-04	3.000e-01	1.356e+00	7.077e-05
    650	240	1060	1.640e-04	3.000e-01	1.080e+00	5.315e-05
    665	260	1070	1.620e-04	3.000e-01	7.723e-01	3.753e-05
    670	270	1070	1.600e-04	2.800e-01	7.634e-01	3.420e-05
    
For each set of SNPs (separated by a line that contains the separator symbol "//" followed by the set index or chromosome name), the RAiSD_Report file contains a set of result lines, one per evaluated genomic location, with each result line providing the genomic location followed by the calculated μ statistic value (default) or the genomic location followed by the start and end positions of the window, the factors VAR, SFS, and LD, and finally the μ statistic (-R command-line flag). 

Required Input Parameters
-------------------------

The in-tool help message generates a list of parameters, as shown above. Those in brackets are optional.
    
    The basic execution mode does NOT require any FREE input parameters. 
   
In addition to the run name ("-n", used to name the output files accordingly) and the path to the input file ("-I") containing SNP data, the region length ("-L") is required only when simulated data are processed, i.e., ms files. The region length, which is required for calculating the μ statistic is extracted from the input file prior to processing when VCF files are analyzed. 

The following command lines provide two examples of command lines for processing ms and VCF files.

Hudson's ms format

    $ ./RAiSD -n ms_run -I input_file.ms -L 1000000

VCF format

    $ ./RAiSD -n vcf_run -I input_file.vcf

Optional Input Parameters
------------------------

In addition to the required set of parameters, several optional ones can be used. The in-tool help message provides a short description for each one of them. 

The -f parameter allows to overwrite existing output files. The default operation mode is to prevent execution in order to avoid accidental overwritting of existing output files.

Parameters -s and -t affect the way output files are generated. The -s parameter splits reports in separate files, one per SNP set, whereas the -t parameter removes the SNP set separator symbols "//".

Processing only a subset of the samples in a VCF file is possible with the use of parameters -p and -S. The former generates a list of all samples in the input file, stored in an output file named RAiSD_Samples.run_name, while the latter allows the user to provide a similar to RAiSD_Samples input file containing only the list of samples to process.

The optional parameters -T, -d, -k, and -l are used for evaluation purposes and are discussed in detail below.

Missing Data Strategies
-----------------------
RAiSD provides four different strategies to handle missing data, using the -M parameter followed by the strategy number.

    0: Discards all SNPs with missing data (default)
    1: Imputes N per SNP   
    2: Creates a mask for valid alleles and treats N as a third state 
    3: Creates a mask for valid alleles and ignores allele pairs with N

Evaluating Accuracy
-------------------
RAiSD provides a series of parameters that facilitate measuring performance when simulated datasets are analyzed. Given a simulated dataset, e.g., in ms format, that comprises several sets of SNPs, the -T parameter can be used in order to direct RAiSD to report accuracy, defined as the average distance between a known sweep location (provided via -T and the reported best-score locations). 

The following command line example for the previously conducted test run demonstrates the use of -T and the respective output. All provided datasets, like the d1 used in the test run, exhibit a selective sweep at the center of the genomic region they simulate. Therefore, RAiSD can be launched as follows:

    $ ./RAiSD -n test_run_accuracy -I d1/msselection1.out -L 100000 -T 50000
    
Note the additional output lines in the RAiSD_Info file, reporting accuracy. The values are in base pairs, averaged over the number of processed sets of SNPs.

    AVERAGE DISTANCE (Target 50000)
    mu-VAR	1022.500
    mu-SFS	26137.655
    mu-LD	18577.810
    MuStat	1253.685

Measuring Success Rate
-----------------------
The -d parameter can be used along with -T to direct RAiSD to report the success rate, defined as the percentage of sets with reported best-score location withing a distance (provided via -d, in base pairs) from the known target of selection (provided via -T, in base pairs). The respective command line for a distance threshold of 1% of the total region length, i.e., 1,000 bp, is shown below.

    $ ./RAiSD -n test_run_success_rate -I d1/msselection1.out -L 100000 -T 50000 -d 1000
    
Note the additional output lines in RAiSD_Info file, reporting success rate. The results show that 62.6% of the runs report locations within 1,000 bp from the known selection target (location 50,000).

    SUCCESS RATE (Distance 1000)
    mu-VAR	0.692
    mu-SFS	0.022
    mu-LD	0.019
    MuStat	0.626

Evaluating Sensitivity (True Positive Rate)
-------------------------------------------

The -k and -l parameters can be used to direct the tool to report sensitivity. This requires to conduct a run on neutral data first and sort all per-set best scores in order to define a threshold for a given False Positive Rate (FPR). RAiSD does this automatically when neutral data are processed, with the use of the -k parameter, providing an FPR. The following command line illustrates this for an FPR of 5%.

    $ ./RAiSD -n test_run_fpr -I d1/msneutral1.out -L 100000 -k 0.05
    
Note the additional lines that report the respective threshold for the chosen FPR value.

    SORTED DATA (FPR 0.050000)
    Size			1000
    Highest Score		0.000450000
    Lowest Score		0.000089600
    FPR Threshold		0.000221867
    Threshold Location	50
    
Thereafter, the FPR Threshold value can be given to RAiSD when selection data are processed (via the -l parameter) as shown below.

    $ ./RAiSD -n test_run_tpr -I d1/msselection1.out -L 100000 -l 0.000221867
    
This will report the respective TPR value as shown in the additional lines in RAiSD_Info file as shown below.

    SCORE COUNT (Threshold 0.000222)
    TPR	    0.995000
    
These results demonstrate a TPR of 99.5% when d1 is analyzed.

Generating μ-statistic plots
----------------------------

The -P parameter can be used to generate a set of four plots in a single PDF file per set of SNPs in the input dataset. This option requires R (https://www.r-project.org/) to be installed and Rscript to be in the default $PATH. By default, -P activates the generation of a separate report per set of SNPs (-s option), the removal of the set separator symbol from each report (-t option), and the inclusion of additional information in each report (-R option). The following command shows the use of -P for the test run.

    $ ./RAiSD -n test_run -I d1/msselection1.out -L 100000 -P
    
This command will generate RAiSD_Plot PDF files with the run name (provided via "-n") followed by the SNP set index (or name in VCF format) preceding the file extension.

RAiSD_Plot.test_run.0.pdf

An example of such file can be found here: http://139.91.162.50/raisd_plots/examples/RAiSD_Plot.test_run.1.pdf

Processing VCF (ANGSD version)
------------------------------

As of version 1.7, RAiSD can process ANGSD (http://www.popgen.dk/angsd/index.php/ANGSD) VCF files. Such files (see a sample here: http://www.popgen.dk/angsd/index.php/Vcf) only include the GP and GL fields, rather than the GT field that is required by RAiSD. In the absence of GT, diploidy is assumed.  

Given a [GL:GP] entry [L1,L2,L3:P1,P2,P3], RAiSD constructs unphased genotype data on the fly as follows:  

    GT="./." for all-zero GL entries    
    GT="0/0" with probability P1   
    GT="0/1" with probability P2     
    GT="1/1" with probability P3        

Processing VCF in GZ file format
--------------------------------
Since version 1.8, RAiSD can process compressed VCF files in GZ file format. To activate this feature, compile the source code using the dedicated makefile MakefileZLIB as follows:

    $ make clean
    $ make -f MakefileZLIB 

    
The zlib library (https://zlib.net/) needs to be installed prior to compilation. In Ubuntu, the zlib library can be installed with the following command:

    $ sudo apt-get install zlib1g-dev


Generating RAiSD_SiteReport
---------------------------

The -D option can be used to generate the RAiSD_SiteReport, i.e., a single file that includes a line per set of SNPs (ms format) or per CHROM (VCF format), providing a breakdown of the dataset in terms of total number of sites, number of SNPs used in the analysis, and total number of discarded sites. The total number of discarded sites is further broken down into site groups based on the reason they were discarded (failed check).  

When the default missing-data strategy is used (-M 0), the total number of discarded sites includes:  

    a) sites dropped due to a failed "header" check (only for VCF), 
    b) sites dropped because of the MAF check, 
    c) sites with missing data, and 
    d) monomorphic sites. 
    
When any other missing-data strategy is used (-M 1,2, or 3), the total number of discarded sites includes:

    a) sites dropped due to a failed "header" check (only for VCF), 
    b) sites dropped because of the MAF check, and 
    c) potentially monomorphic sites with missing data, i.e., sites with missing data and no variation. 
    
When missing data are imputed per SNP (-M 1), the RAiSD_SiteReport additionally includes the total number of sites that have been imputed before applying the rest of the checks (minor allele frequency, monomorphic sites, etc).


Change log
----------

v1.0 (9/6/2017): first release

v1.1 (7/3/2018): MAF threshold option

v1.2 (28/3/2018): mbs format with -b

v1.3 (18/7/2018): -i to impute N per SNP, -a for rand seed

v1.4 (3/8/2018): -M to handle missing data, -O to show progress on the display device

v1.5 (4/8/2018): -R to include additional information in the report (reduced default to location and score)

v1.6 (3/9/2018): -P to create plots per set of SNPs using Rscript

v1.7 (2/10/2018): -y for ploidy, -D for site report, fixed a bug in the plotting routine

v1.8 (31/12/2018): MakefileZLIB to parse VCF files in gzip file format (requires the zlib library)

Support
-------

We provide support for RAiSD through our generic [Google group for Population Genetics](https://groups.google.com/forum/#!forum/popgen-support).
    
    




