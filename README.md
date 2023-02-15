# DIGIUS SDK (iOS)
![version](https://img.shields.io/badge/version-v1.1.1-blue)


Digius SDK is a one-shot solution to securely fetch documents from DigiLocker. It helps to eliminate use of physical documents and enables sharing of e-documents which is stored in DigiLocker account.

You can find the latest changelog at [Changelog](CHANGELOG.md).

# Table Of Content

- [Prerequisite](#prerequisite)
- [Set Up Digius SDK](#set-up-digius-sdk)
    - [Requirements](#requirements)
    - [Install Digius SDK](#install-digius-sdk)
- [Quick Start](#quick-start)
    - [Initialise and run the Digius SDK](#initialise-and-run-the-digius-sdk)
- [Digius Result](#digius-result)
- [Digius Error Codes](#digius-error-codes)
- [Digius SDK API](#digius-sdk-api)
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

#### Importing Digius SDK

Add the following code to your `app-level` `build.gradle` file



And then, add the dependencies

## Quick Start

#### Initialise and run the Digius SDK

Initialize an instance of `Digius` with the appropriate parameters, and call `start()` to invoke the Digius SDK. Refer [Digius SDK API](#digius-sdk-api) for more information regarding the individual APIs.

Given below is the fully configured Digius SDK invokation.


For all `errorCode`'s and their meanings refer [Digius Error Codes](#digius-error-codes).

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

## Digius SDK API

See the below table for the public APIs of Digius SDK,

##### Digius
| Method/Constructor                                   | Comments    |
|:---------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Digius(DigiusConfig digiusConfig)                                                 | Instantiates the Digius Object |    
| *start(Context activityContext, DigiusResultCallback digiusResultCallback)*   | Starts the Digius SDK |


##### DigiusConfig
`DigiusConfig.Builder()` allows to instantiate the `DigiusConfig` object with customisable features. `DigiusConfig` is to be set when instantiating `Digius` object , See [Digius](#digius)
| Method                                               | Default              | Required | Comments    |
|:---------------------------------------------------- |:-------------------- |:-------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| *setLicenceKey(String licenseKey)*                   | NULL                 | Yes      | Sets the License Key needed for Digius SDK                          |
| *setShowInstructions(boolean showInstructions)*      | false                | Optional      | If it is **true** then the instruction screen will be shown to the user before digilocker screen.                         |
| *build()*   | -               | -      | Builds DigiusConfig Instance  |



## Help
For any queries/feedback , contact us at `support@frslabs.com`
