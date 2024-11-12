# PostgreSQL

## 1. Functions

```sql
CREATE OR REPLACE FUNCTION meineFunktion(f1 INTEGER, f2 INTEGER) 
        RETURNS INTEGER  
        AS $$ 
        BEGIN  
           RETURN f1 * f2; 
        END; 
        $$ LANGUAGE plpgsql;
```


