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
# <a name="monitor-a-sharepoint-site-with-application-insights"></a><span data-ttu-id="28445-103">Övervaka en SharePoint-webbplats med Application Insights</span><span class="sxs-lookup"><span data-stu-id="28445-103">Monitor a SharePoint site with Application Insights</span></span>
<span data-ttu-id="28445-104">Azure Application Insights övervakar hello tillgänglighet, prestanda och användning av dina appar.</span><span class="sxs-lookup"><span data-stu-id="28445-104">Azure Application Insights monitors hello availability, performance and usage of your apps.</span></span> <span data-ttu-id="28445-105">Här får du lära dig hur tooset den för en SharePoint-webbplats.</span><span class="sxs-lookup"><span data-stu-id="28445-105">Here you'll learn how tooset it up for a SharePoint site.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="28445-106">Skapa en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="28445-106">Create an Application Insights resource</span></span>
<span data-ttu-id="28445-107">I hello [Azure-portalen](https://portal.azure.com), skapa en ny Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="28445-107">In hello [Azure portal](https://portal.azure.com), create a new Application Insights resource.</span></span> <span data-ttu-id="28445-108">Välj ASP.NET som hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="28445-108">Choose ASP.NET as hello application type.</span></span>

![Klicka på egenskaper, markera hello nyckeln och tryck på ctrl + C](./media/app-insights-sharepoint/01-new.png)

<span data-ttu-id="28445-110">hello-bladet som öppnas är hello plats där du kan se prestanda- och användningsdata om din app.</span><span class="sxs-lookup"><span data-stu-id="28445-110">hello blade that opens is hello place where you'll see performance and usage data about your app.</span></span> <span data-ttu-id="28445-111">tooget tillbaka tooit nästa gång du loggar in tooAzure, bör du hitta en panel för den hello startskärmen.</span><span class="sxs-lookup"><span data-stu-id="28445-111">tooget back tooit next time you login tooAzure, you should find a tile for it on hello start screen.</span></span> <span data-ttu-id="28445-112">Du kan också klicka på Bläddra toofind den.</span><span class="sxs-lookup"><span data-stu-id="28445-112">Alternatively click Browse toofind it.</span></span>

## <a name="add-our-script-tooyour-web-pages"></a><span data-ttu-id="28445-113">Lägga till våra skriptet tooyour webbsidor</span><span class="sxs-lookup"><span data-stu-id="28445-113">Add our script tooyour web pages</span></span>
<span data-ttu-id="28445-114">Hämta hello skript för webbsidor i Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="28445-114">In Quick Start, get hello script for web pages:</span></span>

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

<span data-ttu-id="28445-115">Infoga hello skript innan hello &lt;/head&gt; taggen för varje sida som du vill tootrack.</span><span class="sxs-lookup"><span data-stu-id="28445-115">Insert hello script just before hello &lt;/head&gt; tag of every page you want tootrack.</span></span> <span data-ttu-id="28445-116">Om din webbplats har en huvudsida, kan du placera hello skript det.</span><span class="sxs-lookup"><span data-stu-id="28445-116">If your website has a master page, you can put hello script there.</span></span> <span data-ttu-id="28445-117">Till exempel, i ett ASP.NET MVC-projekt, placerar du det i View\Shared\_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="28445-117">For example, in an ASP.NET MVC project, you'd put it in View\Shared\_Layout.cshtml</span></span>

<span data-ttu-id="28445-118">hello skriptet innehåller hello instrumentation nyckeln som leder hello telemetri tooyour Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="28445-118">hello script contains hello instrumentation key that directs hello telemetry tooyour Application Insights resource.</span></span>

### <a name="add-hello-code-tooyour-site-pages"></a><span data-ttu-id="28445-119">Lägg till hello kodsidor tooyour plats</span><span class="sxs-lookup"><span data-stu-id="28445-119">Add hello code tooyour site pages</span></span>
#### <a name="on-hello-master-page"></a><span data-ttu-id="28445-120">På hello huvudsida</span><span class="sxs-lookup"><span data-stu-id="28445-120">On hello master page</span></span>
<span data-ttu-id="28445-121">Om du kan redigera hello platsens huvudsida, som ger övervakning för varje sida i hello plats.</span><span class="sxs-lookup"><span data-stu-id="28445-121">If you can edit hello site's master page, that will provide monitoring for every page in hello site.</span></span>

<span data-ttu-id="28445-122">Checka ut hello huvudsida och redigera den med hjälp av SharePoint Designer eller någon annan redigerare.</span><span class="sxs-lookup"><span data-stu-id="28445-122">Check out hello master page and edit it using SharePoint Designer or any other editor.</span></span>

![](./media/app-insights-sharepoint/03-master.png)

<span data-ttu-id="28445-123">Lägg till hello kod innan hello </head> tagg.</span><span class="sxs-lookup"><span data-stu-id="28445-123">Add hello code just before hello </head> tag.</span></span> 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a><span data-ttu-id="28445-124">Eller på enskilda sidor</span><span class="sxs-lookup"><span data-stu-id="28445-124">Or on individual pages</span></span>
<span data-ttu-id="28445-125">toomonitor en begränsad uppsättning sidor, lägga till hello skript separat tooeach sidan.</span><span class="sxs-lookup"><span data-stu-id="28445-125">toomonitor a limited set of pages, add hello script separately tooeach page.</span></span> 

<span data-ttu-id="28445-126">Infoga en webbdel och bädda in hello kodstycket i den.</span><span class="sxs-lookup"><span data-stu-id="28445-126">Insert a web part and embed hello code snippet in it.</span></span>

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a><span data-ttu-id="28445-127">Visa data om din app</span><span class="sxs-lookup"><span data-stu-id="28445-127">View data about your app</span></span>
<span data-ttu-id="28445-128">Distribuera om din app.</span><span class="sxs-lookup"><span data-stu-id="28445-128">Redeploy your app.</span></span>

<span data-ttu-id="28445-129">Returnerar tooyour programmet bladet i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="28445-129">Return tooyour application blade in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="28445-130">hello första händelser visas i sökningen.</span><span class="sxs-lookup"><span data-stu-id="28445-130">hello first events will appear in Search.</span></span> 

![](./media/app-insights-sharepoint/09-search.png)

<span data-ttu-id="28445-131">Klicka på Uppdatera efter några sekunder om du väntade dig mer data.</span><span class="sxs-lookup"><span data-stu-id="28445-131">Click Refresh after a few seconds if you're expecting more data.</span></span>

<span data-ttu-id="28445-132">Hello översikt bladet, klickar du på **användningsanalys** toosee toocharts av användare, sessioner och sidvyer:</span><span class="sxs-lookup"><span data-stu-id="28445-132">From hello overview blade, click **Usage analytics** toosee toocharts of users, sessions and page views:</span></span>

![](./media/app-insights-sharepoint/06-usage.png)

<span data-ttu-id="28445-133">På varje diagram toosee mer information – till exempel sidvisningar:</span><span class="sxs-lookup"><span data-stu-id="28445-133">Click any chart toosee more details - for example Page Views:</span></span>

![](./media/app-insights-sharepoint/07-pages.png)

<span data-ttu-id="28445-134">Eller användare:</span><span class="sxs-lookup"><span data-stu-id="28445-134">Or Users:</span></span>

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a><span data-ttu-id="28445-135">Hämta användar-Id</span><span class="sxs-lookup"><span data-stu-id="28445-135">Capturing User Id</span></span>
<span data-ttu-id="28445-136">hello standard webbsida kodstycke fånga inte hello användar-id från SharePoint, men du kan göra det med en mindre ändring.</span><span class="sxs-lookup"><span data-stu-id="28445-136">hello standard web page code snippet doesn't capture hello user id from SharePoint, but you can do that with a small modification.</span></span>

1. <span data-ttu-id="28445-137">Kopiera instrumentation appkey från hello Essentials listrutan i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="28445-137">Copy your app's instrumentation key from hello Essentials drop-down in Application Insights.</span></span> 

    ![](./media/app-insights-sharepoint/02-props.png)

1. <span data-ttu-id="28445-138">Ersätt hello instrumentation nyckeln för 'XXXX' i hello kodfragmentet nedan.</span><span class="sxs-lookup"><span data-stu-id="28445-138">Substitute hello instrumentation key for 'XXXX' in hello snippet below.</span></span> 
2. <span data-ttu-id="28445-139">Bädda in hello skript i din SharePoint-app i stället för hello fragment du får från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="28445-139">Embed hello script in your SharePoint app instead of hello snippet you get from hello portal.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="28445-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28445-140">Next Steps</span></span>
* <span data-ttu-id="28445-141">[Webbtester](app-insights-monitor-web-app-availability.md) toomonitor hello tillgängligheten för din webbplats.</span><span class="sxs-lookup"><span data-stu-id="28445-141">[Web tests](app-insights-monitor-web-app-availability.md) toomonitor hello availability of your site.</span></span>
* <span data-ttu-id="28445-142">[Application Insights](app-insights-overview.md) för andra typer av appar.</span><span class="sxs-lookup"><span data-stu-id="28445-142">[Application Insights](app-insights-overview.md) for other types of app.</span></span>

<!--Link references-->


