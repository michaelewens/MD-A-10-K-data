# MD&A text from 10-K filings

This repository contains an "index" file that can be used to process the raw MD&A data parsed [using this code](https://github.com/apodobytko/10K-MDA-Section). 

Ideally, we would have provided a file or SQL dump with the structure `(cik, date, MD&A_text)`.  But that file would have been enormous to store (that last column can be huge). 

If you don't want to run the scraper code yourself -- it takes days -- then you can follow the instructions below to get MD&A data.   The basic idea is to have the local, already-scraped MD&A sections and grab the ones that you need using the index file and your programming language of choice. The resulting data will be `(cik, filing_date, MD&A_text)`.  Alternatively, you can use the index file as the set of links to all the 10-Ks that were used to create the data. 

Please cite Ewens, Peters and Wang (2019) work "[Acquisition prices and the measurement of intangible capital](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3287437)" if you use the data and make sure to check the license rules.  

## How to use the index file to build your data

- load the csv [index file](https://github.com/michaelewens/md_n_a_10K/blob/master/downloadindex.csv)
- the columns `filing` is a mapping to the raw text files associated with the 10-K scrape.   So for row 6 is that file, the respective txt file is `5.txt`
- email Mike Ewens -- mewens@caltech.edu -- for a link to the big zip file of txt files (they are too big for Github)
- put those files in a folder with your code
- grab the `CIK` and dates that you need from the index and write some code in your language of choice to get the text files.  Example below for Stata.

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
  * You could import each filename in a loop and save as a variable...thay can get big VERY fast, so be careful
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
