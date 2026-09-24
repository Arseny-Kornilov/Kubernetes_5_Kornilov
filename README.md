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
