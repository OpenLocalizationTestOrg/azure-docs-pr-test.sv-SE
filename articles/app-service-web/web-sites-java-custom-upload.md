---
title: aaaUpload en anpassad Java web app tooAzure
description: "Den här kursen visar hur tooupload en anpassad Java web app tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: eb2ccd83-e5c6-444e-a0fc-08fd5cc8326c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 0cb4a682bb25d86ff08bfd03628c89795c58451e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-java-web-app-tooazure"></a><span data-ttu-id="a4c65-103">Ladda upp en anpassad Java web app tooAzure</span><span class="sxs-lookup"><span data-stu-id="a4c65-103">Upload a custom Java web app tooAzure</span></span>
<span data-ttu-id="a4c65-104">Det här avsnittet beskrivs hur tooupload en anpassad Java webbapp för[Azure App Service] Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a4c65-104">This topic explains how tooupload a custom Java web app too[Azure App Service] Web Apps.</span></span> <span data-ttu-id="a4c65-105">Innehåller information som gäller tooany Java-webbplats eller webbprogram och även några exempel på specifika program.</span><span class="sxs-lookup"><span data-stu-id="a4c65-105">Included is information that applies tooany Java website or web app, and also some examples for specific applications.</span></span>

<span data-ttu-id="a4c65-106">Observera att Azure tillhandahåller ett sätt för att skapa Java-webbappar med hjälp av Användargränssnittet för konfigurationen av hello Azure Portal och hello Azure Marketplace, enligt beskrivningen i [skapar en Java-webbapp i Azure App Service](web-sites-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a4c65-106">Note that Azure provides a means for creating Java web apps using hello Azure Portal's configuration UI, and hello Azure Marketplace, as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md).</span></span> <span data-ttu-id="a4c65-107">Den här kursen är för scenarier där du inte vill toouse hello Azure Portal Konfigurationsgränssnittet eller hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a4c65-107">This tutorial is for scenarios in which you do not want toouse hello Azure Portal configuration UI or hello Azure Marketplace.</span></span>  

## <a name="configuration-guidelines"></a><span data-ttu-id="a4c65-108">Riktlinjer för konfiguration</span><span class="sxs-lookup"><span data-stu-id="a4c65-108">Configuration guidelines</span></span>
<span data-ttu-id="a4c65-109">hello nedan beskrivs hello inställningar för anpassad Java-webbappar på Azure.</span><span class="sxs-lookup"><span data-stu-id="a4c65-109">hello following describes hello settings expected for custom Java web apps on Azure.</span></span>

* <span data-ttu-id="a4c65-110">hello HTTP-porten används av hello Java-processen tilldelas dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="a4c65-110">hello HTTP port used by hello Java process is dynamically assigned.</span></span>  <span data-ttu-id="a4c65-111">hello processen måste använda hello port från hello miljövariabeln `HTTP_PLATFORM_PORT`.</span><span class="sxs-lookup"><span data-stu-id="a4c65-111">hello process must use hello port from hello environment variable `HTTP_PLATFORM_PORT`.</span></span>
* <span data-ttu-id="a4c65-112">Alla lyssna portar än hello enskild HTTP-lyssnaren ska inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="a4c65-112">All listen ports other than hello single HTTP listener should be disabled.</span></span>  <span data-ttu-id="a4c65-113">I Tomcat, som innehåller hello avstängning, HTTPS och AJP portar.</span><span class="sxs-lookup"><span data-stu-id="a4c65-113">In Tomcat, that includes hello shutdown, HTTPS, and AJP ports.</span></span>
* <span data-ttu-id="a4c65-114">hello behållaren måste toobe som konfigurerats för IPv4-trafik.</span><span class="sxs-lookup"><span data-stu-id="a4c65-114">hello container needs toobe configured for IPv4 traffic only.</span></span>
* <span data-ttu-id="a4c65-115">Hej **Start** kommandot för hello programmet behöver toobe i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a4c65-115">hello **startup** command for hello application needs toobe set in hello configuration.</span></span>
* <span data-ttu-id="a4c65-116">Program som kräver kataloger med skrivbehörighet måste toobe finns i hello Azure webbapp innehållskatalogen som är **D:\home**.</span><span class="sxs-lookup"><span data-stu-id="a4c65-116">Applications that require directories with write permission need toobe located in hello Azure web app's content directory,  which is **D:\home**.</span></span>  <span data-ttu-id="a4c65-117">hello miljövariabeln `HOME` refererar tooD:\home.</span><span class="sxs-lookup"><span data-stu-id="a4c65-117">hello environmental variable `HOME` refers tooD:\home.</span></span>  

<span data-ttu-id="a4c65-118">Du kan ange miljövariabler som krävs i hello web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="a4c65-118">You can set environment variables as required in hello web.config file.</span></span>

## <a name="webconfig-httpplatform-configuration"></a><span data-ttu-id="a4c65-119">Web.config httpPlatform konfiguration</span><span class="sxs-lookup"><span data-stu-id="a4c65-119">web.config httpPlatform configuration</span></span>
<span data-ttu-id="a4c65-120">hello följande information beskriver hello **httpPlatform** format i web.config.</span><span class="sxs-lookup"><span data-stu-id="a4c65-120">hello following information describes hello **httpPlatform** format within web.config.</span></span>

<span data-ttu-id="a4c65-121">**argument** (standard = ””).</span><span class="sxs-lookup"><span data-stu-id="a4c65-121">**arguments** (Default="").</span></span> <span data-ttu-id="a4c65-122">Argument toohello körbara filen eller skriptet som anges i hello **processPath** inställningen.</span><span class="sxs-lookup"><span data-stu-id="a4c65-122">Arguments toohello executable or script specified in hello **processPath** setting.</span></span>

<span data-ttu-id="a4c65-123">Exempel (visas med **processPath** ingår):</span><span class="sxs-lookup"><span data-stu-id="a4c65-123">Examples (shown with **processPath** included):</span></span>

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


<span data-ttu-id="a4c65-124">**processPath** -sökvägen toohello körbara eller skript som ska starta en process som lyssnar efter HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="a4c65-124">**processPath** - Path toohello executable or script that will launch a process listening for HTTP requests.</span></span>

<span data-ttu-id="a4c65-125">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a4c65-125">Examples:</span></span>

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

<span data-ttu-id="a4c65-126">**rapidFailsPerMinute** (standard = 10.) Antalet gånger som hello process som anges i **processPath** tillåts toocrash per minut.</span><span class="sxs-lookup"><span data-stu-id="a4c65-126">**rapidFailsPerMinute** (Default=10.) Number of times hello process specified in **processPath** is allowed toocrash per minute.</span></span> <span data-ttu-id="a4c65-127">Om den här gränsen överskrids, **HttpPlatformHandler** stoppas starta hello process för hello resten av hello minut.</span><span class="sxs-lookup"><span data-stu-id="a4c65-127">If this limit is exceeded, **HttpPlatformHandler** will stop launching hello process for hello remainder of hello minute.</span></span>

<span data-ttu-id="a4c65-128">**requestTimeout** (standard = ”00: 02:00”.) Varaktighet för som **HttpPlatformHandler** väntar på svar från hello process lyssnar på `%HTTP_PLATFORM_PORT%`.</span><span class="sxs-lookup"><span data-stu-id="a4c65-128">**requestTimeout** (Default="00:02:00".) Duration for which **HttpPlatformHandler** will wait for a response from hello process listening on `%HTTP_PLATFORM_PORT%`.</span></span>

<span data-ttu-id="a4c65-129">**startupRetryCount** (standard = 10.) Antal gånger **HttpPlatformHandler** försöker toolaunch hello process som anges i **processPath**.</span><span class="sxs-lookup"><span data-stu-id="a4c65-129">**startupRetryCount** (Default=10.) Number of times **HttpPlatformHandler** will try toolaunch hello process specified in **processPath**.</span></span> <span data-ttu-id="a4c65-130">Se **startupTimeLimit** för mer information.</span><span class="sxs-lookup"><span data-stu-id="a4c65-130">See **startupTimeLimit** for more details.</span></span>

<span data-ttu-id="a4c65-131">**startupTimeLimit** (standard = 10 sekunder.) Varaktighet för som **HttpPlatformHandler** väntar på att hello körbara filer eller skript toostart en process som lyssnar på port hello.</span><span class="sxs-lookup"><span data-stu-id="a4c65-131">**startupTimeLimit** (Default=10 seconds.) Duration for which **HttpPlatformHandler** will wait for hello executable/script toostart a process listening on hello port.</span></span>  <span data-ttu-id="a4c65-132">Om denna tidsgräns överskrids **HttpPlatformHandler** kommer att avsluta processen hello och försök toolaunch igen **startupRetryCount** gånger.</span><span class="sxs-lookup"><span data-stu-id="a4c65-132">If this time limit is exceeded, **HttpPlatformHandler** will kill hello process and try toolaunch it again **startupRetryCount** times.</span></span>

<span data-ttu-id="a4c65-133">**stdoutLogEnabled** (standard = ”true”.) Om värdet är true **stdout** och **stderr** för hello process som anges i hello **processPath** blir omdirigerade toohello-fil som anges i inställningen ** stdoutLogFile** (se **stdoutLogFile** avsnitt).</span><span class="sxs-lookup"><span data-stu-id="a4c65-133">**stdoutLogEnabled** (Default="true".) If true, **stdout** and **stderr** for hello process specified in hello **processPath** setting will be redirected toohello file specified in **stdoutLogFile** (see **stdoutLogFile** section).</span></span>

<span data-ttu-id="a4c65-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log”.) Absolut sökväg som **stdout** och **stderr** från hello-processen som angetts i **processPath** kommer att loggas.</span><span class="sxs-lookup"><span data-stu-id="a4c65-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolute file path for which **stdout** and **stderr** from hello process specified in **processPath** will be logged.</span></span>

> [!NOTE]
> <span data-ttu-id="a4c65-135">`%HTTP_PLATFORM_PORT%`är en särskild platshållare som måste antingen som en del av toospecified **argument** eller som en del av hello **httpPlatform** **environmentVariables** lista.</span><span class="sxs-lookup"><span data-stu-id="a4c65-135">`%HTTP_PLATFORM_PORT%` is a special placeholder which needs toospecified either as part of **arguments** or as part of hello **httpPlatform** **environmentVariables** list.</span></span> <span data-ttu-id="a4c65-136">Detta kommer att ersättas av ett internt genererade port som **HttpPlatformHandler** så att hello processen som anges av **processPath** kan lyssna på den här porten.</span><span class="sxs-lookup"><span data-stu-id="a4c65-136">This will be replaced by an internally generated port by **HttpPlatformHandler** so that hello process specified by **processPath** can listen on this port.</span></span>
> 
> 

## <a name="deployment"></a><span data-ttu-id="a4c65-137">Distribution</span><span class="sxs-lookup"><span data-stu-id="a4c65-137">Deployment</span></span>
<span data-ttu-id="a4c65-138">Java-baserade webbappar kan distribueras enkelt genom de flesta av samma innebär hello som används med hello Internet Information Services (IIS) baserade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a4c65-138">Java based web apps can be deployed easily through most of hello same means that are used with hello Internet Information Services (IIS) based web applications.</span></span>  <span data-ttu-id="a4c65-139">FTP, Git och Kudu stöds alla som distributionen mekanismer som är hello integrerad SCM kapaciteten för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a4c65-139">FTP, Git and Kudu are all supported as deployment mechanisms, as is hello integrated SCM capability for web apps.</span></span> <span data-ttu-id="a4c65-140">WebDeploy fungerar som ett protokoll, men som Java inte har utvecklats i Visual Studio WebDeploy passar inte med användningsområden för Java web app-distribution.</span><span class="sxs-lookup"><span data-stu-id="a4c65-140">WebDeploy works as a protocol, however, as Java is not developed in Visual Studio, WebDeploy does not fit with Java web app deployment use cases.</span></span>

## <a name="application-configuration-examples"></a><span data-ttu-id="a4c65-141">Programkonfigurationen exempel</span><span class="sxs-lookup"><span data-stu-id="a4c65-141">Application configuration Examples</span></span>
<span data-ttu-id="a4c65-142">För följande program, en web.config-filen och hello hello programkonfigurationen tillhandahålls som exempel tooshow hur tooenable Java-programmet på App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a4c65-142">For hello following applications, a web.config file and hello application configuration is provided as examples tooshow how tooenable your Java application on App Service Web Apps.</span></span>

### <a name="tomcat"></a><span data-ttu-id="a4c65-143">Tomcat</span><span class="sxs-lookup"><span data-stu-id="a4c65-143">Tomcat</span></span>
<span data-ttu-id="a4c65-144">Det finns två varianter av Tomcat som levereras med App Service Web Apps, men det är fortfarande fullt möjligt tooupload kunden specifika instanser.</span><span class="sxs-lookup"><span data-stu-id="a4c65-144">While there are two variations on Tomcat that are supplied with App Service Web Apps, it is still quite possible tooupload customer specific instances.</span></span> <span data-ttu-id="a4c65-145">Nedan visas ett exempel på en installation av Tomcat med en annan Java Virtual Machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="a4c65-145">Below is an example of an install of Tomcat with a different Java Virtual Machine (JVM).</span></span>

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat" 
            arguments="">
          <environmentVariables>
            <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
            <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\bin\tomcat" />
            <environmentVariable name="JRE_HOME" value="%HOME%\site\wwwroot\bin\java" /> <!-- optional, if not specified, this will default too%programfiles%\Java -->
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

<span data-ttu-id="a4c65-146">På hello Tomcat sida finns det några konfigurationsändringar som behöver toobe som gjorts.</span><span class="sxs-lookup"><span data-stu-id="a4c65-146">On hello Tomcat side, there are a few configuration changes that need toobe made.</span></span> <span data-ttu-id="a4c65-147">Hej server.xml måste redigeras toobe tooset:</span><span class="sxs-lookup"><span data-stu-id="a4c65-147">hello server.xml needs toobe edited tooset:</span></span>

* <span data-ttu-id="a4c65-148">Avstängning port = -1</span><span class="sxs-lookup"><span data-stu-id="a4c65-148">Shutdown port = -1</span></span>
* <span data-ttu-id="a4c65-149">HTTP-porten för koppling = ${port.http}</span><span class="sxs-lookup"><span data-stu-id="a4c65-149">HTTP connector port = ${port.http}</span></span>
* <span data-ttu-id="a4c65-150">HTTP-anslutningen adress = ”127.0.0.1”</span><span class="sxs-lookup"><span data-stu-id="a4c65-150">HTTP connector address = "127.0.0.1"</span></span>
* <span data-ttu-id="a4c65-151">Kommentera ut HTTPS och AJP.</span><span class="sxs-lookup"><span data-stu-id="a4c65-151">Comment out HTTPS and AJP connectors</span></span>
* <span data-ttu-id="a4c65-152">hello IPv4 inställningen kan också anges i hello catalina.properties filen där du kan lägga till`java.net.preferIPv4Stack=true`</span><span class="sxs-lookup"><span data-stu-id="a4c65-152">hello IPv4 setting can also be set in hello catalina.properties file where you can add     `java.net.preferIPv4Stack=true`</span></span>

<span data-ttu-id="a4c65-153">Direct3d samtal stöds inte på App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a4c65-153">Direct3d calls are not supported on App Service Web Apps.</span></span> <span data-ttu-id="a4c65-154">toodisable, lägger du till hello följande Java alternativet ditt program bör göra sådana samtal:`-Dsun.java2d.d3d=false`</span><span class="sxs-lookup"><span data-stu-id="a4c65-154">toodisable those, add hello following Java option should your application make such calls: `-Dsun.java2d.d3d=false`</span></span>

### <a name="jetty"></a><span data-ttu-id="a4c65-155">Jetty</span><span class="sxs-lookup"><span data-stu-id="a4c65-155">Jetty</span></span>
<span data-ttu-id="a4c65-156">Som är hello fallet för Tomcat kan kunder överföra sina egna instanser för Jetty.</span><span class="sxs-lookup"><span data-stu-id="a4c65-156">As is hello case for Tomcat, customers can upload their own instances for Jetty.</span></span> <span data-ttu-id="a4c65-157">Hello gäller körs hello fullständig installation av Jetty, hello konfiguration skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="a4c65-157">In hello case of running hello full install of Jetty, hello configuration would look like this:</span></span>

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httppPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" 
             arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP_PLATFORM_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"
            startupTimeLimit="20"
          startupRetryCount="10"
          stdoutLogEnabled="true">
        </httpPlatform>
      </system.webServer>
    </configuration>

<span data-ttu-id="a4c65-158">Hej Jetty konfiguration måste toobe har ändrats i hello start.ini tooset `java.net.preferIPv4Stack=true`.</span><span class="sxs-lookup"><span data-stu-id="a4c65-158">hello Jetty configuration needs toobe changed in hello start.ini tooset `java.net.preferIPv4Stack=true`.</span></span>

### <a name="springboot"></a><span data-ttu-id="a4c65-159">Springboot</span><span class="sxs-lookup"><span data-stu-id="a4c65-159">Springboot</span></span>
<span data-ttu-id="a4c65-160">I ordning tooget ett Springboot program som kör du behöver tooupload JAR eller WAR-filen och Lägg till hello följande web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="a4c65-160">In order tooget a Springboot application running you need tooupload your JAR or WAR file and add hello following web.config file.</span></span> <span data-ttu-id="a4c65-161">hello web.config-filen blir hello wwwroot mapp.</span><span class="sxs-lookup"><span data-stu-id="a4c65-161">hello web.config file goes into hello wwwroot folder.</span></span> <span data-ttu-id="a4c65-162">Justera hello argument toopoint tooyour JAR-filen i hello web.config, i hello följande exempel hello JAR-filen finns i hello Wwwroot-mappen samt.</span><span class="sxs-lookup"><span data-stu-id="a4c65-162">In hello web.config adjust hello arguments toopoint tooyour JAR file, in hello following example hello JAR file is located in hello wwwroot folder as well.</span></span>  

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
            arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\my-web-project.jar&quot;">
        </httpPlatform>
      </system.webServer>
    </configuration>


### <a name="hudson"></a><span data-ttu-id="a4c65-163">Hudson</span><span class="sxs-lookup"><span data-stu-id="a4c65-163">Hudson</span></span>
<span data-ttu-id="a4c65-164">Vår test används hello Hudson 3.1.2 war och hello Tomcat 7.0.50 standardinstansen men utan att hello UI tooset saker.</span><span class="sxs-lookup"><span data-stu-id="a4c65-164">Our test used hello Hudson 3.1.2 war and hello default Tomcat 7.0.50 instance but without using hello UI tooset things up.</span></span>  <span data-ttu-id="a4c65-165">Eftersom Hudson är ett build programverktyg, är det uppmanade tooinstall på dedikerad instanser där hello **AlwaysOn** flaggan anges på hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a4c65-165">Because Hudson is a software build tool, it is advised tooinstall it on dedicated instances where hello **AlwaysOn** flag can be set on hello web app.</span></span>

1. <span data-ttu-id="a4c65-166">I ditt webbprogram rotkatalogen, d.v.s. **d:\home\site\wwwroot**, skapa en **webbappar** katalog (om det inte redan finns), och placera Hudson.war i **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="a4c65-166">In your web app’s root directory, i.e., **d:\home\site\wwwroot**, create a **webapps** directory (if not already present), and place Hudson.war in **d:\home\site\wwwroot\webapps**.</span></span>
2. <span data-ttu-id="a4c65-167">Hämta apache maven 3.0.5 (kompatibelt med Hudson) och placera den i **d:\home\site\wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="a4c65-167">Download apache maven 3.0.5 (compatible with Hudson) and place it in **d:\home\site\wwwroot**.</span></span>
3. <span data-ttu-id="a4c65-168">Skapa web.config i **d:\home\site\wwwroot** och klistra in hello efter innehållet i den:</span><span class="sxs-lookup"><span data-stu-id="a4c65-168">Create web.config in **d:\home\site\wwwroot** and paste hello following contents in it:</span></span>
   
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
          <system.webServer>
            <handlers>
              <add name="httppPlatformHandler" path="*" verb="*" 
        modules="httpPlatformHandler" resourceType="Unspecified" />
            </handlers>
            <httpPlatform processPath="%AZURE_TOMCAT7_HOME%\bin\startup.bat"
        startupTimeLimit="20"
        startupRetryCount="10">
        <environmentVariables>
          <environmentVariable name="HUDSON_HOME" 
        value="%HOME%\site\wwwroot\hudson_home" />
          <environmentVariable name="JAVA_OPTS" 
        value="-Djava.net.preferIPv4Stack=true -Duser.home=%HOME%/site/wwwroot/user_home -Dhudson.DNSMultiCast.disabled=true" />
        </environmentVariables>            
            </httpPlatform>
          </system.webServer>
        </configuration>
   
    <span data-ttu-id="a4c65-169">Hello webbprogrammet kan nu vara startas om tootake hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="a4c65-169">At this point hello web app can be restarted tootake hello changes.</span></span>  <span data-ttu-id="a4c65-170">Ansluta toohttp://yourwebapp/hudson toostart Hudson.</span><span class="sxs-lookup"><span data-stu-id="a4c65-170">Connect toohttp://yourwebapp/hudson toostart Hudson.</span></span>
4. <span data-ttu-id="a4c65-171">När Hudson konfigureras, bör du se hello följande skärm:</span><span class="sxs-lookup"><span data-stu-id="a4c65-171">After Hudson configures itself, you should see hello following screen:</span></span>
   
    ![Hudson](./media/web-sites-java-custom-upload/hudson1.png)
5. <span data-ttu-id="a4c65-173">Åtkomst hello Hudson konfigurationssidan: Klicka på **hantera Hudson**, och klicka sedan på **konfigurera systemet**.</span><span class="sxs-lookup"><span data-stu-id="a4c65-173">Access hello Hudson configuration page: Click **Manage Hudson**, and then click **Configure System**.</span></span>
6. <span data-ttu-id="a4c65-174">Konfigurera hello JDK enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="a4c65-174">Configure hello JDK as shown below:</span></span>
   
    ![Hudson konfiguration](./media/web-sites-java-custom-upload/hudson2.png)
7. <span data-ttu-id="a4c65-176">Konfigurera Maven enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="a4c65-176">Configure Maven as shown below:</span></span>
   
    ![Maven-konfiguration](./media/web-sites-java-custom-upload/maven.png)
8. <span data-ttu-id="a4c65-178">Spara hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="a4c65-178">Save hello settings.</span></span> <span data-ttu-id="a4c65-179">Hudson ska nu vara konfigurerad och klar för användning.</span><span class="sxs-lookup"><span data-stu-id="a4c65-179">Hudson should now be configured and ready for use.</span></span>

<span data-ttu-id="a4c65-180">Ytterligare information om Hudson finns [http://hudson-ci.org](http://hudson-ci.org).</span><span class="sxs-lookup"><span data-stu-id="a4c65-180">For additional information on Hudson, see [http://hudson-ci.org](http://hudson-ci.org).</span></span>

### <a name="liferay"></a><span data-ttu-id="a4c65-181">Liferay</span><span class="sxs-lookup"><span data-stu-id="a4c65-181">Liferay</span></span>
<span data-ttu-id="a4c65-182">Liferay stöds i App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a4c65-182">Liferay is supported on App Service Web Apps.</span></span> <span data-ttu-id="a4c65-183">Eftersom Liferay kan kräva betydande minne, måste hello webbprogrammet toorun på en medelstora eller stora dedikerade arbetarservrar, vilket kan ge tillräckligt med minne.</span><span class="sxs-lookup"><span data-stu-id="a4c65-183">Since Liferay can require significant memory, hello web app needs toorun on a medium or large dedicated worker, which can provide enough memory.</span></span> <span data-ttu-id="a4c65-184">Liferay också tar flera minuter toostart.</span><span class="sxs-lookup"><span data-stu-id="a4c65-184">Liferay also takes several minutes toostart up.</span></span> <span data-ttu-id="a4c65-185">Därför rekommenderas att du har angett hello webbprogrammet för**alltid på**.</span><span class="sxs-lookup"><span data-stu-id="a4c65-185">For that reason, it is recommended that you set hello web app too**Always On**.</span></span>  

<span data-ttu-id="a4c65-186">Med Tomcat Liferay 6.1.2 Community Edition GA3 paketerade har hello följande filer redigerats när du har hämtat Liferay:</span><span class="sxs-lookup"><span data-stu-id="a4c65-186">Using Liferay 6.1.2 Community Edition GA3 bundled with Tomcat, hello following files were edited after downloading Liferay:</span></span>

<span data-ttu-id="a4c65-187">**Server.XML**</span><span class="sxs-lookup"><span data-stu-id="a4c65-187">**Server.xml**</span></span>

* <span data-ttu-id="a4c65-188">Ändra avstängning porten för-1.</span><span class="sxs-lookup"><span data-stu-id="a4c65-188">Change Shutdown port too-1.</span></span>
* <span data-ttu-id="a4c65-189">Ändra HTTP-anslutningen för`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span><span class="sxs-lookup"><span data-stu-id="a4c65-189">Change HTTP connector too       `<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span></span>
* <span data-ttu-id="a4c65-190">Kommentera ut hello AJP anslutning.</span><span class="sxs-lookup"><span data-stu-id="a4c65-190">Comment out hello AJP connector.</span></span>

<span data-ttu-id="a4c65-191">I hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** mapp, skapa en fil med namnet **portal ext.properties**.</span><span class="sxs-lookup"><span data-stu-id="a4c65-191">In hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** folder, create a file named **portal-ext.properties**.</span></span> <span data-ttu-id="a4c65-192">Den här filen måste toocontain en rad, som visas här:</span><span class="sxs-lookup"><span data-stu-id="a4c65-192">This file needs toocontain one line, as shown here:</span></span>

    liferay.home=%HOME%/site/wwwroot/liferay

<span data-ttu-id="a4c65-193">Vid hello samma katalog-nivå som hello tomcat 7.0.40 mapp för att skapa en fil med namnet **web.config** med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a4c65-193">At hello same directory level as hello tomcat-7.0.40 folder, create a file named **web.config** with hello following content:</span></span>

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
    <add name="httpPlatformHandler" path="*" verb="*"
         modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\tomcat-7.0.40\bin\catalina.bat" 
                      arguments="run" 
                      startupTimeLimit="10" 
                      requestTimeout="00:10:00" 
                      stdoutLogEnabled="true">
          <environmentVariables>
      <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
      <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\tomcat-7.0.40" />
            <environmentVariable name="JRE_HOME" value="D:\Program Files\Java\jdk1.7.0_51" /> 
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

<span data-ttu-id="a4c65-194">Under hello **httpPlatform** blocket hello **requestTimeout** anges för ”00: 10:00”.</span><span class="sxs-lookup"><span data-stu-id="a4c65-194">Under hello **httpPlatform** block, hello **requestTimeout** is set too“00:10:00”.</span></span>  <span data-ttu-id="a4c65-195">Du kan minska men är du förmodligen toosee vissa timeout-fel när Liferay startprogram.</span><span class="sxs-lookup"><span data-stu-id="a4c65-195">It can be reduced but then you are likely toosee some timeout errors while Liferay is bootstrapping.</span></span>  <span data-ttu-id="a4c65-196">Om det här värdet har ändrats sedan hello **connectionTimeout** i hello tomcat server.xml bör också ändras.</span><span class="sxs-lookup"><span data-stu-id="a4c65-196">If this value is changed, then hello **connectionTimeout** in hello tomcat server.xml should also be modified.</span></span>  

<span data-ttu-id="a4c65-197">Det är värt att nämna att hello JRE_HOME miljö varariable har angetts i hello ovan web.config toopoint toohello 64-bitars JDK.</span><span class="sxs-lookup"><span data-stu-id="a4c65-197">It is worth noting that hello JRE_HOME environnment varariable is specified in hello above web.config toopoint toohello 64-bit JDK.</span></span> <span data-ttu-id="a4c65-198">hello standardvärdet är 32-bitars, men eftersom Liferay kan kräva höga nivåer av minne, rekommenderas toouse hello 64-bitars JDK.</span><span class="sxs-lookup"><span data-stu-id="a4c65-198">hello default is 32-bit, but since Liferay may require high levels of memory, it is recommended toouse hello 64-bit JDK.</span></span>

<span data-ttu-id="a4c65-199">När du gör dessa ändringar, starta om din webbapp körs Liferay Öppna http://yourwebapp.</span><span class="sxs-lookup"><span data-stu-id="a4c65-199">Once you make these changes, restart your web app running Liferay, Then, open http://yourwebapp.</span></span> <span data-ttu-id="a4c65-200">Hej Liferay portal är tillgänglig från hello web app rot.</span><span class="sxs-lookup"><span data-stu-id="a4c65-200">hello Liferay portal is available from hello web app root.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a4c65-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4c65-201">Next steps</span></span>
<span data-ttu-id="a4c65-202">Läs mer om Liferay [http://www.liferay.com](http://www.liferay.com).</span><span class="sxs-lookup"><span data-stu-id="a4c65-202">For more information about Liferay, see [http://www.liferay.com](http://www.liferay.com).</span></span>

<span data-ttu-id="a4c65-203">Mer information om Java finns [Azure för Java-utvecklare](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="a4c65-203">For more information about Java, visit [Azure for Java developers](/java/azure).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
