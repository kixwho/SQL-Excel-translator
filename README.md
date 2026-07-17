<img width="656" height="155" alt="image" src="https://github.com/user-attachments/assets/ce29d9ff-ad8b-43a3-828b-0f88c14ba2e7" />

**Guide**
* SQL → start from TOP
* Excel → start from BOTTOM

| Task | Standard SQL | SQL Dialect (default=PostgreSQL)  | Excel |
| ------------- | ------------- | ------------- | ------------- |
| Conditional aggregation  | SUM(CASE WHEN retention = TRUE THEN 1 ELSE 0 END)  | COUNT(*) **FILTER** (WHERE retention = TRUE)  | =**COUNTIF**(A:A, TRUE)  |
| String concatenation - Horizontal  | **CONCAT**(A, ' - ', B)  | A \|\| " - " \|\| B  | =A2 **&** " - " & B2  |
| String concatenation - Vertical  | **STRING_AGG**(col, ', ')  | GROUP_CONCAT (MySQL only)  | TEXTJOIN  |
| Convert timestamp to text/string  | DATE_FORMAT(date, '%Y-%m')  | TO_CHAR(date, 'YYYY-MM')  | =TEXT(A2,"yyyy-mm-dd")  |
| Conditional Logic  | **CASE** WHEN A>5 THEN 'High' ELSE 'Low' END  |   | =**IF**(A2>5, "High", "Low")  |
| Look up across tables  | **JOIN** (_or subquery_)  |   | XLOOKUP/VLOOKUP  |
| Wildcard (any number of characters)  | WHERE Singer LIKE **'S%'**  |   | =COUNTIF(Singer,**"S\*"**)  |
| Wildcard (exactly one character)  | WHERE Singer LIKE **'_____'**  |   | =COUNTIF(Singer,**"?????"**)  |
| Substring  | **SUBSTR**(A, 4, 2)  |   | =**MID**(A2, 4, 2)  |
| Average  | **AVG**(col)  |   | =**AVERAGE**(A:A)  |
| Rounding (same)  | ROUND(col, 2)  |   | =ROUND(A2, 2)  |
| Remove duplicates  | DISTINCT  |   | Data ribbon → Remove Duplicates  |
| Sorting  | ORDER BY  |   | Sort A→Z  |
| Pivot table  | SELECT Region, SUM(Revenue) GROUP BY Region  |   | "Sum Revenue by Region"  |


## 🤔 A special note on Excel Lookup and SQL JOIN
Intuitively, Excel's Lookup functions are actually closer in feeling to a SQL subquery. The underlying logic is almost identical. A formula such as

    =VLOOKUP(H18, price_lookup, 2, False)

maps directly to a query with nested structure:

<img width="372" height="162" alt="image" src="https://github.com/user-attachments/assets/b3562642-b840-4af7-8403-140761972341" />

<p>

But I think the most fitting translation for a lookup is JOIN because although the approaches differ, they ultimately perform the **same pillar task in practice**. After all, in a typical JOIN query, you'd select only the columns that are needed anyway. For this task, JOINs scale more naturally and remain readable even as the number of tables grows.

Speaking of powerful, SQL users will appreciate how a properly designed JOIN can comfortably bring together tables with millions of rows! As essential as Excel Lookups are, cross-referencing on the same scale is too memory-intensive and slow. Even Python's equivalent, pandas.merge(), has limitations because it also operates in memory.
