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
# <a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Överbrygga iOS webbvy med inbyggda Mobile Engagement iOS SDK
> [!div class="op_single_selector"]
> * [Android platslänksbrygga](mobile-engagement-bridge-webview-native-android.md)
> * [iOS platslänksbrygga](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Vissa mobila appar är utformade som en hybrid-app där själva hello appen utvecklas med hjälp av inbyggda iOS Objective-C-utveckling men vissa eller alla hello skärmar återges i en iOS webbvy. Du kan fortfarande använda Mobile Engagement iOS SDK i dessa appar och den här självstudiekursen beskriver hur toogo om hur du gör detta. 

Det finns två metoder tooachieve detta även om båda är odokumenterade:

* Först beskrivs en på den här [länk](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) som innebär att registrera en `UIWebViewDelegate` på webbvy och catch-och-omedelbart-Avbryt en ändring i Javascript. 
* Andra en baseras på detta [WWDC 2013 session](https://developer.apple.com/videos/play/wwdc2013/615), en metod som är tydligare än hello först och som vi följer för den här handboken. Observera att den här metoden fungerar bara i iOS7 och senare. 

Följ hello stegen nedan för hello iOS överbrygga exempel:

1. Du måste först och främst tooensure som du har gått igenom våra [komma igång-kursen](mobile-engagement-ios-get-started.md) toointegrate hello Mobile Engagement iOS SDK i din hybrid-app. Alternativt kan aktivera du också test loggning på följande sätt så att du kan se hello SDK metoder som vi utlöser dem från hello webbvy. 
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
             [EngagementAgent setTestLogEnabled:YES];
           ....
        }
2. Nu se till att appen hybrid har en skärm med en webbvy på den. Du kan lägga till den toohello `Main.storyboard` hello-appen. 
3. Koppla den här webbvy med din **ViewController** genom att klicka och dra hello webbvy från hello View Controller scen toohello `ViewController.h` redigera kan du placera den under hello `@interface` rad. 
4. När du gör det visas en dialogruta som frågar om ett namn. Ange hello namn som **webbvy**. Din `ViewController.h` fil ska se ut hello följande:
   
        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
   
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
   
        @end
5. Vi kommer att uppdatera hello `ViewController.m` senare filen men först hello bridge filen som skapar en omslutning över vissa vanliga Mobile Engagement iOS SDK-metoder ska skapas. Skapa en ny rubrikfil kallas **EngagementJsExports.h** som använder hello `JSExport` mekanism som beskrivs i hello ovannämnda [session](https://developer.apple.com/videos/play/wwdc2013/615) tooexpose hello interna iOS-metoder. 
   
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
6. Därefter skapar vi hello andra delen av hello bridge-fil. Skapa en fil med namnet **EngagementJsExports.m** som innehåller hello implementering skapar hello faktiska omslutningar genom att anropa hello Mobile Engagement iOS SDK-metoder. Även Observera att vi parsning hello `extras` som skickas från hello webbvy javascript och skicka som i en `NSMutableDictionary` objektet toobe skickas med hello Engagement SDK-metoden anrop.  
   
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
7. Nu kommer tillbaka toohello **ViewController.m** och uppdatera den med hello följande kod: 
   
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
8. Obs hello följande pekar om hello **ViewController.m** fil:
   
   * I hello `loadWebView` metod, vi läser in en lokal HTML-fil som kallas **LocalPage.html** som koden som vi granskar nästa. 
   * I hello `webViewDidFinishLoad` -metoden vi ta tag hello `JsContext` och associera vår vinjettklass med den. Detta gör att anropa vår wrapper SDK metoder med hello referensen **EngagementJs** från hello webbvy. 
9. Skapa en fil med namnet **LocalPage.html** med hello följande kod:
   
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
10. Obs hello följande pekar om hello HTML-fil ovan:
    
    * Den innehåller en uppsättning inkommande rutor där du kan ange data toobe användas som namn för din händelse, jobb, fel, AppInfo. När du klickar på hello knappen Nästa tooit görs ett anrop toohello Javascript som eventuellt anropar hello metoder från hello bridge filen toopass det här anropet toohello Mobile Engagement iOS SDK. 
    * Vi märkning på vissa statiska extra information toohello händelser, jobb och även fel toodemonstrate hur detta kan göras. Den här extra informationen skickas som en JSON-strängen, som om du tittar i hello `EngagementJsExports.m` filen parsas och skickas tillsammans med skickar händelser, jobb, fel. 
    * Ett jobb för Mobile Engagement har inletts med hello namnet du anger i hello-textrutan kör för 10 sekunder och stänga av. 
    * En Mobile Engagement appinfo eller taggen skickas med 'customer_name' som hello statiska nyckel och hello-värde som du angav i hello indata som hello värde för hello-tagg. 
11. Kör hello appen och du ser hello följande. Nu anger du ett namn för en test som hello följande och klickar på **skicka** nästa tooit. 
    
     ![][1]
12. Nu om du går toohello **övervakaren** fliken i din app och titta under **händelser -> information**, visas den här händelsen visas tillsammans med hello statiska appinfon som vi skickar. 
    
    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
