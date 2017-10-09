---
title: aaaMonitor en SharePoint-webbplats med Application Insights
description: "Börja övervaka ett nytt program med en ny instrumentationsnyckel"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2bfe5910-d673-4cf6-a5c1-4c115eae1be0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2016
ms.author: bwren
ms.openlocfilehash: acfe99c24a4d77daec1017de0442ec952a1faba2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Övervaka en SharePoint-webbplats med Application Insights
Azure Application Insights övervakar hello tillgänglighet, prestanda och användning av dina appar. Här får du lära dig hur tooset den för en SharePoint-webbplats.

## <a name="create-an-application-insights-resource"></a>Skapa en Application Insights-resurs
I hello [Azure-portalen](https://portal.azure.com), skapa en ny Application Insights-resurs. Välj ASP.NET som hello programtyp.

![Klicka på egenskaper, markera hello nyckeln och tryck på ctrl + C](./media/app-insights-sharepoint/01-new.png)

hello-bladet som öppnas är hello plats där du kan se prestanda- och användningsdata om din app. tooget tillbaka tooit nästa gång du loggar in tooAzure, bör du hitta en panel för den hello startskärmen. Du kan också klicka på Bläddra toofind den.

## <a name="add-our-script-tooyour-web-pages"></a>Lägga till våra skriptet tooyour webbsidor
Hämta hello skript för webbsidor i Snabbstart:

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

Infoga hello skript innan hello &lt;/head&gt; taggen för varje sida som du vill tootrack. Om din webbplats har en huvudsida, kan du placera hello skript det. Till exempel, i ett ASP.NET MVC-projekt, placerar du det i View\Shared\_Layout.cshtml

hello skriptet innehåller hello instrumentation nyckeln som leder hello telemetri tooyour Application Insights-resurs.

### <a name="add-hello-code-tooyour-site-pages"></a>Lägg till hello kodsidor tooyour plats
#### <a name="on-hello-master-page"></a>På hello huvudsida
Om du kan redigera hello platsens huvudsida, som ger övervakning för varje sida i hello plats.

Checka ut hello huvudsida och redigera den med hjälp av SharePoint Designer eller någon annan redigerare.

![](./media/app-insights-sharepoint/03-master.png)

Lägg till hello kod innan hello </head> tagg. 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>Eller på enskilda sidor
toomonitor en begränsad uppsättning sidor, lägga till hello skript separat tooeach sidan. 

Infoga en webbdel och bädda in hello kodstycket i den.

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Visa data om din app
Distribuera om din app.

Returnerar tooyour programmet bladet i hello [Azure-portalen](https://portal.azure.com).

hello första händelser visas i sökningen. 

![](./media/app-insights-sharepoint/09-search.png)

Klicka på Uppdatera efter några sekunder om du väntade dig mer data.

Hello översikt bladet, klickar du på **användningsanalys** toosee toocharts av användare, sessioner och sidvyer:

![](./media/app-insights-sharepoint/06-usage.png)

På varje diagram toosee mer information – till exempel sidvisningar:

![](./media/app-insights-sharepoint/07-pages.png)

Eller användare:

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a>Hämta användar-Id
hello standard webbsida kodstycke fånga inte hello användar-id från SharePoint, men du kan göra det med en mindre ändring.

1. Kopiera instrumentation appkey från hello Essentials listrutan i Application Insights. 

    ![](./media/app-insights-sharepoint/02-props.png)

1. Ersätt hello instrumentation nyckeln för 'XXXX' i hello kodfragmentet nedan. 
2. Bädda in hello skript i din SharePoint-app i stället för hello fragment du får från hello-portalen.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that hello SP.UserProfiles.js file is loaded before hello custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get hello current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for hello target user. 
    // tooget hello PersonProperties object for hello current user, use hello 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load hello PersonProperties object and send hello request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if hello executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if hello executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>Nästa steg
* [Webbtester](app-insights-monitor-web-app-availability.md) toomonitor hello tillgängligheten för din webbplats.
* [Application Insights](app-insights-overview.md) för andra typer av appar.

<!--Link references-->


