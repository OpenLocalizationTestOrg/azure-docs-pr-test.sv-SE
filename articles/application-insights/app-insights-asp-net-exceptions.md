---
title: aaaDiagnose fel och undantag i webbappar med Azure Application Insights | Microsoft Docs
description: "Fånga undantag från ASP.NET appar tillsammans med begärandetelemetri."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 8930e6d2b29f83ea635c4ecb7afd11fc1d97d085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Diagnostisera undantag i ditt webbprogram med Application Insights
Undantag i livewebbappar rapporteras av [Programinsikter](app-insights-overview.md). Du kan jämföra misslyckade begäranden med undantag och andra händelser på både hello klienten och servern, så att du snabbt kan diagnostisera hello orsaker.

## <a name="set-up-exception-reporting"></a>Konfigurera undantag reporting
* toohave undantag som rapporterats från din server:
  * Installera [Application Insights SDK](app-insights-asp-net.md) i din Appkod eller
  * IIS-webbservrar: kör [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); eller
  * Azure-webbappar: Lägg till hello [Application Insights Extension](app-insights-azure-web-apps.md)
  * Java-webbappar: Installera hello [Java-agent](app-insights-java-agent.md)
* Installera hello [JavaScript-kodfragment](app-insights-javascript.md) i din webbsidor toocatch webbläsarundantag.
* I vissa ramverk för programmet eller med vissa inställningar, behöver du tootake några extra steg toocatch flera undantag:
  * [Webbformulär](#web-forms)
  * [MVC](#mvc)
  * [Webb-API-1.*](#web-api-1)
  * [Webb-API 2.*](#web-api-2)
  * [WCF](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a>Diagnostisera undantag med Visual Studio
Öppna hello app lösningen i Visual Studio toohelp med felsökning.

Kör hello appen på din server eller på utvecklingsdatorn med F5.

Öppna fönstret för hello Application Insights Sök i Visual Studio och ange toodisplay händelser från din app. Du kan göra detta genom att klicka hello Application Insights-knappen när du felsöker.

![Högerklicka på hello-projektet och välj Application Insights, öppna.](./media/app-insights-asp-net-exceptions/34.png)

Observera att du kan filtrera hello rapporten tooshow bara undantag.

*Inga undantag visar? Se [fånga undantag](#exceptions).*

Klicka på ett undantag rapporten tooshow dess stack-spårning.
Klicka på en radreferens i hello stack-spårning, tooopen hello relevanta kodfilen.  

Observera att CodeLens visar data om hello undantag i hello kod:

![CodeLens meddelande om undantag.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-hello-azure-portal"></a>Felsökning av med hello Azure-portalen
Från hello Application Insights översikten över appen, hello fel innehåller diagram av undantag och misslyckade HTTP-begäranden, tillsammans med en lista över hello begära URL: er som orsakar hello de vanligaste fel.

![Välj inställningar för fel](./media/app-insights-asp-net-exceptions/012-start.png)

Klicka på ett av hello misslyckades undantag typer i hello listan tooget tooindividual förekomster av hello undantag, där du kan se hello information och Stackspårning:

![Välj en instans av en misslyckad begäran och under undantagsinformation, hämta tooinstances hello undantag.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

**Du kan också** du kan starta från hello lista med begäranden och hitta undantag relaterade tooit.

*Inga undantag visar? Se [fånga undantag](#exceptions).*


## <a name="custom-tracing-and-log-data"></a>Anpassade spårning och loggdata
tooget diagnostikdata specifika tooyour app som du kan infoga kod toosend telemetridata. Detta visas i diagnostiska tillsammans med hello begäran, vyn sida och andra automatiskt insamlade data.

Har du flera alternativ:

* [Trackevent ()](app-insights-api-custom-events-metrics.md#trackevent) används vanligtvis för att övervaka användningsmönster, men hello data skickas också visas under Anpassad händelser i diagnostiska sökningen. Händelser är namngivna och kan utföra strängegenskaper och numeriska mått som du kan [filtrera sökningen diagnostiska](app-insights-diagnostic-search.md).
* [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) kan du skicka längre data, till exempel efter information.
* [TrackException()](#exceptions) skickar Stacka spårningar. [Mer information om undantag](#exceptions).
* Om du redan använder ett loggningsramverk som Log4Net eller NLog, kan du [samla in dessa loggar](app-insights-asp-net-trace-logs.md) och se dem i diagnostiska sökningen tillsammans med data för begäran och undantag.

toosee dessa händelser, öppna [Sök](app-insights-diagnostic-search.md), öppna Filter och välj sedan Custom Event, spårning eller undantag.

![Detaljvisning](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> Om din app genererar mycket telemetri, minska hello anpassningsbar provtagning modulen automatiskt hello volymen som skickas toohello portal genom att skicka en representativ del av händelser. Händelser som är en del av hello samma åtgärd ska markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser. [Läs mer om sampling.](app-insights-sampling.md)
>
>

### <a name="how-toosee-request-post-data"></a>Hur toosee begära postdata
Information om begäran med inte hello data som skickas tooyour app i en POST-anrop. toohave rapporteras för dessa data:

* [Installera hello SDK](app-insights-asp-net.md) i projektet program.
* Infoga kod i dina program toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace). Skicka hello postdata i hello message-parameter. Det finns en gräns toohello tillåtna storlek, bör du toosend bara hello viktiga data.
* När du undersöker en misslyckad begäran hitta hello associerade spårningar.  

![Detaljvisning](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <a name="exceptions"></a>Fånga undantag och relaterade diagnostikdata
Först visas inte i hello portal alla hello-undantag som orsakar fel i din app. Du ser alla webbläsarundantag (om du använder hello [JavaScript SDK](app-insights-javascript.md) på webbsidorna). Men de flesta undantag fångas av IIS och du har toowrite lite kod toosee dem.

Du kan:

* **Logga undantag uttryckligen** genom att infoga kod i undantagshanterare tooreport hello undantag.
* **Fånga undantag automatiskt** genom att konfigurera ASP.NET framework. hello nödvändiga tillägg är olika för olika typer av framework.

## <a name="reporting-exceptions-explicitly"></a>Rapportering undantag uttryckligen
hello är enklaste sättet tooinsert en anropet tooTrackException() i en undantagshanterare.

JavaScript

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

C#

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send hello exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

VB

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send hello exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

hello egenskaper och mätningar parametrar är valfria, men är användbara för [filtrering och lägga till](app-insights-diagnostic-search.md) extra information. Till exempel om du har en app som kan köra flera spel hittade alla hello undantag rapporter relaterade tooa visst spel. Du kan lägga till så många objekt som du precis som tooeach ordlistan.

## <a name="browser-exceptions"></a>Webbläsarundantag
De flesta webbläsarundantag rapporteras.

Om din webbsida innehåller skriptfiler från nätverk för innehållsleverans eller andra domäner, kontrollera din skripttypen har hello attributet ```crossorigin="anonymous"```, och den hello-servern skickar [CORS huvuden](http://enable-cors.org/). Detta gör att du tooget stack-spårning och information för ohanterade JavaScript-undantag från dessa resurser.

## <a name="web-forms"></a>Webbformulär
Webbformulär ska hello HTTP-modul kunna toocollect hello undantag när det finns inga omdirigeringar som konfigurerats med CustomErrors.

Men om du har active omdirigeringar, lägger du till följande rader toohello Application_Error funktion i Global.asax.cs hello. (Lägg till en Global.asax-fil om du inte redan har ett.)

*C#*

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a>MVC
Om hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfigurationen är `Off`, undantag ska vara tillgängliga för hello [HTTP-modul](https://msdn.microsoft.com/library/ms178468.aspx) toocollect. Men om det är `RemoteOnly` (standard), eller `On`, hello undantag kommer att tas bort och inte tillgängligt för Application Insights tooautomatically samla in. Du kan åtgärda detta genom att åsidosätta hello [System.Web.Mvc.HandleErrorAttribute klassen](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), och tillämpa hello åsidosätts klass som visas för hello MVC-versionerna nedan ([github-källan](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report hello exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a>MVC 2
Ersätt hello HandleError attribut med din nya attribut i dina domänkontrollanter.

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[Exempel](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Registrera `AiHandleErrorAttribute` som ett globalt filter i Global.asax.cs:

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[Exempel](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
Registrera AiHandleErrorAttribute som ett globalt filter i FilterConfig.cs:

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with hello override tootrack unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[Exempel](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a>Webb-API 1.x
Åsidosätt System.Web.Http.Filters.ExceptionFilterAttribute:

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

Du kan lägga till den här åsidosatt attributet toospecific domänkontrollanter eller lägga till den toohello globala filterkonfiguration i hello WebApiConfig klass:

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[Exempel](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

Det finns ett antal fall som hello-undantagsfilter som inte kan hantera. Exempel:

* Undantag från domänkontrollanten konstruktorer.
* Undantag från meddelandehanterare.
* Undantag under routning.
* Undantag under innehåll serialiseringssvar.

## <a name="web-api-2x"></a>Webb-API 2.x
Lägg till en implementering av IExceptionLogger:

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

Lägga till den här toohello tjänsterna i WebApiConfig:

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  }

[Exempel](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Alternativt kan du:

1. Ersätt hello endast ExceptionHandler med en anpassad implementering av IExceptionHandler. Detta är endast anropas när hello framework fortfarande toochoose vilka svar meddelande toosend (inte när hello-anslutningen avbröts exempelvis)
2. Undantagsfilter (enligt beskrivningen i avsnittet hello på webb-API 1.x domänkontrollanter ovan) - inte anropas i samtliga fall.

## <a name="wcf"></a>WCF
Lägg till en klass som utökar Attribute och implementerar IErrorHandler och IServiceBehavior.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

Lägg till implementeringar av hello attributet toohello tjänsten:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[Exempel](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Prestandaräknare för undantag
Om du har [installerat hello Application Insights Agent](app-insights-monitor-performance-live-website-now.md) på servern, du kan få ett diagram över hello undantag hastighet, mätt av .NET. Detta inkluderar både hanterade och ohanterade undantag för .NET.

Öppna ett mått Explorer-bladet, lägga till ett nytt diagram och välj **undantag hastighet**, listade under prestandaräknare.

hello .NET framework beräknar hello hastighet genom inventering hello antalet undantag i ett intervall och dividera med hello längden på hello intervall.

Observera att det är detsamma som hello undantag antal beräknas genom hello Application Insights-portalen genom att räkna TrackException rapporter. Hej insamlingsintervallen är olika och hello SDK skickar inte TrackException rapporter för alla hanterade och ohanterade undantag.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a>Nästa steg
* [Övervaka REST, SQL och andra anrop toodependencies](app-insights-asp-net-dependencies.md)
* [Övervaka sidinläsningstider, webbläsarundantag och AJAX-anrop](app-insights-javascript.md)
* [Övervaka prestandaräknare](app-insights-performance-counters.md)
