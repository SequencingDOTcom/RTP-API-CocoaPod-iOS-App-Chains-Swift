# CocoaPod-iOS-App-Chains-Swift
App Chains are the easy way to code Real Time Personalization (RTP) into your app. 
Easily add an ```App-Chains``` functionality using this CocoaPods plugin for iOS apps coded in Swift


Contents
=========================================
* Introduction
* Example (of an app using RTP)
* CocoaPods plugin integration
* Configuration
* Troubleshooting
* Resources
* Maintainers
* Contribute


Introduction
=========================================
Search and find app chains -> https://sequencing.com/app-chains/

An app chain is an integration of an API call and an analysis of an app user's genes. Each app chain provides information about a specific trait, condition, disease, supplement or medication. App chains are used to provide genetically tailored content to app users so that the user experience is instantly personalized at the genetic level. This is called [Real Time Personalization (RTP)](https://sequencing.com/developer-documentation/what-is-real-time-personalization-rtp).

Each app chain consists of:

1. **API call**
 * API call that triggers an app hosted by Sequencing.com to perform genetic analysis on your app user's genes
2. **API response**
 * the straightforward, easy-to-use results are sent to your app as the API response
3. **Personalzation**
 * your app uses this information, which is obtained directly from your app user's genes in real-time, to create a truly personalized user experience

For example
* App Chain: It is very important for this person's health to apply sunscreen with SPF +30 whenever it is sunny or even partly sunny.
* Possible responses: Yes, No, Insufficient Data, Error

While there are already app chains to personalize most apps, if you need something but don't see an app chain for it, tell us! (ie email us: gittaca@sequencing.com).

To code Real Time Personalization (RTP) technology into apps, developers may [register for a free account](https://sequencing.com/user/register/) at Sequencing.com. App development with RTP is always free.


Example (of an app using RTP)
======================================
What types of apps can you personalize with app chains? Any type of app... even a weather app. 
* The open source [Weather My Way +RTP app](https://github.com/SequencingDOTcom/Weather-My-Way-RTP-App/) differentiates itself from all other weather apps because it uses app chains to provide genetically tailored content in real-time to each app user.
* Experience it yourself using one of the fun sample genetic data files. These sample files are provided for free to all apps that use app chains.


CocoaPods plugin integration
======================================
Please follow this guide to install App-Chain module in your existed or new project

* see general CocoaPods instruction: ```https://cocoapods.org > getting started```
	
* "App-Chains" CocoaPods plugin prepared as separate module, but it depends on a Token object from OAuth plugin. And it needed fileID which you can get with selecting genetic file via File Selector plugin. 
App-Chains can execute request to server for genetic information with token object and fileID value.
Thus you need 3 modules to be installed and set up: ```OAuth```, ```File Selector``` and ```App-Chains```

* reference to [OAuth plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-OAuth-Swift)
* reference to [File Selector plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-File-Selector-Swift)
	
* create a new project in Xcode
	
* create Podfile in your project directory: 

	```
	$ pod init
	```
		
* specify following parameters in Podfile: 

	```
	pod 'sequencing-oauth-api-swift', '~> 1.0.3'
	pod 'sequencing-file-selector-api-swift', '~> 1.0.1'
	pod 'sequencing-app-chains-api-swift', '~> 1.0.1'
	```		
		
* install the dependency in your project: 

	```
	$ pod install
	```
		
* always open the Xcode workspace instead of the project file: 

	```
	$ open *.xcworkspace
	```


Configuration
======================================
* set up OAuth plugin: reference to [OAuth plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-OAuth-Swift)
* set up File Selector plugin: [File Selector plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-File-Selector-Swift)

* configuration for App-Chains: code snippets below contain the following three placeholders. Please make sure to replace each of the placeholders with real values:
* ```<your token>``` 
 * replace with the oAuth2 access token value obtained from OAuth plugin
  * the code snippet for enabling Sequencing.com's oAuth2 authentication for your app can be found in the [oAuth2 code and demo repo](https://github.com/SequencingDOTcom/oAuth2-code-and-demo)
  * [OAuth plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-OAuth-Swift)

* ```<chain id>``` 
 * replace with the App Chain ID obtained from the list of [App Chains](https://sequencing.com/app-chains)

* ```<file id>``` 
 * replace with the file ID selected by the user while using your app
  * the code snippet for enabling Sequencing.com's File Selector for your app can be found in the [File Selector code repo](https://github.com/SequencingDOTcom/File-Selector-code)
  * [File Selector plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-File-Selector-Swift)


### Swift

AppChains Swift API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`init(token: String, chainsHostname: String)`  | Constructor | **token** - security token provided by sequencing.com <br> **chainsHostname** - API server hostname. api.sequencing.com by default | Constructor used for creating AppChains class instance in case reporting API is needed and where security token is required
`func getReport(remoteMethodName: String, applicationMethodName: String, datasourceId: String) -> ReturnValue<Report>`  | Reporting API | **remoteMethodName** - REST endpoint name, use "StartApp" <br> **applicationMethodName** - name of data processing routine <br> **datasourceId** - input data identifier <br>

Prerequisites:
* Swift v2 compatible compiler

Adding code to the project:
* add import 

	```
	import sequencing_app_chains_api_swift
	```

After that you can start utilizing Reporting API: example

	```
	let appChainsManager = AppChains.init(token: accessToken as String, chainsHostname: "api.sequencing.com")
	
	let returnValue: ReturnValue<Report> = appChainsManager.getReport("StartApp", applicationMethodName: "your chain number", datasourceId: "your fileID value"  as String)
	
	switch returnValue {
            
       	    case .Success(let value):
           	    let report: Report = value
               
               	for result: Result in report.results {
               		let resultValue: ResultValue = result.value
               		
               		if resultValue.type == ResultType.TEXT {
               			print(result.name + " = " + (resultValue as! TextResultValue).data)
               		}
                }
            
	        case .Failure(let error):
	        	print("Error occured while getting genetic information: " + error)
	}
	```


Troubleshooting
======================================
Each app chain code should work straight out-of-the-box without any configuration requirements or issues. 

Other tips

* Ensure that the following three placeholders have been substituted with real values:

1. ```<your token>```
  * replace with the oAuth2 access token value obtained from OAuth plugin
  * the code snippet for enabling Sequencing.com's oAuth2 authentication for your app can be found in the [oAuth2 code and demo repo](https://github.com/SequencingDOTcom/oAuth2-code-and-demo)
  * [OAuth plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-OAuth-Swift)

2. ```<chain id>```
  * replace with the App Chain ID obtained from the list of [App Chains](https://sequencing.com/app-chains)

3. ```<file id>```
  * replace with the file ID selected by the user while using your app. 
   * the code snippet for enabling Sequencing.com's File Selector for your app can be found in the [File Selector code repo](https://github.com/SequencingDOTcom/File-Selector-code)
  * [File Selector plugin Swift (CocoaPods plugin)](https://github.com/SequencingDOTcom/CocoaPod-iOS-File-Selector-Swift)
   
* [Developer Documentation](https://sequencing.com/developer-documentation/)

* [OAuth2 guide](https://sequencing.com/developer-documentation/oauth2-guide/)

* Review the [Weather My Way +RTP app](https://github.com/SequencingDOTcom/Weather-My-Way-RTP-App/), which is an open-source weather app that uses Real-Time Personalization to provide genetically tailored content

* Confirm you have the latest version of the code from this repository.


Resources
======================================
* [App chains](https://sequencing.com/app-chains)
* [File selector code](https://github.com/SequencingDOTcom/File-Selector-code)
* [Developer center](https://sequencing.com/developer-center)
* [Developer Documentation](https://sequencing.com/developer-documentation/)

Maintainers
======================================
This repo is actively maintained by [Sequencing.com](https://sequencing.com/). Email the Sequencing.com bioinformatics team at gittaca@sequencing.com if you require any more information or just to say hola.

Contribute
======================================
We encourage you to passionately fork us. If interested in updating the master branch, please send us a pull request. If the changes contribute positively, we'll let it ride.

