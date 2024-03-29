# Домашнее задание к занятию «Собственные домены ошибок»

Механизм **do** ... **try** ... **catch** c **throws** функциями — удобный метод реакции и обработки не только ошибок, но и других исключительных ситуаций в вашем коде, когда необходимо прервать нормальное течение алгоритма. Потренируемся решать такие задачи.

Для этой работы используйте приложение Navigation после выполнения предыдущего домашнего задания.

## Правила выполнения домашней работы

* Все задачи обязательны к выполнению для получения зачёта, кроме задач со звёздочкой.
* Изучите [инструкцию](https://github.com/netology-code/iosint-homeworks/blob/main/Pull%20request's%20guideline.md) по сдаче домашних заданий через pull request. В поле для сдачи работы прикрепите ссылку на ваш проект на GitHub.
* Любые вопросы по решению задач задавайте в чате учебной группы.

## Задача 1
1. Найдите места в вашем приложении, где есть вероятность неблагоприятного исхода и требуется сообщить об этом пользователю. 
2. Создайте собственный домен ошибок, удовлетворяющий протоколу `Error`. 
3. Добавьте в него перечисление различных случаев, которые могут возникнуть в вашем приложении. 

## Задача 2
1. Примените конструкцию `do { ... } catch { ... }` и вызов `try` для работы с возможной ошибкой.
2. Смоделируйте ситуацию возникновения ошибки и покажите пользователю `Alert`, уведомив его об ошибке.

## Задача 3
Найдите подходящее место в коде и примените тип `Result` для возврата успешного результата операции или возможной ошибки.

## Задача 4*
1. Найдите место в вашей программе, где её дальнейшее выполнение не имеет смысла, если определённое условие не выполнено.
2. Воспользуйтесь проверкой `guard` и `preconditionFailure` для обработки такого случая.
