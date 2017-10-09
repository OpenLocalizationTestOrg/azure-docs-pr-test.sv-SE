---
title: aaaTroubleshoot Application Insights i ett Java-webbprojekt
description: "Felsökningsguide för - övervakning av live Java-appar med Application Insights."
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: c274c01b1992971fae194c3e510512ca06ab76b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Felsökning och vanliga frågor och svar för Application Insights för Java
Frågor eller problem med [Azure Application Insights i Java][java]? Här följer några tips.

## <a name="build-errors"></a>Skapa fel
**I Eclipse när lägger till hello Application Insights SDK via Maven eller Gradle, visas build eller kontrollsumma verifieringsfel.**

* Om hello beroende <version> elementet använder ett mönster med jokertecken (t.ex. (Maven) `<version>[1.0,)</version>` eller (Gradle) `version:'1.0.+'`), pröva en specifik version i stället som `1.0.2`. Se hello [viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) hello senaste versionen.

## <a name="no-data"></a>Inga data
**Jag har lagts till Application Insights och körde min app, men aldrig sett data i hello-portalen.**

* Vänta en minut och klicka på Uppdatera. hello diagram uppdatera själva regelbundet, men du kan också uppdatera manuellt. intervall för uppdatering av hello beror på hello tidsintervall i hello-diagram.
* Kontrollera att du har en instrumentation nyckel som definierats i hello ApplicationInsights.xml-filen (i mappen hello resurser i ditt projekt)
* Kontrollera att det finns inga `<DisableTelemetry>true</DisableTelemetry>` nod i hello XML-fil.
* Du kan ha tooopen TCP-portarna 80 och 443 för utgående trafik toodc.services.visualstudio.com i brandväggen. Se hello [fullständig lista över brandväggsundantag](app-insights-ip-addresses.md)
* Starta board i hello Microsoft Azure, titta på hello tjänstkarta status. Om det finns vissa uppgifter som avisering, vänta tills de returnerade tooOK och Stäng och öppna bladet din Application Insights-programmet igen.
* Aktivera loggning toohello IDE konsolfönstret, genom att lägga till en `<SDKLogger />` hello rotnoden i hello ApplicationInsights.xml fil (i mappen hello resurser i ditt projekt) och Sök efter poster som inleds med [fel]-element.
* Kontrollera att rätt ApplicationInsights.xml filen har har lästs in av hello Java SDK genom att titta på hello konsolens Utdatameddelanden för instruktionen ”konfigurationsfilen har har hittat” Hej.
* Om hello config-fil inte hittas, kontrollera hello utgående meddelanden toosee där hello konfigurationsfilen genomsöks efter och se till att hello ApplicationInsights.xml finns på någon av dessa platser. Du kan placera hello konfigurationsfilen nära hello Application Insights SDK JAR: er som en tumregel. Exempel: i Tomcat, detta betyder att hello WEB-INF/lib mapp.

#### <a name="i-used-toosee-data-but-it-has-stopped"></a>Jag använde toosee data, men den har stoppats
* Kontrollera hello [status blogg](http://blogs.msdn.com/b/applicationinsights-status/).
* Har du uppnått den månatliga kvoten för datapunkter? Öppna inställningar/kvoten och priser toofind ut. I så fall, kan du uppgradera din plan eller betala för extra kapacitet. Se hello [priser schemat](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-hello-data-im-expecting"></a>Alla hello data jag förväntade visas inte
* Öppna hello kvoter och prissättning bladet och kontrollera om [provtagning](app-insights-sampling.md) är i drift. (100% transmission innebär att provtagning inte är i drift.) hello Application Insights-tjänsten kan vara set tooaccept en bråkdel av hello telemetri som tas emot från din app. Detta gör att inom din månatliga kvoten av telemetri. 

## <a name="no-usage-data"></a>Inga användningsdata
**Information om begäranden och svarstider, men inga vyn sida, webbläsare eller användardata visas.**

Du konfigurerat din app toosend telemetri från hello-servern. Nu är nästa steg för[ställa in din webbsidor toosend telemetri från hello webbläsare][usage].

Alternativt, om klienten är en app i en [telefonnummer eller andra][platforms], du kan skicka telemetri därifrån. 

Använd hello samma instrumentation viktiga tooset in telemetri om både klienten och servern. hello data visas i hello samma Application Insights-resurs, och du kan toocorrelate händelser från klienten och servern.


## <a name="disabling-telemetry"></a>Inaktivera telemetri
**Hur inaktiverar jag telemetri samlingen?**

I koden:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Eller** 

Uppdatera ApplicationInsights.xml (i mappen hello resurser i ditt projekt). Lägg till följande hello under hello root-noden:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

Metoden hello XML, har du toorestart hello program när du ändrar hello värde.

## <a name="changing-hello-target"></a>Ändra hello mål
**Hur kan jag ändra vilka Azure-resurs projektet skickar data till?**

* [Hämta hello instrumentation nyckel för hello ny resurs.][java]
* Om du har lagt till Application Insights tooyour projektet med hello Azure Toolkit för Eclipse webbprojektet Högerklicka, Välj **Azure**, **konfigurera Application Insights**, och ändra hello nyckel.
* Annars uppdatera hello nyckeln i ApplicationInsights.xml i hello resurser mappen i projektet.

## <a name="debug-data-from-hello-sdk"></a>Felsöka data från hello SDK

**Hur kan jag ta reda vilka hello SDK göra?**

tooget mer information om vad som händer i hello API, lägga till `<SDKLogger/>` under hello rotnoden hello ApplicationInsights.xml konfigurationsfil.

Du kan också instruera hello loggaren toooutput tooa fil:

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

hello-filer kan hittas `%temp%\javasdklogs` eller `java.io.tmpdir` vid Tomcat-server.


## <a name="hello-azure-start-screen"></a>hello Azure startskärmen
**Jag tittar på [hello Azure-portalen](https://portal.azure.com). Hello kartan berätta något om min app?**

Nej, den visar hello hälsotillståndet för Azure-servrar hello världen.

*Från hello Azure start board (startsidan), hur hittar jag information om min app?*

Om du [konfigurera din app för Application Insights][java], klicka på Bläddra, Välj Application Insights och välj hello app resurs du skapat för din app. tooget det snabbare i framtiden, du kan fästa din app toohello start-kort.

## <a name="intranet-servers"></a>Intranätservrar
**Övervakar jag en server i intranätet?**

Ja, förutsatt att servern kan skicka telemetri toohello Application Insights-portalen via hello offentliga internet. 

I brandväggen kanske tooopen TCP-portarna 80 och 443 för utgående trafik toodc.services.visualstudio.com och f5.services.visualstudio.com.

## <a name="data-retention"></a>Datakvarhållning
**Hur länge sparas data i hello portal? Är det säkra?**

Se [datalagring och sekretess][data].

## <a name="next-steps"></a>Nästa steg
**Jag konfigurera Application Insights för Min server Java-app. Vad kan jag göra?**

* [Övervaka tillgängligheten för webbsidor][availability]
* [Övervaka användningen av webbsida][usage]
* [Spåra användning och diagnostisera problem i dina enhetsappar][platforms]
* [Skriva koden tootrack användning av din app][track]
* [Samla in diagnostikloggar][javalogs]

## <a name="get-help"></a>Få hjälp
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

