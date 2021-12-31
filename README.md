[SwiftUI](swiftui.md)
# Swift Cheatsheet


## Extension
```swift
extension Date {
    // static extension
    static func fromNodetime(_ ts:Int) -> Date {
        return Date(timeIntervalSince1970: Double(ts/1000))
    }
    // instanciated extension
    func yesterday() -> Date? {
        return self.add(days:-1)
    }
}
```


## DateTime
```swift
extension Date {
    // static method
    static func fromNodetime(_ ts:Int) -> Date {
        return Date(timeIntervalSince1970: Double(ts/1000))
    }
    func add(years: Int = 0, months: Int = 0, days: Int = 0, hours: Int = 0, minutes: Int = 0, seconds: Int = 0) -> Date? {
            let comp = DateComponents(year: years, month: months, day: days, hour: hours, minute: minutes, second: seconds)
            return Calendar.current.date(byAdding: comp, to: self)
    }
    
    func yesterday() -> Date? {
        return self.add(days:-1)
    }
}

// yesterdays date
Date().yesterday()
// import from nodejs timestamp to swift
Date.fromNodetime(1640805629848)
```

## XHR Requests
### Alamofire
Alamofire is an HTTP networking library written in Swift. [Features](https://github.com/Alamofire/Alamofire#features), [Docs](https://alamofire.github.io/Alamofire/)
```swift
import Alamofire
//
// basic GET
AF.request("https://ip8.com/echo",method:.get).response { response in
    debugPrint(response)
}

// basic GET with querystring - it is same as calling the url with ?test=1
var qs:[String:Any]=["test":1]
AF.request("https://ip8.com/echo",method:.get,parameters:qs ).response { response in
    debugPrint(response)
}

// GET with url enccoded parameters inline
AF.request("https://ip8.com/echo",parameters: ["key":"value2"]).response { response in
     debugPrint(response)
}


// post json encoded parameters
struct Login : Encodable {
    let email : String
    let password : String
}
AF.request("https://ip8.com/echo",
           method:.post,
           parameters: Login(email:"user@test.com",password:"secret"),
           encoder:JSONParameterEncoder.default).response { response in
    debugPrint(response)
}

// post url encoded parameters $_POST
AF.request("https://ip8.com/echo",method:.post,parameters: ["foo":["bar"],"baz":["a","b"]]).response { response in
    debugPrint("post url encoded parameters",response)
}

// POST with headers
let headers: HTTPHeaders = [
    "Authorization": "Basic VXNlcm5hbWU6UGFzc3dvcmQ=",
    "Accept": "application/json"
]


AF.request("https://ip8.com/echo",method:.post,headers:headers).response { response in
    // Accessing body and statuscode from response
    if let statuscode = response.response?.statusCode {
        print("Statuscode: \(statuscode)")
        if statuscode==200 {
            if let data = response.data  {
                print("Body:")
                print(String(decoding:data, as: UTF8.self))
            }
        }
        
    }
}

```