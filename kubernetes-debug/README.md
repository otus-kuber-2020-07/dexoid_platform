# Комментари по домашнему заданию
1. Работа с kubernetes-debug
 * Установил сразу рабочую версию ```debug version 0.2.0-rc``` которая прописывает нужные capabilities
 * Использовал официальную документацию, запускал так: ```kubectl debug --agentless=false POD_NAME``` предварительно установив агента для версии 1.17 (есть изменения в описании Daemonset)
 * Запускал strace без всяких проблем на nginx воркерах: ```strace -fp PID```
2. Работа с iptables-tailer
 * Установил тестовое приложение netperf-operator
 * Замерил скорость, убедился, что всё ОК
 * Применил предлагаемую политику, ествественно всё сломалось :)
 * Прошёл боль с iptables-tailer с использованем дефолтового манифеста
 * Применил манифест из снипетов, отредактировав его для совместимости с кубером 1.17 (есть изменения в описании Daemonset)
 * Создал сервис аккаунт, и биндинги, чтобы тейлер мог патчить евенты
 * Начали прилетать евенты
 * Исправил политику, чтобы разрешить трафик, снова замерил скорость, убедился, что работает
 * Почему-то перестали писать логи тейлера о блокировках, не могу разобраться :(((, с евентами всё ОК
 * Поправил параметры ENV переменных для Daemonset iptable-tailer чтобы показывает имя пода, но логи не пишутся, не получается проверить...