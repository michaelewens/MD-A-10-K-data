# MD&A text from 10-K filings

This repository contains an "index" file that can be used to process the raw MD&A data parsed [using this code](https://github.com/apodobytko/10K-MDA-Section).[[1]]   After following the simple instructions below, you can have your own panel database of public firm MD&A text.

If you don't want to run the scraper code yourself -- it takes days -- then you can follow the instructions below to get MD&A data.   The basic idea is to have the local, already-scraped MD&A sections as text files and grab the ones that you need using the index file and your programming language of choice. The resulting data will be `(File,CompanyName,CIK,SIC,ReportDate,Section,Extracted_info)`.  

Note that this repository does not actually have the `txt` files of MD&A text.  See the instructions below for the temporary solution to that problem.

Please cite Ewens, Peters and Wang (2019) work "[Acquisition prices and the measurement of intangible capital](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3287437)" if you use the data and make sure to check the license rules.  

## How to use the index file to build your data

- load the txt [index file](https://github.com/michaelewens/md_n_a_10K/blob/master/download_log_filelist.txt).  This file is a simple list of the file names of MD&A text from the original download.  Note that you could probably just get the full set of statements, save the output of `ls` or similar to a variable and loop over the files.  
- the column `Filer` is a mapping to the raw text files associated with the 10-K scrape.   So for row 6 in that file, the respective txt file is `119173.txt`
- email Mike Ewens -- mewens@caltech.edu -- for a link to the big zip file of txt files (they are too big for Github).  The file will be posted somewhere soon.
- put those files in a folder with your code
- use the Python script -- or your code of choice -- to loop through the text files and grab what you need.  The text files have headers (unfortunately, sometimes more than one because data) like this:

```
<HEADER>
COMPANY NAME: SPIRE INC
CIK: 0001126956
SIC: 4924
FORM TYPE: 10-K
REPORT PERIOD END DATE: 20160930
FILE DATE: 20161115
</HEADER>

```

So your program can get all the identifying information once the `txt` is loaded.  

## Do file example

```
* Make sure that the folder of text files is somewhere 
cd ~/Link/To/MDNA_text/

* Load the index (you have downloaded it already)
insheet using "download_log_filelist.txt", tab clear

* Filenames (this is the "trick")
tostring filer, force replace
gen filename = filer + ".txt"

**** DO WHAT YOU WANT HERE
  * Now you have a list of file names associated with each row of interest

global importfolder "~/Link/To/MDNA_text/files"
global filelist : dir "$importfolder"  *.txt
 * Now we have the file list to do your magic on
 
 * Regex to grab CIK / Date / etc (the Python code below has examples)
 
* You could import each filename in a loop and save as a variable...thay can get big VERY fast, so be careful
```

## Python code

The [original scraper](https://github.com/apodobytko/10K-MDA-Section) has an "MDA Cleaner and Tone Calculator.py" script that can be modified to your liking.  The main pieces are `with open(download, 'r') as txtfile:` and the `NEGATIVE` or `POSITIVE` definitions.  This repository also has the [python script](https://github.com/michaelewens/md_n_a_10K/blob/master/example_procesing.py) for Ewens, Peters and Wang (2019), which was interested in counts of words, rather than tone.  

Here are the keywords that we searched for

```
sayings=["the following discussion","this discussion and analysis","should be read in conjunction", "should be read together with", "the following managements discussion and analysis"]
acq=["proprietary","intellectual","patent","trademark","intangible","technology"]    
personnel = ["key personnel", "personnel","talented employee", "key talent"]
```

## Bibtex citation

```
@article{ewensPetersWang2018,
 title={Acquisition prices and the measurement of intangible capital},
 author={Ewens, Michael and Peters, Ryan and Wang, Sean},
 journal={Working Paper}
 year={2018}
 }
```

[[1]]This is a fork of [another project](https://github.com/rflugum/10K-MDA-Section) that we had trouble getting to fuction.
