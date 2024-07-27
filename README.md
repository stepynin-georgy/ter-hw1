# Домашнее задание к занятию «Введение в Terraform»

### Цели задания

1. Установить и настроить Terrafrom.
2. Научиться использовать готовый код.

------

### Чек-лист готовности к домашнему заданию

1. Скачайте и установите **Terraform** версии ~>1.8.4 . Приложите скриншот вывода команды ```terraform --version```.
   ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_35.png)
2. Скачайте на свой ПК этот git-репозиторий. Исходный код для выполнения задания расположен в директории **01/src**.
3. Убедитесь, что в вашей ОС установлен docker.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Репозиторий с ссылкой на зеркало для установки и настройки Terraform: [ссылка](https://github.com/netology-code/devops-materials).
2. Установка docker: [ссылка](https://docs.docker.com/engine/install/ubuntu/). 
------
### Внимание!! Обязательно предоставляем на проверку получившийся код в виде ссылки на ваш github-репозиторий!
------

### Задание 1

1. Перейдите в каталог [**src**](https://github.com/netology-code/ter-homeworks/tree/main/01/src). Скачайте все необходимые зависимости, использованные в проекте.
   
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_32.png)
   
2. Изучите файл **.gitignore**. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
   
Согласно файлу .gitignore в файле personal.auto.tfvars допускается хранить секретные данные.
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_37.png)

3. Выполните код проекта. Найдите  в state-файле секретное содержимое созданного ресурса **random_password**, пришлите в качестве ответа конкретный ключ и его значение.
   
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_40.png)

4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла **main.tf**.
Выполните команду ```terraform validate```. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.

![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_41.png)

Видим две ошибки: Missing name of resource на 24 строке, и Invalide resource name на 29.
На 24 не хватает второго аргемента, имени. На 29 имя начинается с цифры, исправляем на корректное имя.

![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_42.png)

Далее появляется еще одна ошибка на 31 строке: Reference to undeclared validate:

![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_43.png)

Название ресурса в main.tf random_string, а указано random_string_FAKE, еще resulT написан через большие буквы, исправляем.

![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_43.png)
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_45.png)

5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды ```docker ps```.
    
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_46.png)
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_47.png)

6. Замените имя docker-контейнера в блоке кода на ```hello_world```. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду ```terraform apply -auto-approve```.
    
   ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_48.png)
   ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_49.png)
   
Объясните своими словами, в чём может быть опасность применения ключа  ```-auto-approve```. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды ```docker ps```.

--auto-approve автоматичсеки применяет новое описание в манифестах к сущесвующей конфигурации без потверждения. Используется, скорее всего, с CI/CD, ansible, где нет необходимости в интерактивном режиме, чтоб ускорить процесс.
7. Уничтожьте созданные ресурсы с помощью **terraform**. Убедитесь, что все ресурсы удалены. Приложите содержимое файла **terraform.tfstate**

 ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_50.png)
 ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_51.png)
    
8. Объясните, почему при этом не был удалён docker-образ **nginx:latest**. Ответ **ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ**, а затем **ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ** строчкой из документации [**terraform провайдера docker**](https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs).  (ищите в классификаторе resource docker_image )

    ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_52.png)
    ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_53.png)

Образ не удалился из-за тега keep_locally = true. Если он установлен в true - образ остается в локальном хранилище докера, а если значение false - образ будет удален.
[Resource (docker_image)](https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs/resources/image)
------

## Дополнительное задание (со звёздочкой*)

**Настоятельно рекомендуем выполнять все задания со звёздочкой.** Они помогут глубже разобраться в материале.   
Задания со звёздочкой дополнительные, не обязательные к выполнению и никак не повлияют на получение вами зачёта по этому домашнему заданию. 

### Задание 2*

1. Создайте в облаке ВМ. Сделайте это через web-консоль, чтобы не слить по незнанию токен от облака в github(это тема следующей лекции). Если хотите - попробуйте сделать это через terraform, прочитав документацию yandex cloud. Используйте файл ```personal.auto.tfvars``` и гитигнор или иной, безопасный способ передачи токена!
   
   Создал вм через yc. Создал пару ключей и создал ВМ коммандой:
   
```
yc compute instance create --name hw2 --zone ru-central1-a --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --create-boot-disk image-folder-id=standard-images,ima
ge-family=ubuntu-2204-lts --ssh-key /home/user/.ssh/id_rsa.pub
```

3. Подключитесь к ВМ по ssh и установите стек docker.
   
    ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_57.png)
   
5. Найдите в документации docker provider способ настроить подключение terraform на вашей рабочей станции к remote docker context вашей ВМ через ssh.
   На основной машине настроил docker context:
   
```
docker context create ssh --docker "host=ssh://yc-user@89.169.136.253,key=/home/user/.ssh/id_rsa"
docker context use ssh
```

7. Используя terraform и  remote docker context, скачайте и запустите на вашей ВМ контейнер ```mysql:8``` на порту ```127.0.0.1:3306```, передайте ENV-переменные. Сгенерируйте разные пароли через random_password и передайте их в контейнер, используя интерполяцию из примера с nginx.(```name  = "example_${random_password.random_string.result}"```  , двойные кавычки и фигурные скобки обязательны!)

```
    environment:
      - "MYSQL_ROOT_PASSWORD=${...}"
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - "MYSQL_PASSWORD=${...}"
      - MYSQL_ROOT_HOST="%"
```
[main.tf для второго задания](https://github.com/stepynin-georgy/ter-hw1/blob/main/2/main.tf)

![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_60.png)
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_61.png)
![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_62.png)

6. Зайдите на вашу ВМ , подключитесь к контейнеру и проверьте наличие секретных env-переменных с помощью команды ```env```. Запишите ваш финальный код в репозиторий.
   
    ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_64.png)
    ![изображение](https://github.com/stepynin-georgy/ter-hw1/blob/main/img/Screenshot_65.png)

### Задание 3*
1. Установите [opentofu](https://opentofu.org/)(fork terraform с лицензией Mozilla Public License, version 2.0) любой версии
2. Попробуйте выполнить тот же код с помощью ```tofu apply```, а не terraform apply.
------

### Правила приёма работы

Домашняя работа оформляется в отдельном GitHub-репозитории в файле README.md.   
Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

### Критерии оценки

Зачёт ставится, если:

* выполнены все задания,
* ответы даны в развёрнутой форме,
* приложены соответствующие скриншоты и файлы проекта,
* в выполненных заданиях нет противоречий и нарушения логики.

На доработку работу отправят, если:

* задание выполнено частично или не выполнено вообще,
* в логике выполнения заданий есть противоречия и существенные недостатки. 
