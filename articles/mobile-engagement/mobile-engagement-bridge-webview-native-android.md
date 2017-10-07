---
title: aaaBridge Android webbvy med inbyggda Mobile Engagement Android SDK
description: "Beskriver hur toocreate en brygga mellan webbvy kör Javascript och hello inbyggda Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: cf272f3f-2b09-41b1-b190-944cdca8bba2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a7a09bcc156490fe69ad29a67809745dcfc22da6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a><span data-ttu-id="08754-103">Bridge Android webbvy med inbyggda Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="08754-103">Bridge Android WebView with native Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="08754-104">Android platslänksbrygga</span><span class="sxs-lookup"><span data-stu-id="08754-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="08754-105">iOS platslänksbrygga</span><span class="sxs-lookup"><span data-stu-id="08754-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="08754-106">Vissa mobila appar är utformade som en hybrid-app där själva hello appen utvecklas med hjälp av interna Android-utveckling men vissa eller alla hello skärmar återges i en Android webbvy.</span><span class="sxs-lookup"><span data-stu-id="08754-106">Some mobile apps are designed as a hybrid app where hello app itself is developed using native Android development but some or even all of hello screens are rendered within an Android WebView.</span></span> <span data-ttu-id="08754-107">Du kan fortfarande använda Mobile Engagement Android SDK i dessa appar och den här självstudiekursen beskriver hur toogo om hur du gör detta.</span><span class="sxs-lookup"><span data-stu-id="08754-107">You can still consume Mobile Engagement Android SDK within such apps and this tutorial describes how toogo about doing this.</span></span> <span data-ttu-id="08754-108">hello exempelkoden nedan baseras på hello Android dokumentationen [här](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span><span class="sxs-lookup"><span data-stu-id="08754-108">hello sample code below is based on hello Android documentation [here](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span></span> <span data-ttu-id="08754-109">Beskriver hur den här dokumenterade metoden kan användas tooimplement hello samma för Mobile Engagement Android SDK: ns vanliga metoder så att en webbvy från en hybrid-app kan också initiera begäranden tootrack händelser, jobb, fel, app-info när skicka dem via vår Android SDK.</span><span class="sxs-lookup"><span data-stu-id="08754-109">It describes how this documented approach could be used tooimplement hello same for Mobile Engagement Android SDK's commonly used methods such that a Webview from a hybrid app can also initiate requests tootrack events, jobs, errors, app-info while piping them via our Android SDK.</span></span> 

1. <span data-ttu-id="08754-110">Du måste först och främst tooensure som du har gått igenom våra [komma igång-kursen](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK i din hybrid-app.</span><span class="sxs-lookup"><span data-stu-id="08754-110">First of all, you need tooensure that you have gone through our [Getting Started tutorial](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK in your hybrid app.</span></span> <span data-ttu-id="08754-111">När du har gjort det, din `OnCreate` metod ser ut hello följande.</span><span class="sxs-lookup"><span data-stu-id="08754-111">Once you do that, your `OnCreate` method will look like hello following.</span></span>  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. <span data-ttu-id="08754-112">Nu se till att appen hybrid har en skärm med en webbvy på den.</span><span class="sxs-lookup"><span data-stu-id="08754-112">Now make sure that your hybrid app has a screen with a WebView on it.</span></span> <span data-ttu-id="08754-113">hello-koden för den kommer att vara liknande toohello följande där vi läser in en lokal HTML-fil **Sample.html** i hello webbvy i hello `onCreate` metod på skärmen.</span><span class="sxs-lookup"><span data-stu-id="08754-113">hello code for it will be similar toohello following where we are loading a local HTML file **Sample.html** in hello Webview in hello `onCreate` method of your screen.</span></span> 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. <span data-ttu-id="08754-114">Nu skapa en brygga-fil som heter **WebAppInterface** som skapar en omslutning över vissa ofta används Mobile Engagement Android SDK-metoder med hello `@JavascriptInterface` metod som beskrivs i hello [Android-dokumentation ](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span><span class="sxs-lookup"><span data-stu-id="08754-114">Now create a bridge file called **WebAppInterface** which creates a wrapper over some commonly used Mobile Engagement Android SDK methods using hello `@JavascriptInterface` approach described in hello [Android documentation](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span></span>
   
        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
   
        import com.microsoft.azure.engagement.EngagementAgent;
   
        import org.json.JSONArray;
        import org.json.JSONObject;
   
        public class WebAppInterface {
            Context mContext;
   
            /** Instantiate hello interface and set hello context */
            WebAppInterface(Context c) {
                mContext = c;
            }
   
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
   
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
   
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
   
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  
4. <span data-ttu-id="08754-115">När vi har skapat hello ovan bridge-fil, måste vi tooensure att den är associerad med vår webbvy.</span><span class="sxs-lookup"><span data-stu-id="08754-115">Once we have created hello above bridge file, we need tooensure that it is associated with our Webview.</span></span> <span data-ttu-id="08754-116">För den här toohappen måste tooedit din `SetWebview` metoden så att den ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="08754-116">For this toohappen, you need tooedit your `SetWebview` method so that it looks like hello following:</span></span>
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. <span data-ttu-id="08754-117">I hello ovan fragment, vi har ringt `addJavascriptInterface` tooassociate våra bridge klassen med vår webbvy och dessutom skapas en referens som kallas **EngagementJs** toocall hello metoder från hello bridge-fil.</span><span class="sxs-lookup"><span data-stu-id="08754-117">In hello above snippet, we called `addJavascriptInterface` tooassociate our bridge class with our Webview and also created a handle called **EngagementJs** toocall hello methods from hello bridge file.</span></span> 
6. <span data-ttu-id="08754-118">Nu skapa hello följande fil som heter **Sample.html** i ditt projekt i en mapp som kallas **tillgångar** som läses in i hello webbvy och där vi ringer hello metoder från hello bridge-fil.</span><span class="sxs-lookup"><span data-stu-id="08754-118">Now create hello following file called **Sample.html** in your project in a folder called **assets** which is loaded into hello Webview and where we will call hello methods from hello bridge file.</span></span>
   
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
                      log('window.onerror: ' + err);
                    }
   
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with hello Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
   
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
   
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
   
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
   
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
   
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
   
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
   
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>
7. <span data-ttu-id="08754-119">Obs hello följande pekar om hello HTML-fil ovan:</span><span class="sxs-lookup"><span data-stu-id="08754-119">Note hello following points about hello HTML file above:</span></span>
   
   * <span data-ttu-id="08754-120">Den innehåller en uppsättning inkommande rutor där du kan ange data toobe användas som namn för din händelse, jobb, fel, AppInfo.</span><span class="sxs-lookup"><span data-stu-id="08754-120">It contains a set of input boxes where you can provide data toobe used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="08754-121">När du klickar på hello knappen Nästa tooit görs ett anrop toohello Javascript som eventuellt anropar hello metoder från hello bridge filen toopass det här anropet toohello Mobile Engagement Android SDK.</span><span class="sxs-lookup"><span data-stu-id="08754-121">When you click on hello button next tooit, a call is made toohello Javascript which eventually calls hello methods from hello bridge file toopass this call toohello Mobile Engagement Android SDK.</span></span> 
   * <span data-ttu-id="08754-122">Vi märkning på vissa statiska extra information toohello händelser, jobb och även fel toodemonstrate hur detta kan göras.</span><span class="sxs-lookup"><span data-stu-id="08754-122">We are tagging on some static extra info toohello events, jobs and even errors toodemonstrate how this could be done.</span></span> <span data-ttu-id="08754-123">Den här extra informationen skickas som en JSON-strängen, som om du tittar i hello `WebAppInterface` filen parsas och placeras i en Android `Bundle` och skickas tillsammans med skickar händelser, jobb, fel.</span><span class="sxs-lookup"><span data-stu-id="08754-123">This extra info is sent as a JSON string which, if you look in hello `WebAppInterface` file, is parsed and put in an Android `Bundle` and is passed along with sending Events, Jobs, Errors.</span></span> 
   * <span data-ttu-id="08754-124">Ett jobb för Mobile Engagement har inletts med hello namnet du anger i hello-textrutan kör för 10 sekunder och stänga av.</span><span class="sxs-lookup"><span data-stu-id="08754-124">A Mobile Engagement Job is kicked off with hello name you specify in hello input box, run for 10 seconds and shut down.</span></span> 
   * <span data-ttu-id="08754-125">En Mobile Engagement appinfo eller taggen skickas med 'customer_name' som hello statiska nyckel och hello-värde som du angav i hello indata som hello värde för hello-tagg.</span><span class="sxs-lookup"><span data-stu-id="08754-125">A Mobile Engagement appinfo or tag is passed with 'customer_name' as hello static key and hello value that you entered in hello input as hello value for hello tag.</span></span> 
8. <span data-ttu-id="08754-126">Kör hello appen och du ser hello följande.</span><span class="sxs-lookup"><span data-stu-id="08754-126">Run hello app and you will see hello following.</span></span> <span data-ttu-id="08754-127">Nu anger du ett namn för en test som hello följande och klickar på **skicka** under den.</span><span class="sxs-lookup"><span data-stu-id="08754-127">Now provide some name for a test event like hello following and click **Send** below it.</span></span> 
   
    ![][1]
9. <span data-ttu-id="08754-128">Nu om du går toohello **övervakaren** fliken i din app och titta under **händelser -> information**, visas den här händelsen visas tillsammans med hello statiska appinfon som vi skickar.</span><span class="sxs-lookup"><span data-stu-id="08754-128">Now if you go toohello **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with hello static app-info that we are sending.</span></span> 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
