# SwiftOpenAIProxy

### Background

In December 2023, I faced a significant security breach when a group of hackers compromised my OpenAI key. They quickly used up my entire $2,500 monthly limit, resulting in an unexpected bill from OpenAI. This incident also forced me to take my app, Pico, offline, causing me to miss the crucial Christmas sales period, typically the year's most lucrative. For more details on this event, please visit:  https://youtu.be/_ueiYhLwwBc?si=8UC_7VZOrhgcXoKV

As a response to this incident, I developed SwiftOpenAIProxy, the first OpenAI proxy created in Server-side Swift. This tool is especially convenient for Swift developers, as it allows easy customization to meet their specific requirements.

SwiftOpenAIProxy is designed to be compatible with any existing OpenAI library. It works seamlessly with libraries such as [CleverBird](https://github.com/btfranklin/CleverBird), [OpenAISwift](https://github.com/adamrushy/OpenAISwift), [OpenAI-Kit](https://github.com/dylanshine/openai-kit), [MacPaw OpenAI](https://github.com/MacPaw/OpenAI), and can also integrate with your custom code.

### Key features

- SwiftOpenAIProxy operates without user accounts. Instead, it verifies the App Store subscription receipt from the iOS or macOS client app.
- SwiftOpenAIProxy uses Apple's [App Store Server Library](https://github.com/apple/app-store-server-library-swift) for receipt validation
- Once the receipt is validated, SwiftOpenAIProxy issues a JWT token the client can use for subsequent calls
- SwiftOpenAIProxy is API agnostic and forwards any request to https://api.openai.com.
- The forwarding endpoint is customizable, allowing redirection to various non-OpenAI API endpoints
- SwiftOpenAIProxy can optionally track individual users through App Account IDs. This requires the client app to send a unique UUID to the [purchase](https://developer.apple.com/documentation/storekit/product/3791971-purchase) method.

### What's implemented
- [x] Reverse proxy server forwarding calls to OpenAI (or any other endpoint)
- [x] Authenticates using App Store receipt validation
- [ ] [App Attestation](https://developer.apple.com/documentation/devicecheck/preparing_to_use_the_app_attest_service) is on hold, as macOS doesn't support app attestation
- [ ] Rate limiter
- [ ] Account management and black list
- [ ] Automatically translate and forward traffic to other AI APIs based on model setting in API call

## How to Set Up SwiftOpenAIProxy

To set up SwiftOpenAIProxy, you need:
- Your OpenAI API key and organization
- A JWT private key, which can be generated in the terminal
- Your app bundle Id, Apple app Id and team Id
- App Store Server API key, Issuer Id, and Key Id
- Apple root certificates, which are included in the repository but should be updated if Apple updates their certificates

#### OpenAI API key and organization
Generate an OpenAI API key at https://platform.openai.com

#### JWT Private key
Create a new JWT private key in macOS Terminal using `openssl rand -base64 32`

Note: This JWT token is used to authenticate your client app. It is a different JWT token the App Store Server Library uses to communicate with the Apple App Store API.

#### App Ids
Find your App bundle Id, Apple app Id, and team Id on https://appstoreconnect.apple.com/apps Under **App Information** in the **General** section, you will find these details.

Team Id is a 10-character string and can be found in https://developer.apple.com/account under **Membership Details**.

#### App Store Server API key
Generate the key under the **Users and Access** tab in App Store Connect, specifically under **In-app Purchase** [here](https://appstoreconnect.apple.com/access/api/subs). You will also find the Issuer Id and Key Id on the same page.

See for more details [Creating API Keys for App Store Connect API](https://developer.apple.com/documentation/appstoreconnectapi/creating_api_keys_for_app_store_connect_api)

## Run SwiftOpenAIProxy from Xcode

To run SwiftOpenAIProxy from Xcode, set the environment variables and arguments listed below to the information listed in How to Set Up SwiftOpenAIProxy

Both environment variables and arguments can be edited in Xcode using the Target -> Edit scheme.

![Xcode screenshot of edit scheme menu](Images/EditScheme.png)

### Arguments passed on launch

| Argument | Default value | Default in scheme |
| --- | --- | --- |
| --hostname | 0.0.0.0 | |
| --port | 443 | 8081 |
| --target | https://api.openai.com | |

When launched from Xcode, SwiftOpenAIProxy is accessible at http://localhost:8081. When deployed in the cloud, SwiftOpenAIProxy will default to port 443 (https). 

All traffic will be forwarded to `target`. The 'target' can be modified to direct traffic to any API, regardless of whether it conforms to the OpenAI API. , as long as your client application is compatible.
The target is the site where all traffic is forwarded to. You can change the target to any API, even if the API doesn't conform OpenAI (so long as your client app does).

### Environment variables

#### OpenAI environment variables. 
| Variable | Description | reference |
| --- | --- | --- |
| OpenAI-APIKey | OpenAI API key (sk-...) | https://platform.openai.com |
| OpenAI-Organization | OpenAI org identifier (org-...) | https://platform.openai.com |
| allowKeyPassthrough | if 1, requests with a valid OpenAI key and org in the header will be forwarded to OpenAI without modifications |

#### App Store Connect environment variables
| Variable | Description | reference |
| --- | --- | --- |
| appTeamId | Apple Team ID | https://appstoreconnect.apple.com/ |
| appBundleId | E.g. com.example.myapp | https://appstoreconnect.apple.com/ |
| appAppleId | Apple Id under App Information -> General Information | https://appstoreconnect.apple.com/ |

#### App Store Server API environment variables
| Variable | Description | reference |
| --- | --- | --- |
| IAPPrivateKey | IAP private key | https://appstoreconnect.apple.com/access/api/subs |
| IAPIssuerId | IAP Issuer Id | https://appstoreconnect.apple.com/access/api/subs |
| IAPKeyId | IAP Key Id | https://appstoreconnect.apple.com/access/api/subs |

#### JWT environment variables
| Variable | Description | reference |
| --- | --- | --- |
| JWTPrivateKey |  | https://jwt.io/introduction |


## How to call SwiftOpenAIProxy from your iOS or macOS app

Coming soon

## Apps using SwiftOpenAIProxy

- Pico
