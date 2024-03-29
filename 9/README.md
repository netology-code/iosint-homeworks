# Домашнее задание к занятию «Асинхронная разработка. Многопоточность»

Асинхронные функции и **многопоточность** — это то, что спрашивают кандидатов на каждом техническом собеседовании, и понятно почему: современные корпоративные приложения не могут существовать в одном главном потоке, и умение компетентно работать с задачами, выполняющимися на разных потоках, а также делать плавный, отзывчивый и надёжный **UI** вне зависимости от нагрузки процессора ценится в любой команде разработки.

Если вам нравится, как архитектурно работают ваши координаторы и концепция MVVM из прошлого задания, используйте приложение после завершения этой работы; если нет — используйте приложение **Navigation** после завершения домашнего задания **№6**.

## Правила выполнения домашней работы

* Все задачи обязательны к выполнению для получения зачёта, кроме задач со звёздочкой.
* Изучите [инструкцию](https://github.com/netology-code/iosint-homeworks/blob/main/Pull%20request's%20guideline.md) по сдаче домашних заданий через pull request. В поле для сдачи работы прикрепите ссылку на ваш проект на GitHub.
* Любые вопросы по решению задач задавайте в чате учебной группы.

## Задача

В знакомом вам пакете [iOSIntPackage](https://github.com/TrueMax/iOSIntPackage/releases/tag/v3.2.3) в `ImageProcessor` появился новый метод: `processImagesOnThread`. Этот метод умеет открывать собственный `thread` и работать с разным уровнем `qualityOfService`. 

1. Установите пакет или обновите его до последней версии: **3.2.3.**.
2. Удалите предыдущую реализацию поочерёдного добавления фото через подписку с `addImagesWithTimer` в домашнем задании №5. Заполните исходную коллекцию необработанными фото так, как это было ранее, до выполнения этого задания.
3. Встройте в `PhotosViewController` вызов метода `processImagesOnThread` и передайте ему массив исходных изображений в коллекции. Обновите вашу коллекцию изображений после обработки фильтром. 
4. Вызовите метод несколько раз с различными значениями параметра `qos: QualityOfService` и разными исходными параметрами: массивом входящих изображений и выбранным фильтром обработки.
5. Откройте **Debug Navigator**, чтобы наблюдать количество задействованных потоков в каждом случае.
6. С помощью встроенных функций Swift сделайте замер времени исполнения метода `processImagesOnThread` с разной комбинацией параметров. Обратите внимание, чтобы отсчёт времени начинался до вызова метода `processImagesOnThread` и заканчивался **после** завершения обработки изображений.
7. Напишите в комментарии, сколько времени занимает обработка изображений фильтром при различных значениях параметра `qos: QualityOfService`.
