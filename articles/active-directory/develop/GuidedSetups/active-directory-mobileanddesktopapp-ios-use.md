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
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a><span data-ttu-id="2cdf5-103">Använd hello Microsoft Authentication Library (MSAL) tooget en token för hello Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="2cdf5-103">Use hello Microsoft Authentication Library (MSAL) tooget a token for hello Microsoft Graph API</span></span>

<span data-ttu-id="2cdf5-104">Öppna `ViewController.swift` och Ersätt hello kod med:</span><span class="sxs-lookup"><span data-stu-id="2cdf5-104">Open `ViewController.swift` and replace hello code with:</span></span>

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
### <a name="more-information"></a><span data-ttu-id="2cdf5-105">Mer information</span><span class="sxs-lookup"><span data-stu-id="2cdf5-105">More Information</span></span>
#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="2cdf5-106">Hämta token för en användare interaktivt</span><span class="sxs-lookup"><span data-stu-id="2cdf5-106">Getting a user token interactively</span></span>
<span data-ttu-id="2cdf5-107">Anropar hello `acquireToken` metoden resulterar i en webbläsare fönstret fråga hello användaren toosign i.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-107">Calling hello `acquireToken` method results in a browser window prompting hello user toosign in.</span></span> <span data-ttu-id="2cdf5-108">Program kräver vanligen en användare toosign i interaktivt hello första gången som de behöver tooaccess en skyddad resurs eller när en tyst åtgärden tooacquire en token misslyckas (t.ex. hello användarens lösenord har upphört att gälla).</span><span class="sxs-lookup"><span data-stu-id="2cdf5-108">Applications usually require a user toosign in interactively hello first time they need tooaccess a protected resource, or when a silent operation tooacquire a token fails (e.g. hello user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="2cdf5-109">Hämta token för en användare tyst</span><span class="sxs-lookup"><span data-stu-id="2cdf5-109">Getting a user token silently</span></span>
<span data-ttu-id="2cdf5-110">Hej `acquireTokenSilent` metoden hanterar token förvärv av organisationer och förnyelse utan någon användarinteraktion.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-110">hello `acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="2cdf5-111">Efter `acquireToken` för hello körs första gången `acquireTokenSilent` hello metod som används ofta tooobtain tokens som används för tooaccess skyddade resurser för efterföljande anrop - som anropar toorequest eller förnya token görs tyst.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-111">After `acquireToken` is executed for hello first time, `acquireTokenSilent` is hello method commonly used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>

<span data-ttu-id="2cdf5-112">Slutligen `acquireTokenSilent` misslyckas – t.ex. hello användaren har loggat ut eller har ändrat sitt lösenord på en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-112">Eventually, `acquireTokenSilent` will fail – e.g. hello user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="2cdf5-113">När MSAL känner av att hello problemet kan lösas genom att kräva en interaktiv åtgärd den utlöses en `MSALErrorCode.interactionRequired` undantag.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-113">When MSAL detects that hello issue can be resolved by requiring an interactive action, it fires an `MSALErrorCode.interactionRequired` exception.</span></span> <span data-ttu-id="2cdf5-114">Programmet kan hantera det här undantaget på två sätt:</span><span class="sxs-lookup"><span data-stu-id="2cdf5-114">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="2cdf5-115">Gör ett anrop mot `acquireToken` omedelbart, vilket resulterar i att fråga hello användaren toosign i.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-115">Make a call against `acquireToken` immediately, which results in prompting hello user toosign in.</span></span> <span data-ttu-id="2cdf5-116">Det här mönstret används vanligtvis i Onlineprogram där det finns inget offline innehåll i hello program tillgängliga för hello användare.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-116">This pattern is usually used in online applications where there is no offline content in hello application available for hello user.</span></span> <span data-ttu-id="2cdf5-117">hello exempelprogrammet som genererats av den här interaktiv installation använder det här mönstret: du kan se den i åtgärden hello första gången du kör programmet hello.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-117">hello sample application generated by this guided setup uses this pattern: you can see it in action hello first time you execute hello application.</span></span> <span data-ttu-id="2cdf5-118">Eftersom ingen användare har använt hello programmet `applicationContext.users().first` innehåller ett null-värde och ett ` MSALErrorCode.interactionRequired ` undantag.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-118">Because no user ever used hello application, `applicationContext.users().first` will contain a null value, and an ` MSALErrorCode.interactionRequired ` exception will be thrown.</span></span> <span data-ttu-id="2cdf5-119">Hej koden i hello exempel sedan handtag hello undantag genom att anropa `acquireToken` vilket resulterar i att fråga hello användaren toosign i.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-119">hello code in hello sample then handles hello exception by calling `acquireToken` resulting in prompting hello user toosign in.</span></span>

2.  <span data-ttu-id="2cdf5-120">Program kan också göra en indikering toohello användare som en interaktiv inloggning krävs, så hello användaren kan välja hello rätt tidpunkt toosign i eller hello program kan försöka `acquireTokenSilent` vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-120">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="2cdf5-121">Detta används vanligtvis när hello användare kan använda andra funktioner i programmet hello utan något stör – t.ex, det finns innehåll offline i hello program.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-121">This is usually used when hello user can use other functionality of hello application without being disrupted - for example, there is offline content available in hello application.</span></span> <span data-ttu-id="2cdf5-122">I det här fallet hello användare kan bestämma när de vill toosign i tooaccess hello skyddade resursen toorefresh hello inaktuell information eller ditt program kan bestämma tooretry `acquireTokenSilent` när nätverket återställs efter att ha tillfälligt otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-122">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information, or your application can decide tooretry `acquireTokenSilent` when network is restored after being unavailable temporarily.</span></span>

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="2cdf5-123">Anropa hello Microsoft Graph API använder du bara fick hello-token</span><span class="sxs-lookup"><span data-stu-id="2cdf5-123">Call hello Microsoft Graph API using hello token you just obtained</span></span>

<span data-ttu-id="2cdf5-124">Lägg till hello nya metoden nedan för`ViewController.swift`.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-124">Add hello new method below too`ViewController.swift`.</span></span> <span data-ttu-id="2cdf5-125">Den här metoden är att använda toomake en `GET` begäran mot hello Microsoft Graph API med hjälp av en *HTTP Authorization-huvud*:</span><span class="sxs-lookup"><span data-stu-id="2cdf5-125">This method is used toomake a `GET` request against hello Microsoft Graph API using an *HTTP Authorization header*:</span></span>

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
### <a name="making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="2cdf5-126">Att göra ett REST-anrop mot ett skyddade API</span><span class="sxs-lookup"><span data-stu-id="2cdf5-126">Making a REST call against a protected API</span></span>

<span data-ttu-id="2cdf5-127">I det här exempelprogrammet hello `getContentWithToken()` metod är att använda toomake HTTP `GET` begäran mot en skyddad resurs som kräver en token och sedan returnera hello innehåll toohello anropare.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-127">In this sample application, hello `getContentWithToken()` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="2cdf5-128">Den här metoden lägger till hello hämta token i hello *HTTP Authorization-huvud*.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-128">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="2cdf5-129">För det här exemplet hello resursen är hello Microsoft Graph API *mig* slutpunkt – som visar hello användarens profilinformation.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-129">For this sample, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>
<!--end-collapse-->

## <a name="set-up-sign-out"></a><span data-ttu-id="2cdf5-130">Ställ in utloggning</span><span class="sxs-lookup"><span data-stu-id="2cdf5-130">Set up sign-out</span></span>

<span data-ttu-id="2cdf5-131">Lägg till följande metod för hello`ViewController.swift` toosign ut hello användare:</span><span class="sxs-lookup"><span data-stu-id="2cdf5-131">Add hello following method too`ViewController.swift` toosign out hello user:</span></span>

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
### <a name="more-info-on-sign-out"></a><span data-ttu-id="2cdf5-132">Mer information om utloggning</span><span class="sxs-lookup"><span data-stu-id="2cdf5-132">More info on sign-out</span></span>

<span data-ttu-id="2cdf5-133">Hej `signoutButton` metoden tar bort hello användaren från hello MSAL användarcachen – detta effektivt talar MSAL tooforget hello aktuell användare så att en begäran om framtida tooacquire en token lyckas bara om det görs toobe interaktiva.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-133">hello `signoutButton` method removes hello user from hello MSAL user cache – this will effectively tell MSAL tooforget hello current user so a future request tooacquire a token will only succeed if it is made toobe interactive.</span></span>

<span data-ttu-id="2cdf5-134">Hello-programmet i det här exemplet stöder en enskild användare, MSAL har stöd för scenarier där flera konton kan logga in på hello samtidigt – ett exempel är ett e-postprogram där en användare har flera konton.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-134">Although hello application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed in at hello same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="register-hello-callback"></a><span data-ttu-id="2cdf5-135">Registrera hello motringning</span><span class="sxs-lookup"><span data-stu-id="2cdf5-135">Register hello callback</span></span>

<span data-ttu-id="2cdf5-136">När hello användare autentiseras omdirigerar hello webbläsare hello tillbaka toohello användarprogram.</span><span class="sxs-lookup"><span data-stu-id="2cdf5-136">Once hello user authenticates, hello browser redirects hello user back toohello application.</span></span> <span data-ttu-id="2cdf5-137">Gör hello nedan tooregister denna motringning:</span><span class="sxs-lookup"><span data-stu-id="2cdf5-137">Follow hello steps below tooregister this callback:</span></span>

1.  <span data-ttu-id="2cdf5-138">Öppna `AppDelegate.swift` och importera MSAL:</span><span class="sxs-lookup"><span data-stu-id="2cdf5-138">Open `AppDelegate.swift` and import MSAL:</span></span>

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="2cdf5-139">Lägg till följande metod tooyour hello <code>AppDelegate</code> klassen toohandle återanrop:</span><span class="sxs-lookup"><span data-stu-id="2cdf5-139">Add hello following method tooyour <code>AppDelegate</code> class toohandle callbacks:</span></span>
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

