<img width="765" height="155" alt="image" src="https://github.com/user-attachments/assets/46c71b60-3c01-411c-9301-c8e3e9b2144f" />


| Task | Standard SQL | SQL Dialect (default=PostgreSQL)  | Excel |
| ------------- | ------------- | ------------- | ------------- |
| Conditional aggregation  | SUM(CASE WHEN retention = TRUE THEN 1 ELSE 0 END)  | COUNT(*) **FILTER** (WHERE retention = TRUE)  | =**COUNTIF**(A:A, TRUE)  |
| String concatenation - Horizontal  | **CONCAT**(A, ' - ', B)  | A2 \|\| " - " \|\| B2  | =A2 **&** " - " & B2  |
| String concatenation - Vertical  | **STRING_AGG**(column, ', ')  | GROUP_CONCAT (MySQL only)  | TEXTJOIN  |
| Convert timestamp to text/string  | DATE_FORMAT(date, '%Y-%m')  | TO_CHAR(date, 'YYYY-MM')  | =TEXT(A2,"yyyy-mm-dd")  |
| Conditional Logic  | **CASE** WHEN A>5 THEN 'High' ELSE 'Low' END  |   | =**IF**(A2>5, "High", "Low")  |
| Look up across tables  | **JOIN** (_or subquery_)  |   | XLOOKUP/VLOOKUP  |
| Wildcard (any number of characters)  | WHERE Singer LIKE **'S%'**  |   | =COUNTIF(Singer,**"S\*"**)  |
| Wildcard (exactly one character)  | WHERE Singer LIKE **'_____'**  |   | =COUNTIF(Singer,**"?????"**)  |
| Substring  | **SUBSTR**(A, 4, 2)  |   | =**MID**(A2, 4, 2)  |
| Average  | **AVG**  |   | =**AVERAGE**(A:A)  |
| Rounding (same)  | ROUND(col, 2)  |   | =ROUND(A:A, 2)  |
| Remove duplicates  | DISTINCT  |   | Data ribbon → Remove Duplicates  |
| Sorting  | ORDER BY  |   | Sort A→Z  |
| Pivot table  | SELECT Region, SUM(Revenue) GROUP BY Region  |   | "Sum Revenue by Region"  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |


## special note on
SQL can comfortably join tables with millions of rows when properly designed, whereas Excel XLOOKUP across multiple tables can get too memory-intensive and slow. For the same task, even Python's equivalent, pandas.merge(), has limitations because it operates in memory.

(Interestingly, Power Query in Excel does have a true Merge operation, which is much closer to SQL JOIN.)

Don't highlight syntax you almost never use. or intuitive
