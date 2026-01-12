2020-09-14

mysql> select count(*) from amavisd_sauserpref where preference = 'score MR_PHISH_ADJ' and value = "3.0";
+----------+
| count(*) |
+----------+
|      358 |
+----------+
1 row in set (0.01 sec)

mysql> select count(*) from amavisd_sauserpref where preference = 'score MR_PHISH_ADJ' and value = "5.0";
+----------+
| count(*) |
+----------+
|      111 |
+----------+
1 row in set (0.01 sec)


2020-09-28

mysql> select count(*) from amavisd_sauserpref where preference = 'score MR_PHISH_ADJ' and value = "3.0";
+----------+
| count(*) |
+----------+
|      364 |
+----------+
1 row in set (0.01 sec)

mysql> select count(*) from amavisd_sauserpref where preference = 'score MR_PHISH_ADJ' and value = "5.0";
+----------+
| count(*) |
+----------+
|      116 |
+----------+
