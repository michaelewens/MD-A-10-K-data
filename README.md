# MD&A text from 10-K filings

[![DOI](https://data.caltech.edu/badge/192034546.svg)](https://data.caltech.edu/badge/latestdoi/192034546)

This repository contains an "index" file that can be used to process the raw MD&A data parsed [using this code](https://github.com/apodobytko/10K-MDA-Section), which is a fork/update of [this repo](https://github.com/rflugum/10K-MDA-Section).   After following the instructions below, you can have your own panel database of public firm MD&A text.  

As an example of how this data can be used, Ewens, Peters and Wang (2024) searched all the MD&A sections for references to personnel or human capital risk (following Eisfeldt and Papanikolaou, 2013) to sort public firms and compare their organizational capital stocks:

![Org. stock sorts](https://github.com/michaelewens/MD-A-10-K-data/blob/5efc8cfb9105de493c6b97d49433c7103066eb68/example_analysis2023.png)

If you don't want to run the scraper code yourself ([this](https://github.com/apodobytko/10K-MDA-Section) or [this](https://github.com/rflugum/10K-MDA-Section))-- it takes days -- then you can follow the instructions below to get the raw MD&A data gathered for [Ewens, Peters and Wang (2023)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3287437).   The basic idea is to have the local, already-scraped MD&A sections as text files and grab the ones that you need using the index file and your programming language of choice. The resulting data will be `(File,CompanyName,CIK,SIC,ReportDate,Section,Extracted_info)`.  

Note that this repository does not actually have the `txt` files of MD&A text.  See the instructions below for the temporary solution to that problem.

Please cite Ewens, Peters and Wang (2023) work "[Measuring Intangible Capital with Market Prices](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3287437)" if you use the data,  [Berns, Bick, Flugum and Houston](https://sites.google.com/site/ryanflugum/research/tone-changes-and-m-a) if you use the scraper and make sure to check the license rules for either.  

## How to use the index file to build your data

- load the txt [index file](https://github.com/michaelewens/MD-A-10-K-data/blob/master/mda_sections.csv).  This file is a simple list of the file names of MD&A text from the original download.  Note that you could probably just get the full set of statements, save the output of `ls` or similar to a variable and loop over the files.  
- the column `Filer` is a mapping to the raw text files associated with the 10-K scrape.   So for row 6 in that file, the respective txt file is `119173.txt`
- download the [txt files from here](https://doi.org/10.22002/D1.1249) (they are too big for GitHub).   For space reasons, the files were split into folders.  They will have to be combined into one folder if the code below is to work.
- We ask that this data not be forwarded on to others.  This ensures that everyone receives the latest version of the data that's available.
- use the [Python script](https://github.com/michaelewens/MD-A-10K/blob/master/example_procesing.py) -- or your code of choice -- to loop through the text files and grab what you need.  The text files have headers (unfortunately, sometimes more than one because data) like this:

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

So with the `txt` file loaded, all the identifying information is available.  

## Do file example

```
* Make sure that the folder of text files is somewhere 
cd ~/Link/To/MDNA_text/

* Load the index (you have downloaded it already)
insheet using "mda_sections.csv", tab clear

** Filer == variable of interest!

**** DO WHAT YOU WANT HERE
  * Now you have a list of file names associated with each row of interest

global importfolder "~/Link/To/MDNA_text/files"
global filelist : dir "$importfolder"  *.txt
 * Now we have the file list to do your magic on
 
 * Regex to grab CIK / Date / etc (the Python code below has examples)
 
* You could import each filename in a loop and save as a variable...thay can get big VERY fast, so be careful
```

## Python code

The [original scraper](https://github.com/apodobytko/10K-MDA-Section) has an "MDA Cleaner and Tone Calculator.py" script that can be modified to your liking.  The main pieces are `with open(download, 'r') as txtfile:` and the `NEGATIVE` or `POSITIVE` definitions.  This repository also has the [python script](https://github.com/michaelewens/md_n_a_10K/blob/master/example_procesing.py) for Ewens, Peters and Wang (2024, citation below), which was interested in counts of words, rather than tone.  

Here are the keywords that we searched for

```
sayings=["the following discussion","this discussion and analysis","should be read in conjunction", "should be read together with", "the following managements discussion and analysis"]
acq=["proprietary","intellectual","patent","trademark","intangible","technology"]    
personnel = ["key personnel", "personnel","talented employee", "key talent"]
```

## Bibtex citation

```
@article{ewensPetersWang2024,
 title={Measuring Intangible Capital with Market Prices },
 author={Ewens, Michael and Peters, Ryan and Wang, Sean},
 journal={Management Science}
 year={2024}
 }
```
