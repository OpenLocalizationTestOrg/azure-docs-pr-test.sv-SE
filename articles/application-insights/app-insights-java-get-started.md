---
title: aaaJava web app analytics med Azure Application Insights | Microsoft Docs
description: "Övervakning av programprestanda för Java-webbappar med Application Insights. "
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6555ee53a44f937350e4fa296080f7dce4f45226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a><span data-ttu-id="0c319-103">Komma igång med Application Insights i ett Java-webbprojekt</span><span class="sxs-lookup"><span data-stu-id="0c319-103">Get started with Application Insights in a Java web project</span></span>


<span data-ttu-id="0c319-104">[Application Insights](https://azure.microsoft.com/services/application-insights/) är en utökningsbar analytics-tjänsten för webbutvecklare som hjälper dig att förstå hello prestanda och användning av tillämpningsprogrammet live.</span><span class="sxs-lookup"><span data-stu-id="0c319-104">[Application Insights](https://azure.microsoft.com/services/application-insights/) is an extensible analytics service for web developers that helps you understand hello performance and usage of your live application.</span></span> <span data-ttu-id="0c319-105">Använd den för[identifiera och diagnostisera prestandaproblem och undantag](app-insights-detect-triage-diagnose.md), och [skriva kod] [ api] tootrack vad användare göra med din app.</span><span class="sxs-lookup"><span data-stu-id="0c319-105">Use it too[detect and diagnose performance issues and exceptions](app-insights-detect-triage-diagnose.md), and [write code][api] tootrack what users do with your app.</span></span>

![exempeldata](./media/app-insights-java-get-started/5-results.png)

<span data-ttu-id="0c319-107">Application Insights har stöd för Java-appar som körs på Linux, Unix eller Windows.</span><span class="sxs-lookup"><span data-stu-id="0c319-107">Application Insights supports Java apps running on Linux, Unix, or Windows.</span></span>

<span data-ttu-id="0c319-108">Du behöver:</span><span class="sxs-lookup"><span data-stu-id="0c319-108">You need:</span></span>

* <span data-ttu-id="0c319-109">Oracle JRE 1.6 eller senare eller Zulu JRE 1.6 eller senare</span><span class="sxs-lookup"><span data-stu-id="0c319-109">Oracle JRE 1.6 or later, or Zulu JRE 1.6 or later</span></span>
* <span data-ttu-id="0c319-110">En prenumeration för[Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="0c319-110">A subscription too[Microsoft Azure](https://azure.microsoft.com/).</span></span>

<span data-ttu-id="0c319-111">*Om du har ett webbprogram som redan är live, du kan följa hello alternativa proceduren för[lägga till hello SDK vid körning i hello webbserver](app-insights-java-live.md). Det alternativt undviker att återskapa hello koden, men du får inte hello alternativet toowrite kod tootrack användaraktivitet.*</span><span class="sxs-lookup"><span data-stu-id="0c319-111">*If you have a web app that's already live, you could follow hello alternative procedure too[add hello SDK at runtime in hello web server](app-insights-java-live.md). That alternative avoids rebuilding hello code, but you don't get hello option toowrite code tootrack user activity.*</span></span>

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="0c319-112">1. Hämta en Application Insights-instrumenteringsnyckel</span><span class="sxs-lookup"><span data-stu-id="0c319-112">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="0c319-113">Logga in toohello [Microsoft Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0c319-113">Sign in toohello [Microsoft Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0c319-114">Skapa en Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="0c319-114">Create an Application Insights resource.</span></span> <span data-ttu-id="0c319-115">Ange hello programmet typen tooJava webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0c319-115">Set hello application type tooJava web application.</span></span>

    ![Fyll i ett namn, välj Java-webbapp och klicka på Skapa](./media/app-insights-java-get-started/02-create.png)
3. <span data-ttu-id="0c319-117">Hitta hello instrumentation nyckeln för hello ny resurs.</span><span class="sxs-lookup"><span data-stu-id="0c319-117">Find hello instrumentation key of hello new resource.</span></span> <span data-ttu-id="0c319-118">Du behöver toopaste nyckeln i projektet koden inom kort.</span><span class="sxs-lookup"><span data-stu-id="0c319-118">You'll need toopaste this key into your code project shortly.</span></span>

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-get-started/03-key.png)

## <a name="2-add-hello-application-insights-sdk-for-java-tooyour-project"></a><span data-ttu-id="0c319-120">2. Lägg till hello Application Insights SDK för Java tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="0c319-120">2. Add hello Application Insights SDK for Java tooyour project</span></span>
<span data-ttu-id="0c319-121">*Välj hello lämpligt sätt för projektet.*</span><span class="sxs-lookup"><span data-stu-id="0c319-121">*Choose hello appropriate way for your project.*</span></span>

#### <a name="if-youre-using-eclipse-toocreate-a-maven-or-dynamic-web-project-"></a><span data-ttu-id="0c319-122">Om du använder-solförmörkelse toocreate ett Maven eller dynamisk projekt...</span><span class="sxs-lookup"><span data-stu-id="0c319-122">If you're using Eclipse toocreate a Maven or Dynamic Web project ...</span></span>
<span data-ttu-id="0c319-123">Använd hello [Application Insights SDK för Java-plugin-programmet][eclipse].</span><span class="sxs-lookup"><span data-stu-id="0c319-123">Use hello [Application Insights SDK for Java plug-in][eclipse].</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="0c319-124">Om du använder Maven …</span><span class="sxs-lookup"><span data-stu-id="0c319-124">If you're using Maven...</span></span>
<span data-ttu-id="0c319-125">Om projektet har redan ställts in toouse Maven för version, kod merge hello följande tooyour pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="0c319-125">If your project is already set up toouse Maven for build, merge hello following code tooyour pom.xml file.</span></span>

<span data-ttu-id="0c319-126">Sedan uppdatera hello projektet beroenden tooget hello binärfiler hämtas.</span><span class="sxs-lookup"><span data-stu-id="0c319-126">Then, refresh hello project dependencies tooget hello binaries downloaded.</span></span>

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>
```

* <span data-ttu-id="0c319-127">*Stöter du på utvecklingsfel eller fel relaterade till verifieringen av kontrollsummor?*</span><span class="sxs-lookup"><span data-stu-id="0c319-127">*Build or checksum validation errors?*</span></span> <span data-ttu-id="0c319-128">Prova att använda en specifik version, t.ex.: `<version>1.0.n</version>`.</span><span class="sxs-lookup"><span data-stu-id="0c319-128">Try using a specific version, such as: `<version>1.0.n</version>`.</span></span> <span data-ttu-id="0c319-129">Du hittar hello senaste versionen i hello [SDK viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) eller i vår [Maven artefakter](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).</span><span class="sxs-lookup"><span data-stu-id="0c319-129">You'll find hello latest version in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) or in our [Maven artifacts](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).</span></span>
* <span data-ttu-id="0c319-130">*Behöver tooupdate tooa nya SDK?*</span><span class="sxs-lookup"><span data-stu-id="0c319-130">*Need tooupdate tooa new SDK?*</span></span> <span data-ttu-id="0c319-131">Uppdatera ditt projekts beroenden.</span><span class="sxs-lookup"><span data-stu-id="0c319-131">Refresh your project's dependencies.</span></span>

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="0c319-132">Om du använder Gradle …</span><span class="sxs-lookup"><span data-stu-id="0c319-132">If you're using Gradle...</span></span>
<span data-ttu-id="0c319-133">Om projektet har redan ställts in toouse Gradle för version, kod merge hello följande tooyour build.gradle fil.</span><span class="sxs-lookup"><span data-stu-id="0c319-133">If your project is already set up toouse Gradle for build, merge hello following code tooyour build.gradle file.</span></span>

<span data-ttu-id="0c319-134">Sedan uppdatera hello projektet beroenden tooget hello binärfiler laddas ned.</span><span class="sxs-lookup"><span data-stu-id="0c319-134">Then refresh hello project dependencies tooget hello binaries downloaded.</span></span>

```JSON

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }
```

* <span data-ttu-id="0c319-135">*Stöter du på utvecklingsfel eller fel relaterade till verifieringen av kontrollsummor? Prova att använda en specifik version, t.ex.:* `version:'1.0.n'`.</span><span class="sxs-lookup"><span data-stu-id="0c319-135">*Build or checksum validation errors? Try using a specific version, such as:* `version:'1.0.n'`.</span></span> <span data-ttu-id="0c319-136">*Du hittar hello senaste versionen i hello [SDK viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*</span><span class="sxs-lookup"><span data-stu-id="0c319-136">*You'll find hello latest version in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*</span></span>
* <span data-ttu-id="0c319-137">*tooupdate tooa nya SDK*</span><span class="sxs-lookup"><span data-stu-id="0c319-137">*tooupdate tooa new SDK*</span></span>
  * <span data-ttu-id="0c319-138">Uppdatera ditt projekts beroenden.</span><span class="sxs-lookup"><span data-stu-id="0c319-138">Refresh your project's dependencies.</span></span>

#### <a name="otherwise-"></a><span data-ttu-id="0c319-139">Eller …</span><span class="sxs-lookup"><span data-stu-id="0c319-139">Otherwise ...</span></span>
<span data-ttu-id="0c319-140">Lägg manuellt till hello SDK:</span><span class="sxs-lookup"><span data-stu-id="0c319-140">Manually add hello SDK:</span></span>

1. <span data-ttu-id="0c319-141">Hämta hello [Application Insights SDK för Java](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="0c319-141">Download hello [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span>
2. <span data-ttu-id="0c319-142">Extrahera hello binärfiler från hello zip-filen och Lägg till dem tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="0c319-142">Extract hello binaries from hello zip file and add them tooyour project.</span></span>

### <a name="questions"></a><span data-ttu-id="0c319-143">Frågor …</span><span class="sxs-lookup"><span data-stu-id="0c319-143">Questions...</span></span>
* <span data-ttu-id="0c319-144">*Vad är hello förhållandet mellan hello `-core` och `-web` komponenter i hello zip?*</span><span class="sxs-lookup"><span data-stu-id="0c319-144">*What's hello relationship between hello `-core` and `-web` components in hello zip?*</span></span>

  * <span data-ttu-id="0c319-145">`applicationinsights-core`ger du hello utan API.</span><span class="sxs-lookup"><span data-stu-id="0c319-145">`applicationinsights-core` gives you hello bare API.</span></span> <span data-ttu-id="0c319-146">Du behöver alltid ha den här komponenten.</span><span class="sxs-lookup"><span data-stu-id="0c319-146">You always need this component.</span></span>
  * <span data-ttu-id="0c319-147">`applicationinsights-web` ger dig mått som spårar antalet HTTP-förfrågningar och svarstider.</span><span class="sxs-lookup"><span data-stu-id="0c319-147">`applicationinsights-web` gives you metrics that track HTTP request counts and response times.</span></span> <span data-ttu-id="0c319-148">Du kan utelämna den här komponenten om du inte vill att den här telemetrin ska samlas in automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0c319-148">You can omit this component if you don't want this telemetry automatically collected.</span></span> <span data-ttu-id="0c319-149">Till exempel om du vill toowrite egna.</span><span class="sxs-lookup"><span data-stu-id="0c319-149">For example, if you want toowrite your own.</span></span>
* <span data-ttu-id="0c319-150">*tooupdate hello SDK när vi publicerar ändringar*</span><span class="sxs-lookup"><span data-stu-id="0c319-150">*tooupdate hello SDK when we publish changes*</span></span>

  * <span data-ttu-id="0c319-151">Hämta senaste hello [Application Insights SDK för Java](https://aka.ms/qqkaq6) och Ersätt hello gamla.</span><span class="sxs-lookup"><span data-stu-id="0c319-151">Download hello latest [Application Insights SDK for Java](https://aka.ms/qqkaq6) and replace hello old ones.</span></span>
  * <span data-ttu-id="0c319-152">Beskrivs i hello [SDK viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).</span><span class="sxs-lookup"><span data-stu-id="0c319-152">Changes are described in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="0c319-153">3. Lägga till en XML-fil för Application Insights</span><span class="sxs-lookup"><span data-stu-id="0c319-153">3. Add an Application Insights .xml file</span></span>
<span data-ttu-id="0c319-154">Lägg till mapp som ApplicationInsights.xml toohello resurser i projektet eller kontrollera läggs tooyour projekt klassökvägen för distribution.</span><span class="sxs-lookup"><span data-stu-id="0c319-154">Add ApplicationInsights.xml toohello resources folder in your project, or make sure it is added tooyour project’s deployment class path.</span></span> <span data-ttu-id="0c319-155">Kopiera hello följande XML till den.</span><span class="sxs-lookup"><span data-stu-id="0c319-155">Copy hello following XML into it.</span></span>

<span data-ttu-id="0c319-156">Ersätt hello instrumentation nyckel som du har fått från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c319-156">Substitute hello instrumentation key that you got from hello Azure portal.</span></span>

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- hello key from hello portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data tooeach event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```


* <span data-ttu-id="0c319-157">hello instrumentation nyckel skickas tillsammans med alla element på telemetri och talar om Application Insights toodisplay den i din resurs.</span><span class="sxs-lookup"><span data-stu-id="0c319-157">hello instrumentation key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>
* <span data-ttu-id="0c319-158">hello HTTP-begäran-komponenten är valfritt.</span><span class="sxs-lookup"><span data-stu-id="0c319-158">hello HTTP Request component is optional.</span></span> <span data-ttu-id="0c319-159">Telemetri om begäran och svar gånger toohello portal skickas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0c319-159">It automatically sends telemetry about requests and response times toohello portal.</span></span>
* <span data-ttu-id="0c319-160">Händelser korrelationen är en komponent som tillägg toohello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="0c319-160">Events correlation is an addition toohello HTTP request component.</span></span> <span data-ttu-id="0c319-161">Den tilldelas en identifierare tooeach begäran tas emot av hello-servern och lägger till den här identifieraren som ett objekt med egenskapen tooevery av telemetri som hello egenskapen 'Operation.Id'.</span><span class="sxs-lookup"><span data-stu-id="0c319-161">It assigns an identifier tooeach request received by hello server, and adds this identifier as a property tooevery item of telemetry as hello property 'Operation.Id'.</span></span> <span data-ttu-id="0c319-162">Det gör att du toocorrelate hello telemetri som är associerade med varje begäran genom att ange ett filter i [diagnostiska Sök][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="0c319-162">It allows you toocorrelate hello telemetry associated with each request by setting a filter in [diagnostic search][diagnostic].</span></span>
* <span data-ttu-id="0c319-163">hello Application Insights nyckel kan skickas dynamiskt från hello Azure-portalen som en systemegenskap (-DAPPLICATION_INSIGHTS_IKEY = your_ikey).</span><span class="sxs-lookup"><span data-stu-id="0c319-163">hello Application Insights key can be passed dynamically from hello Azure portal as a system property (-DAPPLICATION_INSIGHTS_IKEY=your_ikey).</span></span> <span data-ttu-id="0c319-164">Om det finns inte någon definierad egenskap sker sökning efter miljövariabeln (APPLICATION_INSIGHTS_IKEY) i Azure App-inställningarna.</span><span class="sxs-lookup"><span data-stu-id="0c319-164">If there is no property defined, it checks for environment variable (APPLICATION_INSIGHTS_IKEY) in Azure App Settings.</span></span> <span data-ttu-id="0c319-165">Om båda hello är odefinierad, används hello standard InstrumentationKey från ApplicationInsights.xml.</span><span class="sxs-lookup"><span data-stu-id="0c319-165">If both hello properties are undefined, hello default InstrumentationKey is used from ApplicationInsights.xml.</span></span> <span data-ttu-id="0c319-166">Den här sekvensen hjälper dig att toomanage olika InstrumentationKeys för olika miljöer dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="0c319-166">This sequence helps you toomanage different InstrumentationKeys for different environments dynamically.</span></span>

### <a name="alternative-ways-tooset-hello-instrumentation-key"></a><span data-ttu-id="0c319-167">Alternativa sätt tooset hello instrumentation nyckel</span><span class="sxs-lookup"><span data-stu-id="0c319-167">Alternative ways tooset hello instrumentation key</span></span>
<span data-ttu-id="0c319-168">Application Insights SDK söker efter hello nyckel i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="0c319-168">Application Insights SDK looks for hello key in this order:</span></span>

1. <span data-ttu-id="0c319-169">Systemegenskap: -DAPPLICATION_INSIGHTS_IKEY=your_ikey</span><span class="sxs-lookup"><span data-stu-id="0c319-169">System property: -DAPPLICATION_INSIGHTS_IKEY=your_ikey</span></span>
2. <span data-ttu-id="0c319-170">Miljövariabel: APPLICATION_INSIGHTS_IKEY</span><span class="sxs-lookup"><span data-stu-id="0c319-170">Environment variable: APPLICATION_INSIGHTS_IKEY</span></span>
3. <span data-ttu-id="0c319-171">Konfigurationsfil: ApplicationInsights.xml</span><span class="sxs-lookup"><span data-stu-id="0c319-171">Configuration file: ApplicationInsights.xml</span></span>

<span data-ttu-id="0c319-172">Du kan också [ange den i koden](app-insights-api-custom-events-metrics.md#ikey):</span><span class="sxs-lookup"><span data-stu-id="0c319-172">You can also [set it in code](app-insights-api-custom-events-metrics.md#ikey):</span></span>

```Java

    telemetryClient.InstrumentationKey = "...";
```

## <a name="4-add-an-http-filter"></a><span data-ttu-id="0c319-173">4. Lägga till ett HTTP-filter</span><span class="sxs-lookup"><span data-stu-id="0c319-173">4. Add an HTTP filter</span></span>
<span data-ttu-id="0c319-174">hello sista konfigurationssteget kan hello HTTP-begäran komponenten toolog varje webbegäran.</span><span class="sxs-lookup"><span data-stu-id="0c319-174">hello last configuration step allows hello HTTP request component toolog each web request.</span></span> <span data-ttu-id="0c319-175">(Krävs inte om du bara behöver hello utan API.)</span><span class="sxs-lookup"><span data-stu-id="0c319-175">(Not required if you just want hello bare API.)</span></span>

<span data-ttu-id="0c319-176">Leta upp och öppna hello web.xml filen i projektet och merge hello följande kod under hello webbprogrammet nod där programmet filter har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="0c319-176">Locate and open hello web.xml file in your project, and merge hello following code under hello web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="0c319-177">tooget hello mest korrekta resultat, hello filter ska mappas innan alla filter.</span><span class="sxs-lookup"><span data-stu-id="0c319-177">tooget hello most accurate results, hello filter should be mapped before all other filters.</span></span>

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a><span data-ttu-id="0c319-178">Om du använder Spring Web MVC 3.1 or later</span><span class="sxs-lookup"><span data-stu-id="0c319-178">If you're using Spring Web MVC 3.1 or later</span></span>
<span data-ttu-id="0c319-179">Redigera de här elementen i *-servlet.xml tooinclude hello Application Insights paketet:</span><span class="sxs-lookup"><span data-stu-id="0c319-179">Edit these elements in *-servlet.xml tooinclude hello Application Insights package:</span></span>

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a><span data-ttu-id="0c319-180">Om du använder Struts 2</span><span class="sxs-lookup"><span data-stu-id="0c319-180">If you're using Struts 2</span></span>
<span data-ttu-id="0c319-181">Lägg till den här artikeln toohello andra ojämnheter konfigurationsfilen (vanligtvis namngivna struts.xml eller andra ojämnheter default.xml):</span><span class="sxs-lookup"><span data-stu-id="0c319-181">Add this item toohello Struts configuration file (usually named struts.xml or struts-default.xml):</span></span>

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

<span data-ttu-id="0c319-182">(Om du har interceptorerna som definierats i en standard-stack hello spärren kan bara läggas toothat stack)</span><span class="sxs-lookup"><span data-stu-id="0c319-182">(If you have interceptors defined in a default stack, hello interceptor can simply be added toothat stack.)</span></span>

## <a name="5-run-your-application"></a><span data-ttu-id="0c319-183">5. Köra ditt program</span><span class="sxs-lookup"><span data-stu-id="0c319-183">5. Run your application</span></span>
<span data-ttu-id="0c319-184">Kör det i felsökningsläge på utvecklingsdatorn eller publicera tooyour server.</span><span class="sxs-lookup"><span data-stu-id="0c319-184">Either run it in debug mode on your development machine, or publish tooyour server.</span></span>

## <a name="6-view-your-telemetry-in-application-insights"></a><span data-ttu-id="0c319-185">6. Visa telemetrin i Application Insights</span><span class="sxs-lookup"><span data-stu-id="0c319-185">6. View your telemetry in Application Insights</span></span>
<span data-ttu-id="0c319-186">Returnera tooyour Application Insights-resurs i [Microsoft Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0c319-186">Return tooyour Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="0c319-187">HTTP-begäranden data visas på hello översikt bladet.</span><span class="sxs-lookup"><span data-stu-id="0c319-187">HTTP requests data appears on hello overview blade.</span></span> <span data-ttu-id="0c319-188">(Om informationen inte visas väntar du några sekunder och klickar på Uppdatera.)</span><span class="sxs-lookup"><span data-stu-id="0c319-188">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![exempeldata](./media/app-insights-java-get-started/5-results.png)

<span data-ttu-id="0c319-190">[Lär dig mer om mätvärden.][metrics]</span><span class="sxs-lookup"><span data-stu-id="0c319-190">[Learn more about metrics.][metrics]</span></span>

<span data-ttu-id="0c319-191">Klicka dig igenom några diagram toosee mer detaljerad samman mått.</span><span class="sxs-lookup"><span data-stu-id="0c319-191">Click through any chart toosee more detailed aggregated metrics.</span></span>

![](./media/app-insights-java-get-started/6-barchart.png)

> <span data-ttu-id="0c319-192">Application Insights förutsätter hello format för HTTP-begäranden för MVC-program: `VERB controller/action`.</span><span class="sxs-lookup"><span data-stu-id="0c319-192">Application Insights assumes hello format of HTTP requests for MVC applications is: `VERB controller/action`.</span></span> <span data-ttu-id="0c319-193">`GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` och `GET Home/Product/sdf96vws` grupperas t.ex. i `GET Home/Product`.</span><span class="sxs-lookup"><span data-stu-id="0c319-193">For example, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` and `GET Home/Product/sdf96vws` are grouped into `GET Home/Product`.</span></span> <span data-ttu-id="0c319-194">Denna gruppering gör det möjligt att skapa meningsfulla förfrågningsaggregeringar, t.ex. antal förfrågningar och genomsnittlig körningstid för förfrågningarna.</span><span class="sxs-lookup"><span data-stu-id="0c319-194">This grouping enables meaningful aggregations of requests, such as number of requests and average execution time for requests.</span></span>
>
>

### <a name="instance-data"></a><span data-ttu-id="0c319-195">Instansdata</span><span class="sxs-lookup"><span data-stu-id="0c319-195">Instance data</span></span>
<span data-ttu-id="0c319-196">Klicka dig igenom en specifik begäran Skriv toosee enskilda instanser.</span><span class="sxs-lookup"><span data-stu-id="0c319-196">Click through a specific request type toosee individual instances.</span></span>

<span data-ttu-id="0c319-197">Två typer av data visas i Application Insights: aggregerade data, som lagras och visas som medelvärden, antal och belopp; och instansdata, som är enskilda rapporter över HTTP-begäranden, undantag, sidvisningar eller anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="0c319-197">Two kinds of data are displayed in Application Insights: aggregated data, stored and displayed as averages, counts, and sums; and instance data - individual reports of HTTP requests, exceptions, page views, or custom events.</span></span>

<span data-ttu-id="0c319-198">När du visar hello egenskaperna för en begäran kan se du hello telemetriska händelser som är kopplade till den, till exempel begäranden och undantag.</span><span class="sxs-lookup"><span data-stu-id="0c319-198">When viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a><span data-ttu-id="0c319-199">Analytics: Kraftfullt frågespråk</span><span class="sxs-lookup"><span data-stu-id="0c319-199">Analytics: Powerful query language</span></span>
<span data-ttu-id="0c319-200">När du samlar in mer data, kan du köra frågor båda tooaggregate data och toofind enskilda instanser.</span><span class="sxs-lookup"><span data-stu-id="0c319-200">As you accumulate more data, you can run queries both tooaggregate data and toofind individual instances.</span></span>  <span data-ttu-id="0c319-201">[Analytics](app-insights-analytics.md) är ett kraftfullt verktyg både för att bättre förstå prestanda och användning, och för diagnostikändamål.</span><span class="sxs-lookup"><span data-stu-id="0c319-201">[Analytics](app-insights-analytics.md) is a powerful tool for both for understanding performance and usage, and for diagnostic purposes.</span></span>

![Exempel med Analytics](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-hello-server"></a><span data-ttu-id="0c319-203">7. Installera appen på hello-server</span><span class="sxs-lookup"><span data-stu-id="0c319-203">7. Install your app on hello server</span></span>
<span data-ttu-id="0c319-204">Nu publicera din app toohello server, låta personer använder den, och titta på hello telemetri som visas på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c319-204">Now publish your app toohello server, let people use it, and watch hello telemetry show up on hello portal.</span></span>

* <span data-ttu-id="0c319-205">Kontrollera att brandväggen tillåter programmet-toosend telemetri toothese portar:</span><span class="sxs-lookup"><span data-stu-id="0c319-205">Make sure your firewall allows your application toosend telemetry toothese ports:</span></span>

  * <span data-ttu-id="0c319-206">dc.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="0c319-206">dc.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="0c319-207">f5.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="0c319-207">f5.services.visualstudio.com:443</span></span>

* <span data-ttu-id="0c319-208">Om utgående trafik måste dirigeras via en brandvägg definierar du systemegenskaper `http.proxyHost` och `http.proxyPort`.</span><span class="sxs-lookup"><span data-stu-id="0c319-208">If outgoing traffic must be routed through a firewall, define system properties `http.proxyHost` and `http.proxyPort`.</span></span>

* <span data-ttu-id="0c319-209">Installera följande på Windows-servrar:</span><span class="sxs-lookup"><span data-stu-id="0c319-209">On Windows servers, install:</span></span>

  * [<span data-ttu-id="0c319-210">Microsoft Visual C++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="0c319-210">Microsoft Visual C++ Redistributable</span></span>](http://www.microsoft.com/download/details.aspx?id=40784)

    <span data-ttu-id="0c319-211">(Den här komponenten gör det möjligt att använda prestandaräknare.)</span><span class="sxs-lookup"><span data-stu-id="0c319-211">(This component enables performance counters.)</span></span>


## <a name="exceptions-and-request-failures"></a><span data-ttu-id="0c319-212">Fel relaterade till begäranden och undantag</span><span class="sxs-lookup"><span data-stu-id="0c319-212">Exceptions and request failures</span></span>
<span data-ttu-id="0c319-213">Ohanterade undantag samlas in automatiskt:</span><span class="sxs-lookup"><span data-stu-id="0c319-213">Unhandled exceptions are automatically collected:</span></span>

![Öppna Inställningar, Fel](./media/app-insights-java-get-started/21-exceptions.png)

<span data-ttu-id="0c319-215">toocollect data på andra undantag, har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="0c319-215">toocollect data on other exceptions, you have two options:</span></span>

* <span data-ttu-id="0c319-216">[Infoga anropar tootrackException() i koden][apiexceptions].</span><span class="sxs-lookup"><span data-stu-id="0c319-216">[Insert calls tootrackException() in your code][apiexceptions].</span></span>
* <span data-ttu-id="0c319-217">[Installera hello Java-agenten på servern](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="0c319-217">[Install hello Java Agent on your server](app-insights-java-agent.md).</span></span> <span data-ttu-id="0c319-218">Du kan ange hello-metoder som du vill toowatch.</span><span class="sxs-lookup"><span data-stu-id="0c319-218">You specify hello methods you want toowatch.</span></span>

## <a name="monitor-method-calls-and-external-dependencies"></a><span data-ttu-id="0c319-219">Övervaka metodanrop och externa beroenden</span><span class="sxs-lookup"><span data-stu-id="0c319-219">Monitor method calls and external dependencies</span></span>
<span data-ttu-id="0c319-220">[Installera hello agenten Java](app-insights-java-agent.md) toolog angivna interna metoder och anrop som görs via JDBC, med tidsinställning data.</span><span class="sxs-lookup"><span data-stu-id="0c319-220">[Install hello Java Agent](app-insights-java-agent.md) toolog specified internal methods and calls made through JDBC, with timing data.</span></span>

## <a name="performance-counters"></a><span data-ttu-id="0c319-221">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="0c319-221">Performance counters</span></span>
<span data-ttu-id="0c319-222">Öppna **inställningar**, **servrar**, toosee ett antal prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="0c319-222">Open **Settings**, **Servers**, toosee a range of performance counters.</span></span>

![](./media/app-insights-java-get-started/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a><span data-ttu-id="0c319-223">Anpassa samlingen med prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="0c319-223">Customize performance counter collection</span></span>
<span data-ttu-id="0c319-224">toodisable samling hello standarduppsättning prestandaräknare, lägger du till följande kod under hello rotnoden hello ApplicationInsights.xml filen hello:</span><span class="sxs-lookup"><span data-stu-id="0c319-224">toodisable collection of hello standard set of performance counters, add hello following code under hello root node of hello ApplicationInsights.xml file:</span></span>

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a><span data-ttu-id="0c319-225">Samla in data från fler prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="0c319-225">Collect additional performance counters</span></span>
<span data-ttu-id="0c319-226">Du kan ange ytterligare prestandaräknare toobe samlas in.</span><span class="sxs-lookup"><span data-stu-id="0c319-226">You can specify additional performance counters toobe collected.</span></span>

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a><span data-ttu-id="0c319-227">Räknare för JMX (som exponeras av hello Java Virtual Machine)</span><span class="sxs-lookup"><span data-stu-id="0c319-227">JMX counters (exposed by hello Java Virtual Machine)</span></span>

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* <span data-ttu-id="0c319-228">`displayName`– hello-namnet som visas i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c319-228">`displayName` – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="0c319-229">`objectName`– hello JMX-objektnamn.</span><span class="sxs-lookup"><span data-stu-id="0c319-229">`objectName` – hello JMX object name.</span></span>
* <span data-ttu-id="0c319-230">`attribute`– hello-attributet för hello JMX objektet namn toofetch</span><span class="sxs-lookup"><span data-stu-id="0c319-230">`attribute` – hello attribute of hello JMX object name toofetch</span></span>
* <span data-ttu-id="0c319-231">`type`(valfritt) – hello JMX objektets attributtyp:</span><span class="sxs-lookup"><span data-stu-id="0c319-231">`type` (optional) - hello type of JMX object’s attribute:</span></span>
  * <span data-ttu-id="0c319-232">Standardvärde: en enkel typ, till exempel int eller long.</span><span class="sxs-lookup"><span data-stu-id="0c319-232">Default: a simple type such as int or long.</span></span>
  * <span data-ttu-id="0c319-233">`composite`: hello prestandaräknardata har formatet hello av 'Attribute.Data'</span><span class="sxs-lookup"><span data-stu-id="0c319-233">`composite`: hello perf counter data is in hello format of 'Attribute.Data'</span></span>
  * <span data-ttu-id="0c319-234">`tabular`: hello prestandaräknardata har hello format för en tabellrad</span><span class="sxs-lookup"><span data-stu-id="0c319-234">`tabular`: hello perf counter data is in hello format of a table row</span></span>

#### <a name="windows-performance-counters"></a><span data-ttu-id="0c319-235">Windows-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="0c319-235">Windows performance counters</span></span>
<span data-ttu-id="0c319-236">Varje [Windows-prestandaräknare](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) är medlem i en kategori (i hello samma sätt som att ett fält är medlem av en klass).</span><span class="sxs-lookup"><span data-stu-id="0c319-236">Each [Windows performance counter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is a member of a category (in hello same way that a field is a member of a class).</span></span> <span data-ttu-id="0c319-237">Kategorier kan antingen vara globala eller ha numrerade eller namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="0c319-237">Categories can either be global, or can have numbered or named instances.</span></span>

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* <span data-ttu-id="0c319-238">displayName – hello namnet visas i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c319-238">displayName – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="0c319-239">Kategorinamn – hello prestandaräknarkategorin (prestandaobjekt) som är kopplad till den här prestandaräknaren.</span><span class="sxs-lookup"><span data-stu-id="0c319-239">categoryName – hello performance counter category (performance object) with which this performance counter is associated.</span></span>
* <span data-ttu-id="0c319-240">counterName – hello namn för hello prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="0c319-240">counterName – hello name of hello performance counter.</span></span>
* <span data-ttu-id="0c319-241">Instansnamn – hello kategori prestandaräknarinstans hello namn eller en tom sträng (””), om hello kategori innehåller en enda instans.</span><span class="sxs-lookup"><span data-stu-id="0c319-241">instanceName – hello name of hello performance counter category instance, or an empty string (""), if hello category contains a single instance.</span></span> <span data-ttu-id="0c319-242">Om hello categoryName är processen och hello prestandaräknaren som toocollect är från hello aktuella JVM-processen på som din app körs, ange `"__SELF__"`.</span><span class="sxs-lookup"><span data-stu-id="0c319-242">If hello categoryName is Process, and hello performance counter you'd like toocollect is from hello current JVM process on which your app is running, specify `"__SELF__"`.</span></span>

<span data-ttu-id="0c319-243">Dina prestandaräknare visas som anpassade mått i [Metrics Explorer][metrics].</span><span class="sxs-lookup"><span data-stu-id="0c319-243">Your performance counters are visible as custom metrics in [Metrics Explorer][metrics].</span></span>

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a><span data-ttu-id="0c319-244">Unix-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="0c319-244">Unix performance counters</span></span>
* <span data-ttu-id="0c319-245">[Installera collectd med Application Insights-plugin-programmet hello](app-insights-java-collectd.md) tooget en mängd olika system- och data.</span><span class="sxs-lookup"><span data-stu-id="0c319-245">[Install collectd with hello Application Insights plugin](app-insights-java-collectd.md) tooget a wide variety of system and network data.</span></span>

## <a name="get-user-and-session-data"></a><span data-ttu-id="0c319-246">Samla in användar- och sesionsdata</span><span class="sxs-lookup"><span data-stu-id="0c319-246">Get user and session data</span></span>
<span data-ttu-id="0c319-247">Du skickar telemetri från webbservern.</span><span class="sxs-lookup"><span data-stu-id="0c319-247">OK, you're sending telemetry from your web server.</span></span> <span data-ttu-id="0c319-248">Nu hello tooget 360 graders vy av programmet, du kan lägga till fler övervakning:</span><span class="sxs-lookup"><span data-stu-id="0c319-248">Now tooget hello full 360-degree view of your application, you can add more monitoring:</span></span>

* <span data-ttu-id="0c319-249">[Lägga till webbsidor för telemetri tooyour] [ usage] toomonitor sidan vyer och användaren mått.</span><span class="sxs-lookup"><span data-stu-id="0c319-249">[Add telemetry tooyour web pages][usage] toomonitor page views and user metrics.</span></span>
* <span data-ttu-id="0c319-250">[Ställ in webbtester] [ availability] toomake säker tillämpningsprogrammet är live och svarstid.</span><span class="sxs-lookup"><span data-stu-id="0c319-250">[Set up web tests][availability] toomake sure your application stays live and responsive.</span></span>

## <a name="capture-log-traces"></a><span data-ttu-id="0c319-251">Samla in loggspårningar</span><span class="sxs-lookup"><span data-stu-id="0c319-251">Capture log traces</span></span>
<span data-ttu-id="0c319-252">Du kan använda Application Insights tooslice och undersök loggar från Log4J eller Logback andra ramverk för loggning.</span><span class="sxs-lookup"><span data-stu-id="0c319-252">You can use Application Insights tooslice and dice logs from Log4J, Logback, or other logging frameworks.</span></span> <span data-ttu-id="0c319-253">Du kan jämföra hello loggar med HTTP-begäranden och andra telemetri.</span><span class="sxs-lookup"><span data-stu-id="0c319-253">You can correlate hello logs with HTTP requests and other telemetry.</span></span> <span data-ttu-id="0c319-254">[Lär dig mer][javalogs].</span><span class="sxs-lookup"><span data-stu-id="0c319-254">[Learn how][javalogs].</span></span>

## <a name="send-your-own-telemetry"></a><span data-ttu-id="0c319-255">Skicka din egen telemetri</span><span class="sxs-lookup"><span data-stu-id="0c319-255">Send your own telemetry</span></span>
<span data-ttu-id="0c319-256">Nu när du har installerat hello SDK kan använda du hello API toosend egna telemetri.</span><span class="sxs-lookup"><span data-stu-id="0c319-256">Now that you've installed hello SDK, you can use hello API toosend your own telemetry.</span></span>

* <span data-ttu-id="0c319-257">[Spåra anpassade händelser och mått] [ api] toolearn vad användarna gör med ditt program.</span><span class="sxs-lookup"><span data-stu-id="0c319-257">[Track custom events and metrics][api] toolearn what users are doing with your application.</span></span>
* <span data-ttu-id="0c319-258">[Söka efter händelser och loggar] [ diagnostic] toohelp diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="0c319-258">[Search events and logs][diagnostic] toohelp diagnose problems.</span></span>

## <a name="availability-web-tests"></a><span data-ttu-id="0c319-259">Webbtester för tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="0c319-259">Availability web tests</span></span>
<span data-ttu-id="0c319-260">Application Insights kan testa din webbplats på regelbundet toocheck som är det upp och svarar också.</span><span class="sxs-lookup"><span data-stu-id="0c319-260">Application Insights can test your website at regular intervals toocheck that it's up and responding well.</span></span> <span data-ttu-id="0c319-261">[tooset in][availability], klicka på webbtester.</span><span class="sxs-lookup"><span data-stu-id="0c319-261">[tooset up][availability], click Web tests.</span></span>

![Klicka först på Webbtester och sedan på Lägg till webbtest](./media/app-insights-java-get-started/31-config-web-test.png)

<span data-ttu-id="0c319-263">Du kan visa diagram över svarstider, samt få e-postaviseringar om platsen kraschar.</span><span class="sxs-lookup"><span data-stu-id="0c319-263">You'll get charts of response times, plus email notifications if your site goes down.</span></span>

![Exempel på webbtest](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

<span data-ttu-id="0c319-265">[Läs mer om webbtester för tillgänglighet.][availability]</span><span class="sxs-lookup"><span data-stu-id="0c319-265">[Learn more about availability web tests.][availability]</span></span>

## <a name="questions-problems"></a><span data-ttu-id="0c319-266">Frågor?</span><span class="sxs-lookup"><span data-stu-id="0c319-266">Questions?</span></span> <span data-ttu-id="0c319-267">Har du problem?</span><span class="sxs-lookup"><span data-stu-id="0c319-267">Problems?</span></span>
[<span data-ttu-id="0c319-268">Felsöka Java</span><span class="sxs-lookup"><span data-stu-id="0c319-268">Troubleshooting Java</span></span>](app-insights-java-troubleshoot.md)

## <a name="video"></a><span data-ttu-id="0c319-269">Video</span><span class="sxs-lookup"><span data-stu-id="0c319-269">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="0c319-270">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c319-270">Next steps</span></span>
* [<span data-ttu-id="0c319-271">Övervaka beroende anrop</span><span class="sxs-lookup"><span data-stu-id="0c319-271">Monitor dependency calls</span></span>](app-insights-java-agent.md)
* [<span data-ttu-id="0c319-272">Övervaka Unix-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="0c319-272">Monitor Unix performance counters</span></span>](app-insights-java-collectd.md)
* <span data-ttu-id="0c319-273">Lägg till [övervakning tooyour webbsidor](app-insights-javascript.md) toomonitor sidinläsning gånger AJAX-anrop, webbläsarundantag.</span><span class="sxs-lookup"><span data-stu-id="0c319-273">Add [monitoring tooyour web pages](app-insights-javascript.md) toomonitor page load times, AJAX calls, browser exceptions.</span></span>
* <span data-ttu-id="0c319-274">Skriva [telemetri om anpassade](app-insights-api-custom-events-metrics.md) tootrack användning i hello webbläsare eller hello-servern.</span><span class="sxs-lookup"><span data-stu-id="0c319-274">Write [custom telemetry](app-insights-api-custom-events-metrics.md) tootrack usage in hello browser or at hello server.</span></span>
* <span data-ttu-id="0c319-275">Skapa [instrumentpaneler](app-insights-dashboards.md) toobring tillsammans hello viktiga diagram för övervakning av systemet.</span><span class="sxs-lookup"><span data-stu-id="0c319-275">Create [dashboards](app-insights-dashboards.md) toobring together hello key charts for monitoring your system.</span></span>
* <span data-ttu-id="0c319-276">Använd [Analytics](app-insights-analytics.md) för kraftfulla frågor via telemetri från din app</span><span class="sxs-lookup"><span data-stu-id="0c319-276">Use  [Analytics](app-insights-analytics.md) for powerful queries over telemetry from your app</span></span>
* <span data-ttu-id="0c319-277">Mer information finns i [Azure för Java-utvecklare](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="0c319-277">For more information, visit [Azure for Java developers](/java/azure).</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
