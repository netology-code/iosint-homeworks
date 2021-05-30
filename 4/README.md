# Домашнее задание к занятию 4 	Структурные паттерны: Delegate, Singleton, Factory

## Правила выполнения домашней работы:

* Базовый проект для выполнения домашней работы - последний проект с домашней работой курса iOSUI, где присутствуют LoginViewController + FeedViewController.
* Все задачи обязательны к выполнению для получения зачета, кроме задач со звездочкой. Присылать на проверку можно каждую задачу по отдельности или все задачи вместе. Во время проверки по частям ваша домашняя работа будет со статусом "На доработке".
* Любые вопросы по решению задач задавайте в чате Slack (ссылку вы найдете в письме на вашей эл. почте).

## Оформление результата:

[Инструкция по использованию Pull Request для сдачи дз](https://github.com/netology-code/iosint-homeworks/blob/main/Pull%20request's%20guideline.md)

## Задача 1: паттерны проектирования Delegate + Singleton

1. Создаем сервис для проверки логина и пароля `Checker` или любым другим названием, у сервиса (это Singleton) будет 1 __интерфейсный метод__ для проверки логина и пароля 
2. Пусть свойства (логин и пароль) существуют, для начала, в hardcode виде, по выбору студента (`private let login = "Vasily", private let pswd = "StrongPassword"` и тд). Важно, что свойства __приватные__. Для тех, кто любит задачи со звездочкой *, можно проверять не значения свойств напрямую, а сгенерированный hash. Для этого достаточно вызвать у строки библиотечный метод Swift* `МояСтрока.hash` Проверка хэша - необязательное условие, ДЗ будет принято, как обычно, если будет проверяться plain String. 
3. Для `LoginViewController` прописываем протокол делегата `LoginViewControllerDelegate`. Метод делегата проверяет значения, введенные в 2 `UITextField` контроллера. Напрямую вызывать из контроллера сервис `Checker` нельзя! 
4. Создаем произвольный класс/структуру `LoginInspector` (или придумайте свое название), который подписывается на протокол `LoginViewControllerDelegate`, реализуем в нем протокольный метод. 
5. `LoginInspector` проверяет точность введенного пароля с помощью синглтона `Checker`. 
6. Важный момент: чтобы делегат мог сообщить контроллеру результат проверки логина и пароля, методы протокола делегата будут содержать возвращаемое значение
7. Внедрите зависимость контроллера от `LoginInspector`, то есть присвойте значение свойству делегата в `SceneDelegate` или `AppDelegate`

Для доступа к контроллерам в коде воспользуйтесь штатным `SceneDelegate` и его стандартным методом. Или `AppDelegate`, если кто-то ранее избавился от `SceneDelegate`. С UI тоже возможны варианты: проще всего будет тем, кто использует верстку кодом. Вариант, как добраться до контроллеров через Storyboard, ниже: 

```
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        guard let _ = (scene as? UIWindowScene) else { return }
        
        // инициализация LoginInspector
        
        if let tabController = window?.rootViewController as? UITabBarController, let loginNavigation = tabController.viewControllers?.last as? UINavigationController, let loginController = loginNavigation.viewControllers.first as? LogInViewController {
            loginController.delegate = // экземпляр LoginInspector
        }
    }
```

## Задача 2: 

1. Создайте `protocol LoginFactory` с 1 методом без параметров, который возвращает `LoginInspector`
2. Вынесите генерацию `LoginInspector` из `SceneDelegate (или AppDelegate)` в фабрику: создайте объект `MyLoginFactory` (название на ваше усмотрение), подпишите на протокол
3. Инициализируйте в `SceneDelegate / AppDelegate` только фабрику 
4. Внедрите зависимость контроллера от `LoginInspector`, создав инспектора с помощью фабричного метода
