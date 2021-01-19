# Домашнее задание к занятию 6. Operation, OperationQueue, GCD

## Оформление домашней работы:
* При сдаче лекции прикрепите ссылку на гитхаб с выполненным заданием.

[Инструкция по использованию Pull Request для сдачи дз](https://github.com/netology-code/iosint-homeworks/blob/main/Pull%20request's%20guideline.md)

**Важно!** В этом задании успешным выполнением ДЗ будет считаться выполнение **любого из перечисленных ниже пунктов - НЕ ВЫПОЛНЯЙТЕ ВСЕ** (игнорируйте этот пункт, если задача для вас очень интересная, и вы хотите попробовать все).
* Любые вопросы по решению задач задавайте в чате Slack (ссылку вы найдете в письме на вашей эл. почте).


## Дополнительные материалы по теме лекции:
В дополнение к материалам лекции мы рекомендуем вам изучить 2 статьи. В них очень хорошо рассказано, что такое GCD и Operation:
* https://habr.com/ru/post/350096/
* https://habr.com/ru/post/320152/


В прошлой лекции вы познакомились с многопоточностью и поняли, где можете использовать ее в своем приложении. Возможно, у вас нет проекта, где производились действия, которые можно было бы вынести в отдельную асинхронную очередь. Далее будут вариант сегодняшнего домашнего задания. Выберете то, что подходит вам:

### Вариант 1
Если у вас есть в проекте запросы к бд / в сеть / обработка изображений, то вынесите это с помощью GCD или Operation с главного потока, чтобы оно выполнялось параллельно. 
Если для запросов вы пользуетесь фреймворками, то откажитесь от них на время и создайте запросы с помощью встроенных средств xCode, например:

    let url = URL(string: "http://www.stackoverflow.com")!
    
    let task = URLSession.shared.dataTask(with: url) {(data, response, error) in
        guard let data = data else { return }
        print(String(data: data, encoding: .utf8)!)
    }

    task.resume()
    

**Важно! НИ В КОЕМ СЛУЧАЕ НЕ ИСПОЛЬЗУЙТЕ FORCE UNWRAP**

https://stackoverflow.com/questions/24016142/how-do-i-make-an-http-request-in-swift

### Вариант 2
Если у вас нет такого проекта, то задание будет состоять в том, что вы с помощью [запроса](https://jsonplaceholder.typicode.com/photos) забираете JSON с сревера, вот такого вида
[
  {
    "albumId": 1,
    "id": 1,
    "title": "accusamus beatae ad facilis cum similique qui sunt",
    "url": "https://via.placeholder.com/600/92c952",
    "thumbnailUrl": "https://via.placeholder.com/150/92c952"
  }
  ........
  ]
 Это, по сути представляет из себя массив элементов - 
 {
    "albumId": 1,
    "id": 1,
    "title": "accusamus beatae ad facilis cum similique qui sunt",
    "url": "https://via.placeholder.com/600/92c952",
    "thumbnailUrl": "https://via.placeholder.com/150/92c952"
  },
  
  Здесь есть поле url. По этому полю вы без труда сможете загрузить картинку (это тоже надо делать асинхронно)
 <details>
<summary>Интрукция как скачивать картинку</summary>
/// забираете данные из сети по URL
func getData(from url: URL, completion: @escaping (Data?, URLResponse?, Error?) -> ()) {
    URLSession.shared.dataTask(with: url, completionHandler: completion).resume()
}


/// преобразуете их в картинку
func downloadImage(from url: URL) {
    print("Download Started")
    getData(from: url) { data, response, error in
        guard let data = data, error == nil else { return }
        print(response?.suggestedFilename ?? url.lastPathComponent)
        print("Download Finished")
        DispatchQueue.main.async() { [weak self] in
            self?.imageView.image = UIImage(data: data)
        }
    }
}

/// пример использования
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
    print("Begin of code")
    let url = URL(string: "https://via.placeholder.com/600/92c952")! // сюда вставляете URL
    downloadImage(from: url)
    print("End of code. The image will continue downloading in the background and it will be loaded when it ends.")
}

//// Или можете написать extension к UIImageView
extension UIImageView {
    func downloaded(from url: URL, contentMode mode: UIView.ContentMode = .scaleAspectFit) {
        contentMode = mode
        URLSession.shared.dataTask(with: url) { data, response, error in
            guard
                let httpURLResponse = response as? HTTPURLResponse, httpURLResponse.statusCode == 200,
                let mimeType = response?.mimeType, mimeType.hasPrefix("image"),
                let data = data, error == nil,
                let image = UIImage(data: data)
                else { return }
            DispatchQueue.main.async() { [weak self] in
                self?.image = image
            }
        }.resume()
    }
    func downloaded(from link: String, contentMode mode: UIView.ContentMode = .scaleAspectFit) { 
        guard let url = URL(string: link) else { return }
        downloaded(from: url, contentMode: mode)
    }
}

imageView.downloaded(from: "Здесь ваш URL")
</details>
  
  
(они там хранятся как url), а дальше выберите одну из опций:
 
1) простая - вы создаете tableView в пустом проекте, в ячейки выводите url фотографий%
2) сложная - вы создаете collectionView? выводите в нее все фотографии, загруженные по URL.

Для парсинга JSON можете пользоваться [SWIFTYJSON](https://github.com/SwiftyJSON/SwiftyJSON) или чем вам будет удобно.

3) Если вас не устраивает ни одна из опций выше, то загрузите к себе в проект (в xcassets) любые 5-10 картинок. А потом просто создайте коллекцию, в которую вы будете их выводить. Только перед этим наложите на них фильтр. Любой, например [сепия](https://developer.apple.com/documentation/coreimage/processing_an_image_using_built-in_filters)

<details>
<summary>Подсказка</summary>
Это сделано здесь https://habr.com/ru/post/320152/

</details>

### Вариант 3
Если вы все еще в замешательстве, то подумайте еще раз над своим проектом: куда можете добавить GCD или Operation и задавайте вопросы лектору в чате Slack. 

## Дополнительные материалы
Это учебный проект от Максима Абакумова. Этот проект нужен вам для того, чтобы просто попрактиковаться в изучении много поточности. При первом запуске вы получите рантайм ошибку, из-за того, что у вас UI меняется не в главном потоке. Можете ее поправить. Это тоже будет считаться выполнением ДЗ
Учебный проект - https://github.com/TrueMax/iOSIntPackage
