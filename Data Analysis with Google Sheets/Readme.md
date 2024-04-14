# Gsheet Formula

`UNIQUE(A1:A15)`
Return unique value  

```
COUNTUNIQUE(A1:A15)
```
Count unique value  

```
VLOOKUP(target_cell, lookup_table, lookup_col, is_sorted) 
VLOOKUP (C2, category!$A$2:$B$11,2,false)
```
$ -> inform sheet always look for the column  

`SUMIF(ref_col, ref_cell, value_col)`
SUMIF(H2:H115 (col with category), J1 (Rent, condition cell), D2:D115 (value col)  

`SUMIFS(val_col, cri_col1, criterion1, cri_col2, criterion2)`
Multiple conditions  

`COUNTIF(count_col, criterion)`
Count row which fulfill criteria  

`MONTH(col)`
Get month  

`IF(logical_expression, if true, else)`
If condition statement  

`LEFT(cell, num)`
Return the num of string start from left  

`FIND(‘a’,cell)`
Find the location of char in string  

`MID(cell, start,end)`
Return the char fulfill the condition  

`INDEX(condition,RANGE)`
Return index of selected condition  

`MATCH(RANGE, condition1, condition2)`
Return match of index  

`FILTER(RANGE,CONDITION = ‘a’)`
Return all in range that fulfill condition  

`TEXTJOIN(delimiter,if empty,range)`
Join the text with delimiter  
