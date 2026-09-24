# Домашнее задание к занятию «Helm»
## Задание 1. Подготовить Helm-чарт для приложения
 - Необходимо упаковать приложение в чарт для деплоя в разные окружения.
 - Каждый компонент приложения деплоится отдельным deployment’ом или statefulset’ом.
 - В переменных чарта измените образ приложения для изменения версии
   
 ### ОТВЕТ:

   #### Backend (Deployment+Service+Ingress)
   https://github.com/Arseny-Kornilov/Kubernetes_5_Kornilov/blob/e2e737b75414a6f5358331b8c22f2c91e19ae061/backend.yaml
   
   #### Frontend (Deployment+Service+Ingress)
   https://github.com/Arseny-Kornilov/Kubernetes_5_Kornilov/blob/acb3dd20384e0385870f0a433e00339c688502a3/frontend.yaml
   #### PostgreSQL (StatefuleSet+Service)
   https://github.com/Arseny-Kornilov/Kubernetes_5_Kornilov/blob/acb3dd20384e0385870f0a433e00339c688502a3/postgres.yaml

   #### Helm Chart 
   https://github.com/Arseny-Kornilov/Kubernetes_5_Kornilov/blob/671b42b121f54f31b2a18053ab888f6c4fe4e793/Chart.yaml

   #### Values
   https://github.com/Arseny-Kornilov/Kubernetes_5_Kornilov/blob/671b42b121f54f31b2a18053ab888f6c4fe4e793/values.yaml

## Задание 2. Запустить две версии в разных неймспейсах
Подготовив чарт, необходимо его проверить. Запуститe несколько копий приложения.
Одну версию в namespace=app1, вторую версию в том же неймспейсе, третью версию в namespace=app2.
Продемонстрируйте результат.

 ### ОТВЕТ:
 Деплоим первый релиз `alpha` в namespace `app1`

```console
vagrant@vagrant:/$ helm -n app1 install alpha newspaper
NAME: alpha
LAST DEPLOYED: Tue Sep 27 21:55:56 2022
NAMESPACE: app1
STATUS: deployed
REVISION: 1
```
Деплоим второй релиз `beta` в тот же namespace

```console
vagrant@vagrant:~/$ helm -n app1 install beta newspaper --set hostname=beta
NAME: beta
LAST DEPLOYED: Tue Sep 27 22:04:04 2022
NAMESPACE: app1
STATUS: deployed
REVISION: 1
```

Деплоим релиз `gamma` в namespace `app2`

```console
vagrant@vagrant:~/$ helm -n app2 install gamma newspaper --set hostname=gamma
NAME: gamma
LAST DEPLOYED: Tue Sep 27 22:06:56 2022
NAMESPACE: app2
STATUS: deployed
REVISION: 1
```

Тестируем все три релиза

```console
vagrant@vagrant:~/$ helm -n app1 test alpha
NAME: alpha
LAST DEPLOYED: Tue Sep 27 21:55:56 2022
NAMESPACE: app1
STATUS: deployed
REVISION: 1
TEST SUITE:     alpha-test-backend-connection
Last Started:   Tue Sep 27 22:11:16 2022
Last Completed: Tue Sep 27 22:11:20 2022
Phase:          Succeeded
TEST SUITE:     alpha-test-frontend-connection
Last Started:   Tue Sep 27 22:11:20 2022
Last Completed: Tue Sep 27 22:11:23 2022
Phase:          Succeeded
vagrant@vagrant:~/$ helm -n app1 test beta
NAME: beta
LAST DEPLOYED: Tue Sep 27 22:04:04 2022
NAMESPACE: app1
STATUS: deployed
REVISION: 1
TEST SUITE:     beta-test-backend-connection
Last Started:   Tue Sep 27 22:11:29 2022
Last Completed: Tue Sep 27 22:11:32 2022
Phase:          Succeeded
TEST SUITE:     beta-test-frontend-connection
Last Started:   Tue Sep 27 22:11:32 2022
Last Completed: Tue Sep 27 22:11:35 2022
Phase:          Succeeded
vagrant@vagrant:~/$ helm -n app2 test gamma
NAME: gamma
LAST DEPLOYED: Tue Sep 27 22:06:56 2022
NAMESPACE: app2
STATUS: deployed
REVISION: 1
TEST SUITE:     gamma-test-backend-connection
Last Started:   Tue Sep 27 22:11:53 2022
Last Completed: Tue Sep 27 22:11:57 2022
Phase:          Succeeded
TEST SUITE:     gamma-test-frontend-connection
Last Started:   Tue Sep 27 22:11:57 2022
Last Completed: Tue Sep 27 22:12:00 2022
Phase:          Succeeded
```

Проверяем поды:

```console
vagrant@vagrant:~/$ kubectl get pods -A | (head -n 1; grep app)
NAMESPACE       NAME                                  READY   STATUS    RESTARTS        AGE
app1            alpha-backend-5ddd775798-mgrl9        1/1     Running   0               22m
app1            alpha-frontend-786bb5cd7c-cx952       1/1     Running   0               22m
app1            alpha-postgres-0                      1/1     Running   0               22m
app1            beta-backend-6454bb8878-jj6wl         1/1     Running   0               13m
app1            beta-frontend-754945d9f5-lg8tk        1/1     Running   0               14m
app1            beta-postgres-0                       1/1     Running   0               14m
app2            gamma-backend-f7f54546f-nw7hh         1/1     Running   0               11m
app2            gamma-frontend-787bfbbf79-z8g89       1/1     Running   0               11m
app2            gamma-postgres-0                      1/1     Running   0               11m
```

---
