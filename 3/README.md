# Домашнее задание к занятию 1.3 	Паттерны и архитектура мобильных приложений

## Правила выполнения домашней работы:

* Базовый проект для выполнения домашней работы - последний проект с домашней работой курса iOS UI, где присутствуют LoginViewController + FeedViewController
* Все задачи обязательны к выполнению для получения зачета, кроме задач со звездочкой. Присылать на проверку можно каждую задачу по отдельности или все задачи вместе. Во время проверки по частям ваша домашняя работа будет со статусом "На доработке".
* Любые вопросы по решению задач задавайте в чате Slack (ссылку вы найдете в письме на вашей эл. почте).

## Оформление результата:

* ТУТ ИНСТРУКЦИЯ ПО СДАЧЕ ДЗ ЧЕРЕЗ PULL REQUEST на GITHUB

## Задача 1: паттерны проектирования

$ Создаем Singleton для проверки логина и пароля - пускай свойства существуют в hardcode виде, по выбору студента (let login = "Vasily", let pswd = "Masha"). Singleton - класс или структура, назовем его, для примера, Checker (или выберите свое название), имеет 1 метод для проверки логина и пароля. 
$ Для LoginViewController прописываем протокол делегата LoginViewControllerDelegate. У делегата 2 метода - проверка логина и пароля. 
$ Создаем произвольный класс/структуру LoginInspector (в задании выбрать свое название), который подписываем на протокол LoginViewControllerDelegate, реализуем в нем протокольные методы. 
$ LoginInspector проверяет точность введенного пароля с помощью синглтона Checker. 
$ Важный момент: чтобы делегат мог вернуть контроллеру результат проверки логина и пароля, в методах протокола делегата должны быть коллбэки 

Поскольку у нас нет DI-контейнеров и вспомогательных абстракций в виде роутеров, координаторов, воспользуемся штатным `SceneDelegate` и его стандартным методом.

```
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        guard let _ = (scene as? UIWindowScene) else { return }
        
        // инициализация LoginInspector
        
        if let tabController = window?.rootViewController as? UITabBarController, let loginNavigation = tabController.viewControllers?.last as? UINavigationController, let loginController = loginNavigation.viewControllers.first as? LogInViewController {
            loginController.delegate = // экземпляр LoginInspector
        }
    }
```


