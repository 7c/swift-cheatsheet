## Swift Cheatsheet

## XHR Requests
### Alamofire
[Docs](https://alamofire.github.io/Alamofire/)
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