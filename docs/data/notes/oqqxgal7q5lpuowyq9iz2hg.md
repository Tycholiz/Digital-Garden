
Unlike Android, you can't install any app on an iOS device. It has to be signed by Apple first.

## Test Deploy
There are two ways to distribute a test version of your app for iOS: TestFlight and Ad Hoc.
- TestFlight involves submitting a build to App Store Connect where it can be downloaded by other users.

## App signing
Signing your app allows the iOS system to identify who signed your app and to verify that your app hasn't been modified since you signed it. 
- before an app can be installed on a device (or even submitted to the App Store), the developer must sign the `.ipa` executable (code signing) by using the certificate (spec: the `.p12` file) procured from Apple's developer portal.
  - when an app is signed with a certificate, Apple can be reasonably assured that the signer is also the person whom the certificate was issued to.
- The Signing Identity consists of a [[public-private|crypt.public-key]] key pair that Apple creates for you.
  - remember, certificate is another name for public key.

Process:
- Create a Certificate Signing Request (CSR) through the Keychain Access application.
- Keychain Access will create a private key (to be stored in the keychain) and a `certSigningRequest` file which you'll then upload through Apple's web portal.
- Apple will proof the request and issue a certificate for you. The Certificate will contain the public key that can be downloaded to your system. After you downloaded it you need to put it into Keychain Access by double clicking it. The Certificate will be pushed into the Keychain and paired with the private key to form the Code Signing Identity.
- Finally, as the app is being installed on the device, the private key used to sign the app matches the public key in the certificate. If it fails, app is not installed.

## Terms
### Provisioning profile (or simply Profile)
The provisioning profile acts as a link between the device and the developer account. 
- therefore the certificate and the device are the 2 main parts of the provisioning profile.
    - getting a certificate involves going to the Apple portal to get a development certificate, which involes registering your device.

Distribution Profiles are used to submit an app to the App Store for distribution. After the app is reviewed by apple they sign in the app with their own signature that can run on any device.

We use the profile for testing our app prior to sending it to Apple for approval.

A provisioning profile is downloaded from your developer account on Apple's web portal and embedded in the app bundle, and the entire bundle is code-signed.

A Development Provisioning Profile must be installed on each device on which you wish to run your application code.
- once made, we can download it, then drag it onto the Xcode icon of the MacOS Dock
- doing this will create a `mobileprovision` file in `~/Library/MobileDevice/Provisioning Profiles`. The filename takes the form `<ProfileUUID>.mobileprovision`

We must generate provisioning profiles in the `developer.apple.com` portal.

Provisioning profiles are not stored with the project. Xcode has a common area and the profile is pulled when you build and bundled with the `ipa` (the binary file of your app).

Each provisioning profile contains:
- Development Certificates — development certificate. These are for developers who want to test the app on a physical device while writing code.
- Unique Device Identifiers (List of devices that the app can run on)
- an App ID — An App ID is a two-part string used to identify one or more apps from a single development team.
    - this can include a `*` wild card to be used for many applications with similar bundle identifiers

When you install the application on a device the following things happen:
- the provisioning profile in the Mac goes to the developer certificate in your key chain.
- xcode uses the certificate to sign the code.
- device's UUID is matched with the IDs in the provisioning profile.
- AppID in the provisioning profile is matched with the bundle identifier in the app.
- The entitlements required are associated with the App ID.
- The private key used to sign the app matches the public key in the certificate.
- the signed binary is sent to the device and is validated against the same provisioning profile in the app and finally launches.

![](/assets/images/2022-04-25-09-15-11.png)

In simple terms, Provisioning profiles say, “applications with this Identifier signed with this Certificate’s private key are okay to run on these devices.”
- Therefore a provisioning profile is tied to a certificate

An App ID is a two-part string used to identify one or more apps from a single development team. The string consists of a Team ID and a bundle ID search string, with a period (.) separating the two parts. The Team ID is supplied by Apple and is unique to a specific development team, while
The bundle ID search string is supplied by the developer to match either the bundle ID of a single app or a set of bundle IDs for a group of apps.

The bundle ID uniquely defines each App. It is specified in Xcode. A single Xcode project can have multiple Targets and therefore output multiple apps. A common use case for this is an app that has both lite/free and pro/full versions or is branded multiple ways.

spec: is provisioning profile a private key?

### Archive
An archive is a bundle that includes your app binary along with symbol information. 
- Archive is the format our app is in when we distribute it for testing or validate and submit it to App Store Connect.
  - in other words, to upload our app to App Store, we must first archive it.

The archive contains debugging information.
- While testing the app through TestFlight, keep the archive with the debugging information to make it easier to interpret crash reports later.

Archives for each version of our app should be saved.

Using Archives:
- Archives can be found in *Window > Organizer*
- Archives can be made in *Product > Archive*

### Symbolication
Symbolication deals with translating how a computer sees code at runtime (that is, machine code dealing with low-level processes) and how we as developers see code.
- without symbolication, we would be reading machine code debug logs.

### Asset Catalog
Asset catalogs simplify access to app resources by mapping between named assets and one or more files targeted for different device attributes.
- ex. depending on whether the user is using an iPad or iPhone, we want to serve up different versions of each Asset. It is a waste to download iPhone 12 optimized images for an iPad user of our app.

Asset Catalog is found in a `Assets.xcassets` or `Images.xcassets` directory

Contents.json files encode the attributes for elements represented by folders in the hierarchy. Each folder can contain one Contents.json file that encodes the attributes for the asset or group it contains.

spec: Android parallel: Drawable Folder

## Resources
- https://abhimuralidharan.medium.com/what-is-a-provisioning-profile-in-ios-77987a7c54c2
- [deploying iOS as github action without Fastlane](https://zach.codes/ios-builds-using-github-actions-without-fastlane/)