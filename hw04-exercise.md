---
title: "hw04-exercise"
output:
   html_document:
      self_contained: false
      keep_md: true
---

# Tidy data and joins

## Initialize the data

* Load the gapminder, tidyverse and knitr libraries:


```r
suppressPackageStartupMessages(library(tidyverse))
suppressPackageStartupMessages(library(gapminder))
suppressPackageStartupMessages(library(knitr))
suppressPackageStartupMessages(library(kableExtra))
```

* The kableExtra package gives new options for styling tables, but since they are in HTML, we won't get to see the effects in this .md file
* We will take a quick look at the data to *sanity check* that the data and variables appear as we expect:


```r
(head(gapminder))
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

* Everything looks as expected, so let's start exploring the data

## Data Re-shaping Prompt
*Activity 2: Make a tibble with one row per year and columns for life expectancy for two or more countries. Use kable() to make the table look pretty. Scatterplot life expectancy for one country against that of another.*

### Method
* Reduce the data subset using select and group_by
* Spread the countries across the columns
* Build a table and a scatterplot


```r
q1_table <- gapminder %>% #Create a tibble so we can re-use it later
  select(year, country, lifeExp) %>% #Reduce the data set size for faster processing
    group_by(year) %>% 
      spread(key = "country", value = "lifeExp") %>% 
        select("Canada", "Germany", "Brazil")# %>%
```

```
## Adding missing grouping variables: `year`
```

```r
q1_table %>% 
  kable(col.names = c("Year", "Canada", "Germany", "Brazil")) %>% #Use kable to enhance the output
    kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> Year </th>
   <th style="text-align:right;"> Canada </th>
   <th style="text-align:right;"> Germany </th>
   <th style="text-align:right;"> Brazil </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1952 </td>
   <td style="text-align:right;"> 68.750 </td>
   <td style="text-align:right;"> 67.500 </td>
   <td style="text-align:right;"> 50.917 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1957 </td>
   <td style="text-align:right;"> 69.960 </td>
   <td style="text-align:right;"> 69.100 </td>
   <td style="text-align:right;"> 53.285 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1962 </td>
   <td style="text-align:right;"> 71.300 </td>
   <td style="text-align:right;"> 70.300 </td>
   <td style="text-align:right;"> 55.665 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1967 </td>
   <td style="text-align:right;"> 72.130 </td>
   <td style="text-align:right;"> 70.800 </td>
   <td style="text-align:right;"> 57.632 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1972 </td>
   <td style="text-align:right;"> 72.880 </td>
   <td style="text-align:right;"> 71.000 </td>
   <td style="text-align:right;"> 59.504 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1977 </td>
   <td style="text-align:right;"> 74.210 </td>
   <td style="text-align:right;"> 72.500 </td>
   <td style="text-align:right;"> 61.489 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1982 </td>
   <td style="text-align:right;"> 75.760 </td>
   <td style="text-align:right;"> 73.800 </td>
   <td style="text-align:right;"> 63.336 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1987 </td>
   <td style="text-align:right;"> 76.860 </td>
   <td style="text-align:right;"> 74.847 </td>
   <td style="text-align:right;"> 65.205 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 77.950 </td>
   <td style="text-align:right;"> 76.070 </td>
   <td style="text-align:right;"> 67.057 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 78.610 </td>
   <td style="text-align:right;"> 77.340 </td>
   <td style="text-align:right;"> 69.388 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 79.770 </td>
   <td style="text-align:right;"> 78.670 </td>
   <td style="text-align:right;"> 71.006 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 80.653 </td>
   <td style="text-align:right;"> 79.406 </td>
   <td style="text-align:right;"> 72.390 </td>
  </tr>
</tbody>
</table>

```r
#Check that the data is a tibble
#Change my graph theme
```
#Plot


```r
q1_table %>%
  ggplot(aes(x = year, y = value, color = variable)) +
  geom_point(aes(y = Canada, col = "Canada"), size=5) +
  geom_line(aes(y = Canada, col = "Canada")) +
  geom_point(aes(y = Germany, col = "Germany"), size=5) +
  geom_line(aes(y = Germany, col = "Germany")) +
  geom_point(aes(y = Brazil, col = "Brazil"), size=5) +
  geom_line(aes(y = Brazil, col = "Brazil")) +
  #Add labels
  labs(title = "Life expectancy from 1952-2007, (Canada, Germany, Brazil)",
    x = "Year", y = "Life Expectancy",
    color = "Countries\n")
```

![](hw04-exercise_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

### Observations & Analyses
I didn't end up using gather() (which effectively does the opposite as spread.

## Join Prompt (join, merge, look up)
*Activity 2: Make your own cheatsheet; iterate between your data prep and your joining to make your explorations comprehensive and interesting. Demonstrate all the types of joins.*

### Method
* Create two tables with some overlapping columns and rows
* Explore different types of joins


```r
medieval_weapons <- "
name,        attack_type, wielding,       damage
shuriken,    ranged,      one-handed,     2       
flail,       melee,       one-handed,     4        
great_sword, melee,       two-handed,     8      
crossbow,    ranged,      one-handed,     2        
Excalibur,   melee,       one-handed,     6        
dagger,      melee,       one-handed,     3       
staff,       melee,       two-handed,     5       
"
medieval_weapons <- read_csv(medieval_weapons, skip = 1)

stun_bonus <- "
  wielding,   chance_to_stun,   stun_damage
  two-handed, 0.15,             10
"
stun_bonus <- read_csv(stun_bonus, skip = 1)
```

### Left and Right Joins


```r
lj <- left_join(medieval_weapons, stun_bonus, by = "wielding")
rj <- right_join(medieval_weapons, stun_bonus, by = "wielding")
kable(
  list(lj, rj),
  col.names = c("Weapon", "Attack Type", "Wielding Type", "Damage (HP)", "Chance to Stun", "Stun Damage (HP)"))
```

<table class="kable_wrapper">
<tbody>
  <tr>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Weapon </th>
   <th style="text-align:left;"> Attack Type </th>
   <th style="text-align:left;"> Wielding Type </th>
   <th style="text-align:right;"> Damage (HP) </th>
   <th style="text-align:right;"> Chance to Stun </th>
   <th style="text-align:right;"> Stun Damage (HP) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> shuriken </td>
   <td style="text-align:left;"> ranged </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> flail </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> great_sword </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> crossbow </td>
   <td style="text-align:left;"> ranged </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Excalibur </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dagger </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> staff </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
</tbody>
</table>

 </td>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Weapon </th>
   <th style="text-align:left;"> Attack Type </th>
   <th style="text-align:left;"> Wielding Type </th>
   <th style="text-align:right;"> Damage (HP) </th>
   <th style="text-align:right;"> Chance to Stun </th>
   <th style="text-align:right;"> Stun Damage (HP) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> great_sword </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> staff </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
</tbody>
</table>

 </td>
  </tr>
</tbody>
</table>

On the left is a left join, on the right is a right join.
### Inner and Full Joins


```r
ij <- inner_join(medieval_weapons, stun_bonus, by = "wielding") 
fj <- full_join(medieval_weapons, stun_bonus, by = "wielding") 
kable(
  list(ij, fj),
  col.names = c("Weapon", "Attack Type", "Wielding Type", "Damage (HP)", "Chance to Stun", "Stun Damage (HP)"))
```

<table class="kable_wrapper">
<tbody>
  <tr>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Weapon </th>
   <th style="text-align:left;"> Attack Type </th>
   <th style="text-align:left;"> Wielding Type </th>
   <th style="text-align:right;"> Damage (HP) </th>
   <th style="text-align:right;"> Chance to Stun </th>
   <th style="text-align:right;"> Stun Damage (HP) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> great_sword </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> staff </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
</tbody>
</table>

 </td>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Weapon </th>
   <th style="text-align:left;"> Attack Type </th>
   <th style="text-align:left;"> Wielding Type </th>
   <th style="text-align:right;"> Damage (HP) </th>
   <th style="text-align:right;"> Chance to Stun </th>
   <th style="text-align:right;"> Stun Damage (HP) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> shuriken </td>
   <td style="text-align:left;"> ranged </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> flail </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> great_sword </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> crossbow </td>
   <td style="text-align:left;"> ranged </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Excalibur </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dagger </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:left;"> staff </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
</tbody>
</table>

 </td>
  </tr>
</tbody>
</table>

### Semi and Anti Joins


```r
sj <- semi_join(medieval_weapons, stun_bonus, by = "wielding") 
aj <- anti_join(medieval_weapons, stun_bonus, by = "wielding") 
kable(
  list(sj, aj),
  col.names = c("Weapon", "Attack Type", "Wielding Type", "Damage (HP)"))
```

<table class="kable_wrapper">
<tbody>
  <tr>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Weapon </th>
   <th style="text-align:left;"> Attack Type </th>
   <th style="text-align:left;"> Wielding Type </th>
   <th style="text-align:right;"> Damage (HP) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> great_sword </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> staff </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> two-handed </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
</tbody>
</table>

 </td>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Weapon </th>
   <th style="text-align:left;"> Attack Type </th>
   <th style="text-align:left;"> Wielding Type </th>
   <th style="text-align:right;"> Damage (HP) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> shuriken </td>
   <td style="text-align:left;"> ranged </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> flail </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> crossbow </td>
   <td style="text-align:left;"> ranged </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Excalibur </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dagger </td>
   <td style="text-align:left;"> melee </td>
   <td style="text-align:left;"> one-handed </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
</tbody>
</table>

 </td>
  </tr>
</tbody>
</table>

*Activity 3: Explore the merge() and match() functions. Contrast and compare.*


```r
merge_test <- merge(medieval_weapons, stun_bonus, by = "wielding") %>% 
kable(col.names = c("Weapon", "Attack Type", "Wielding Type", "Damage (HP)", "Chance to Stun", "Stun Damage (HP)"))

match_test <- match(medieval_weapons, stun_bonus)
```
