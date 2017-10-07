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
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Bridge Android webbvy med inbyggda Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Android platslänksbrygga](mobile-engagement-bridge-webview-native-android.md)
> * [iOS platslänksbrygga](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Vissa mobila appar är utformade som en hybrid-app där själva hello appen utvecklas med hjälp av interna Android-utveckling men vissa eller alla hello skärmar återges i en Android webbvy. Du kan fortfarande använda Mobile Engagement Android SDK i dessa appar och den här självstudiekursen beskriver hur toogo om hur du gör detta. hello exempelkoden nedan baseras på hello Android dokumentationen [här](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Beskriver hur den här dokumenterade metoden kan användas tooimplement hello samma för Mobile Engagement Android SDK: ns vanliga metoder så att en webbvy från en hybrid-app kan också initiera begäranden tootrack händelser, jobb, fel, app-info när skicka dem via vår Android SDK. 

1. Du måste först och främst tooensure som du har gått igenom våra [komma igång-kursen](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK i din hybrid-app. När du har gjort det, din `OnCreate` metod ser ut hello följande.  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. Nu se till att appen hybrid har en skärm med en webbvy på den. hello-koden för den kommer att vara liknande toohello följande där vi läser in en lokal HTML-fil **Sample.html** i hello webbvy i hello `onCreate` metod på skärmen. 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. Nu skapa en brygga-fil som heter **WebAppInterface** som skapar en omslutning över vissa ofta används Mobile Engagement Android SDK-metoder med hello `@JavascriptInterface` metod som beskrivs i hello [Android-dokumentation ](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):
   
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
4. När vi har skapat hello ovan bridge-fil, måste vi tooensure att den är associerad med vår webbvy. För den här toohappen måste tooedit din `SetWebview` metoden så att den ser ut som följande hello:
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. I hello ovan fragment, vi har ringt `addJavascriptInterface` tooassociate våra bridge klassen med vår webbvy och dessutom skapas en referens som kallas **EngagementJs** toocall hello metoder från hello bridge-fil. 
6. Nu skapa hello följande fil som heter **Sample.html** i ditt projekt i en mapp som kallas **tillgångar** som läses in i hello webbvy och där vi ringer hello metoder från hello bridge-fil.
   
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
7. Obs hello följande pekar om hello HTML-fil ovan:
   
   * Den innehåller en uppsättning inkommande rutor där du kan ange data toobe användas som namn för din händelse, jobb, fel, AppInfo. När du klickar på hello knappen Nästa tooit görs ett anrop toohello Javascript som eventuellt anropar hello metoder från hello bridge filen toopass det här anropet toohello Mobile Engagement Android SDK. 
   * Vi märkning på vissa statiska extra information toohello händelser, jobb och även fel toodemonstrate hur detta kan göras. Den här extra informationen skickas som en JSON-strängen, som om du tittar i hello `WebAppInterface` filen parsas och placeras i en Android `Bundle` och skickas tillsammans med skickar händelser, jobb, fel. 
   * Ett jobb för Mobile Engagement har inletts med hello namnet du anger i hello-textrutan kör för 10 sekunder och stänga av. 
   * En Mobile Engagement appinfo eller taggen skickas med 'customer_name' som hello statiska nyckel och hello-värde som du angav i hello indata som hello värde för hello-tagg. 
8. Kör hello appen och du ser hello följande. Nu anger du ett namn för en test som hello följande och klickar på **skicka** under den. 
   
    ![][1]
9. Nu om du går toohello **övervakaren** fliken i din app och titta under **händelser -> information**, visas den här händelsen visas tillsammans med hello statiska appinfon som vi skickar. 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
