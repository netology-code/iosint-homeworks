# Домашнее задание к занятию 1.3 	Паттерны и архитектура мобильных приложений

## Правила выполнения домашней работы:

* Базовый проект для выполнения домашней работы - последний проект с домашней работой курса iOSUI, где присутствуют LoginViewController + FeedViewController
* Все задачи обязательны к выполнению для получения зачета, кроме задач со звездочкой. Присылать на проверку можно каждую задачу по отдельности или все задачи вместе. Во время проверки по частям ваша домашняя работа будет со статусом "На доработке".
* Любые вопросы по решению задач задавайте в чате Slack (ссылку вы найдете в письме на вашей эл. почте).

## Оформление результата:

[Инструкция по использованию Pull Request для сдачи дз](https://github.com/netology-code/iosint-homeworks/blob/main/Pull%20request's%20guideline.md)

## Задача 1: паттерны проектирования

* Создаем Singleton с названием `Checker` (или любым другим), с 1 методом для проверки логина и пароля. Singleton - класс или структура, на ваш выбор. Пусть свойства (логин и пароль) существуют в hardcode виде, по выбору студента (`let login = "Vasily", let pswd = "Masha"` и тд)
* Для `LoginViewController` прописываем протокол делегата `LoginViewControllerDelegate`. У делегата 2 метода - проверка логина отдельно, пароля отдельно. 
* Создаем произвольный класс/структуру `LoginInspector` (придумайте свое название), который реализует протокол `LoginViewControllerDelegate`, реализуем в нем протокольные методы. 
* `LoginInspector` проверяет точность введенного пароля с помощью синглтона `Checker`. 
* Важный момент: чтобы делегат мог вернуть контроллеру результат проверки логина и пароля, в методах протокола делегата должны быть коллбэки/замыкания 

Для доступа к контроллерам в коде воспользуемся штатным `SceneDelegate` и его стандартным методом.

```
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        guard let _ = (scene as? UIWindowScene) else { return }
        
        // инициализация LoginInspector
        
        if let tabController = window?.rootViewController as? UITabBarController, let loginNavigation = tabController.viewControllers?.last as? UINavigationController, let loginController = loginNavigation.viewControllers.first as? LogInViewController {
            loginController.delegate = // экземпляр LoginInspector
        }
    }
```


