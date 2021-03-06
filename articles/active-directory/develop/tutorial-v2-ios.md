---
title: 'Zelfstudie: MSAL voor iOS en macOS -Microsoft-identiteitsplatform | Azure'
description: Leer hoe u met apps van iOS en macOS (Swift) een API kunt aanroepen waarvoor toegangstokens zijn vereist, met behulp van het Microsoft-identiteitsplatform
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 06/16/2020
ms.author: jmprieur
ms.reviewer: oldalton
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 61a3884ce4d1075dbcac61a0c202c3a393d2d943
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2020
ms.locfileid: "88118141"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-or-macos-app"></a>Gebruikers aanmelden en Microsoft Graph aanroepen vanuit een iOS- of macOS-app

In deze zelfstudie leert u hoe u een iOS- of macOS-app integreert met het Microsoft-identiteitsplatform. Via de app wordt een gebruiker aangemeld, een toegangstoken opgehaald om de Microsoft Graph API aan te roepen, en een aanvraag ingediend bij de Microsoft Graph API.  

Wanneer u de handleiding hebt voltooid, accepteert uw toepassing aanmeldingen van persoonlijke Microsoft-accounts (waaronder outlook.com, live.com en overige accounts), en werk- of schoolaccounts van elk bedrijf of elke organisatie waar Azure Active Directory wordt gebruikt.

>[!NOTE]
> Als u niet bekend bent met het Microsoft-identiteitsplatform, raden we u aan om eerst [Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een iOS- of macOS-app](quickstart-v2-ios.md) te raadplegen.

## <a name="how-this-tutorial-works"></a>Hoe deze zelfstudie werkt

![Er wordt getoond hoe de voorbeeld-app werkt die wordt gegenereerd in deze zelfstudie](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Met de app in deze zelfstudie worden gebruikers aangemeld, en worden namens hen gegevens opgehaald.  Deze gegevens worden geopend via een beveiligde API (Microsoft Graph API in dit geval), waarvoor autorisatie is vereist en die is beveiligd met het Microsoft-identiteitsplatform.

Met name:

* Met uw app wordt de gebruiker aangemeld via een browser of via Microsoft Authenticator.
* De eindgebruiker accepteert de machtigingen die met de toepassing zijn aangevraagd.
* Aan de app wordt een toegangstoken verleend voor de Microsoft Graph API.
* Het toegangstoken is opgenomen in de HTTP-aanvraag voor de web-API.
* Verwerk het Microsoft Graph-antwoord.

In dit voorbeeld wordt de MSAL (Microsoft Authentication Library) gebruikt om Authentication te implementeren. In MSAL worden tokens automatisch vernieuwd, eenmalige aanmelding (SSO) geboden tussen andere apps op het apparaat, en de accounts beheerd.

Deze zelfstudie is van toepassing op iOS- en macOS-apps. Houd er rekening mee dat sommige stappen verschillen voor deze twee platforms. 

## <a name="prerequisites"></a>Vereisten

- Xcode-versie 11.x of hoger is vereist om de app in deze handleiding te bouwen. U kunt Xcode downloaden op de [Mac App Store](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "Download-URL voor Xcode").
- Microsoft Authentication Library ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). U kunt afhankelijkheidsbeheer gebruiken of de bibliotheek handmatig toevoegen. In de onderstaande instructies ziet u hoe u dit doet.

In deze zelfstudie maakt u een nieuw project. Als u in plaats hiervan de voltooide zelfstudie wilt downloaden, downloadt u de code:
- [iOS-voorbeeldcode](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
- [macOS-voorbeeldcode](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

## <a name="create-a-new-project"></a>Een nieuw project maken

1. Open Xcode en selecteer **Een nieuw Xcode-project maken**.
2. Selecteer voor iOS-apps **iOS** > **App met één weergave**, en selecteer **Volgende**.
3. Selecteer voor macOS-apps **macOS** > **Cocoa -app** ,en selecteer **Volgende**.
4. Geef een projectnaam op.
5. Stel de **Taal** in op **Swift**, en selecteer **Volgende**.
6. Selecteer een map om de app in te maken, en klik op **Maken**.

## <a name="register-your-application"></a>Uw toepassing registreren

1. Ga naar de [Azure-portal](https://aka.ms/MobileAppReg)
2. Open de blade App-registraties en klik op **+ Nieuwe registratie**.
3. Voer een **Naam** in voor de app, zonder een omleidings-URI in te stellen.
4. Selecteer **Accounts in een organisatiemap (alle Azure AD-mappen - meerdere tenants) en persoonlijke Microsoft-accounts (bijvoorbeeld Skype, Xbox)** onder **Ondersteunde accounttypen**
5. Klik op **Registreren**
6. Selecteer in de sectie **Beheren** van het deelvenster dat verschijnt, de optie **Verificatie**.

7. Klik bovenaan het scherm op **De nieuwe ervaring uitproberen** om de nieuwe versie van App-registratie te openen. Klik vervolgens op **+ Nieuwe registraties** >  **+ Een platform toevoegen** > **iOS/macOS**.
    - Voer de bundel-id van het project in. Als u de code hebt gedownload, is de id `com.microsoft.identitysample.MSALiOS`. Als u een eigen project wilt maken, selecteert u uw project in Xcode en opent u het tabblad **Algemeen**. De bundel-id wordt weergegeven in de sectie **Identiteit**.
8. Klik op `Configure` en sla de **MSAL-configuratie** op die wordt weergegeven op de pagina **MSAL-configuratie**, zodat u deze kunt invoeren wanneer u de app later configureert.  Klik op **Gereed**.

## <a name="add-msal"></a>MSAL toevoegen

Kies een van de volgende manieren om de MSAL in uw app te installeren:

### <a name="cocoapods"></a>CocoaPods

1. Als u [CocoaPods](https://cocoapods.org/) gebruikt, installeert u `MSAL` door eerst een leeg bestand te maken met de naam `podfile`, in dezelfde map als het bestand `.xcodeproj` van uw project. Voeg het volgende toe in `podfile`:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL'
   end
   ```

2. Vervang `<your-target-here>` door de naam van uw project.
3. Navigeer in een terminalvenster naar de map die het `podfile` bevat dat u hebt gemaakt, en voer `pod install` uit om de MSAL te installeren.
4. Sluit Xcode en open `<your project name>.xcworkspace` om het project opnieuw te laden in Xcode.

### <a name="carthage"></a>Carthage

Als u [Carthage](https://github.com/Carthage/Carthage)gebruikt, installeert u `MSAL` door het toe te voegen aan uw `Cartfile`:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

Voer vanuit een terminalvenster, in dezelfde map als het bijgewerkte `Cartfile`, de volgende opdracht uit om de afhankelijkheden in uw project bij te werken met Carthage.

iOS:

```bash
carthage update --platform iOS
```

macOS:

```bash
carthage update --platform macOS
```

### <a name="manually"></a>Handmatig

U kunt ook een Git-submodule gebruiken of de meest recente release bekijken om te gebruiken als framework in uw toepassing.

## <a name="add-your-app-registration"></a>Uw app-registratie toevoegen

Vervolgens voegen we de app-registratie toe aan uw code. 

Voeg eerst de volgende importinstructie toe aan de bovenkant van de `ViewController.swift`, en ook `AppDelegate.swift`- of `SceneDelegate.swift`-bestanden:

```swift
import MSAL
```

Voeg vervolgens de volgende code toe aan `ViewController.swift` voor `viewDidLoad()`:

```swift
// Update the below to your client ID you received in the portal. The below is for running the demo only
let kClientID = "Your_Application_Id_Here"
let kGraphEndpoint = "https://graph.microsoft.com/" // the Microsoft Graph endpoint
let kAuthority = "https://login.microsoftonline.com/common" // this authority allows a personal Microsoft account and a work or school account in any organization’s Azure AD tenant to sign in
    
let kScopes: [String] = ["user.read"] // request permission to read the profile of the signed-in user
    
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
var webViewParameters : MSALWebviewParameters?
var currentAccount: MSALAccount?
```

De enige waarde die u hierboven moet wijzigen, is de waarde die is toegewezen `kClientID`als uw [Toepassings-id](./developer-glossary.md#application-id-client-id). Deze waarde maakt deel uit van de MSAL-configuratiegegevens die u hebt opgeslagen tijdens de stap aan het begin van deze zelfstudie om de toepassing te registreren in de Azure-portal.

## <a name="configure-xcode-project-settings"></a>Xcode-projectinstellingen configureren

Voeg een nieuwe sleutelketengroep toe aan uw project **Ondertekening en mogelijkheden**. De sleutelketengroep moet zijn: `com.microsoft.adalcache` in iOS en `com.microsoft.identity.universalstorage` in macOS.

![Xcode-gebruikersinterface waarin wordt weergegeven hoe de sleutelketen moet zijn ingesteld](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro-keychainShare.png)

## <a name="for-ios-only-configure-url-schemes"></a>Alleen voor iOS: URL-schema's configureren

In deze stap registreert u `CFBundleURLSchemes`, zodat de gebruiker na aanmelding kan worden omgeleid naar de app. Met `LSApplicationQueriesSchemes` kan uw app bovendien ook gebruikmaken van Microsoft Authenticator.

Open `Info.plist` in Xcode als een broncodebestand, en voeg het volgende toe in de sectie `<dict>`. Vervang `[BUNDLE_ID]` door de waarde die u hebt gebruikt in de Azure-portal. Als u de code hebt gedownload, is deze waarde: `com.microsoft.identitysample.MSALiOS`. Als u een eigen project wilt maken, selecteert u uw project in Xcode en opent u het tabblad **Algemeen**. De bundel-id wordt weergegeven in de sectie **Identiteit**.

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauthv2</string>
    <string>msauthv3</string>
</array>
```

## <a name="for-macos-only-configure-app-sandbox"></a>Alleen voor macOS: App-sandbox configureren

1. Ga naar de instellingen voor uw Xcode-project > **tabblad Mogelijkheden** > **App-sandbox**
2. Selecteer het selectie vakje **Uitgaande verbindingen (client)** . 

## <a name="create-your-apps-ui"></a>De app-gebruikersinterface maken

Maak nu een gebruikersinterface die een knop voor het aanroepen van de Microsoft Graph API, een knop voor afmelden, en een tekstweergave voor uitvoer bevat, door de volgende code toe te voegen aan de klasse `ViewController`:

### <a name="ios-ui"></a>iOS-gebruikersinterface

```swift
var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!
var usernameLabel: UILabel!

func initUI() {
        
    usernameLabel = UILabel()
    usernameLabel.translatesAutoresizingMaskIntoConstraints = false
    usernameLabel.text = ""
    usernameLabel.textColor = .darkGray
    usernameLabel.textAlignment = .right
        
    self.view.addSubview(usernameLabel)
        
    usernameLabel.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
    usernameLabel.rightAnchor.constraint(equalTo: view.rightAnchor, constant: -10.0).isActive = true
    usernameLabel.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
    usernameLabel.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add call Graph button
    callGraphButton  = UIButton()
    callGraphButton.translatesAutoresizingMaskIntoConstraints = false
    callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
    callGraphButton.setTitleColor(.blue, for: .normal)
    callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
    self.view.addSubview(callGraphButton)
        
    callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 120.0).isActive = true
    callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
    callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add sign out button
    signOutButton = UIButton()
    signOutButton.translatesAutoresizingMaskIntoConstraints = false
    signOutButton.setTitle("Sign Out", for: .normal)
    signOutButton.setTitleColor(.blue, for: .normal)
    signOutButton.setTitleColor(.gray, for: .disabled)
    signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
    self.view.addSubview(signOutButton)
        
    signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
    signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
    signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    let deviceModeButton = UIButton()
    deviceModeButton.translatesAutoresizingMaskIntoConstraints = false
    deviceModeButton.setTitle("Get device info", for: .normal);
    deviceModeButton.setTitleColor(.blue, for: .normal);
    deviceModeButton.addTarget(self, action: #selector(getDeviceMode(_:)), for: .touchUpInside)
    self.view.addSubview(deviceModeButton)
        
    deviceModeButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    deviceModeButton.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
    deviceModeButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
    deviceModeButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add logging textfield
    loggingText = UITextView()
    loggingText.isUserInteractionEnabled = false
    loggingText.translatesAutoresizingMaskIntoConstraints = false
        
    self.view.addSubview(loggingText)
        
    loggingText.topAnchor.constraint(equalTo: deviceModeButton.bottomAnchor, constant: 10.0).isActive = true
    loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
    loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: -10.0).isActive = true
    loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
}

func platformViewDidLoadSetup() {
                
    NotificationCenter.default.addObserver(self,
                        selector: #selector(appCameToForeGround(notification:)),
                        name: UIApplication.willEnterForegroundNotification,
                        object: nil)
        
}
    
@objc func appCameToForeGround(notification: Notification) {
    self.loadCurrentAccount()
}

```

### <a name="macos-ui"></a>macOS-gebruikersinterface

```swift

var callGraphButton: NSButton!
var loggingText: NSTextView!
var signOutButton: NSButton!
    
var usernameLabel: NSTextField!

func initUI() {
        
    usernameLabel = NSTextField()
    usernameLabel.translatesAutoresizingMaskIntoConstraints = false
    usernameLabel.stringValue = ""
    usernameLabel.isEditable = false
    usernameLabel.isBezeled = false
    self.view.addSubview(usernameLabel)
        
    usernameLabel.topAnchor.constraint(equalTo: view.topAnchor, constant: 30.0).isActive = true
    usernameLabel.rightAnchor.constraint(equalTo: view.rightAnchor, constant: -10.0).isActive = true
        
    // Add call Graph button
    callGraphButton  = NSButton()
    callGraphButton.translatesAutoresizingMaskIntoConstraints = false
    callGraphButton.title = "Call Microsoft Graph API"
    callGraphButton.target = self
    callGraphButton.action = #selector(callGraphAPI(_:))
    callGraphButton.bezelStyle = .rounded
    self.view.addSubview(callGraphButton)
        
    callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
    callGraphButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
        
    // Add sign out button
    signOutButton = NSButton()
    signOutButton.translatesAutoresizingMaskIntoConstraints = false
    signOutButton.title = "Sign Out"
    signOutButton.target = self
    signOutButton.action = #selector(signOut(_:))
    signOutButton.bezelStyle = .texturedRounded
    self.view.addSubview(signOutButton)
        
    signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
    signOutButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
    signOutButton.isEnabled = false
        
    // Add logging textfield
    loggingText = NSTextView()
    loggingText.translatesAutoresizingMaskIntoConstraints = false
        
    self.view.addSubview(loggingText)
        
    loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
    loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
    loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: -10.0).isActive = true
    loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: -10.0).isActive = true
    loggingText.widthAnchor.constraint(equalToConstant: 500.0).isActive = true
    loggingText.heightAnchor.constraint(equalToConstant: 300.0).isActive = true
}

func platformViewDidLoadSetup() {}

```

Vervang vervolgens in de klasse `ViewController` de methode `viewDidLoad()` door:

```swift
    override func viewDidLoad() {

        super.viewDidLoad()

        initUI()
        
        do {
            try self.initMSAL()
        } catch let error {
            self.updateLogging(text: "Unable to create Application Context \(error)")
        }
        
        self.loadCurrentAccount()
        self.platformViewDidLoadSetup()
    }
```

## <a name="use-msal"></a>MSAL gebruiken

### <a name="initialize-msal"></a>MSAL initialiseren

Voeg de volgende methode `initMSAL` toe aan de klasse `ViewController`:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.updateLogging(text: "Unable to create authority URL")
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
        self.initWebViewParams()
    }
```

Voeg na methode `initMSAL` het volgende toe aan de klasse `ViewController`.

### <a name="ios-code"></a>iOS-code:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters(authPresentationViewController: self)
    }
```

### <a name="macos-code"></a>macOS-code:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters()
    }
```

### <a name="for-ios-only-handle-the-sign-in-callback"></a>Alleen voor iOS: callback na aanmelding afhandelen

Open het `AppDelegate.swift`-bestand. Als u de callback na aanmelding wilt afhandelen, voegt u `MSALPublicClientApplication.handleMSALResponse` toe aan de klasse `appDelegate`, zoals:

```swift
// Inside AppDelegate...
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}

```

**Als u Xcode 11 gebruikt**, moet u MSAL-callback in plaats hiervan in de `SceneDelegate.swift` plaatsen.
Als u ondersteuning biedt voor UISceneDelegate en UIApplicationDelegate voor compatibiliteit met oudere iOS-versies, moet MSAL-callback in beide bestanden worden geplaatst.

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        
        guard let urlContext = URLContexts.first else {
            return
        }
        
        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication
        
        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
```

#### <a name="acquire-tokens"></a>Tokens verkrijgen

Nu kunnen we verwerkingslogica voor de gebruikersinterface van de toepassing implementeren en interactief tokens ophalen via MSAL.

MSAL biedt twee primaire methoden voor het ophalen van tokens: `acquireTokenSilently()` en `acquireTokenInteractively()`: 

- Met `acquireTokenSilently()` wordt geprobeerd om een gebruiker aan te melden en tokens op te halen zonder enige gebruikersinteractie, mits een account aanwezig is. Voor `acquireTokenSilently()` moet een geldig `MSALAccount` worden geboden, wat kan worden opgehaald met behulp van een van de accountopsommings-API’s van MSAL. In dit voorbeeld wordt `applicationContext.getCurrentAccount(with: msalParameters, completionBlock: {})` gebruikt om het huidige account op te halen. 

- Met `acquireTokenInteractively()` wordt altijd de gebruikersinterface weergegeven bij een poging om de gebruiker aan te melden. Voor een interactieve ervaring met eenmalige aanmelding kunnen sessiecookies in de browser worden gebruikt of een account in Microsoft Authenticator.

Voeg de volgende code toe aan de klasse `ViewController`:

```swift
    func getGraphEndpoint() -> String {
        return kGraphEndpoint.hasSuffix("/") ? (kGraphEndpoint + "v1.0/me/") : (kGraphEndpoint + "/v1.0/me/");
    }

    @objc func callGraphAPI(_ sender: AnyObject) {
        
        self.loadCurrentAccount { (account) in
            
            guard let currentAccount = account else {
                
                // We check to see if we have a current logged in account.
                // If we don't, then we need to sign someone in.
                self.acquireTokenInteractively()
                return
            }
            
            self.acquireTokenSilently(currentAccount)
        }
    }

    typealias AccountCompletion = (MSALAccount?) -> Void

    func loadCurrentAccount(completion: AccountCompletion? = nil) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        let msalParameters = MSALParameters()
        msalParameters.completionBlockQueue = DispatchQueue.main
                
        applicationContext.getCurrentAccount(with: msalParameters, completionBlock: { (currentAccount, previousAccount, error) in
            
            if let error = error {
                self.updateLogging(text: "Couldn't query current account with error: \(error)")
                return
            }
            
            if let currentAccount = currentAccount {
                
                self.updateLogging(text: "Found a signed in account \(String(describing: currentAccount.username)). Updating data for that account...")
                
                self.updateCurrentAccount(account: currentAccount)
                
                if let completion = completion {
                    completion(self.currentAccount)
                }
                
                return
            }
            
            self.updateLogging(text: "Account signed out. Updating UX")
            self.accessToken = ""
            self.updateCurrentAccount(account: nil)
            
            if let completion = completion {
                completion(nil)
            }
        })
    }
```

#### <a name="get-a-token-interactively"></a>Interactief een token ophalen

Met de onderstaande code wordt de eerste keer een token opgehaald door een object `MSALInteractiveTokenParameters` te maken en `acquireToken` aan te roepen. Vervolgens voegt u code toe waarmee de volgende acties worden uitgevoerd:

1. `MSALInteractiveTokenParameters` maken met bereiken.
2. `acquireToken()` aanroepen met de gemaakte parameters.
3. Fouten afhandelen. Raadpleeg [Handleiding voor foutafhandeling in MSAL voor iOS en macOS](msal-handling-exceptions.md) voor meer details.
4. De geslaagde aanvraag afhandelen.

Voeg de volgende code toe aan de klasse `ViewController`.

```swift
func acquireTokenInteractively() {
        
    guard let applicationContext = self.applicationContext else { return }
    guard let webViewParameters = self.webViewParameters else { return }
        
    // #1
    let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: webViewParameters)
    parameters.promptType = .selectAccount
        
    // #2
    applicationContext.acquireToken(with: parameters) { (result, error) in
            
        // #3
        if let error = error {
                
            self.updateLogging(text: "Could not acquire token: \(error)")
            return
        }
            
        guard let result = result else {
                
            self.updateLogging(text: "Could not acquire token: No result returned")
            return
        }
            
        // #4
        self.accessToken = result.accessToken
        self.updateLogging(text: "Access token is \(self.accessToken)")
        self.updateCurrentAccount(account: result.account)
        self.getContentWithToken()
    }
}    
```


#### <a name="get-a-token-silently"></a>Een token op de achtergrond ophalen

Als u een bijgewerkt token op de achtergrond wilt ophalen, voegt u de volgende code toe aan de klasse `ViewController`. Hiermee maakt u een object `MSALSilentTokenParameters` en roept u `acquireTokenSilent()` aan:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        /**
         
         Acquire a token for an existing account silently
         
         - forScopes:           Permissions you want included in the access token received
         in the result in the completionBlock. Not all scopes are
         guaranteed to be included in the access token returned.
         - account:             An account object that we retrieved from the application object before that the
         authentication flow will be locked down to.
         - completionBlock:     The completion block that will be called when the authentication
         flow completes, or encounters an error.
         */
        
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in
            
            if let error = error {
                
                let nsError = error as NSError
                
                // interactionRequired means we need to ask the user to sign-in. This usually happens
                // when the user's Refresh Token is expired or if the user has changed their password
                // among other possible reasons.
                
                if (nsError.domain == MSALErrorDomain) {
                    
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>De Microsoft Graph API aanroepen 

Zodra u een token hebt, kan de app deze gebruiken in de HTTP-koptekst om een geautoriseerde aanvraag te doen bij Microsoft Graph:

| sleutel voor koptekst    | waarde                 |
| ------------- | --------------------- |
| Autorisatie | Bearer \<access-token> |

Voeg de volgende code toe aan de klasse `ViewController`:

```swift
    func getContentWithToken() {
        
        // Specify the Graph API endpoint
        let graphURI = getGraphEndpoint()
        let url = URL(string: graphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
        
        URLSession.shared.dataTask(with: request) { data, response, error in
            
            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }
            
            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
                
                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }
            
            self.updateLogging(text: "Result from Graph: \(result))")
            
            }.resume()
    }
```

Zie [Microsoft Graph API](https://graph.microsoft.com) voor meer informatie over de Microsoft Graph API.

### <a name="use-msal-for-sign-out"></a>MSAL gebruiken voor afmelden

Voeg vervolgens ondersteuning voor afmelden toe.

> [!Important]
> Bij afmelden met MSAL wordt alle bekende informatie over een gebruiker verwijderd uit de toepassing. Ook wordt een actieve sessie verwijderd op het apparaat van de gebruiker wanneer dit is toegestaan op basis van de apparaatconfiguratie. U kunt de gebruiker ook afmelden vanuit de browser.

Als u de mogelijkheid voor afmelden wilt toevoegen, voegt u de volgende code toe binnen de klasse `ViewController`. 

```swift 
@objc func signOut(_ sender: AnyObject) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            let signoutParameters = MSALSignoutParameters(webviewParameters: self.webViewParameters!)
            signoutParameters.signoutFromBrowser = false // set this to true if you also want to signout from browser or webview
            
            applicationContext.signout(with: account, signoutParameters: signoutParameters, completionBlock: {(success, error) in
                
                if let error = error {
                    self.updateLogging(text: "Couldn't sign out account with error: \(error)")
                    return
                }
                
                self.updateLogging(text: "Sign out completed successfully")
                self.accessToken = ""
                self.updateCurrentAccount(account: nil)
            })
            
        }
    }
```

### <a name="enable-token-caching"></a>Tokencaching inschakelen

De tokens van uw app worden met MSAL standaard in de cache opgeslagen in de sleutelketen in iOS of macOS. 

Tokencaching inschakelen:
1. Zorg ervoor dat de toepassing juist is afgemeld
2. Ga naar de instellingen voor uw Xcode-project > **tabblad Mogelijkheden** > **Delen van sleutelketens inschakelen**
3. Klik op **+** en voer een vermelding voor een **Sleutelketengroep** in: 3.a Voor iOS voert u `com.microsoft.adalcache` in 3.b Voor macOS voert u `com.microsoft.identity.universalstorage` in

### <a name="add-helper-methods"></a>Help-methoden toevoegen
Voeg de volgende Help-methode toe aan de klasse `ViewController` om het voorbeeld te voltooien.

### <a name="ios-ui"></a>iOS-gebruikersinterface:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
    
    func updateAccountLabel() {
        
        guard let currentAccount = self.currentAccount else {
            self.usernameLabel.text = "Signed out"
            return
        }
        
        self.usernameLabel.text = currentAccount.username
    }
    
    func updateCurrentAccount(account: MSALAccount?) {
        self.currentAccount = account
        self.updateAccountLabel()
        self.updateSignOutButton(enabled: account != nil)
    }
```

### <a name="macos-ui"></a>macOS-gebruikersinterface:

```swift
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.string = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.string = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
    
     func updateAccountLabel() {

         guard let currentAccount = self.currentAccount else {
            self.usernameLabel.stringValue = "Signed out"
            return
        }

        self.usernameLabel.stringValue = currentAccount.username ?? ""
        self.usernameLabel.sizeToFit()
     }

     func updateCurrentAccount(account: MSALAccount?) {
        self.currentAccount = account
        self.updateAccountLabel()
        self.updateSignOutButton(enabled: account != nil)
    }
```

### <a name="for-ios-only-get-additional-device-information"></a>Alleen voor iOS: aanvullende informatie over het apparaat ophalen

Gebruik de volgende code om de huidige apparaatconfiguratie te lezen, inclusief of het apparaat is geconfigureerd als gedeeld:

```swift
    @objc func getDeviceMode(_ sender: AnyObject) {
        
        if #available(iOS 13.0, *) {
            self.applicationContext?.getDeviceInformation(with: nil, completionBlock: { (deviceInformation, error) in
                
                guard let deviceInfo = deviceInformation else {
                    self.updateLogging(text: "Device info not returned. Error: \(String(describing: error))")
                    return
                }
                
                let isSharedDevice = deviceInfo.deviceMode == .shared
                let modeString = isSharedDevice ? "shared" : "private"
                self.updateLogging(text: "Received device info. Device is in the \(modeString) mode.")
            })
        } else {
            self.updateLogging(text: "Running on older iOS. GetDeviceInformation API is unavailable.")
        }
    }
```

### <a name="multi-account-applications"></a>Toepassingen met meerdere accounts

Deze app is gebouwd voor een enkel accountscenario. MSAL biedt ook ondersteuning voor scenario's met meerdere accounts, maar hiervoor is extra werk van apps vereist. U moet een gebruikersinterface maken om gebruikers te helpen bij het selecteren van het account dat ze willen gebruiken voor elke actie waarvoor tokens zijn vereist. Uw app kan ook een heuristiek implementeren om te selecteren welk account moet worden gebruikt, door alle account van MSAL te doorzoeken. Zie bijvoorbeeld `accountsFromDeviceForParameters:completionBlock:` [API](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplication.html#/c:objc(cs)MSALPublicClientApplication(im)accountsFromDeviceForParameters:completionBlock:)

## <a name="test-your-app"></a>Uw app testen

### <a name="run-locally"></a>Lokaal uitvoeren

Bouw en implementeer de app op een testapparaat of simulator. U moet zich kunnen aanmelden en tokens ontvangen voor Azure AD of persoonlijke Microsoft-accounts.

De eerste keer dat een gebruiker zich aanmeldt bij uw app, wordt hun door Microsoft gevraagd toestemming te geven voor de aangevraagde machtigingen.  Hoewel de meeste gebruikers toestemming kunnen geven, is voor sommige Azure AD-tenants gebruikerstoestemming uitgeschakeld. In dit geval moeten beheerders toestemming geven namens alle gebruikers. Registreer de bereiken van de app in de Azure-portal om dit scenario te ondersteunen.

Nadat u zich hebt aangemeld, verschijnen in de app de gegevens die zijn geretourneerd via het Microsoft Graph `/me`-eindpunt.

## <a name="get-help"></a>Help opvragen

Ga naar [Help en ondersteuning](./developer-support-help-options.md) als u problemen ondervindt met deze zelfstudie of met het Microsoft-identiteitsplatform.