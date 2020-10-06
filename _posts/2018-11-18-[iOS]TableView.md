---
title: "[iOS] TableView(ListView in Android)"
date: 2018-11-18 14:26:28
categories: iOS
permalink: /iOS/
---

# TableView
**TableView**의 사용법에 대해 알아보겠습니다. iOS의 TableView는 Android의 ListView와 유사합니다. 그럼 어떻게 사용하는지 같이 확인해 보겠습니다.

# Usage
우선 UITableViewDelegate, UITableViewDataSource를 구현해주어야 합니다.
UITableViewCell(RowCell)을 CustomCell 클라스에 연결(Custom Class에서 CustomCell로 세팅) 시켜줍니다. 그러면, storyboard의 각 component(image, label)을 ViewController에 맴핑 시킬 수 있습니다. dequeueReusableCell로 "RowCell" indentifier 을 가져오면, 각 행을 불러오게 됩니다. 그러면, 각 component에 값을 setting 할 수 있습니다.

[ViewController.swift]
```python
import UIKit

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {

    var Users = [UserDTO]();

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return Users.count;
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

        let cell = tableView.dequeueReusableCell(withIdentifier: "RowCell", for: indexPath) as! CustomCell

        cell.profileImage.image = UIImage(named: Users[indexPath.row].image)
        cell.profileImage.layer.cornerRadius = (cell.profileImage.frame.width) / 2
        cell.profileImage.layer.masksToBounds = true

        cell.profileName.text = Users[indexPath.row].name
        cell.profileMessage.text = Users[indexPath.row].message

        cell.bubbleSpeech.translatesAutoresizingMaskIntoConstraints = false

        cell.bubbleSpeech.leftAnchor.constraint(equalTo: cell.profileMessage.leftAnchor, constant: -10).isActive = true
        cell.bubbleSpeech.topAnchor.constraint(equalTo: cell.profileMessage.topAnchor, constant: -10).isActive = true
        cell.bubbleSpeech.rightAnchor.constraint(equalTo: cell.profileMessage.rightAnchor, constant: 10).isActive = true
        cell.bubbleSpeech.bottomAnchor.constraint(equalTo: cell.profileMessage.bottomAnchor, constant: 10).isActive = true

        cell.bubbleSpeech.layer.cornerRadius = 10
        cell.bubbleSpeech.layer.masksToBounds = true

        return cell
    }

    override func viewDidLoad() {
        super.viewDidLoad()

        Users.append(UserDTO(image: "market", name: "peter", message: "hard to learn"))
        Users.append(UserDTO(image: "orange", name: "serena", message: "easy to learn"))
        Users.append(UserDTO(image: "strawberry", name: "study", message: "make to learn"))
    }
}

class CustomCell: UITableViewCell {
    @IBOutlet weak var profileImage: UIImageView!
    @IBOutlet weak var profileName: UILabel!
    @IBOutlet weak var profileMessage: UILabel!
    @IBOutlet weak var bubbleSpeech: UIView!
}

```
***
UserDTO 클래스를 생성하여, 값을 넣을 수 있다.

```
var Users = [UserDTO]();

Users.append(UserDTO(image: "market", name: "peter", message: "hard to learn"))
Users.append(UserDTO(image: "orange", name: "serena", message: "easy to learn"))
Users.append(UserDTO(image: "strawberry", name: "study", message: "make to learn"))
```


[UserDTO.swift]
```java
class UserDTO {
    var image :String!
    var name :String!
    var message :String!

    init(image :String, name :String, message :String!) {
        self.image = image
        self.name = name
        self.message = message
    }
}
```
