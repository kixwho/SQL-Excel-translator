# SQL Excel translator

| Function | Standard SQL | PostgreSQL Specific  | Excel |
| ------------- | ------------- | ------------- | ------------- |
| Conditional aggregation  | SUM(CASE WHEN _retention = TRUE_ THEN 1 ELSE 0 END)  | COUNT(*) **FILTER** (WHERE _retention = TRUE_)  | =**COUNTIF**(A:A, TRUE)  |
| Horizontal string concatenation  | **CONCAT**(A, ' - ', B)  | A2 \|\| " - " \|\| B2  | =A2 **&** " - " & B2  |
| Vertical string concatenation  | **STRING_AGG**(column, ', ')  | GROUP_CONCAT (MySQL only)  | TEXTJOIN  |
| Convert timestamp to text/string  | DATE_FORMAT(date, '%Y-%m')  | TO_CHAR(date, 'YYYY-MM')  | =TEXT(A2,"yyyy-mm-dd")  |
| Conditional Logic  | **CASE** WHEN A>5 THEN 'High' ELSE 'Low' END  |   | =**IF**(A2>5, "High", "Low")  |
| Pivot table  | SELECT Region, SUM(Revenue) GROUP BY Region  |   | "Sum Revenue by Region"  |
| Wildcard (any number of characters)  | WHERE Singer LIKE **'S%'**  |   | =COUNTIF(Singer,**"S\*"**)  |
| Wildcard (exactly one character)  | WHERE Singer LIKE **'_____'**  |   | =COUNTIF(Singer,**"?????"**)  |
| Substring  | **SUBSTR**(A, 4, 2)  |   | =**MID**(A2, 4, 2)  |
| Average  | **AVG**  |   | =**AVERAGE**(A:A)  |
| Rounding (same)  | ROUND(col, 2)  |   | =ROUND(A:A, 2)  |
| Sorting  | ORDER BY  |   | Sort A→Z  |
| Remove duplicates  | DISTINCT  |   | Data ribbon → Remove Duplicates  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
