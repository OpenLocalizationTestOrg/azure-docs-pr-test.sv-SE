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
# <a name="upload-a-custom-java-web-app-tooazure"></a>Ladda upp en anpassad Java web app tooAzure
Det här avsnittet beskrivs hur tooupload en anpassad Java webbapp för[Azure App Service] Web Apps. Innehåller information som gäller tooany Java-webbplats eller webbprogram och även några exempel på specifika program.

Observera att Azure tillhandahåller ett sätt för att skapa Java-webbappar med hjälp av Användargränssnittet för konfigurationen av hello Azure Portal och hello Azure Marketplace, enligt beskrivningen i [skapar en Java-webbapp i Azure App Service](web-sites-java-get-started.md). Den här kursen är för scenarier där du inte vill toouse hello Azure Portal Konfigurationsgränssnittet eller hello Azure Marketplace.  

## <a name="configuration-guidelines"></a>Riktlinjer för konfiguration
hello nedan beskrivs hello inställningar för anpassad Java-webbappar på Azure.

* hello HTTP-porten används av hello Java-processen tilldelas dynamiskt.  hello processen måste använda hello port från hello miljövariabeln `HTTP_PLATFORM_PORT`.
* Alla lyssna portar än hello enskild HTTP-lyssnaren ska inaktiveras.  I Tomcat, som innehåller hello avstängning, HTTPS och AJP portar.
* hello behållaren måste toobe som konfigurerats för IPv4-trafik.
* Hej **Start** kommandot för hello programmet behöver toobe i hello konfiguration.
* Program som kräver kataloger med skrivbehörighet måste toobe finns i hello Azure webbapp innehållskatalogen som är **D:\home**.  hello miljövariabeln `HOME` refererar tooD:\home.  

Du kan ange miljövariabler som krävs i hello web.config-filen.

## <a name="webconfig-httpplatform-configuration"></a>Web.config httpPlatform konfiguration
hello följande information beskriver hello **httpPlatform** format i web.config.

**argument** (standard = ””). Argument toohello körbara filen eller skriptet som anges i hello **processPath** inställningen.

Exempel (visas med **processPath** ingår):

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


**processPath** -sökvägen toohello körbara eller skript som ska starta en process som lyssnar efter HTTP-begäranden.

Exempel:

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

**rapidFailsPerMinute** (standard = 10.) Antalet gånger som hello process som anges i **processPath** tillåts toocrash per minut. Om den här gränsen överskrids, **HttpPlatformHandler** stoppas starta hello process för hello resten av hello minut.

**requestTimeout** (standard = ”00: 02:00”.) Varaktighet för som **HttpPlatformHandler** väntar på svar från hello process lyssnar på `%HTTP_PLATFORM_PORT%`.

**startupRetryCount** (standard = 10.) Antal gånger **HttpPlatformHandler** försöker toolaunch hello process som anges i **processPath**. Se **startupTimeLimit** för mer information.

**startupTimeLimit** (standard = 10 sekunder.) Varaktighet för som **HttpPlatformHandler** väntar på att hello körbara filer eller skript toostart en process som lyssnar på port hello.  Om denna tidsgräns överskrids **HttpPlatformHandler** kommer att avsluta processen hello och försök toolaunch igen **startupRetryCount** gånger.

**stdoutLogEnabled** (standard = ”true”.) Om värdet är true **stdout** och **stderr** för hello process som anges i hello **processPath** blir omdirigerade toohello-fil som anges i inställningen ** stdoutLogFile** (se **stdoutLogFile** avsnitt).

**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log”.) Absolut sökväg som **stdout** och **stderr** från hello-processen som angetts i **processPath** kommer att loggas.

> [!NOTE]
> `%HTTP_PLATFORM_PORT%`är en särskild platshållare som måste antingen som en del av toospecified **argument** eller som en del av hello **httpPlatform** **environmentVariables** lista. Detta kommer att ersättas av ett internt genererade port som **HttpPlatformHandler** så att hello processen som anges av **processPath** kan lyssna på den här porten.
> 
> 

## <a name="deployment"></a>Distribution
Java-baserade webbappar kan distribueras enkelt genom de flesta av samma innebär hello som används med hello Internet Information Services (IIS) baserade webbprogram.  FTP, Git och Kudu stöds alla som distributionen mekanismer som är hello integrerad SCM kapaciteten för webbprogram. WebDeploy fungerar som ett protokoll, men som Java inte har utvecklats i Visual Studio WebDeploy passar inte med användningsområden för Java web app-distribution.

## <a name="application-configuration-examples"></a>Programkonfigurationen exempel
För följande program, en web.config-filen och hello hello programkonfigurationen tillhandahålls som exempel tooshow hur tooenable Java-programmet på App Service Web Apps.

### <a name="tomcat"></a>Tomcat
Det finns två varianter av Tomcat som levereras med App Service Web Apps, men det är fortfarande fullt möjligt tooupload kunden specifika instanser. Nedan visas ett exempel på en installation av Tomcat med en annan Java Virtual Machine (JVM).

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

På hello Tomcat sida finns det några konfigurationsändringar som behöver toobe som gjorts. Hej server.xml måste redigeras toobe tooset:

* Avstängning port = -1
* HTTP-porten för koppling = ${port.http}
* HTTP-anslutningen adress = ”127.0.0.1”
* Kommentera ut HTTPS och AJP.
* hello IPv4 inställningen kan också anges i hello catalina.properties filen där du kan lägga till`java.net.preferIPv4Stack=true`

Direct3d samtal stöds inte på App Service Web Apps. toodisable, lägger du till hello följande Java alternativet ditt program bör göra sådana samtal:`-Dsun.java2d.d3d=false`

### <a name="jetty"></a>Jetty
Som är hello fallet för Tomcat kan kunder överföra sina egna instanser för Jetty. Hello gäller körs hello fullständig installation av Jetty, hello konfiguration skulle se ut så här:

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

Hej Jetty konfiguration måste toobe har ändrats i hello start.ini tooset `java.net.preferIPv4Stack=true`.

### <a name="springboot"></a>Springboot
I ordning tooget ett Springboot program som kör du behöver tooupload JAR eller WAR-filen och Lägg till hello följande web.config-filen. hello web.config-filen blir hello wwwroot mapp. Justera hello argument toopoint tooyour JAR-filen i hello web.config, i hello följande exempel hello JAR-filen finns i hello Wwwroot-mappen samt.  

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


### <a name="hudson"></a>Hudson
Vår test används hello Hudson 3.1.2 war och hello Tomcat 7.0.50 standardinstansen men utan att hello UI tooset saker.  Eftersom Hudson är ett build programverktyg, är det uppmanade tooinstall på dedikerad instanser där hello **AlwaysOn** flaggan anges på hello webbprogrammet.

1. I ditt webbprogram rotkatalogen, d.v.s. **d:\home\site\wwwroot**, skapa en **webbappar** katalog (om det inte redan finns), och placera Hudson.war i **d:\home\site\wwwroot\webapps**.
2. Hämta apache maven 3.0.5 (kompatibelt med Hudson) och placera den i **d:\home\site\wwwroot**.
3. Skapa web.config i **d:\home\site\wwwroot** och klistra in hello efter innehållet i den:
   
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
   
    Hello webbprogrammet kan nu vara startas om tootake hello ändringar.  Ansluta toohttp://yourwebapp/hudson toostart Hudson.
4. När Hudson konfigureras, bör du se hello följande skärm:
   
    ![Hudson](./media/web-sites-java-custom-upload/hudson1.png)
5. Åtkomst hello Hudson konfigurationssidan: Klicka på **hantera Hudson**, och klicka sedan på **konfigurera systemet**.
6. Konfigurera hello JDK enligt nedan:
   
    ![Hudson konfiguration](./media/web-sites-java-custom-upload/hudson2.png)
7. Konfigurera Maven enligt nedan:
   
    ![Maven-konfiguration](./media/web-sites-java-custom-upload/maven.png)
8. Spara hello inställningar. Hudson ska nu vara konfigurerad och klar för användning.

Ytterligare information om Hudson finns [http://hudson-ci.org](http://hudson-ci.org).

### <a name="liferay"></a>Liferay
Liferay stöds i App Service Web Apps. Eftersom Liferay kan kräva betydande minne, måste hello webbprogrammet toorun på en medelstora eller stora dedikerade arbetarservrar, vilket kan ge tillräckligt med minne. Liferay också tar flera minuter toostart. Därför rekommenderas att du har angett hello webbprogrammet för**alltid på**.  

Med Tomcat Liferay 6.1.2 Community Edition GA3 paketerade har hello följande filer redigerats när du har hämtat Liferay:

**Server.XML**

* Ändra avstängning porten för-1.
* Ändra HTTP-anslutningen för`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`
* Kommentera ut hello AJP anslutning.

I hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** mapp, skapa en fil med namnet **portal ext.properties**. Den här filen måste toocontain en rad, som visas här:

    liferay.home=%HOME%/site/wwwroot/liferay

Vid hello samma katalog-nivå som hello tomcat 7.0.40 mapp för att skapa en fil med namnet **web.config** med hello följande innehåll:

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

Under hello **httpPlatform** blocket hello **requestTimeout** anges för ”00: 10:00”.  Du kan minska men är du förmodligen toosee vissa timeout-fel när Liferay startprogram.  Om det här värdet har ändrats sedan hello **connectionTimeout** i hello tomcat server.xml bör också ändras.  

Det är värt att nämna att hello JRE_HOME miljö varariable har angetts i hello ovan web.config toopoint toohello 64-bitars JDK. hello standardvärdet är 32-bitars, men eftersom Liferay kan kräva höga nivåer av minne, rekommenderas toouse hello 64-bitars JDK.

När du gör dessa ändringar, starta om din webbapp körs Liferay Öppna http://yourwebapp. Hej Liferay portal är tillgänglig från hello web app rot. 

## <a name="next-steps"></a>Nästa steg
Läs mer om Liferay [http://www.liferay.com](http://www.liferay.com).

Mer information om Java finns [Azure för Java-utvecklare](/java/azure).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
