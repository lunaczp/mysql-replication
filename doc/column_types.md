# column types

## 5.6.24

```
1 MYSQL_TYPE_DECIMAL
2 MYSQL_TYPE_TINY
3 MYSQL_TYPE_SHORT
4 MYSQL_TYPE_LONG
5 MYSQL_TYPE_FLOAT
6 MYSQL_TYPE_DOUBLE
7 MYSQL_TYPE_NULL
8 MYSQL_TYPE_TIMESTAMP
9 MYSQL_TYPE_LONGLONG
10 MYSQL_TYPE_INT24
11 MYSQL_TYPE_DATE
12 MYSQL_TYPE_TIME
13 MYSQL_TYPE_DATETIME
14 MYSQL_TYPE_YEAR
15 MYSQL_TYPE_NEWDATE
16 MYSQL_TYPE_VARCHAR
17 MYSQL_TYPE_BIT
18 MYSQL_TYPE_TIMESTAMP2
19 MYSQL_TYPE_DATETIME2
20 MYSQL_TYPE_TIME2
21 MYSQL_TYPE_NEWDECIMAL=246
22 MYSQL_TYPE_ENUM=247
23 MYSQL_TYPE_SET=248
24 MYSQL_TYPE_TINY_BLOB=249
25 MYSQL_TYPE_MEDIUM_BLOB=250
26 MYSQL_TYPE_LONG_BLOB=251
27 MYSQL_TYPE_BLOB=252
28 MYSQL_TYPE_VAR_STRING=253
29 MYSQL_TYPE_STRING=254
30 MYSQL_TYPE_GEOMETRY=255
```

```
//include/mysql_com.h:369
enum enum_field_types { MYSQL_TYPE_DECIMAL, MYSQL_TYPE_TINY,
			MYSQL_TYPE_SHORT,  MYSQL_TYPE_LONG,
			MYSQL_TYPE_FLOAT,  MYSQL_TYPE_DOUBLE,
			MYSQL_TYPE_NULL,   MYSQL_TYPE_TIMESTAMP,
			MYSQL_TYPE_LONGLONG,MYSQL_TYPE_INT24,
			MYSQL_TYPE_DATE,   MYSQL_TYPE_TIME,
			MYSQL_TYPE_DATETIME, MYSQL_TYPE_YEAR,
			MYSQL_TYPE_NEWDATE, MYSQL_TYPE_VARCHAR,
			MYSQL_TYPE_BIT,
			MYSQL_TYPE_TIMESTAMP2,
			MYSQL_TYPE_DATETIME2,
			MYSQL_TYPE_TIME2,
                        MYSQL_TYPE_NEWDECIMAL=246,
			MYSQL_TYPE_ENUM=247,
			MYSQL_TYPE_SET=248,
			MYSQL_TYPE_TINY_BLOB=249,
			MYSQL_TYPE_MEDIUM_BLOB=250,
			MYSQL_TYPE_LONG_BLOB=251,
			MYSQL_TYPE_BLOB=252,
			MYSQL_TYPE_VAR_STRING=253,
			MYSQL_TYPE_STRING=254,
			MYSQL_TYPE_GEOMETRY=255

};
```
