# nearmrg: Nearest Match Merging for Stata

`nearmrg` is a Stata command that performs nearest match merging of two datasets based on a numeric variable. It is ideal for matching data where keys are not exact, such as dates, timestamps, or measurements with rounding differences.

## Features
- **Nearest Neighbor Matching**: Finds the closest value in the using dataset for every master observation.
- **Directional Matching**: Options for `lower` (nearest value ≤ master) or `upper` (nearest value ≥ master).
- **Exact Match Subsets**: Match within categories using an exact match `varlist`.
- **Distance Calculation**: Optionally save the absolute difference between matched values.
- **Flexible Naming**: Supports different variable names in master and using datasets.
- **Standard Merge Integration**: Passes all standard `merge` options (like `keepusing`, `update`, `replace`).

## Installation
You can install `nearmrg` directly from GitHub in Stata:
```stata
net install nearmrg, from("https://raw.githubusercontent.com/ericbooth/nearmrg-stata/master/")
```

## Syntax
```stata
nearmrg [exact_match_vars] using filename, nearvar(master_var [=using_var]) [options]
```

### Key Options
- `nearvar()`: The numeric variable to match on (Required).
- `limit()`: Maximum absolute distance allowed for a match.
- `lower` / `upper`: Restrict match direction.
- `distance(newvar)`: Create a variable with the match distance.
- `genmatch(newvar)`: Create a variable with the matched value from the using data.
- `type()`: Merge type (default is `m:1`).
- `nokeep`: Drop master observations that don't find a match.

## Examples

### 1. Match on Nearest Price
```stata
sysuse auto, clear
tempfile subset
preserve
    keep if _n <= 20
    replace price = price + runiform(-50, 50)
    save `subset'
restore

nearmrg using `subset', nearvar(price) genmatch(price_matched) distance(diff)
```

### 2. Match on Nearest Date within Individuals
```stata
* Suppose master has 'date' and using has 'event_date'
nearmrg person_id using "events.dta", nearvar(date = event_date) lower limit(30)
```

## Authors
**Eric A. Booth**  
eric.a.booth@gmail.com  
[https://github.com/ericbooth/nearmrg-stata](https://github.com/ericbooth/nearmrg-stata)

*Original package co-authored by Michael Blasnik and Katherine Smith (2003). Eric Booth took over and rewrote the package starting in 2010.*
