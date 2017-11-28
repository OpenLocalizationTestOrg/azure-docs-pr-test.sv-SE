---
title: "Azure AD v2 iOS komma igång - Använd | Microsoft Docs"
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
ms.openlocfilehash: 2ac1117a31a101705539a1f75520ce8de43809a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a><span data-ttu-id="ceb3c-103">Använd Microsoft Authentication Library (MSAL) för att hämta en token för Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="ceb3c-103">Use the Microsoft Authentication Library (MSAL) to get a token for the Microsoft Graph API</span></span>

<span data-ttu-id="ceb3c-104">Öppna `ViewController.swift` och Ersätt Koden med:</span><span class="sxs-lookup"><span data-stu-id="ceb3c-104">Open `ViewController.swift` and replace the code with:</span></span>

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

     // This button will invoke the call to the Microsoft Graph API. It uses the
     // built in Swift libraries to create a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check to see if we have a current logged in user. If we don't, then we need to sign someone in.
            // We throw an interactionRequired so that we trigger the interactive signin.
            
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
            
            // interactionRequired means we need to ask the user to sign-in. This usually happens
            // when the user's Refresh Token is expired or if the user has changed their password
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
            
            // This is the catch all error.
            
            self.loggingText.text = "Unable to acquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable to create Application Context. Error: \(error)"
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
### <a name="more-information"></a><span data-ttu-id="ceb3c-105">Mer information</span><span class="sxs-lookup"><span data-stu-id="ceb3c-105">More Information</span></span>
#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="ceb3c-106">Hämta token för en användare interaktivt</span><span class="sxs-lookup"><span data-stu-id="ceb3c-106">Getting a user token interactively</span></span>
<span data-ttu-id="ceb3c-107">Anropar den `acquireToken` metoden resulterar i ett webbläsarfönster som uppmanar användaren att logga in.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-107">Calling the `acquireToken` method results in a browser window prompting the user to sign in.</span></span> <span data-ttu-id="ceb3c-108">Program kräver vanligen en användare att logga in interaktivt första gången de behöver för att få åtkomst till en skyddad resurs, eller när en tyst åtgärd att hämta en token misslyckas (t.ex. användarens lösenord har upphört att gälla).</span><span class="sxs-lookup"><span data-stu-id="ceb3c-108">Applications usually require a user to sign in interactively the first time they need to access a protected resource, or when a silent operation to acquire a token fails (e.g. the user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="ceb3c-109">Hämta token för en användare tyst</span><span class="sxs-lookup"><span data-stu-id="ceb3c-109">Getting a user token silently</span></span>
<span data-ttu-id="ceb3c-110">Den `acquireTokenSilent` metoden hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-110">The `acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="ceb3c-111">Efter `acquireToken` körs för första gången `acquireTokenSilent` är den metod som används ofta för att hämta token som används för att komma åt skyddade resurser för efterföljande anrop - eftersom anrop till begära eller förnya token görs tyst.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-111">After `acquireToken` is executed for the first time, `acquireTokenSilent` is the method commonly used to obtain tokens used to access protected resources for subsequent calls - as calls to request or renew tokens are made silently.</span></span>

<span data-ttu-id="ceb3c-112">Slutligen `acquireTokenSilent` misslyckas – t.ex. användaren har loggat ut eller har ändrat sitt lösenord på en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-112">Eventually, `acquireTokenSilent` will fail – e.g. the user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="ceb3c-113">När MSAL upptäcker att problemet kan lösas genom att kräva en interaktiv åtgärd, den utlöses en `MSALErrorCode.interactionRequired` undantag.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-113">When MSAL detects that the issue can be resolved by requiring an interactive action, it fires an `MSALErrorCode.interactionRequired` exception.</span></span> <span data-ttu-id="ceb3c-114">Programmet kan hantera det här undantaget på två sätt:</span><span class="sxs-lookup"><span data-stu-id="ceb3c-114">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="ceb3c-115">Gör ett anrop mot `acquireToken` direkt, vilket innebär att användaren uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-115">Make a call against `acquireToken` immediately, which results in prompting the user to sign in.</span></span> <span data-ttu-id="ceb3c-116">Det här mönstret används vanligtvis i Onlineprogram där det finns inget offline innehåll i programmet för användaren.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-116">This pattern is usually used in online applications where there is no offline content in the application available for the user.</span></span> <span data-ttu-id="ceb3c-117">Exempelprogrammet som genererats av den här interaktiv installation använder det här mönstret: du kan se den i åtgärden första gången du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-117">The sample application generated by this guided setup uses this pattern: you can see it in action the first time you execute the application.</span></span> <span data-ttu-id="ceb3c-118">Eftersom ingen användare har använt programmet, `applicationContext.users().first` innehåller ett null-värde och ett ` MSALErrorCode.interactionRequired ` undantag.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-118">Because no user ever used the application, `applicationContext.users().first` will contain a null value, and an ` MSALErrorCode.interactionRequired ` exception will be thrown.</span></span> <span data-ttu-id="ceb3c-119">Koden i exemplet hanterar undantaget genom att anropa `acquireToken` ledde till att användaren uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-119">The code in the sample then handles the exception by calling `acquireToken` resulting in prompting the user to sign in.</span></span>

2.  <span data-ttu-id="ceb3c-120">Program kan också göra en indikering för användaren som en interaktiv inloggning krävs, så att användaren kan välja att logga in rätt tidpunkt eller programmet kan försöka `acquireTokenSilent` vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-120">Applications can also make a visual indication to the user that an interactive sign-in is required, so the user can select the right time to sign in, or the application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="ceb3c-121">Detta används vanligtvis när användaren kan använda andra funktioner i programmet utan att något stör – t.ex, det finns offline innehåll i programmet.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-121">This is usually used when the user can use other functionality of the application without being disrupted - for example, there is offline content available in the application.</span></span> <span data-ttu-id="ceb3c-122">I det här fallet användaren kan välja när de vill logga in till den skydda resursen eller uppdatera inaktuell information eller ditt program kan välja att försök `acquireTokenSilent` när nätverket återställs efter att ha tillfälligt otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-122">In this case, the user can decide when they want to sign in to access the protected resource, or to refresh the outdated information, or your application can decide to retry `acquireTokenSilent` when network is restored after being unavailable temporarily.</span></span>

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a><span data-ttu-id="ceb3c-123">Anropa använder token som du precis har köpt Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="ceb3c-123">Call the Microsoft Graph API using the token you just obtained</span></span>

<span data-ttu-id="ceb3c-124">Lägg till den nya metoden nedan till `ViewController.swift`.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-124">Add the new method below to `ViewController.swift`.</span></span> <span data-ttu-id="ceb3c-125">Den här metoden används för att göra en `GET` begäran mot Microsoft Graph API med hjälp av en *HTTP Authorization-huvud*:</span><span class="sxs-lookup"><span data-stu-id="ceb3c-125">This method is used to make a `GET` request against the Microsoft Graph API using an *HTTP Authorization header*:</span></span>

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify the Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
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
### <a name="making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="ceb3c-126">Att göra ett REST-anrop mot ett skyddade API</span><span class="sxs-lookup"><span data-stu-id="ceb3c-126">Making a REST call against a protected API</span></span>

<span data-ttu-id="ceb3c-127">I det här exempelprogrammet i `getContentWithToken()` metoden används för att en HTTP `GET` begäran mot en skyddad resurs som kräver ett token och returnera innehållet till anroparen.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-127">In this sample application, the `getContentWithToken()` method is used to make an HTTP `GET` request against a protected resource that requires a token and then return the content to the caller.</span></span> <span data-ttu-id="ceb3c-128">Den här metoden lägger till anskaffats token i den *HTTP Authorization-huvud*.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-128">This method adds the acquired token in the *HTTP Authorization header*.</span></span> <span data-ttu-id="ceb3c-129">Resursen för det här exemplet är Microsoft Graph API *mig* slutpunkt – som visar information om användarens profil.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-129">For this sample, the resource is the Microsoft Graph API *me* endpoint – which displays the user's profile information.</span></span>
<!--end-collapse-->

## <a name="set-up-sign-out"></a><span data-ttu-id="ceb3c-130">Ställ in utloggning</span><span class="sxs-lookup"><span data-stu-id="ceb3c-130">Set up sign-out</span></span>

<span data-ttu-id="ceb3c-131">Lägg till följande metod för att `ViewController.swift` logga ut användaren:</span><span class="sxs-lookup"><span data-stu-id="ceb3c-131">Add the following method to `ViewController.swift` to sign out the user:</span></span>

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from the cache for this application for the provided user
        // first parameter:   The user to remove from the cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a><span data-ttu-id="ceb3c-132">Mer information om utloggning</span><span class="sxs-lookup"><span data-stu-id="ceb3c-132">More info on sign-out</span></span>

<span data-ttu-id="ceb3c-133">Den `signoutButton` metoden tar bort användaren från användarcachen MSAL – detta effektivt talar MSAL att glömma den aktuella användaren, så en framtida begäran om att hämta en token lyckas bara om det görs för att interaktivt.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-133">The `signoutButton` method removes the user from the MSAL user cache – this will effectively tell MSAL to forget the current user so a future request to acquire a token will only succeed if it is made to be interactive.</span></span>

<span data-ttu-id="ceb3c-134">Även om programmet i det här exemplet har stöd för en enskild användare, stöder MSAL scenarier där flera konton kan logga in på samma gång – ett exempel är ett e-postprogram där en användare har flera konton.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-134">Although the application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed in at the same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="register-the-callback"></a><span data-ttu-id="ceb3c-135">Registrera återanropet</span><span class="sxs-lookup"><span data-stu-id="ceb3c-135">Register the callback</span></span>

<span data-ttu-id="ceb3c-136">När användaren autentiseras omdirigeras användaren till programmet i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ceb3c-136">Once the user authenticates, the browser redirects the user back to the application.</span></span> <span data-ttu-id="ceb3c-137">Följ stegen nedan för att registrera den här motringning:</span><span class="sxs-lookup"><span data-stu-id="ceb3c-137">Follow the steps below to register this callback:</span></span>

1.  <span data-ttu-id="ceb3c-138">Öppna `AppDelegate.swift` och importera MSAL:</span><span class="sxs-lookup"><span data-stu-id="ceb3c-138">Open `AppDelegate.swift` and import MSAL:</span></span>

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="ceb3c-139">Lägg till följande metod för att din <code>AppDelegate</code> klassen för att hantera återanrop:</span><span class="sxs-lookup"><span data-stu-id="ceb3c-139">Add the following method to your <code>AppDelegate</code> class to handle callbacks:</span></span>
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if the URL matches the redirect URI for a pending AppAuth
// authorization request and if so, will look for the code in the response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```

