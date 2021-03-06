# Incorporate non-compete law changes from Ewens and Marx (2017, RFS)

[Ewens](https://ewens.caltech.edu/research/) and [Marx](http://mattmarx.com/) ([2017](https://academic.oup.com/rfs/article/31/4/1532/4604800?guestAccessKey=c295e5d3-ff6e-4257-8dea-4d3167a9a7f5)) use a series of state-level law changes in the U.S. from 1995--2016 to study the impact of founder replacement on startup outcomes.   The data and code below allow others to incorporate the law changes into their research.

## CSV file (data)

[The csv file](https://github.com/michaelewens/noncompete_laws/blob/master/ncLawChanges.csv) contains four variables: `year`,`state`, `cncChange` and `note_LawChange` defined as:
* `year`: the year of the law change.  The best year to assume is _treated_ years are those after this year.
* `state`: two-letter abbreviation of state.
* `cncChange`: -1 or 1 which represents the incremental change in the strength of the law change.  -1 indicates a weakening, 1 indicates a strengthening, relative to the previous year.
* `note_LawChange`: string containing notes about the change and whether it was used in the original study.

## Stata code

[The do file](createCNCvariables.do) can be run in any program as `do createCNCvariables.do` after you update the two global variables in the file.  The simple file creates the variable `cncChange` and `note_LawChange` for state-years with non-compete law changes.   See above for variable definitions.

Alternatively, you can take the [csv file](https://github.com/michaelewens/noncompete_laws/blob/master/ncLawChanges.csv) and merge it into your data:

```Stata
* your data is temp.dta
* Load up the csv and save as local tempfile
insheet using "stateYear.csv", comma clear
tempfile state_years
save `state_years'
* Load up your main data
use temp, clear
* Merge on the CNC law changes
merge m:1 state year using `state_years', keep(1 3) nogen
```

## R code

[The R file](https://github.com/michaelewens/noncompete_laws/blob/master/createCNCvariables.R) can be loaded with `source` and will add two new variables to your data `cncChange` and `note_LawChange`.

Alternatively, you can take the [csv file](https://github.com/michaelewens/noncompete_laws/blob/master/ncLawChanges.csv) and merge it into your data:

```R
# Load the csv file
state_years <- read.csv("stateYear.csv", stringsAsFactors=FALSE)
# your data is currentData 
# Merge onto the current data.  Result is newData with new columns.
newData<- merge(state_years, currentData, by.x=c("state", "year"), by.y=c("state_yourData", "year_yourData"), all.y = TRUE)
```

## Python code

Some sample code to import the [csv file](https://github.com/michaelewens/noncompete_laws/blob/master/ncLawChanges.csv) and merge onto your existing data:

```python
import numpy as np
import pandas as pd

# Assume that you have currentData as your data with 'state' and 'year'
stateYear = read_csv("stateYear.csv")
newData = pd.merge(currentData,
                 stateYear,
                 on=['state','year'])
```

## Citation

```Latex
@article{ewensMarx2017founder,
  title={Founder replacement and startup performance},
  author={Ewens, Michael and Marx, Matt},
  journal={The Review of Financial Studies},
  volume={31},
  number={4},
  pages={1532--1565},
  year={2017},
  publisher={Oxford University Press}
}
```

Ewens, Michael, and Matt Marx. "[Founder replacement and startup performance](https://academic.oup.com/rfs/article/31/4/1532/4604800?guestAccessKey=c295e5d3-ff6e-4257-8dea-4d3167a9a7f5)." _The Review of Financial Studies_ 31.4 (2017): 1532-1565.
