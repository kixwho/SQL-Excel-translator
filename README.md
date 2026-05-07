# PostgreSQL-translator

| PostgreSQL Operator  | MySQL Operator | stuff |
| ------------- | ------------- | ------------- |
| COUNT(*) **FILTER** (WHERE _retention = TRUE_)  | SUM(CASE WHEN _retention = TRUE_ THEN 1 ELSE 0 END)  | Content Cell  |
| **\|\|**  | CONCAT  | Content Cell  |
| **STRING_AGG**(column, ', ')  | GROUP_CONCAT  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  |
| Content Cell  | Content Cell  | Content Cell  |
