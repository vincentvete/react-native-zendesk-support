# react-native-zendesk-support
React Native bridge to Zendesk Support SDK on iOS and Android. This currently only supports using the out of the box views the Zendesk Support SDK provides. At the moment, only anonymous authentication is supported.

## React Native Version Support

This has only been tested to work with React Native 0.47, probably works in earlier versions.

## Getting started

### Installing via RNPM (Common)
```
react-native link react-native-zendesk-support
```

### Installing via Cocoapods (Not Common)

Add the following line to your Podfile:

```
  pod 'react-native-zendesk-support', :path => '../node_modules/react-native-zendesk-support'
```

Then add this post-install hook:

```
  post_install do |installer|
    installer.pods_project.targets.each do |target|
      target.build_configurations.each do |config|
        target.build_settings(config.name)['CLANG_ALLOW_NON_MODULAR_INCLUDES_IN_FRAMEWORK_MODULES'] = 'YES'
      end
    end
  end
```

### Configure Android (Must Do)
You need to add the following repository to your `android/app/build.gradle` file. If you do not already have a `repositories` section, add it at the root level of the file right before the `dependencies` section

```
repositories {
    maven { url 'https://zendesk.jfrog.io/zendesk/repo' }
}
```

### Installing Zendesk SDK (Optional, Not Common)
If you'd like to define your `appId`, `zendeskUrl`, and `clientId` inside your iOS and Android project, rather than pass them as params via `ZendeskSupport.initialize` through this module, you can do so by integrating the Zendesk Support SDK into your react-native project.

Follow the instructions to install the Zendesk Support SDK for [iOS](https://developer.zendesk.com/embeddables/docs/ios/integrate_sdk) and [Android](https://developer.zendesk.com/embeddables/docs/android/integrate_sdk#adding-the-support-sdk-with-gradle) (Gradle).

## Usage

Import the module
```js
import ZendeskSupport from 'react-native-zendesk-support';
```

Initialize Zendesk
```js
const config = {
  appId: 'your_app_id',
  zendeskUrl: 'your_zendesk_url',
  clientId: 'your_client_id'
}
ZendeskSupport.initialize(config)
```
###### Note: You must initialize Zendesk prior to calling setupIdentity. Best place for it would be inside `componentWillMount`

Define an identity
```js
const identity = {
  customerEmail: 'foo@bar.com',
  customerName: 'Foo Bar'
}
ZendeskSupport.setupIdentity(identity)
```
###### Note: You must define an identity prior to calling any support ticket or help center methods. Suggested places are inside `componentWillMount` or `componentWillReceiveProps` if your identity details aren't immediately available

### Support Tickets

File a ticket
```js
const customFields = {
  customFieldId: 'Custom Field Value'
}
ZendeskSupport.callSupport(customFields)
```

Bring up ticket history
```js
ZendeskSupport.supportHistory()
```

### Help Center

Show help center
```js
ZendeskSupport.showHelpCenter()
```

Show categories, e.g., FAQ
```js
ZendeskSupport.showCategories(['categoryId'])
```

Show sections, e.g., Account Questions
```js
ZendeskSupport.showSections(['sectionId'])
```

Show labels, e.g., tacocat
```js
ZendeskSupport.showLabels(['tacocat'])
```

#### Options
The Help Center functions above support a second parameter, an object of options.
```js
const options = {
  articleVotingEnabled: false,
  hideContactSupport: false,
  showConversationsMenuButton: false,
  withContactUsButtonVisibility: 'OFF'
}
ZendeskSupport.showHelpCenterWithOptions({ options })
ZendeskSupport.showCategoriesWithOptions(['categoryId'], { options })
ZendeskSupport.showSectionsWithOptions(['sectionId'], { options })
ZendeskSupport.showLabelsWithOptions(['tacocat'], { options })
```

##### articleVotingEnabled _boolean_
* **true** _(default)_ – Show voting buttons on articles
* **false** – Hide voting buttons on articles.

##### hideContactSupport _boolean_
* **true** _(default)_ – Shows contact support option in empty results on iOS
* **false** – Hides contact support option in empty results on iOS

##### showConversationsMenuButton _boolean_
* **true** _(default)_ – Shows the right menu on Android which shows tickets
* **false** – Hides the right menu on Android which shows tickets

##### withContactUsButtonVisibility _string (case sensitive)_
* **ARTICLE_LIST_AND_ARTICLE** _(default)_ – Show floating action button in list and article view
* **ARTICLE_LIST_ONLY** – Show floating action button only in list views
* **OFF** – Hide floating action button on articles and list views

## Known bugs
* Disappearing help center category headers on android

## Upcoming Features
* Authenticate using JWT endpoint
* Theme support
* Show article by id
* Hiding "Contact us" on iOS from article and list view
