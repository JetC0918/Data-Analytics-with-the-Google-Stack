# Gsheet Formula

Return unique value  
`UNIQUE(A1:A15)`

Count unique value  
`COUNTUNIQUE(A1:A15)`

$ -> inform sheet always look for the column  
`VLOOKUP(target_cell, lookup_table, lookup_col, is_sorted) 
VLOOKUP (C2, category!$A$2:$B$11,2,false)`

SUMIF(H2:H115 (col with category), J1 (Rent, condition cell), D2:D115 (value col)  
`SUMIF(ref_col, ref_cell, value_col)`

Multiple conditions  
`SUMIFS(val_col, cri_col1, criterion1, cri_col2, criterion2)`

Count row which fulfill criteria  
`COUNTIF(count_col, criterion)`

Get month  
`MONTH(col)`

If condition statement  
`IF(logical_expression, if true, else)`

Return the num of string start from left  
`LEFT(cell, num)`

Find the location of char in string  
`FIND(‘a’,cell)`

Return the char fulfill the condition  
`MID(cell, start,end)`

Return index of selected condition  
`INDEX(condition,RANGE)`

Return match of index  
`MATCH(RANGE, condition1, condition2)`

Return all in range that fulfill condition  
`FILTER(RANGE,CONDITION = ‘a’)`

Join the text with delimiter  
`TEXTJOIN(delimiter,if empty,range)`

