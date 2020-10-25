# Комментари по домашнему заданию
## Репы
https://gitlab.com/dexik/microservices-demo
https://gitlab.com/dexik/gke-tf
## Что делал.
0. Создал репозиторий для микросерввисов и будуще terraform конфигурации для gke кластера
1. Создал Helm чарты для микросервисов
2. Подготовил terraform конфигурацию для созданиея кластера в gke
3. Создал pipeline для сборки образов микросервисов в gitlab ci
3. Установил fluxctl
4. Установил flux и helm плагин для него и проверил, что он работает
5. Для каждого из микросервисов создал объекты HelmRelease и проверил, что всё работает как надо, меняя версии образов микросервисов
6. Отключил плагин isio gke и установил его через istioctl с профилем default согласно документации
7. Установил flagger в кластер, добавил метку к неймспейсу для добавления контейнера с прокси в каждый под
8. Настроил доступ извне к frontend средствами istion gateway и istion virtualservice 
9. Поправил метрику в канареечном релизе frontend и адрес prometheus, предварительно установив его
10. Выводы команд:
~~~
➜  frontend git:(master) kubectl get canaries -n microservices-demo
NAME       STATUS      WEIGHT   LASTTRANSITIONTIME
frontend   Succeeded   0        2020-10-25T15:40:24Z
~~~
~~~
➜  frontend git:(master) kubectl describe canary -n microservices-demo frontend
Name:         frontend
Namespace:    microservices-demo
Labels:       app.kubernetes.io/managed-by=Helm
Annotations:  helm.fluxcd.io/antecedent: microservices-demo:helmrelease/frontend
              meta.helm.sh/release-name: frontend
              meta.helm.sh/release-namespace: microservices-demo
API Version:  flagger.app/v1beta1
Kind:         Canary
Metadata:
  Creation Timestamp:  2020-10-25T14:10:14Z
  Generation:          3
  Resource Version:    3576092
  Self Link:           /apis/flagger.app/v1beta1/namespaces/microservices-demo/canaries/frontend
  UID:                 7c63ac03-6e1d-43d5-bb50-44e46ebf2825
Spec:
  Analysis:
    Interval:    1m
    Max Weight:  30
    Metrics:
      Interval:               1m
      Name:                   istio_requests_total
      Threshold:              99
    Step Weight:              10
    Threshold:                1
  Progress Deadline Seconds:  60
  Service:
    Gateways:
      frontend-gateway
    Hosts:
      hipster.00aa.ru
    Port:  80
    Retries:
      Attempts:         3
      Per Try Timeout:  1s
      Retry On:         gateway-error,connect-failure,refused-stream
    Target Port:        8080
    Traffic Policy:
      Tls:
        Mode:  DISABLE
  Target Ref:
    API Version:  apps/v1
    Kind:         Deployment
    Name:         frontend
Status:
  Canary Weight:  0
  Conditions:
    Last Transition Time:  2020-10-25T15:40:24Z
    Last Update Time:      2020-10-25T15:40:24Z
    Message:               Canary analysis completed successfully, promotion finished.
    Reason:                Succeeded
    Status:                True
    Type:                  Promoted
  Failed Checks:           0
  Iterations:              0
  Last Applied Spec:       68f7b9cf68
  Last Promoted Spec:      68f7b9cf68
  Last Transition Time:    2020-10-25T15:40:24Z
  Phase:                   Succeeded
  Tracked Configs:
Events:
  Type     Reason  Age                 From     Message
  ----     ------  ----                ----     -------
  Warning  Synced  41m                 flagger  Halt frontend.microservices-demo advancement external check load-test failed
  Normal   Synced  15m (x2 over 42m)   flagger  Advance frontend.microservices-demo canary weight 5
  Warning  Synced  14m                 flagger  Halt advancement no values found for istio metric request-success-rate probably frontend.microservices-demo is not receiving traffic: running query failed: no values found
  Warning  Synced  13m (x2 over 40m)   flagger  Canary failed! Scaling down frontend.microservices-demo
  Warning  Synced  13m (x2 over 40m)   flagger  Rolling back frontend.microservices-demo failed checks threshold reached 1
  Normal   Synced  6m9s (x3 over 43m)  flagger  New revision detected! Scaling up frontend.microservices-demo
  Normal   Synced  5m9s (x3 over 42m)  flagger  Starting canary analysis for frontend.microservices-demo
  Normal   Synced  5m9s                flagger  Advance frontend.microservices-demo canary weight 10
  Normal   Synced  4m9s                flagger  Advance frontend.microservices-demo canary weight 20
  Normal   Synced  3m8s                flagger  Advance frontend.microservices-demo canary weight 30
  Normal   Synced  2m8s                flagger  Copying frontend.microservices-demo template spec to frontend-primary.microservices-demo
  Normal   Synced  69s                 flagger  Routing all traffic to primary
  Normal   Synced  8s                  flagger  Promotion completed! Scaling down frontend.microservices-demo
  ~~~