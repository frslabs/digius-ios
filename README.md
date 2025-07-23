# DIGIUS SDK (iOS)
![version](https://img.shields.io/badge/version-v1.0.4-blue)


Digius SDK is a one-shot solution to securely fetch documents from DigiLocker. It helps to eliminate use of physical documents and enables sharing of e-documents which is stored in DigiLocker account.

You can find the latest changelog at [Changelog](CHANGELOG.md).

# Table Of Content

- [Prerequisite](#prerequisite)
- [Set Up Digius SDK](#set-up-digius-sdk)
    - [Requirements](#requirements)
    - [Install Digius SDK](#install-digius-sdk)
- [Getting Started](#getting-started)
- [Digius Result](#digius-result)
- [Digius Error Codes](#digius-error-codes)
- [Help](#help)

## Prerequisite

You will need a valid license to use the Digius SDK, which can be obtained by contacting `support@frslabs.com` .

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.

Once you have the license , follow the below instructions for a successful integration of Digius SDK onto your iOS Application.

## Set Up Digius SDK

#### Requirements
Make sure that your project meets these requirements:
- Xcode 13.0
- iOS 13.0+
- Swift 5.0

#### Install Digius SDK

### Cocoapods


You can use [CocoaPods](http://cocoapods.org/) to install `digius` by adding it to your `Podfile`:

```ruby
source 'https://gitlab.com/frslabs-public/ios/digius-ios.git'
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '13.0'
target '<Your Target Name>' do
use_frameworks!
pod 'Digius', '1.0.4'
end
```
###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install digius from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine www.repo2.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod 

   pod install or pod update or pod install --repo-update.

To get the full benefits import `Digius` wherever you import UIKit

``` swift
import UIKit
import Digius
```
## Getting Started

### Swift

1. Initialize the input parameters and import delegate DigiusControllerDelegate

```swift
class YourViewController: UIViewController,DigiusControllerDelegate {

    func digiusControllerSuccess(_ scanner: Digius.DigiusController, didFinishScanningWithResults results: Digius.digiusResult) {
        var getData = [String:Any]()
        getData = (results.result) // Return the document result
        print(results.digiusDocumentType) // Return type of document
    }
    
    func digiusControllerFailed(_ scanner: Digius.DigiusController, didFailWithError error: Int) {
        print(error)
    }
}
```

2. Invoke Digius SDK

```swift
    // ...
    
    override func viewDidLoad(_ animated: Bool) {
        let digius = DigiusController(showInstruction: true, delegate:self)
        digius.modalPresentationStyle = .fullScreen
        digius.licenceKey = "DIGIUS_LICENCE_KEY"
        present(digius, animated: false, completion: nil)
    }
    
    // ...    
```

## Digius Result

You can use the following methods in the `DigiusResult` instance to parse the success result:

| Return Type              | Method                        | Usage                                                            |
| ------------------------ | ----------------------------- | ---------------------------------------------------------------- |
| DigiusDocumentType | results.digiusDocumentType          | Returns selected document type                                   |
| AadhaarResult            |results.result            | Returns aadhaar data if document type is aadhaar                 |
| PanResult | results.result| Returns pan data if document type is pan | 

`results.result` returns `AadhaarResult` instance with following methods if the document selected is Aadhaar:

```swift
    var getAadharData = [String:Any]()
    getAadharData = (results.AadhaarResult)
```

| Return Type | Method                               | Usage                            |
| ----------- | ------------------------------------ | ---------------------------------|
| String      | *getAadharData["documentNumber"]*                | Returns document number          |
| String      | *getAadharData["name"]*                          | Returns document's name          |
| String      | *getAadharData["age"]*                           | Returns age                      |
| String      | *getAadharData["dob"]*                           | Returns date of birth            |
| String      | *getAadharData["gender"]*                        | Returns gender                   |
| String      | *getAadharData["address"]*                       | Returns full address             |
| String      | *getAadharData["house"]*                         | Returns house name               |
| String      | *getAadharData["landmark"]*                      | Returns landmark                 |
| String      | *getAadharData["location"]*                      | Returns location                 |
| String      | *getAadharData["district"]*                      | Returns district                 |
| String      | *getAadharData["state"]*                         | Returns state                    |
| String      | *getAadharData["country"]*                       | Returns country                  |
| String      | *getAadharData["pincode"]*                       | Returns pincode                  |
| String      | *getAadharData["photo"]*                         | Returns user's photo in BASE64 String            |
| String      | *getAadharData["lastFourDigits"]*         | Returns last four digits of aadhaar     |
| String      | *getAadharData["issuedDate"]*             | Returns aadhaar issued date and time     |

PHOTO: Convert BASE64 into Image
```swift
  func convertBase64StringToImage (imageBase64String:String) -> UIImage {
      let imageData = Data(base64Encoded: imageBase64String)
      let image = UIImage(data: imageData!)
      return image!
  }
  
  convertBase64StringToImage(imageBase64String: (getAadharData["photo"]) as! String)
```

`results.result` returns `PanResult` instance with following methods if the document selected is PAN:

```swift
    var getPanData = [String:Any]()
    getPanData = (results.result)
```

| Return Type | Method                               | Usage                            |
| ----------- | ------------------------------------ | ---------------------------------|
| String      | *getPanData["documentNumber"]*                | Returns document number          |
| String      | *getPanData["name"]*                          | Returns document's name          |
| String      | *getPanData["dob"]*                           | Returns date of birth            |
| String      | *getPanData["gender"]*                        | Returns gender                   |
| String      | *getPanData["verifiedOn"]*             | Returns pan verified date     |


Available document types are

  | Method                      | Value |  Document            |
  | --------------------------- | ------- | ------------------- |
  | results.digiusDocumentType  | AADHAAR |  Aadhaar Card        |
  | results.digiusDocumentType  | PAN |  Pan Card        |


## Digius Error Codes

Following error codes will be returned on the `didFailWithError` method of the callback

| CODE | DESCRIPTION                                     |
| ---- | ------------------------------------------------|
| 401  | Invalid licence                                 |
| 402  | Licence expired                                 |
| 403  | User canceled                                   |
| 404  | Unable to ping                                  |
| 405  | Transaction limit exceeded                      |
| 406  | No internet connection                          |
| 407  | Document read permission denied                 |
| 408  | Invalid settings                                |
| 409  | Failed to retrieve data                         |



## Help
For any queries/feedback , contact us at `support@frslabs.com`
