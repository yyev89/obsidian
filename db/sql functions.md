return next day of week after today:
```sql
CREATE OR REPLACE FUNCTION tomorrow_dow() RETURNS INT AS
$$
DECLARE result INT;
BEGIN
	SELECT EXTRACT(isodow FROM NOW() + INTERVAL '1 day')
	INTO result;
	RETURN result;
END
$$
LANGUAGE PLPGSQL;
--usage: SELECT tomorrow_dow()
```