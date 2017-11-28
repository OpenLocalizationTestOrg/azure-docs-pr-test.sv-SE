---
title: aaaBridge iOS webbvy med inbyggda Mobile Engagement iOS SDK
description: "Beskriver hur toocreate en brygga mellan webbvy kör Javascript och hello inbyggda Mobile Engagement iOS SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: e1d6ff6f-cd67-4131-96eb-c3d6318de752
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 089ed8484722cb5ba624e5dce0e670ab56de514d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a><span data-ttu-id="35d83-103">Överbrygga iOS webbvy med inbyggda Mobile Engagement iOS SDK</span><span class="sxs-lookup"><span data-stu-id="35d83-103">Bridge iOS WebView with native Mobile Engagement iOS SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="35d83-104">Android platslänksbrygga</span><span class="sxs-lookup"><span data-stu-id="35d83-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="35d83-105">iOS platslänksbrygga</span><span class="sxs-lookup"><span data-stu-id="35d83-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="35d83-106">Vissa mobila appar är utformade som en hybrid-app där själva hello appen utvecklas med hjälp av inbyggda iOS Objective-C-utveckling men vissa eller alla hello skärmar återges i en iOS webbvy.</span><span class="sxs-lookup"><span data-stu-id="35d83-106">Some mobile apps are designed as a hybrid app where hello app itself is developed using native iOS Objective-C development but some or even all of hello screens are rendered within an iOS WebView.</span></span> <span data-ttu-id="35d83-107">Du kan fortfarande använda Mobile Engagement iOS SDK i dessa appar och den här självstudiekursen beskriver hur toogo om hur du gör detta.</span><span class="sxs-lookup"><span data-stu-id="35d83-107">You can still consume Mobile Engagement iOS SDK within such apps and this tutorial describes how toogo about doing this.</span></span> 

<span data-ttu-id="35d83-108">Det finns två metoder tooachieve detta även om båda är odokumenterade:</span><span class="sxs-lookup"><span data-stu-id="35d83-108">There are two approaches tooachieve this though both are undocumented:</span></span>

* <span data-ttu-id="35d83-109">Först beskrivs en på den här [länk](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) som innebär att registrera en `UIWebViewDelegate` på webbvy och catch-och-omedelbart-Avbryt en ändring i Javascript.</span><span class="sxs-lookup"><span data-stu-id="35d83-109">First one is described on this [link](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) which involves registering a `UIWebViewDelegate` on your web view and catch-and-immediatly-cancel a location change done in Javascript.</span></span> 
* <span data-ttu-id="35d83-110">Andra en baseras på detta [WWDC 2013 session](https://developer.apple.com/videos/play/wwdc2013/615), en metod som är tydligare än hello först och som vi följer för den här handboken.</span><span class="sxs-lookup"><span data-stu-id="35d83-110">Second one is based on this [WWDC 2013 session](https://developer.apple.com/videos/play/wwdc2013/615), an approach which is cleaner than hello first and which we will follow for this guide.</span></span> <span data-ttu-id="35d83-111">Observera att den här metoden fungerar bara i iOS7 och senare.</span><span class="sxs-lookup"><span data-stu-id="35d83-111">Note that this approach only works on iOS7 and above.</span></span> 

<span data-ttu-id="35d83-112">Följ hello stegen nedan för hello iOS överbrygga exempel:</span><span class="sxs-lookup"><span data-stu-id="35d83-112">Follow hello steps below for hello iOS bridge sample:</span></span>

1. <span data-ttu-id="35d83-113">Du måste först och främst tooensure som du har gått igenom våra [komma igång-kursen](mobile-engagement-ios-get-started.md) toointegrate hello Mobile Engagement iOS SDK i din hybrid-app.</span><span class="sxs-lookup"><span data-stu-id="35d83-113">First of all, you need tooensure that you have gone through our [Getting Started tutorial](mobile-engagement-ios-get-started.md) toointegrate hello Mobile Engagement iOS SDK in your hybrid app.</span></span> <span data-ttu-id="35d83-114">Alternativt kan aktivera du också test loggning på följande sätt så att du kan se hello SDK metoder som vi utlöser dem från hello webbvy.</span><span class="sxs-lookup"><span data-stu-id="35d83-114">Optionally, you can also enable test logging as follows so that you can see hello SDK methods as we trigger them from hello webview.</span></span> 
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
             [EngagementAgent setTestLogEnabled:YES];
           ....
        }
2. <span data-ttu-id="35d83-115">Nu se till att appen hybrid har en skärm med en webbvy på den.</span><span class="sxs-lookup"><span data-stu-id="35d83-115">Now make sure that your hybrid app has a screen with a webview on it.</span></span> <span data-ttu-id="35d83-116">Du kan lägga till den toohello `Main.storyboard` hello-appen.</span><span class="sxs-lookup"><span data-stu-id="35d83-116">You can add it toohello `Main.storyboard` of hello app.</span></span> 
3. <span data-ttu-id="35d83-117">Koppla den här webbvy med din **ViewController** genom att klicka och dra hello webbvy från hello View Controller scen toohello `ViewController.h` redigera kan du placera den under hello `@interface` rad.</span><span class="sxs-lookup"><span data-stu-id="35d83-117">Associate this webview with your **ViewController** by clicking and dragging hello webview from hello View Controller Scene toohello `ViewController.h` edit screen, placing it just below hello `@interface` line.</span></span> 
4. <span data-ttu-id="35d83-118">När du gör det visas en dialogruta som frågar om ett namn.</span><span class="sxs-lookup"><span data-stu-id="35d83-118">Once you do this, a dialog box will pop up asking for a name.</span></span> <span data-ttu-id="35d83-119">Ange hello namn som **webbvy**.</span><span class="sxs-lookup"><span data-stu-id="35d83-119">Provide hello name as **webView**.</span></span> <span data-ttu-id="35d83-120">Din `ViewController.h` fil ska se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="35d83-120">Your `ViewController.h` file should look like hello following:</span></span>
   
        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
   
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
   
        @end
5. <span data-ttu-id="35d83-121">Vi kommer att uppdatera hello `ViewController.m` senare filen men först hello bridge filen som skapar en omslutning över vissa vanliga Mobile Engagement iOS SDK-metoder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="35d83-121">We will update hello `ViewController.m` file later but first we will create hello bridge file which creates a wrapper over some commonly used Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="35d83-122">Skapa en ny rubrikfil kallas **EngagementJsExports.h** som använder hello `JSExport` mekanism som beskrivs i hello ovannämnda [session](https://developer.apple.com/videos/play/wwdc2013/615) tooexpose hello interna iOS-metoder.</span><span class="sxs-lookup"><span data-stu-id="35d83-122">Create a new header file called **EngagementJsExports.h** which uses hello `JSExport` mechanism described in hello aforementioned [session](https://developer.apple.com/videos/play/wwdc2013/615) tooexpose hello native iOS methods.</span></span> 
   
        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
   
        @protocol EngagementJsExports <JSExport>
   
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
   
        @end
   
        @interface EngagementJs : NSObject <EngagementJsExports>
   
        @end
6. <span data-ttu-id="35d83-123">Därefter skapar vi hello andra delen av hello bridge-fil.</span><span class="sxs-lookup"><span data-stu-id="35d83-123">Next we will create hello second part of hello bridge file.</span></span> <span data-ttu-id="35d83-124">Skapa en fil med namnet **EngagementJsExports.m** som innehåller hello implementering skapar hello faktiska omslutningar genom att anropa hello Mobile Engagement iOS SDK-metoder.</span><span class="sxs-lookup"><span data-stu-id="35d83-124">Create a file called **EngagementJsExports.m** which will contain hello implementation creating hello actual wrappers by calling hello Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="35d83-125">Även Observera att vi parsning hello `extras` som skickas från hello webbvy javascript och skicka som i en `NSMutableDictionary` objektet toobe skickas med hello Engagement SDK-metoden anrop.</span><span class="sxs-lookup"><span data-stu-id="35d83-125">Also note that we are parsing hello `extras` being passed from hello webview javascript and putting that into an `NSMutableDictionary` object toobe passed with hello Engagement SDK method calls.</span></span>  
   
        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
   
        @implementation EngagementJs
   
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
   
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
   
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
   
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
   
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
   
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
   
           return extras;
        }
   
        @end
7. <span data-ttu-id="35d83-126">Nu kommer tillbaka toohello **ViewController.m** och uppdatera den med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="35d83-126">Now we come back toohello **ViewController.m** and update it with hello following code:</span></span> 
   
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
   
        @interface ViewController ()
   
        @end
   
        @implementation ViewController
   
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
   
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
   
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
   
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
   
           context[@"EngagementJs"] = [EngagementJs class];
        }
   
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
   
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
   
        @end
8. <span data-ttu-id="35d83-127">Obs hello följande pekar om hello **ViewController.m** fil:</span><span class="sxs-lookup"><span data-stu-id="35d83-127">Note hello following points about hello **ViewController.m** file:</span></span>
   
   * <span data-ttu-id="35d83-128">I hello `loadWebView` metod, vi läser in en lokal HTML-fil som kallas **LocalPage.html** som koden som vi granskar nästa.</span><span class="sxs-lookup"><span data-stu-id="35d83-128">In hello `loadWebView` method, we are loading a local HTML file called **LocalPage.html** whose code we will review next.</span></span> 
   * <span data-ttu-id="35d83-129">I hello `webViewDidFinishLoad` -metoden vi ta tag hello `JsContext` och associera vår vinjettklass med den.</span><span class="sxs-lookup"><span data-stu-id="35d83-129">In hello `webViewDidFinishLoad` method, we are grabbing hello `JsContext` and associating our wrapper class with it.</span></span> <span data-ttu-id="35d83-130">Detta gör att anropa vår wrapper SDK metoder med hello referensen **EngagementJs** från hello webbvy.</span><span class="sxs-lookup"><span data-stu-id="35d83-130">This will allow calling our wrapper SDK methods using hello handle **EngagementJs** from hello webView.</span></span> 
9. <span data-ttu-id="35d83-131">Skapa en fil med namnet **LocalPage.html** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="35d83-131">Create a file called **LocalPage.html** with hello following code:</span></span>
   
        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
   
               <script type="text/javascript">
   
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
   
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with hello Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
   
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
   
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
   
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
   
           </head>
           <body>
               <h1>Bridge Tester</h1>
   
               <div id='engagement'>
   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
   
               </div>
           </body>
        </html>
10. <span data-ttu-id="35d83-132">Obs hello följande pekar om hello HTML-fil ovan:</span><span class="sxs-lookup"><span data-stu-id="35d83-132">Note hello following points about hello HTML file above:</span></span>
    
    * <span data-ttu-id="35d83-133">Den innehåller en uppsättning inkommande rutor där du kan ange data toobe användas som namn för din händelse, jobb, fel, AppInfo.</span><span class="sxs-lookup"><span data-stu-id="35d83-133">It contains a set of input boxes where you can provide data toobe used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="35d83-134">När du klickar på hello knappen Nästa tooit görs ett anrop toohello Javascript som eventuellt anropar hello metoder från hello bridge filen toopass det här anropet toohello Mobile Engagement iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="35d83-134">When you click on hello button next tooit, a call is made toohello Javascript which eventually calls hello methods from hello bridge file toopass this call toohello Mobile Engagement iOS SDK.</span></span> 
    * <span data-ttu-id="35d83-135">Vi märkning på vissa statiska extra information toohello händelser, jobb och även fel toodemonstrate hur detta kan göras.</span><span class="sxs-lookup"><span data-stu-id="35d83-135">We are tagging on some static extra info toohello events, jobs and even errors toodemonstrate how this could be done.</span></span> <span data-ttu-id="35d83-136">Den här extra informationen skickas som en JSON-strängen, som om du tittar i hello `EngagementJsExports.m` filen parsas och skickas tillsammans med skickar händelser, jobb, fel.</span><span class="sxs-lookup"><span data-stu-id="35d83-136">This extra info is sent as a JSON string which, if you look in hello `EngagementJsExports.m` file, is parsed and passed along with sending Events, Jobs, Errors.</span></span> 
    * <span data-ttu-id="35d83-137">Ett jobb för Mobile Engagement har inletts med hello namnet du anger i hello-textrutan kör för 10 sekunder och stänga av.</span><span class="sxs-lookup"><span data-stu-id="35d83-137">A Mobile Engagement Job is kicked off with hello name you specify in hello input box, run for 10 seconds and shut down.</span></span> 
    * <span data-ttu-id="35d83-138">En Mobile Engagement appinfo eller taggen skickas med 'customer_name' som hello statiska nyckel och hello-värde som du angav i hello indata som hello värde för hello-tagg.</span><span class="sxs-lookup"><span data-stu-id="35d83-138">A Mobile Engagement appinfo or tag is passed with 'customer_name' as hello static key and hello value that you entered in hello input as hello value for hello tag.</span></span> 
11. <span data-ttu-id="35d83-139">Kör hello appen och du ser hello följande.</span><span class="sxs-lookup"><span data-stu-id="35d83-139">Run hello app and you will see hello following.</span></span> <span data-ttu-id="35d83-140">Nu anger du ett namn för en test som hello följande och klickar på **skicka** nästa tooit.</span><span class="sxs-lookup"><span data-stu-id="35d83-140">Now provide some name for a test event like hello following and click **Send** next tooit.</span></span> 
    
     ![][1]
12. <span data-ttu-id="35d83-141">Nu om du går toohello **övervakaren** fliken i din app och titta under **händelser -> information**, visas den här händelsen visas tillsammans med hello statiska appinfon som vi skickar.</span><span class="sxs-lookup"><span data-stu-id="35d83-141">Now if you go toohello **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with hello static app-info that we are sending.</span></span> 
    
    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
