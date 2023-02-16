# DIGIUS SDK (iOS)
![version](https://img.shields.io/badge/version-v1.0.0-blue)


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
pod 'Digius', '1.0.0'
end
```
###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install digius from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine digius-ios.repo.frslabs.space
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
        var getAadharData = [String:Any]()
        getAadharData = (results.AadhaarResult)
        resultTextView.text = "Name:\(getAadharData["name"] ?? ""), \n Age: \(getAadharData["age"] ?? ""), \n DOB: \(getAadharData["dob"] ?? ""), \n Gender: \(getAadharData["gender"] ?? ""), \n Address: \(getAadharData["address"] ?? ""), \n House: \(getAadharData["house"] ?? ""), \n Landmark: \(getAadharData["landmark"] ?? ""), \n Location: \(getAadharData["location"] ?? ""), \n District: \(getAadharData["district"] ?? ""), \n State: \(getAadharData["state"] ?? ""), \n Country: \(getAadharData["country"] ?? ""), \n Pincode: \(getAadharData["pincode"] ?? ""), \n LastFourDigits: \(getAadharData["lastFourDigits"] ?? ""), \n DocumentNumber: \(getAadharData["documentNumber"] ?? ""), \n IssuedDate: \(getAadharData["issuedDate"] ?? ""), \n Photo: \(getAadharData["photo"] ?? "")"
        
        imageView.image = convertBase64StringToImage(imageBase64String: (getAadharData["photo"]) as! String)
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
| DigiusDocumentType(Enum) | *getDocumentType()*           | Returns selected document type                                   |
| AadhaarResult            | *getAadhaarData()*            | Returns aadhaar data if document type is aadhaar                 |

`getAadhaarData()` returns `AadhaarResult` instance with following methods:

| Return Type | Method                               | Usage                            |
| ----------- | ------------------------------------ | ---------------------------------|
| String      | *getDocumentNumber()*                | Returns document number          |
| String      | *getName()*                          | Returns document's name          |
| String      | *getAge()*                           | Returns age                      |
| String      | *getDob()*                           | Returns date of birth            |
| String      | *getGender()*                        | Returns gender                   |
| String      | *getAddress()*                       | Returns full address             |
| String      | *getHouse()*                         | Returns house name               |
| String      | *getLandmark()*                      | Returns landmark                 |
| String      | *getLocation()*                      | Returns location                 |
| String      | *getDistrict()*                      | Returns district                 |
| String      | *getState()*                         | Returns state                    |
| String      | *getCountry()*                       | Returns country                  |
| String      | *getPincode()*                       | Returns pincode                  |
| String      | *getPhoto()*                         | Returns user's photo             |
| String      | *getAadhaarLastFourDigits()*         | Returns last four digits of aadhaar     |
| String      | *getAadhaarIssuedDate()*             | Returns aadhaar issued date and time     |

Available document types are

  | Value                       | Document            |
  | --------------------------- | ------------------- |
  | DigiusDocumentType.AADHAAR  | Aadhaar Card        |


## Digius Error Codes

Following error codes will be returned on the `onError` method of the callback

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
