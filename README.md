# Домашнее задание к занятию «Использование Terraform в команде»

### Задание 1

1. Возьмите код:
- из [ДЗ к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/src),
- из [демо к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/demonstration1).
2. Проверьте код с помощью tflint и checkov. Вам не нужно инициализировать этот проект.
3. Перечислите, какие **типы** ошибок обнаружены в проекте (без дублей).

### Ответ

Тфлинт выдавал три типа ошибок: **неуказанная версия провайдера, неиспользуемые в коде переменные и в модуле с удаленного репозитория - ветка мейн**. А Чеков находил только проблему в коде демонстрации с веткой мейн

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled.png "1")

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled4.png "1")

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled3.png "1")

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled1.png "1")


------

### Задание 2

1. Возьмите ваш GitHub-репозиторий с **выполненным ДЗ 4** в ветке 'terraform-04' и сделайте из него ветку 'terraform-05'.
2. Повторите демонстрацию лекции: настройте YDB, S3 bucket, yandex service account, права доступа и мигрируйте state проекта в S3 с блокировками. Предоставьте скриншоты процесса в качестве ответа.
3. Закоммитьте в ветку 'terraform-05' все изменения.
4. Откройте в проекте terraform console, а в другом окне из этой же директории попробуйте запустить terraform apply.
5. Пришлите ответ об ошибке доступа к state.
6. Принудительно разблокируйте state. Пришлите команду и вывод.

### Ответ

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled8.png "1")

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled9.png "1")

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled10.png "1")

Разлочить: **terraform force-unlock id-лока-из-ошибки** :

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled11.png "1")

------

### Задание 3  

1. Сделайте в GitHub из ветки 'terraform-05' новую ветку 'terraform-hotfix'.
2. Проверье код с помощью tflint и checkov, исправьте все предупреждения и ошибки в 'terraform-hotfix', сделайте коммит.
3. Откройте новый pull request 'terraform-hotfix' --> 'terraform-05'. 
4. Вставьте в комментарий PR результат анализа tflint и checkov, план изменений инфраструктуры из вывода команды terraform plan.
5. Пришлите ссылку на PR для ревью. Вливать код в 'terraform-05' не нужно.

### Ответ

https://github.com/Jlljully/terr04_push/pull/2

------

### Задание 4

1. Напишите переменные с валидацией и протестируйте их, заполнив default верными и неверными значениями. Предоставьте скриншоты проверок из terraform console. 

- type=string, description="ip-адрес" — проверка, что значение переменной содержит верный IP-адрес с помощью функций cidrhost() или regex(). Тесты:  "192.168.0.1" и "1920.1680.0.1";
- type=list(string), description="список ip-адресов" — проверка, что все адреса верны. Тесты:  ["192.168.0.1", "1.1.1.1", "127.0.0.1"] и ["192.168.0.1", "1.1.1.1", "1270.0.0.1"].

### Ответ


```hcl
  variable "iptest" {
    type          = string
    description   = "ip-адрес"
    validation {
        condition = can(regex("^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$",var.iptest))
        error_message = "Invalid IP address"
    }
}
```

![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled13.png "1")

  
```hcl
variable "test_ip_list" {
    type          = list(string)
    description   = "список ip-адресов"
    validation {
        condition =  alltrue([
        for i in var.test_ip_list : can(regex("^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$",i))
        ])
        error_message = "Invalid IP address"
    }
}
```
  
![Скрин](https://github.com/Jlljully/terr05/blob/main/Untitled14.png "1")

------

