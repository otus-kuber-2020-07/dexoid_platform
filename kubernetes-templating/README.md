# Комментари по домашнему заданию
1. Создание кластера в Google Cloud.
 * Создал кластер с параметрами по умолчание из веб консоли.
2. Установка готовых helm charts
 * Обновил Helm 3 через brew до последней версии
 * Добавил репозиторий https://kubernetes-charts.storage.googleapis.com
 * Установил чарт nginx-ingress
 * Установил чарт cert-manager и CRD для него, также создал ClusterIssuer для LE
 * Установил и сконфигурировал chartmuseum с ingress и генерацией сертификата от LE. Немного помучался с доменами xip.io и nip.io. Сертификаты не всегда хотели выпускаться. Решил использовать свой домен 00aa.ru
 * Убедился, что helm 3 харнит информацию в секретах
 * Загрузил чарт nginx-ingress и залил его в свой chartmuseum предварительно включив API для него (см. values.yaml для chartmuseum)
   ~~~
   helm pull stable/nginx-ingress
   tar -xf nginx-ingress-1.41.3.tgz
   helm plugin install https://github.com/chartmuseum/helm-push.git
   helm push nginx-ingress/ chartmuseum
   ~~~
  * Изучил дефолты для чарта harbor и на основе них составил файл ```values.yaml```. Установил harbor и проверил, что он работает. Предварительно добавил chart repo harbor.
  * Составил helmfile и проверил, изучил документацию по хукам в helmfile. Пришлось установить плагин diff для helmfile: ```helm plugin install https://github.com/databus23/helm-diff``` После чего конфигурация применилась без ошибок.
3. Создание своего helm chart
 * Создал свой helm chart для hipster shop c зависимостями от frontend и redis (задание со звездочкой)
 * Проверял шаблонизацию так:
 ~~~
 helm upgrade --install hipster-shop kubernetes-templating/hipster-shop --namespace hipster-shop --set frontend.service.type=NodePort
 ~~~
4. Работа с kubecfg
  * Установил kubecfg из brew
  * Убрал из основногочарта сервисы paymentservice и shippingservice (магазин сломался) и перенес из в ```services.jsonnet```
  * Применил конфигурацию из ```services.jsonnet```. После чего магазин починился :)

5. Работа с kustomize
  * Создал конфигурацию для Kustomize
  * Проверил, что она успешно применяется
