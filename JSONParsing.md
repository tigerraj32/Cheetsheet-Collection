#  JSON Parsing


## Representing JSON string in swift

In order to represent json string in swift we will put multi-line json formatted string inside """

```swift
let personJson = """
{
    "name": "Rajan",
    "gender": "Male",
    "age": 30,
    "partner": "Rama"
    "website":"http://rajantwanabashu.com.np"
}
"""

print(personJson)
```

## Decoding JSON String

We will require 3 steps to decode json string
- Prepare **Person**  struct  to represent json string. Conform this struct to **Decodable** which will convert all the value  types to swift types as needed. 
- Convert json string to data
- Decode json data using **JSONDecoder()** 

```swift
struct Person: Decodable {
    let name: String
    let gender: String
    let age: Int
    let partner: String
    let website: URL
}


let personJSONData = personJson.data(using: .utf8)!
let decoder = JSONDecoder()
let person = try! decoder.decode(Person.self, from: personJSONData)

 print(person.name)
```
Output
>Rajan

## Handling optional key-value pair in Json string

Following json have missing **partner** and **website** information.  This will require to change our **person struct** to have optional **partne and website** property.

```swift
let personJson = """
{
    "name": "Rajesh",
    "gender": "Male",
    "age": 30,
    
}
"""

struct Person: Decodable {
    let name: String
    let gender: String
    let age: Int
    let partner: String?
    let website: URL?
}

let personJSONData = personJson.data(using: .utf8)!
let decoder = JSONDecoder()
let person = try! decoder.decode(Person.self, from: personJSONData)
priint(person.partner) 
```
Output
>nil


## Handle JSON Array

Following json consist of  array of persons. This will require to pass array of **Person** to JSONDecoder.

```swift
let personsJson = """
[
  {
    "name": "Rajan",
    "gender": "Male",
    "age": 30,
    "partner": "Rama"
  },
  {
    "name": "Rajesh",
    "gender": "Male",
    "age": 30,
    
  }
]
"""

let personsJSONData = personsJson.data(using: .utf8)!

let decoder = JSONDecoder()
let persons = try! decoder.decode([Person].self, from: personsJSONData)

for person in persons {
    print(person.name)
}
```
Output
> Rajan

>Rajesh


## Handling more complex JSON String

Following json consist of family **surname** as string and **members** as array of **person**. So we will require to create new struct  **Family** that also conform to **Decodable** protocol and finally we will pass Family Object to JSONDecoder() 

```swift
let familyJson = """
{
  "surname": "Twanabashu",
  "members": [
    {
      "name": "Rajan",
      "gender": "Male",
      "age": 30,
      "partner": "Rama"
    },
    {
      "name": "Rajesh",
      "gender": "Male",
      "age": 30,
      
    }
  ]
}
"""

struct Family: Decodable {
    let surname: String
    let members: [Person]
}

let familyJSONData = familyJson.data(using: .utf8)!

let decoder = JSONDecoder()
let family = try! decoder.decode(Family.self, from: familyJSONData)

for person in family.members {
    print(person.name)
}

```
Output
> Rajan

>Rajesh

### Better way of creating **Family** Struct

Here we can crete **Person** struct  inside **Family** struct.  If we look closely, the gender can have only three values here (Male, Female and other). So we can create new enum **Gender** with 3 cases and change the property type of **gender to Gender** type in **Person** struct.
Rest will be handled by **JSONDecoder** and **Decodable Protocol**.
```swift
truct Family: Decodable {
    enum Gender: String, Decodable {
        case Male, Female, Other
    }
    struct Person: Decodable {
        let name: String
        let gender: Gender
        let age: Int
        let partner: String?
    }
    let surname: String
    let members: [Person]
}

```

## Parsing JSON file

Let assume that we have a json file **Family.json** which contain json string that is same as **familyJson**. So here's how we can read the file and decode

File: **Family.json**
```swift

{
  "surname": "Twanabashu",
  "members": [
    {
      "name": "Rajan",
      "gender": "Male",
      "age": 30,
      "partner": "Rama"
    },
    {
      "name": "Rajesh",
      "gender": "Male",
      "age": 30,
      
    }
  ]
}

```

```swift
guard let jsonFile = Bundle.main.url(forResource: "Family", withExtension: "json") else {
    fatalError("Could not parse json file")
}

guard let familyJSONData = try? Data(contentsOf: jsonFile) else{
    fatalError("Cound not convert to data")
}
let decoder = JSONDecoder()
let family = try! decoder.decode(Family.self, from: familyJSONData)

for person in family.members {
    print(person.name)
}
```


## Decoding Dates from JSON string

Following json consist of **name** and **birthdat**. Birthday will have **timestamp** value of date of birth.   To decode **timestamp** we will need to configure JSONDecoder **dateDecodingStrategy** to    **secondsSince1970**.

```swift
let personJson = """
{
    "name": "Rajan",
    "birthday": 433815285

}
"""

struct Person: Decodable {
    let name: String
    let birthday: Date
}

let personData = personJson.data(using: .utf8)!

let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .secondsSince1970

let person  = try! decoder.decode(Person.self, from: personData)

print(person.birthday)
```

Output:
> 1983-10-01 00:14:45 +0000

some of the other dateDecodingStrategy are  **.iso8601**,  **.millisecondsSince1970**

### Decoding JSOn with Custom Date Formatter.

If the json contains the actual date as string then we will need to create custom date formatter  and configure  dateDecodingStrategy to that custom date formatter. For eg. following json have birthday value to string that represent the readable date.

```swift
let personJson = """
{
    "name": "Rajan",
    "birthday": "1983-10-01"

}
"""
....

let formatter = DateFormatter()
formatter.dateFormat = "yyyy-MM-dd"

let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .formatted(formatter)

print(person.birthday)
```

Output:
> 1983-10-01 00:14:45 +0000

>Note: More  date format can be found at [nsdateformatter](https://nsdateformatter.com/)


## JSON CodingKeys
In the following json string the key are in **snake_case** and we are following same **snake_case** in **Person** Struct also.

```swift
let personJson = """
{
    "person_name": "Rajan"

}
"""

struct Person: Decodable {
    let person_name: String
    
}
```

But in reality we use **camelCase** in programming rather than **snake_case**. So  we will use another decoding stratedgy **keyDecodingStrategy**

```swift
let personJson = """
{
    "person_name": "Rajan"

}
"""

struct Person: Decodable {
    let personName: String
    
}

let personData = personJson.data(using: .utf8)!
let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

guard let person  = try? decoder.decode(Person.self, from: personData) else{
    fatalError("Invalid JSON format")
}

print(person.personName)
```

Output:
> Rajan


### Using different property name to represent  json keys

Some time its more convinent to use different  property name to represent the json key, because json keys some time gets to boaring and long. For this we will use  String enum that conform to CodingKey. And this enum consist of property that maps to each and every keys in json. Let's take a look at following code.

```swift
let personJson = """
{
    "person_name": "Rajan"

}
"""

struct Person: Decodable {
    enum CodingKeys: String, CodingKey {
        case name = "person_name"
    }
    let name: String
    
}

.......

print(person.name)
```

Output:
> Rajan



