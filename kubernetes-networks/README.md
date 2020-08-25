# Комментари по домашнему заданию
## Работа с тестовым веб приложением
1. Работа с пробами
 * Посмотрел ещё раз, как работают пробы.
 * Делать пробу на наличие процесса бессмысленно (конкретно веб севрера) потому что это не гарантирует, что сервис работает как нужно.
 * Такие проверки возможно стоит использовать для тех случаев, когда наличие процесса гарантирует, что всё работает как нужно.
 * Нельзя использовать ```maxSurge``` и ```maxUnavailable``` оба 0 или оба 100
2. Работа с Service и ClusterIP
 * Изучил для чего нужен Service
 * Изучил, для чего нужен ClusterIP
 * Очень долго смотрел правила для iptables, в которых обозначена работа ClusterIP изнутри. Пока сложно, буду дальше погружаться.
 * Попробовал IPVS, ipset всегда работает быстрее с большим количеством данных (правил ...), поэтому видимо рекомендуется для больших кластеров именно его.
3. Работа с LoadBalancer
 * Установил MetalLB и настроил его.
 * Создал Service типа LoadBalancer для шанего приложения.
 * Теперь к прилоению можно постучаться чере EXTERNAL-IP!
 * Проверил, работает.
 * В процессе выполенения задания по DNS столкунулся с проблемой:
  ~~~
 Invalid value: []core.ServicePort{core.ServicePort{Name:"", Protocol:"TCP", AppProtocol:(*string)(nil), Port:53, TargetPort:intstr.IntOrString{Type:0, IntVal:53, StrVal:""}, NodePort:0}, core.ServicePort{Name:"", Protocol:"UDP", AppProtocol:(*string)(nil), Port:53, TargetPort:intstr.IntOrString{Type:0, IntVal:53, StrVal:""}, NodePort:0}}: cannot create an external load balancer with mix protocols
 ~~~
   Изучил, документацию, добавил к сервисам один и тот же ```loadBalancerIP``` и ```metallb.universe.tf/allow-shared-ip``` в annotations.
4. Работа с Ingress
  * Установил Ingree в кластер.
  * Создал LoadBalancer сервис для Ingress.
  * Проверил, что всё работает, отправивив запрос на EXTERNAL-IP.
  * Создал и проверил Headless-сервис.
5. Ingress для Dashboard
  * Создал Ingress для Dashboard
  * Пришлось пошаманить с реврайтом, чтобы вырезать ```/dashboard``` и с протоколом backend HTTPS.
  * Создал ServiceAccount и RoleBinding для доступа к Dashboard. Токен получил командой:
  ~~~
  kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
  ~~~
  * Залогинился в Dashboard и проверил, что он работает.
6. Canary для Ingress
  * Создал Deplyment для web-canary.
  * Создал haedless сервис для web-canary.
  * Создал Ingress для web-canary, согласно документации ingress-nginx по canary.
