# PostgreSQL-translator

| Function | PostgreSQL Operator  | MySQL Operator | stuff |
| ------------- | ------------- | ------------- | ------------- |
| Conditional aggregation  | COUNT(*) **FILTER** (WHERE _retention = TRUE_)  | SUM(CASE WHEN _retention = TRUE_ THEN 1 ELSE 0 END)  | Content Cell  |
| Horizontal string concatenation  | **\|\|**  | CONCAT  | Content Cell  |
| Vertical string concatenation  | **STRING_AGG**(column, ', ')  | GROUP_CONCAT  | Content Cell  |
| Convert timestamp to text/string  | TO_CHAR(date, 'YYYY-MM')  | DATE_FORMAT(date, '%Y-%m')  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  | Content Cell  |
