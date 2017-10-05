---
title: "Skapa en Webbapp i Azure App Service med hjälp av Azure SDK för Java"
description: "Lär dig hur du skapar en Webbapp i Azure App Service via programmering med Azure SDK för Java."
tags: azure-classic-portal
services: app-service-web
documentationcenter: Java
author: donntrenton
manager: erikre
editor: jimbe
ms.assetid: 8954c456-1275-4d57-aff4-ca7d6374b71e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/25/2016
ms.author: v-donntr
ms.openlocfilehash: 08bb53de8cf437a5a2b1c3b38bce9f81b8349493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Skapa en Webbapp i Azure App Service med hjälp av Azure SDK för Java
<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Översikt
Den här genomgången visar hur du skapar ett Azure SDK för Java-program som skapar en Webbapp i [Azure App Service][Azure App Service], distribuera program till den. Det består av två delar:

* Del 1 visar hur du skapar en Java-program som skapar ett webbprogram.
* Del 2 visar hur du skapar en enkel JSP ”Hello World” program och sedan använda en FTP-klient för att distribuera kod till App Service.

## <a name="prerequisites"></a>Krav
### <a name="software-installations"></a>Programinstallationer
Programkoden AzureWebDemo i den här artikeln skrevs med Azure Java SDK 0.7.0, som du kan installera med hjälp av den [installationsprogram för webbplattform] [ Web Platform Installer] (WebPI). Dessutom se till att använda den senaste versionen av den [Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]. När du har installerat SDK uppdatera beroenden i Eclipse-projektet genom att köra **uppdatera Index** i **Maven databaser**, sedan lägger till den senaste versionen av varje paket i den **beroenden** fönster. Du kan kontrollera vilken version av installerad programvara i Eclipse genom att klicka på **Hjälp > installationsinformationen**; du bör ha minst följande versioner:

* Paket för Microsoft Azure-biblioteken för Java 0.7.0.20150309
* Eclipse IDE för Java EE-utvecklare 4.4.2.20150219

### <a name="create-and-configure-cloud-resources-in-azure"></a>Skapa och konfigurera Molnresurser i Azure
Innan du påbörjar den här proceduren måste du har en aktiv Azure-prenumeration och skapa en standard-Active Directory (AD) på Azure.

### <a name="create-an-active-directory-ad-in-azure"></a>Skapa en Active Directory (AD) i Azure
Om du inte redan har en Active Directory (AD) på din Azure-prenumeration logga in på den [klassiska Azure-portalen] [ Azure classic portal] med ditt Microsoft-konto. Om du har flera prenumerationer, klickar du på **prenumerationer** och välj standardkatalogen för den prenumeration som du vill använda för det här projektet. Klicka på **tillämpa** växla till den prenumerationsvyn.

1. Välj **Active Directory** på menyn till vänster. **Klicka på Ny > Directory > Skapa anpassade**.
2. I **Lägg till katalog**väljer **Skapa ny katalog**.
3. I **namn**, ange namnet på en katalog.
4. I **domän**, ange ett domännamn. Detta är ett grundläggande domännamn som ingår som standard med din katalog. den har formatet `<domain_name>.onmicrosoft.com`. Du kan kalla det baserat på katalognamnet eller ett annat domännamn som du äger. Du kan senare lägga till ett annat domännamn som din organisation redan använder.
5. I **land eller region**, Välj ditt språk.

Mer information om AD finns [vad är Azure AD-katalog][What is an Azure AD directory]?

### <a name="create-a-management-certificate-for-azure"></a>Skapa ett Hanteringscertifikat för Azure
Azure SDK för Java använder certifikat för att autentisera med Azure-prenumerationer. Dessa är X.509 v3-certifikat som du använder för att autentisera ett klientprogram som använder Service Management API för att agera för att hantera prenumerationsresurser prenumerationsägaren.

Koden i den här proceduren använder ett självsignerat certifikat för att autentisera med Azure. För den här proceduren måste du skapa ett certifikat och överföra den till den [klassiska Azure-portalen] [ Azure classic portal] i förväg. Detta omfattar följande steg:

* Generera en PFX-fil som representerar klientcertifikatet och spara den lokalt.
* Skapa ett hanteringscertifikat (CER-fil) från PFX-filen.
* Överföra CER-filen till din Azure-prenumeration.
* Konvertera PFX-filen till JKS, eftersom Java används formatet för att autentisera med certifikat.
* Skriva programmets Autentiseringskod som refererar till den lokala JKS-filen.

När du slutför den här proceduren CER certifikatet ska placeras i din Azure-prenumeration och JKS certifikat kommer att finnas på en lokal hårddisk. Mer information om certifikat finns [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].

#### <a name="create-a-certificate"></a>Skapa ett certifikat
Öppna en kommandokonsolen på operativsystemet och kör följande kommandon om du vill skapa ett eget självsignerat certifikat.

> **Obs:** datorn där du kör det här kommandot måste ha JDK installerad. Sökvägen till keytool beror också på den plats där du installerar JDK. Mer information finns i [nyckeln och certifikatet Management Tool (keytool)] [ Key and Certificate Management Tool (keytool)] i Java online dokumenten.
> 
> 

Skapa .pfx-filen:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

Så här skapar du den .cer-fil:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Där:

* `<java-install-dir>`är sökvägen till den katalog där du installerade Java.
* `<keystore-id>`identifierare för keystore-post (till exempel `AzureRemoteAccess`).
* `<cert-store-dir>`sökvägen till katalogen där du vill lagra certifikat (till exempel `C:/Certificates`).
* `<cert-file-name>`är namnet på certifikatfilen (till exempel `AzureWebDemoCert`).
* `<password>`är det lösenord som du väljer att skydda certifikatet. Det måste vara minst 6 tecken. Du kan ange inga lösenord, men detta inte rekommenderas.
* `<dname>`är det unika namnet X.500 som ska associeras med alias och används som utfärdare och ämne fält i ett självsignerat certifikat.

Mer information finns i [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].

#### <a name="upload-the-certificate"></a>Ladda upp certifikatet
Om du vill överföra ett självsignerat certifikat till Azure, gå till den **inställningar** sidan i den klassiska portalen och klicka sedan på den **Hanteringscertifikat** fliken. Klicka på **överför** längst ned på sidan och navigera till platsen för den CER-fil som du skapade.

#### <a name="convert-the-pfx-file-into-jks"></a>Konvertera PFX-filen till JKS
I Windows kommandotolk (Kör som administratör), CD: n till den katalog som innehåller certifikat och kör följande kommando, där `<java-install-dir>` är den katalog där du installerade Java på datorn:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. När du uppmanas ange mål keystore lösenord. Det här är lösenordet för filen JKS.
2. När du uppmanas ange lösenord för datakällan keystore; Detta är det lösenord som du angav för PFX-filen.

De båda lösenorden behöver inte vara samma. Du kan ange inga lösenord, men detta inte rekommenderas.

## <a name="build-a-web-app-creation-application"></a>Skapa ett program för att skapa webbprogram
### <a name="create-the-eclipse-workspace-and-maven-project"></a>Skapa Eclipse arbetsytan och Maven-projekt
I det här avsnittet skapar du en arbetsyta och ett Maven-projekt för appen att skapa webbprogrammet med namnet AzureWebDemo.

1. Skapa ett nytt Maven-projekt. Klicka på **Arkiv > Nytt > Maven-projekt**. I **nya Maven-projekt**väljer **skapa ett enkelt projekt** och **använda standardplatsen för arbetsytan**.
2. På den andra sidan av **nya Maven-projekt**, anger du följande:
   
   * Grupp-ID:`com.<username>.azure.webdemo`
   * Artefakt-ID: AzureWebDemo
   * Version: 0.0.1-SNAPSHOT
   * Paketera: jar
   * Namn: AzureWebDemo
     
     Klicka på **Slutför**.
3. Öppna filen för det nya projektet pom.xml i Projektutforskaren. Välj den **beroenden** fliken. Eftersom det är ett nytt projekt visas inga paket ännu.
4. Öppna Maven-databaser. **Klicka på fönstret > Visa > andra > Maven > Maven databaser** och på **OK**. Den **Maven databaser** vyn visas längst ned i IDE.
5. Öppna **globala databaser**, högerklicka på den **centrala** databasen och välj **Rebuild Index**.
   
    ![][1]
   
    Det här steget kan ta flera minuter beroende på anslutningen. När indexet återskapas, bör du se Microsoft Azure-paket i den **centrala** Maven-databasen.
6. I **beroenden**, klickar du på **Lägg till**. I **Ange grupp-ID....**  ange `azure-management`. Välj paket för grundläggande hantering och hantering av App Service Web Apps:
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > **Obs:** om du uppdaterar beroenden när en ny version-version du måste lägga till var och en av beroenden i den här listan igen.
   > När du klickar på **Lägg till** och markera varje beroende, visas det med ett nytt versionsnummer i den **beroenden** lista.
   > 
   > 

Klicka på **OK**. Azure-paket visas sedan i den **beroenden** lista.

### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Skriver Java-kod för att skapa ett webbprogram genom att anropa Azure SDK
Skriv sedan den kod som anropar API: er i Azure SDK för Java skapa App Service webbapp.

1. Skapa en Java-klass för att innehålla huvudsakliga post punkt kod. Högerklicka på projektnoden i Projektutforskaren, och välj **New > klassen**.
2. I **ny Java-klass**, klassen namnet `WebCreator` och kontrollera den **offentliga statiska void main** kryssrutan. Valen ska visas på följande sätt:
   
    ![][2]
3. Klicka på **Slutför**. Filen WebCreator.java visas i Projektutforskaren.

### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Anropar Azure API för att skapa en Apptjänst-Webbapp
#### <a name="add-necessary-imports"></a>Lägga till nödvändiga importer
I WebCreator.java, lägger du till följande importer; importen ger åtkomst till klasserna i management-bibliotek för förbrukning av Azure API: er:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;

    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;

    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;

    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;

    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Definiera klassen huvudsakliga post point
Eftersom programmet AzureWebDemo syftar till att skapa en App Service Web App, huvudsakliga klassen namnet för det här programmet `WebAppCreator`. Den här klassen innehåller den huvudsakliga post punkt kod som anropar Azure Service Management API för att skapa webbprogrammet.

Lägg till följande parameterdefinitioner för webbapp och webbutrymmet. Du måste ange din egen Azure-prenumeration ID eller certifikat.

    public class WebAppCreator {

        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";

        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

Där:

* `<subscription-id>`är Azure prenumerations-ID som du vill skapa resursen.
* `<certificate-store-path>`är sökvägen och filnamnet till JKS-filen i katalogen lagra lokala certifikatarkivet. Till exempel `C:/Certificates/CertificateName.jks` för Linux och `C:\Certificates\CertificateName.jks` för Windows.
* `<certificate-password>`är det lösenord som du angav när du skapade ditt JKS certifikat.
* `webAppName`kan vara vilket namn som du väljer; den här proceduren används namnet `WebDemoWebApp`. Det fullständiga domännamnet är den `webAppName` med den `domainName` läggs, så i detta fall hela domännamnet är `webdemowebapp.azurewebsites.net`.
* `domainName`ska anges som ovan.
* `webSpaceName`måste vara ett av de värden som definieras i den [WebSpaceNames] [ WebSpaceNames] klass.
* `appServicePlanName`ska anges som ovan.

> **Obs:** varje gång du kör det här programmet måste du ändra värdet för `webAppName` och `appServicePlanName` (eller ta bort webbprogrammet på Azure Portal) innan du kör programmet igen. Annars misslyckas körningen eftersom samma resursen finns redan på Azure.
> 
> 

#### <a name="define-the-web-creation-method"></a>Definiera metod för skapande av webbprogram
Sedan definiera en metod för att skapa webbprogrammet. Den här metoden `createWebApp`, anger du parametrarna för webbappen och webbutrymmet. Dessutom skapar och konfigurerar Hanteringsklient App Service Web Apps som definieras av den [WebSiteManagementClient] [ WebSiteManagementClient] objekt. Management-klienten är nyckeln till att skapa webbprogram. Det ger RESTful-webbtjänster som gör att program för att hantera webbappar (utför åtgärder som skapar, uppdatera och ta bort) genom att anropa service management API.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Koden kommer utgående HTTP-status i svaret som anger lyckad eller misslyckad, och om det lyckas, kommer utdata namnet på webbappen.

#### <a name="define-the-main-method"></a>Definiera main()-metoden
Ange den main() metod kod som anropar createWebApp() för att skapa webbprogrammet.

Slutligen anropa `createWebApp` från `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Kör programmet och kontrollera att skapa en webbapp
Kontrollera att programmet körs, klicka på **kör > kör**. När programmet har slutförts körs, bör du se följande utdata i Eclipse-konsolen:

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

Logga in på den klassiska Azure-portalen och på **Web Apps**. Den nya webbappen ska visas i listan över Web Apps inom några minuter.

## <a name="deploying-an-application-to-the-web-app"></a>Distribuera ett program till Webbappen
När du har kört AzureWebDemo och skapa den nya webbappen, logga in på den klassiska portalen på **Web Apps**, och välj **WebDemoWebApp** i den **Web Apps** lista. På webbappens instrumentpanelen klickar du på **Bläddra** (eller klicka på Webbadressen `webdemowebapp.azurewebsites.net`) att navigera till den. En tom platshållare-sida visas eftersom inget innehåll inte har ännu publicerats till webbappen.

Nästa du skapar ett ”Hello World”-program och distribuera den till webbappen.

### <a name="create-a-jsp-hello-world-application"></a>Skapa ett JSP Hello World-program
#### <a name="create-the-application"></a>Skapa programmet
För att demonstrera hur du distribuerar ett program på webben visar i följande procedur hur du skapar ett enkelt ”Hello World” Java-program och överföra den till App Service Web App som skapats av ditt program.

1. Klicka på **Arkiv > Nytt > dynamiskt webbprojekt**. Ge den namnet `JSPHello`. Du behöver inte göra några ändringar i den här dialogrutan. Klicka på **Slutför**.
   
    ![][3]
2. I Projektutforskaren, expanderar den **JSPHello** projekt, högerklicka på **webbinnehåll**, klicka på **New > JSP-fil**. I dialogrutan Ny JSP-fil namn för den nya filen `index.jsp`. Klicka på **Nästa**.
3. I den **Välj JSP-mall** väljer **ny JSP-fil (html)** och på **Slutför**.
4. I index.jsp, lägger du till följande kod i den `<head>` och `<body>` tagga avsnitt:
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, the time is <%= date %> 
        </body>

#### <a name="run-the-hello-world-application-in-localhost"></a>Kör programmet Hello World i localhost
Innan du kör det här programmet måste du konfigurera några egenskaper.

1. Högerklicka på den **JSPHello** projektet och välj **egenskaper**.
2. I den **egenskaper** dialogrutan: Välj **Java byggsökväg**, Välj den **ordning och exportera** markerar **JRE systembibliotek**, klicka sedan på **in** att flytta den till överst i listan.
   
    ![][4]
3. Även i den **egenskaper** dialogrutan: Välj **Körningsmål** och på **ny**.
4. I den **nya Server-körningsmiljön** dialogrutan, Välj en server som **Apache Tomcat v7.0** och på **nästa**. I den **Tomcat Server** dialogrutan, ange **namn** till `Apache Tomcat v7.0`, och ange **Tomcat installationskatalog** till den katalog där du installerade versionen av Tomcat-server som du vill använda.
   
    ![][5]
   
    Klicka på **Slutför**.
5. Gå tillbaka till den **Körningsmål** sida av den **egenskaper** dialogrutan. Välj **Apache Tomcat v7.0**, klicka på **OK**.
   
    ![][6]
6. I Eclipse **kör** -menyn klickar du på **kör**. I den **kör som** markerar **körs på servern**. I den **körs på servern** markerar **Tomcat v7.0-Server**:
   
    ![][7]
   
    Klicka på **Slutför**.
7. När programmet körs, bör du se den **JSPHello** visas sidan i ett fönster för localhost i Eclipse (`http://localhost:8080/JSPHello/`), visas följande meddelande:
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-the-application-as-a-war"></a>Exportera det som en WAR-fil
Exportera project web-filer som en webb-arkivfil (WAR) så att du kan distribuera den till webbappen. Följande web projektfiler finns i mappen webbinnehåll:

    META-INF
    WEB-INF
    index.jsp

1. Högerklicka på mappen webbinnehåll och välj **exportera**.
2. I den **exportera Välj** dialogrutan klickar du på **Web > WAR** filen och klicka sedan på **nästa**.
3. I den **WAR-Export** dialogrutan Välj src-katalog i det aktuella projektet och inkludera namnet på WAR-filen i slutet. Exempel:
   
    `<project-path>/JSPHello/src/JSPHello.war`

Mer information om hur du distribuerar WAR-filerna finns [lägga till en Java-program i Azure App Service Web Apps](web-sites-java-add-app.md).

### <a name="deploying-the-hello-world-application-using-ftp"></a>Distribuera programmet Hello World med FTP
Välj en tredje parts FTP-klient för att publicera programmet. Den här proceduren beskrivs två alternativ: Kudu-konsol som är inbyggda i Azure; och FileZilla, ett populära verktyg med en lämplig, grafiskt användargränssnitt.

> **Obs:** Azure Toolkit för Eclipse har stöd för distribution till storage-konton och cloud services, men inte stöder distribution av webbappar. Du kan distribuera till storage-konton och molntjänster med ett projekt för distribution av Azure enligt beskrivningen i [skapar en Hello World program för Azure i Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), men inte för web apps. Använda andra metoder som FTP- eller GitHub för att överföra filer till ditt webbprogram.
> 
> **Obs:** rekommenderar vi inte med FTP från Windows kommandotolk (FTP.EXE kommandoradsverktyget som levereras med Windows). FTP-klienter som använder active FTP, till exempel FTP.EXE, misslyckas ofta ska fungera över brandväggar. Aktiva FTP anger en intern nätverksbaserade adress som FTP-servern misslyckas som sannolikt att ansluta.
> 
> 

Mer information om distribution till en Apptjänst-webbapp med hjälp av FTP finns i följande avsnitt:

* [Distribuera med hjälp av ett FTP-verktyg](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a>Ställ in autentiseringsuppgifter för distribution
Kontrollera att du har kört den **AzureWebDemo** programmet för att skapa ett webbprogram. Du kommer att överföra filer till den här platsen.

1. Logga in på den klassiska portalen och på **Web Apps**. Kontrollera att **WebDemoWebApp** visas i listan över webbappar och se till att den körs. Klicka på **WebDemoWebApp** att öppna dess **instrumentpanelen** sidan.
2. På den **instrumentpanelen** sidan under **snabb i korthet**, klickar du på **ställa in dina autentiseringsuppgifter för distribution** (om du redan har autentiseringsuppgifter för distribution läser detta **återställa dina autentiseringsuppgifter för distribution**).
   
    Autentiseringsuppgifter för distribution är associerade med ett Microsoft-konto. Du måste ange ett användarnamn och lösenord som du kan använda för att distribuera med Git och FTP. Du kan använda dessa autentiseringsuppgifter för att distribuera till ett webbprogram i alla Azure-prenumerationer som är kopplade till ditt Microsoft-konto. Ange autentiseringsuppgifter för Git och FTP-distribution i dialogrutan och anteckna användarnamnet och lösenordet för framtida användning.

#### <a name="get-ftp-connection-information"></a>Hämta information om FTP-anslutning
Om du vill använda FTP för att distribuera programfiler till den nya webbappen, behöver du anslutningsinformationen. Det finns två sätt att hämta anslutningsinformationen. Ett sätt är att besöka webbappens **instrumentpanelen** sidan; det andra sättet är för att hämta webben appens publiceringsprofil. Profilen som är en XML-fil som innehåller information som FTP-värden namn och inloggning autentiseringsuppgifter för dina webbappar i Azure App Service. Du kan använda det här användarnamnet och lösenordet för att distribuera till ett webbprogram i alla prenumerationer som är kopplade till det Azure-kontot, inte bara i en.

Att hämta information om FTP-anslutning från den webbapps blad i den [Azure Portal][Azure Portal]:

1. Under **Essentials**, hitta och kopiera den **värdnamn för FTP-**. Detta är en URI som liknar `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.
2. Under **Essentials**, hitta och kopiera **användarnamn för FTP-/ Distributionsanvändare**. Detta innebär att formuläret *webappname\deployment-username*, till exempel `WebDemoWebApp\deployer77`.

Du kan hämta information om FTP-anslutning från profilen:

1. I den webbapps blad klickar du på **Get publiceringsprofil**. En .publishsettings-fil hämtas till din lokala enhet.
2. Öppna .publishsettings-fil i en XML-redigerare eller textredigerare och hitta det `<publishProfile>` element som innehåller `publishMethod="FTP"`. Det bör se ut ungefär så här:
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. Observera att webbappens `publishProfile` inställningar karta till FileZilla Platshanteraren inställningarna på följande sätt:

* `publishUrl`samma som **värdnamn för FTP-**, värdet i **värden**.
* `publishMethod="FTP"`innebär att du ställer in **protokollet** till **FTP - File Transfer Protocol**, och **kryptering** till **använder vanlig FTP**.
* `userName`och `userPWD` är nycklarna för den faktiska användarnamn och lösenord som du angav när du återställer autentiseringsuppgifter för distribution. `userName`samma som **distribution / FTP-användare**. Mappa till **användare** och **lösenord** i FileZilla.
* `ftpPassiveMode="True"`innebär att FTP-platsen använder passiv FTP-överföringen. Välj **passiva** på den **Överföringsinställningar** fliken.

#### <a name="configure-the-web-app-to-host-a-java-application"></a>Konfigurera Webbappen som värd för ett Java-program
Innan du publicerar program måste du ändra några konfigurationsinställningar för så att webbprogrammet kan vara värd för ett Java-program.

1. I den klassiska portalen går du till webbappens **instrumentpanelen** och klickar på **konfigurera**. På den **konfigurera** anger du följande inställningar.
2. I **Java version** standardvärdet är **av**; Välj Java version program-mål, till exempel 1.7.0_51. När du gör det också se till att **webbehållaren** anges till en version av Tomcat-Server.
3. I **standarddokument**, lägga till index.jsp och flytta upp i listan. (Standardfilen för webbprogram är hostingstart.html.)
4. Klicka på **Spara**.

#### <a name="publish-your-application-using-kudu"></a>Publicera programmet med hjälp av Kudu
Ett sätt att publicera programmet är att använda Kudu-Felsökningskonsolen som är inbyggda i Azure. Kudu har visat sig vara stabila och konsekvent med App Service Web Apps och Tomcat-Server. Du har åtkomst till konsolen för webbappen genom att bläddra till URL: en följande format:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Den här proceduren finns Kudu-konsolen på följande URL; Bläddra till den här platsen:
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. På den översta menyn, Välj **Felsökningskonsolen > CMD**.
3. I kommandoraden konsolen navigerar du till `/site/wwwroot` (eller klicka på `site`, sedan `wwwroot` i vyn directory överst på sidan):
   
    `cd /site/wwwroot`
4. När du har angett **Java version**, Tomcat-server ska skapa en katalog för webbappar. Gå till katalogen webbappar på kommandoraden konsolen:
   
    `mkdir webapps`
   
    `cd webapps`
5. Dra JSPHello.war från `<project-path>/JSPHello/src/` och släpp det i vyn Kudu directory under `/site/wwwroot/webapps`. Dra inte den till området ”dra hit om du vill ladda upp och zip” eftersom Tomcat kommer packa upp den.
   
   ![][8]

Första JSPHello.war visas i området directory ensamt:

  ![][9]

Under en kort tid (förmodligen mindre än 5 minuter) kommer Tomcat Server packa WAR-filen till en uppackade JSPHello katalog. Klicka på rotkatalogen för att se om index.jsp har uppackade och kopieras dit. I så fall, gå tillbaka till katalogen webbappar att se om den uppackade JSPHello katalogen har skapats. Om du inte kan se dessa objekt vänta och upprepa.

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a>Publicera programmet med hjälp av FileZilla (valfritt)
Ett annat verktyg som du kan använda för att publicera programmet är FileZilla, en populär från tredje part FTP-klient med en lämplig, grafiskt användargränssnitt. Du kan hämta och installera FileZilla från [http://filezilla-project.org/](http://filezilla-project.org/) om du inte redan har det. Mer information om hur du använder klienten finns i [FileZilla dokumentationen](https://wiki.filezilla-project.org/Documentation) och det här blogginlägget på [FTP-klienter - del 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Klicka på FileZilla, **fil > Platshanteraren**.
2. I den **Platshanteraren** dialogrutan klickar du på **ny plats**. En ny tom FTP-plats visas i **Välj posten** där du uppmanas att ange ett namn. Den här proceduren namnet `AzureWebDemo-FTP`.
   
    På den **allmänna** anger du följande inställningar:
   
   * **Värden:** ange den **värdnamn för FTP-** som du kopierade från instrumentpanelen.
   * **Port:** (lämna det tomt eftersom det här är en passiv överföring och servern avgör vilken port som används.)
   * **Protokoll:** FTP File Transfer Protocol
   * **Kryptering:** använder vanlig FTP
   * **Inloggningstyp:** Normal
   * **Användare:** ange distributionen / FTP-användare som du kopierade från instrumentpanelen. Detta är fullständig FTP-användarnamnet som har formatet *webappname\username*.
   * **Lösenord:** ange lösenordet som du angav när du anger autentiseringsuppgifter för distribution.
     
     På den **Överföringsinställningar** väljer **passiva**.
3. Klicka på **Anslut**. Om lyckas Filezillas konsolen visas en `Status: Connected` meddelande och utfärda en `LIST` kommando för att visa kataloginnehållet.
4. I den **lokala** plats Kontrollpanelen, Välj den katalog där filen JSPHello.war finns; sökvägen ska vara liknar följande:
   
    `<project-path>/JSPHello/src/`
5. I den **Remote** plats Kontrollpanelen, Välj målmappen. Du ska distribuera WAR-filen till den `webapps` katalog under roten för den webbapp. Gå till `/site/wwwroot`, högerklicka på `wwwroot`, och välj **skapa directory**. Namnge katalogen `webapps` och ange den katalogen.
6. Överför JSPHello.war till `/site/wwwroot/webapps`. Välj JSPHello.war i den **lokala** filen listan högerklickar du på den och väljer **överför**. Du bör se den visas i `/site/wwwroot/webapps`.
7. När du har kopierat JSPHello.war till katalogen webbappar Tomcat Server kommer automatiskt att packa upp (packa) filer i WAR-filen. Även om Tomcat servern börjar uppackning nästan omedelbart, kan det ta lång tid (möjligen timmar) för filerna som ska visas i FTP-klienten.

#### <a name="run-the-hello-world-application-on-the-web-app"></a>Kör programmet Hello World i Webbappen
1. När du har överfört WAR-filen och verifiera att Tomcat-server har skapat en uppackade `JSPHello` directory, bläddra till `http://webdemowebapp.azurewebsites.net/JSPHello` att köra programmet.
   
   > **Obs:** om du klickar på **Bläddra** från den klassiska portalen kan du få standard-webbsidan säger ”Java-baserad webbprogrammet har skapats”. Du kan behöva uppdatera webbsida för att kunna visa programinnehåll i stället för standard-webbsidan.
   > 
   > 
2. När programmet körs, bör du se en webbsida med följande utdata:
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a>Rensa Azure-resurser
Den här proceduren skapar en webbapp i Apptjänst. Du kommer att debiteras för resursen som den finns. Om du planerar att fortsätta att använda webbprogrammet för testning och utveckling, bör du stoppa eller ta bort den. Ett webbprogram som har stoppats medför fortfarande en liten kostnad, men du kan starta om den när som helst. Om du tar bort ett webbprogram raderas alla data som du har laddat upp till den.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

[1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
[2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
[3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
[4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
[5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
[6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
[7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
[8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
[9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
[10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png


[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure classic portal]: https://manage.windowsazure.com
[What is an Azure AD directory]: http://technet.microsoft.com/library/jj573650.aspx
[Create and Upload a Management Certificate for Azure]: ../cloud-services/cloud-services-certs-create.md
[Key and Certificate Management Tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
