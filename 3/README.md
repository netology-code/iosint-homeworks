# Домашнее задание к занятию 1.3 	Паттерны и архитектура мобильных приложений

## Правила выполнения домашней работы:

* Базовый проект для выполнения домашней работы - последний проект с домашней работой курса iOSUI, где присутствуют LoginViewController + FeedViewController
* При сдаче домашнего задания в поле для сдачи работы прикрепите ссылку на Pull Request с кодом на проверку внутри вашего проекта в Git.
* Все задачи обязательны к выполнению для получения зачета, кроме задач со звездочкой. Присылать на проверку можно каждую задачу по отдельности или все задачи вместе. Во время проверки по частям ваша домашняя работа будет со статусом "На доработке".
* Любые вопросы по решению задач задавайте в чате Slack (ссылку вы найдете в письме на вашей эл. почте).

## Задача 1: паттерны проектирования

* Создаем Singleton с названием `Checker` (или любым другим), с 1 методом для проверки логина и пароля. Singleton - класс или структура, на ваш выбор. Пусть свойства (логин и пароль) существуют в hardcode виде, по выбору студента (`let login = "Vasily", let pswd = "Masha"` и тд)
* Для `LoginViewController` прописываем протокол делегата `LoginViewControllerDelegate`. У делегата 2 метода - проверка логина отдельно, пароля отдельно. 
* Создаем произвольный класс/структуру `LoginInspector` (придумайте свое название), который реализует протокол `LoginViewControllerDelegate`, реализуем в нем протокольные методы. 
* `LoginInspector` проверяет точность введенного пароля с помощью синглтона `Checker`. 
* Важный момент: чтобы делегат мог сообщить контроллеру результат проверки логина и пароля, методы протокола делегата будут содержать возвращаемое значение

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

## Задача 2: 

Оптимально сделать: 
* на базе версии домашнего задания UI, где в `FeedViewController` у вас есть 2 кнопки
* в новом проекте с `UITabBarController`, где повторяется навигация домашнего задания из блока UI с `FeedViewController` -> `PostViewController`
 
Кнопки выполняют задачи: 
1. создание нового PostViewController
2. отображение (навигация - push) нового PostViewController

Задание: 
* Вынести кнопки из контроллера в отдельный дочерний файл/класс `UIView / UIStackView`, возможна разная реализация, в дальнейшем назовем `ContainerView`
* Реализовать action-методы (`@objc private func() {}`) кнопок в этом классе
* Каждый action-метод будет вызывать замыкание `onTap: (() -> Void)?`. Название произвольное - обычно применяются `onAction, onSelect, onTap...`
Единственным самодельным элементом интерфейса `ContainerView` будет это замыкание. Имеется в виду, что остальной интерфейс штатный, наследуется от родителя.
* Определить замыкание `onTap` надо в контроллере `FeedViewController`, когда будете определять/конфигурировать `ContainerView`. В замыкании будет только вызов единственного метода свойства `output` (тип `FeedViewOutput`). 
* Создаем свойство `output` у контроллера `FeedViewController` - оно будет протокольного типа `FeedViewOutput`. 
* Инъекция зависимости `FeedViewController` от `FeedViewOutput` происходит через инициализатор.
* У вашего протокола `FeedViewOutput` будет 1 метод `showPost()` (название на ваш вкус), в котором нужно инициализировать `PostViewController`. А также 1 свойство `var navigationController: UINavigationController? { get set }`
* Класс `PostPresenter`, реализуя протокол `FeedViewOutput`, создает `PostViewController`, а далее использует протокольное свойство `navigationController`, чтобы презентовать `PostViewController`. Чтобы `PostPresenter` мог использовать свойство `navigationController` от `FeedViewController`, нужно передать `navigationController` от `FeedViewController` -> `PostPresenter`. Например, в `viewDidLoad()` контроллера.
* Доступ к `FeedViewController` и `PostPresenter` возможен через `SceneDelegate`, как в первой задаче. Главный нюанс в том, что `FeedViewController` нужно инициализировать программно, и инжектить экземпляр презентера при инициализации контроллера или через свойство. 

Откуда у `FeedViewController` свойство `navigationController`? Например, в `storyboard`, когда вы создавали `FeedViewController`, он "обернут" (Embed in...) в `UINavigationController`, поэтому у `FeedViewController` свойство `navigationController` не `nil`. Его метод `.push` можно применять. 

Когда вы создаете первоначальный стек `UITabBarController` программно, в качестве `child` контроллеров вы используете `UINavigationController`. 
`UITabBarController` -> `UINavigationController` -> `.rootViewController: FeedViewController` 
У `FeedViewController` появится рабочее свойство `navigationController?` 

*Какие альтернативы есть для построения навигации не через `SceneDelegate`?* 
* Создать файл / класс / протокол `AppCoordinator`
* инициализировать экземпляр `AppCoordinator` в `SceneDelegate`
* последний пункт задачи: "Доступ к `FeedViewController` и `PostPresenter` возможен через... - реализовать в координаторе
