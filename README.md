# MD&A text from 10-K filings

This repository contains an "index" file that can be used to process the raw MD&A data parsed [using this code](https://github.com/apodobytko/10K-MDA-Section):   

If you don't want to run that code yourself -- it takes days -- then you can follow the instructions below to get MD&A data.  The resulting data will be `(cik, filing_date, MD&A_text)`.  Alternatively, you can use the index file as the set of links to all the 10-Ks that were used to create the data. 

## How to use the index

- load the csv [index file](LINK)
- the columns `filing` is a mapping to the raw text files associated with the 10-K scrape.   So for row 6 is that file, the respective txt file is `5.txt`
- email Mike Ewens -- mewens@caltech.edu -- for a link to the big zip file of txt files (they are too big for Github)
- put those files in a folder with your code
- grab the `CIK` and dates that you need from the index and write some code in your language of choice to get the text files.  Example below for Stata

## Do file example

```
* Make sure that the folder of text files is somewhere 
cd ~/Link/To/MDNA_text/

# Load the index
insheet using XXX.csv, comma clear

# 

```
