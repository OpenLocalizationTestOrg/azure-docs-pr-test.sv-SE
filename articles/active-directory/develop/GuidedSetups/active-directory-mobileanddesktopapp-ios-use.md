---
title: "aaaAzure AD v2 iOS komma igång - Använd | Microsoft Docs"
description: "Hur iOS (Swift)-program kan anropa ett API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 22e67850e2e0b14b6d68815d8f23e18ce2e878ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a>Använd hello Microsoft Authentication Library (MSAL) tooget en token för hello Microsoft Graph API

Öppna `ViewController.swift` och Ersätt hello kod med:

```swift
import UIKit
import MSAL

class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    let kClientID = "Your_Application_Id_Here"
    let kAuthority = "https://login.microsoftonline.com/common/v2.0"

    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    
    var accessToken = String()
    var applicationContext = MSALPublicClientApplication.init()

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

     // This button will invoke hello call toohello Microsoft Graph API. It uses the
     // built in Swift libraries toocreate a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check toosee if we have a current logged in user. If we don't, then we need toosign someone in.
            // We throw an interactionRequired so that we trigger hello interactive signin.
            
            if  try self.applicationContext.users().isEmpty {
                throw NSError.init(domain: "MSALErrorDomain", code: MSALErrorCode.interactionRequired.rawValue, userInfo: nil)
            } else {
            
            // Acquire a token for an existing user silently
            
            try self.applicationContext.acquireTokenSilent(forScopes: self.kScopes, user: applicationContext.users().first) { (result, error) in
    
                    if error == nil {
                        self.accessToken = (result?.accessToken)!
                        self.loggingText.text = "Refreshing token silently)"
                        self.loggingText.text = "Refreshed Access token is \(self.accessToken)"
                        
                        self.signoutButton.isEnabled = true;
                        self.getContentWithToken()
    
                    } else {
                        self.loggingText.text = "Could not acquire token silently: \(error ?? "No error information" as! Error)"
    
                    }
                }
            }
        }  catch let error as NSError {
            
            // interactionRequired means we need tooask hello user toosign-in. This usually happens
            // when hello user's Refresh Token is expired or if hello user has changed their password
            // among other possible reasons.
            
            if error.code == MSALErrorCode.interactionRequired.rawValue {
                
                self.applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in
                        if error == nil {
                            self.accessToken = (result?.accessToken)!
                            self.loggingText.text = "Access token is \(self.accessToken)"
                            self.signoutButton.isEnabled = true;
                            self.getContentWithToken()
                            
                        } else  {
                            self.loggingText.text = "Could not acquire token: \(error ?? "No error information" as! Error)"
                        }
                }
                
            }
            
        } catch {
            
            // This is hello catch all error.
            
            self.loggingText.text = "Unable tooacquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable toocreate Application Context. Error: \(error)"
        }
    }


    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        
        if self.accessToken.isEmpty {
            signoutButton.isEnabled = false; 
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Mer information
#### <a name="getting-a-user-token-interactively"></a>Hämta token för en användare interaktivt
Anropar hello `acquireToken` metoden resulterar i en webbläsare fönstret fråga hello användaren toosign i. Program kräver vanligen en användare toosign i interaktivt hello första gången som de behöver tooaccess en skyddad resurs eller när en tyst åtgärden tooacquire en token misslyckas (t.ex. hello användarens lösenord har upphört att gälla).

#### <a name="getting-a-user-token-silently"></a>Hämta token för en användare tyst
Hej `acquireTokenSilent` metoden hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion. Efter `acquireToken` för hello körs första gången `acquireTokenSilent` hello metod som används ofta tooobtain tokens som används för tooaccess skyddade resurser för efterföljande anrop - som anropar toorequest eller förnya token görs tyst.

Slutligen `acquireTokenSilent` misslyckas – t.ex. hello användaren har loggat ut eller har ändrat sitt lösenord på en annan enhet. När MSAL känner av att hello problemet kan lösas genom att kräva en interaktiv åtgärd den utlöses en `MSALErrorCode.interactionRequired` undantag. Programmet kan hantera det här undantaget på två sätt:

1.  Gör ett anrop mot `acquireToken` omedelbart, vilket resulterar i att fråga hello användaren toosign i. Det här mönstret används vanligtvis i Onlineprogram där det finns inget offline innehåll i hello program tillgängliga för hello användare. hello exempelprogrammet som genererats av den här interaktiv installation använder det här mönstret: du kan se den i åtgärden hello första gången du kör programmet hello. Eftersom ingen användare har använt hello programmet `applicationContext.users().first` innehåller ett null-värde och ett ` MSALErrorCode.interactionRequired ` undantag. Hej koden i hello exempel sedan handtag hello undantag genom att anropa `acquireToken` vilket resulterar i att fråga hello användaren toosign i.

2.  Program kan också göra en indikering toohello användare som en interaktiv inloggning krävs, så hello användaren kan välja hello rätt tidpunkt toosign i eller hello program kan försöka `acquireTokenSilent` vid ett senare tillfälle. Detta används vanligtvis när hello användare kan använda andra funktioner i programmet hello utan något stör – t.ex, det finns innehåll offline i hello program. I det här fallet hello användare kan bestämma när de vill toosign i tooaccess hello skyddade resursen toorefresh hello inaktuell information eller ditt program kan bestämma tooretry `acquireTokenSilent` när nätverket återställs efter att ha tillfälligt otillgänglig.

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Anropa hello Microsoft Graph API använder du bara fick hello-token

Lägg till hello nya metoden nedan för`ViewController.swift`. Den här metoden är att använda toomake en `GET` begäran mot hello Microsoft Graph API med hjälp av en *HTTP Authorization-huvud*:

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify hello Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set hello Authorization header for hello request. We use Bearer tokens, so we specify Bearer + hello token we got from hello result
    request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
    let urlSession = URLSession(configuration: sessionConfig, delegate: self, delegateQueue: OperationQueue.main)
    
    urlSession.dataTask(with: request) { data, response, error in
        let result = try? JSONSerialization.jsonObject(with: data!, options: [])
                    if result != nil {
                
                self.loggingText.text = result.debugDescription
            }
        }.resume()
}
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Att göra ett REST-anrop mot ett skyddade API

I det här exempelprogrammet hello `getContentWithToken()` metod är att använda toomake HTTP `GET` begäran mot en skyddad resurs som kräver en token och sedan returnera hello innehåll toohello anropare. Den här metoden lägger till hello hämta token i hello *HTTP Authorization-huvud*. För det här exemplet hello resursen är hello Microsoft Graph API *mig* slutpunkt – som visar hello användarens profilinformation.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Ställ in utloggning

Lägg till följande metod för hello`ViewController.swift` toosign ut hello användare:

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from hello cache for this application for hello provided user
        // first parameter:   hello user tooremove from hello cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Mer information om utloggning

Hej `signoutButton` metoden tar bort hello användaren från hello MSAL användarcachen – detta effektivt talar MSAL tooforget hello aktuell användare så att en begäran om framtida tooacquire en token lyckas bara om det görs toobe interaktiva.

Hello-programmet i det här exemplet stöder en enskild användare, MSAL har stöd för scenarier där flera konton kan logga in på hello samtidigt – ett exempel är ett e-postprogram där en användare har flera konton.
<!--end-collapse-->

## <a name="register-hello-callback"></a>Registrera hello motringning

När hello användare autentiseras omdirigerar hello webbläsare hello tillbaka toohello användarprogram. Gör hello nedan tooregister denna motringning:

1.  Öppna `AppDelegate.swift` och importera MSAL:

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Lägg till följande metod tooyour hello <code>AppDelegate</code> klassen toohandle återanrop:
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if hello URL matches hello redirect URI for a pending AppAuth
// authorization request and if so, will look for hello code in hello response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```

