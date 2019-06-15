# MD&A text from 10-K filings

This repository contains an "index" file that can be used to process the raw MD&A data parsed [using this code](https://github.com/apodobytko/10K-MDA-Section).   After following the simple instructions below, you can have your own panel database of public firm MD&A text.

If you don't want to run the scraper code yourself -- it takes days -- then you can follow the instructions below to get MD&A data.   The basic idea is to have the local, already-scraped MD&A sections as text files and grab the ones that you need using the index file and your programming language of choice. The resulting data will be `(cik, filing_date, MD&A_text)`.  

Note that this repository does not actually have the `txt` files of MD&A text.  See the instructions below for the temporary solution to that problem.

Please cite Ewens, Peters and Wang (2019) work "[Acquisition prices and the measurement of intangible capital](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3287437)" if you use the data and make sure to check the license rules.  

## How to use the index file to build your data

- load the csv [index file](https://github.com/michaelewens/md_n_a_10K/blob/master/downloadindex.csv)
- the column `filing` is a mapping to the raw text files associated with the 10-K scrape.   So for row 6 is that file, the respective txt file is `5.txt`
- email Mike Ewens -- mewens@caltech.edu -- for a link to the big zip file of txt files (they are too big for Github).  If you have any suggestions of good (affordable) places to host the 10gb of data, please let me know.
- put those files in a folder with your code
- grab the `CIK` and dates that you need from the index and write some code in your language of choice to get the text files.  Example below for Stata and a reference to the Python code from the original scraper.

## Do file example

```
* Make sure that the folder of text files is somewhere 
cd ~/Link/To/MDNA_text/

* Fake data on my CIKs, years of interest
 * This comes from your project (e.g. all financial firms in 1998 or all widgets producers based in Maine)
insheet using "my_cik_years.csv", comma clear
tempfile my_comps
save `my_comps'

* Load the index (you have downloaded it already)
insheet using "downloadindex.csv", comma clear

* Create the date
gen fd = date(filingdate, "DMY", 2018)
gen year = year(fd)

*  Keep the ones that we want
merge m:1 cik year using `my_comp', keep(3) nogen

* Filenames (this is the "trick")
tostring filing, force replace
gen filename = filing + ".txt"

**** DO WHAT YOU WANT HERE
  * Now you have a list of file names associated with each row of interest

global importfolder "~/Link/To/MDNA_text/files"
global filelist : dir "$importfolder"  *.txt
 * Now we have the file list to do your magic on
 
* You could import each filename in a loop and save as a variable...thay can get big VERY fast, so be careful
```

## Python code

The [original scraper](https://github.com/apodobytko/10K-MDA-Section) has an "MDA Cleaner and Tone Calculator.py" script that can be modified to your liking.  The main pieces are `with open(download, 'r') as txtfile:` and the `NEGATIVE` or `POSITIVE` definitions.  This repository also has the python script for Ewens, Peters and Wang (2019), which was interested in counts of words, rather than tone.  

## Bibtex citation

```
@article{ewensPetersWang2018,
 title={Acquisition prices and the measurement of intangible capital},
 author={Ewens, Michael and Peters, Ryan and Wang, Sean},
 journal={Working Paper}
 year={2018}
 }
```
