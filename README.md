<img width="851" height="155" alt="image" src="https://github.com/user-attachments/assets/69284862-aa22-483d-8c23-a32dd799dd87" />

Need a shorter reference? [SQL cheatsheet](https://github.com/kixwho/PostgreSQL-cheatsheet)

If this repo helped you, please consider giving it a ⭐. It helps me showcase my work :)

| Task | SQL | SQL Dialect (default=PostgreSQL)  | Excel |
| ------------- | ------------- | ------------- | ------------- |
| **CALCULATIONS** |  |  |  |
| Moving average (e.g. 5-day) | **AVG**(price) **OVER** (ORDER BY date <br> ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) |  | =AVERAGE(A2:A6) → drag down |
| Percentage share of total | ROUND(<br> sales / **SUM**(sales) **OVER ()** *100, 2) |  | =[@Sales]/SUM(Table1[Sales]) → click % button |
| Common ratio  | ROUND(<br> col_A ***1.0** / **NULLIF**(col_B, 0), 2)  |   | =A2/B2 → IFERROR  |
| Average  | **AVG**(col)  |   | =**AVERAGE**(A:A)  |
| Rounding  | ROUND(col, 2)  |   | =ROUND(A2, 2)  |
| **DATA LOGIC** |  |  |  |
| Conditional aggregation  | SUM(CASE WHEN retention = TRUE THEN 1 ELSE 0 END)  | COUNT(*) **FILTER** (WHERE retention = TRUE)  | =**COUNTIF**(A:A, TRUE)  |
| Conditional Logic  | **CASE** WHEN A>5 THEN 'High' ELSE 'Low' END  |   | =**IF**(A2>5, "High", "Low")  |
| Find values missing from another table (**anti-join**\*)  | **WHERE NOT EXISTS** (SELECT 1 FROM table_2 WHERE table_1.id = table_2.id) |   | Manual filtering  |
| Find duplicates (**self-join**) | FROM customers c1 <br> JOIN customers c2 <br> ON c1.email=c2.email AND c1.id>c2.id | FROM customers c1 <br> WHERE EXISTS <br> (SELECT 1 FROM customers c2 WHERE c1.email=c2.email AND c1.id>c2.id) | Conditional Formatting → <br> Highlight Cells Rules → <br> Duplicate Values |
| [Look up across tables](#-a-special-note-on-excel-lookup-and-sql-join)  | JOIN (or subquery) [⚠️ Common trap](#quick-help-for-join-fanout)  |   | XLOOKUP/VLOOKUP  |
| Latest record - <br> Subquery | **WHERE** (id, date) **IN** <br> (SELECT id, MAX(date) FROM orders GROUP BY id) |  | Helper table → lookup |
| **DATE & TIME** |  |  |  |
| Latest record - <br> Window function | **ROW_NUMBER() OVER** (PARTITION BY id ORDER BY date DESC) → WHERE rn = 1 |  | Sort by id + date → remove duplicates by id |
| Convert timestamp to text/string  | DATE_FORMAT(date, '%Y-%m')  | TO_CHAR(date, 'YYYY-MM')  | =TEXT(A2,"yyyy-mm-dd")  |
| Date arithmetic | date_col + INTERVAL 1 DAY | date_col + INTERVAL '1 DAY' | =A2+1 |
| **TEXT OPERATIONS**  |   |   |
| String concatenation - Horizontal  | **CONCAT**(first_name, ' ', last_name)  | first_name \|\| ' ' \|\| last_name  | =A2 **&** " - " & B2  |
| String concatenation - Vertical  | **STRING_AGG**(col, ', ')  | GROUP_CONCAT (MySQL only)  | TEXTJOIN  |
| Wildcard (any number of characters)  | WHERE Singer LIKE **'S%'**  |   | =COUNTIF(Singer,**"S\*"**)  |
| Wildcard (exactly one character)  | WHERE Singer LIKE **'_____'**  |   | =COUNTIF(Singer,**"?????"**)  |
| Case conversion | **UPPER**(SUBSTRING(name, 1, 1) <br> **LOWER**(email) | INITCAP (Capitalizes 1st letter of every word) | =UPPER(A2) <br> 	=LOWER(A2) <br> =PROPER(A2) |
| Substring  | **SUBSTRING**(col_A, 4, 2)  |   | =**MID**(A2, 4, 2)  |
| **DATA ORGANIZATION** |  |  |  |
| Filter for customers with only one order | GROUP BY customer_id <br> **HAVING** COUNT(order_id) = 1 |  | Helper column → Filter |
| Remove duplicates  | DISTINCT  |   | Data ribbon → Remove Duplicates  |
| Sorting  | ORDER BY  |   | Sort A→Z  |
| Pivot table  | SELECT Region, SUM(Revenue) <br> GROUP BY Region  |   | "Sum Revenue by Region"  |

\* Unlike Excel blanks, SQL NULLs can affect comparison logic. Anti-joins avoid these issues and are robust to NULLs.

<br>

<img align="left" width="50" height="50" alt="image" src="https://github.com/user-attachments/assets/cddb5430-c859-47e4-91b6-56028b886b46" />

## Quick Help for JOIN Fanout
|  | SQL JOIN | Excel |
| ------------- | ------------- | ------------- |
| Behavior  | Unexpected row multiplication after JOIN  | Lookup functions assume single, unique match  |
| Visual Clues (use the query below to isolate problematic keys)  | Nearly identical rows clustered right next to each other  |   |
| Fix  | Match table granularity. Composite-key tables (e.g. transaction records) are especially vulnerable  |   |
| Failure Mode  | **No error, valid-looking wrong answer**  | Errors such as #SPILL!  |
| Habit for Success  | Checking visually not sustainable. Need to understand table logic  | Often working visually, shape mismatch immediately visible  |

<br>
    
        -- Practical fanout debugging
        
        SELECT join_key, COUNT(*) AS row_ct
        FROM joined_table
        GROUP BY join_key
        HAVING COUNT(*) > 10
        ORDER BY row_ct DESC;

- [ ] Inspect the most duplicated keys first
- [ ] Repeated clusters → strong clue that JOIN may have fanned out
- [ ] Confirm by checking table grain and **key uniqueness**

Excel users are familiar with #SPILL! errors: a formula returns more results than expected, and Excel forces you to resolve the shape mismatch. SQL behaves differently. JOINs can silently expand rows because returning multiple matches is a perfectly valid relational operation. Subqueries are not immune, although they make it harder to fanout...

Ultimately an analyst's responsibility shifts from fixing errors to understanding table relationships.

[My repo on Fanout](https://github.com/kixwho/Fanout)

## 🤔 A special note on Excel Lookup and SQL JOIN
Intuitively, Excel's Lookup functions are actually closer in feeling to a SQL subquery. The underlying logic is almost identical. A formula such as

    =VLOOKUP(H18, price_lookup, 2, False)

maps directly to a query with nested structure:

<img width="372" height="162" alt="image" src="https://github.com/user-attachments/assets/b3562642-b840-4af7-8403-140761972341" />

<p>

But I think the most fitting translation for a lookup is JOIN because although the approaches differ, they ultimately perform the **same pillar task in practice**. After all, in a typical JOIN query, you'd select only the columns that are needed anyway. For this task, JOINs scale more naturally and remain readable even as the number of tables grows.

Speaking of powerful, SQL users will appreciate how a properly designed JOIN can comfortably bring together tables with millions of rows! As essential as Excel Lookups are, cross-referencing on the same scale is too memory-intensive and slow. Even Python's equivalent, pandas.merge(), has limitations because it also operates in memory.
