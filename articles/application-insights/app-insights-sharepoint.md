---
title: "Övervaka en SharePoint-webbplats med Application Insights"
description: "Börja övervaka ett nytt program med en ny instrumentationsnyckel"
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 2bfe5910-d673-4cf6-a5c1-4c115eae1be0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2016
ms.author: mbullwin
ms.openlocfilehash: 9c07ba125e0f9eae2b8f94661abf6dc1efc0cdad
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Övervaka en SharePoint-webbplats med Application Insights
Azure Application Insights övervakar dina appars tillgänglighet, prestanda och användning. Här lär du dig att ställa in det för en SharePoint-webbplats.

## <a name="create-an-application-insights-resource"></a>Skapa en Application Insights-resurs
Skapa en ny Application Insights-resurs på [Azure Portal](https://portal.azure.com). Välj ASP.NET som programtypen.

![Klicka på Egenskaper, markera nyckeln och tryck på CTRL + C.](./media/app-insights-sharepoint/01-new.png)

Bladet som öppnas är den plats där du kan se prestanda- och användningsdata om din app. Om du vill komma tillbaka till det nästa gång du loggar in på Azure bör du hitta en panel för det på startskärmen. Du kan också klicka på Bläddra för att hitta det.

## <a name="add-our-script-to-your-web-pages"></a>Lägga till våra skript på dina webbsidor
Hämta skriptet för webbsidor i Snabbstart:

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

Infoga skriptet precis före &lt;/head&gt;-taggen för alla sidor som du vill spåra. Om din webbplats har en huvudsida kan du placera skriptet där. Till exempel, i ett ASP.NET MVC-projekt, placerar du det i View\Shared\_Layout.cshtml

Skriptet innehåller instrumenteringsnyckeln som dirigerar telemetri till din Application Insights-resurs.

### <a name="add-the-code-to-your-site-pages"></a>Lägg till kod på dina webbsidor
#### <a name="on-the-master-page"></a>På huvudsidan
Om du kan redigera webbplatsens huvudsida kan du övervaka alla sidor på webbplatsen.

Spara huvudsidan och redigera den med hjälp av SharePoint Designer eller någon annan redigerare.

![](./media/app-insights-sharepoint/03-master.png)

Lägg till kod precis före taggen </head>. 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>Eller på enskilda sidor
Lägg till skript på varje enskild sida som du vill övervaka. 

Infoga en webbdel och bädda in kodfragmentet i den.

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Visa data om din app
Distribuera om din app.

Gå tillbaka till ditt programblad i [Azure Portal](https://portal.azure.com).

De första händelserna visas i sökningen. 

![](./media/app-insights-sharepoint/09-search.png)

Klicka på Uppdatera efter några sekunder om du väntade dig mer data.

Klicka på bladet översikt och klicka på **användningsanalys** för att se diagram för användare, sessioner och sidvyer:

![](./media/app-insights-sharepoint/06-usage.png)

Klicka på diagram om du vill se mer information – till exempel sidvyer:

![](./media/app-insights-sharepoint/07-pages.png)

Eller användare:

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a>Hämta användar-Id
Kodfragmentet för standardwebbplatsen hämtar inte användar-ID från SharePoint, men du kan göra detta med en liten ändring.

1. Kopiera din apps instrumentationsnyckeln från Essentials listrutan i Application Insights. 

    ![](./media/app-insights-sharepoint/02-props.png)

1. Ersätta instrumentationsnyckeln 'XXXX' i kodfragmentet nedan. 
2. Bädda in skriptet i en SharePoint-app i stället för utdraget som du får från portalen.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that the SP.UserProfiles.js file is loaded before the custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get the current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for the target user. 
    // To get the PersonProperties object for the current user, use the 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load the PersonProperties object and send the request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if the executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if the executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>Nästa steg
* [Webbtest](app-insights-monitor-web-app-availability.md) för att övervaka tillgängligheten för webbplatsen.
* [Application Insights](app-insights-overview.md) för andra typer av appar.

<!--Link references-->


