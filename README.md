# Выполнено ДЗ №4

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 -  Работа с контроллером StatefulSet;
 -  Создание PersistentVolume и PersistentVolumeClaim;
 -  Создание секретов и использование их в манифестах создания подов.


## Как запустить проект:
 - kubectl apply -f *.yaml

## Как проверить работоспособность:
- kubectl get statefulsets
- kubectl get pods
- kubectl get pvc
- kubectl get pv 

## Задание со *

Создан файл с секретами и задействовано в манифесте. Очевидно, что при использовании GIT в качестве хранения манифестов
этот способ также не является надежным, поэтому разумно использовать внешние сервисы для хранения секретов (SealesSecrets, Hashicorp Vault и тд)


# Выполнено ДЗ №2

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 -  Развернули кластер с помощью Kind
 -  Поработали с контроллером ReplicaSet, исследовали его ограничения
 -  Поработали с контроллером Deployment, научились обновлять и откатывать версии ПО,
     запустили обновление с помощью стратегий обновления ПО blue green и reverse rolling update
 -  Изучили контроллер DaemonSet, возможность запуска подов на всех серверах кластера, включая мастеры - 
     серверы.

## Как запустить проект:
 - kubectl apply -f *.yaml, после чего меняем в манифестах ревизии ПО, запускаем еще раз и смотрим за изменениями

## Как проверить работоспособность:
 - За применениями различных стратегий  деплоя удобно наблюдать в динамике через Pipe, например:
   kubectl apply -f frontend-deployment.yaml | kubectl get pods -l app=frontend -w

#ДЗ 1 (Kubernetes Intro)

1.1 Начало работы с Кубером.

Развернул куб локально, используя minikube + virtualbox. Попробовал удалять поды - кластер восстанавливается.
Отвечаю на вопрос из ДЗ:

> Разберитесь почему все pod в namespace kube-system восстановились
> после удаления.

В кубере один из низкоуровневых компонентов - демон kubelet, который запускает статик - поды в неймспейсе kube-system, такие как апи-сервер, шедулер, etcd и тд. Для них я вижу описанные манифесты в виде конфигов в /etc/kubernetes/manifests. И если мы дропаем все эти поды, kubelet их пересоздает, используя эти манифесты.
Манифесты же для днс и прокси, несмотря на принадлежность неймспейсу kube-system, правятся уже черех kubetctl,
что говорит о том, что эти сущности управляются уже связкой api-server + etcd и конфиги хранятся в etcd (то есть это уже deployment resourse).
Другими словами, когда kubelet поднимает связку api-server + etcd, используя манифесты статик - подов, дальше уже из etcd берется последнее состояние coredns и этот под приводится к этому состоянию, то есть запускается. Примерно так.

1.2 Создал тестовый Dockerfile, из него собрал докер - образ и загрузил в Докер - хаб. <br />
1.3 Создал манифест для запуска пода, который подтянул докер-образ из хаба. <br />
1.4 Добавил в манифест запуск инит - пода, который кладет html - файл в папку app в основной под. <br />
1.5 Пересоздал основной под, пробросил порт и убедился, что содержимое папки app отображается. <br />
1.6 Cклонировал репо hipster-shop, собрал образ из докер - файла фронтенда. <br />
1.7 Попробовал запуск подов в режиме  ad-hoc. <br />

Задание со *

После анализа логов запуска стало очевидно, что не заданы параметры переменных среды.
Исправленный манифест прилагается.





