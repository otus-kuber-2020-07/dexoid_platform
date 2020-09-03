# Комментари по домашнему заданию
0. Создал CR и CRD для MySQL.
1. Создал оператора MySQL согласно инструкции.
2. Собрал docker образ для оператора.
~~~
(kopf) ➜  deploy git:(kubernetes-operators) ✗ kubectl exec -it $MYSQLPOD -- mysql -potuspassword -e "select * from test;" otus-database
mysql: [Warning] Using a password on the command line interface can be insecure.
+----+-------------+
| id | name        |
+----+-------------+
|  1 | some data   |
|  2 | some data-2 |
+----+-------------+
~~~
~~~
NAME                         COMPLETIONS   DURATION   AGE
backup-mysql-instance-job    1/1           2s         28m
restore-mysql-instance-job   1/1           46s        26m
~~~
