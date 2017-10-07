---
title: "aaaCreate en Webbapp i Azure App Service med hello Azure SDK för Java"
description: "Lär dig hur toocreate en Webbapp i Azure App Service via programmering med hello Azure SDK för Java."
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
ms.openlocfilehash: 42ba86b7fbb5668b3675198d0c5bb454525f706b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-hello-azure-sdk-for-java"></a>Skapa en Webbapp i Azure App Service med hello Azure SDK för Java
<!-- Azure Active Directory workflow is not yet available on hello Azure Portal -->

## <a name="overview"></a>Översikt
Den här genomgången visar hur toocreate ett Azure SDK för Java-program som skapar en Webbapp i [Azure App Service][Azure App Service], distribuera ett program tooit. Det består av två delar:

* Del 1 visar hur toobuild ett Java-program som skapar ett webbprogram.
* Del 2 visar hur toocreate en enkel JSP ”Hello World” program och sedan använda en FTP-klient toodeploy kod tooApp Service.

## <a name="prerequisites"></a>Krav
### <a name="software-installations"></a>Programinstallationer
Hej AzureWebDemo programkod i den här artikeln skrevs med Azure Java SDK 0.7.0, som du kan installera med hjälp av hello [installationsprogram för webbplattform] [ Web Platform Installer] (WebPI). Se också till att toouse hello senaste versionen av hello [Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]. När du har installerat hello SDK uppdatera hello beroenden i Eclipse-projektet genom att köra **uppdatera Index** i **Maven databaser**, sedan lägger till hello senaste versionen av varje paket i hello  **Beroenden** fönster. Du kan verifiera hello version av installerad programvara i Eclipse genom att klicka på **Hjälp > installationsinformationen**; du bör ha minst hello följande versioner:

* Paket för Microsoft Azure-biblioteken för Java 0.7.0.20150309
* Eclipse IDE för Java EE-utvecklare 4.4.2.20150219

### <a name="create-and-configure-cloud-resources-in-azure"></a>Skapa och konfigurera Molnresurser i Azure
Innan du börjar den här proceduren måste du behöver toohave en aktiv Azure-prenumeration och skapa en standard-Active Directory (AD) på Azure.

### <a name="create-an-active-directory-ad-in-azure"></a>Skapa en Active Directory (AD) i Azure
Om du inte redan har en Active Directory (AD) på din Azure-prenumeration logga in på hello [klassiska Azure-portalen] [ Azure classic portal] med ditt Microsoft-konto. Om du har flera prenumerationer, klickar du på **prenumerationer** och välj hello standardkatalogen hello prenumeration du vill toouse för det här projektet. Klicka på **tillämpa** tooswitch toothat prenumerationsvyn.

1. Välj **Active Directory** hello menyn till vänster. **Klicka på Ny > Directory > Skapa anpassade**.
2. I **Lägg till katalog**väljer **Skapa ny katalog**.
3. I **namn**, ange namnet på en katalog.
4. I **domän**, ange ett domännamn. Detta är ett grundläggande domännamn som ingår som standard med din katalog. den har hello formuläret `<domain_name>.onmicrosoft.com`. Du kan kalla det baserat på hello directory eller ett annat domännamn som du äger. Du kan senare lägga till ett annat domännamn som din organisation redan använder.
5. I **land eller region**, Välj ditt språk.

Mer information om AD finns [vad är Azure AD-katalog][What is an Azure AD directory]?

### <a name="create-a-management-certificate-for-azure"></a>Skapa ett Hanteringscertifikat för Azure
hello Azure SDK för Java använder management certifikat tooauthenticate med Azure-prenumerationer. Dessa är X.509 v3-certifikat som du använder tooauthenticate ett klientprogram som använder hello Service Management API tooact uppdrag hello ägare toomanage prenumeration prenumerationsresurser.

hello koden i den här proceduren använder ett självsignerat certifikat tooauthenticate med Azure. För den här proceduren ska du behöver toocreate ett certifikat och överföra den toohello [klassiska Azure-portalen] [ Azure classic portal] i förväg. Detta omfattar hello följande steg:

* Generera en PFX-fil som representerar klientcertifikatet och spara den lokalt.
* Skapa ett hanteringscertifikat (CER-fil) från hello PFX-filen.
* Överför hello CER-fil tooyour Azure-prenumeration.
* Konvertera hello PFX-filen till JKS, eftersom Java använder den format tooauthenticate med certifikat.
* Skriva hello autentisering programkoden, vilket refererar toohello lokal JKS fil.

När du slutför den här proceduren hello CER certifikatet ska placeras i din Azure-prenumeration och hello JKS certifikat kommer att finnas på en lokal hårddisk. Mer information om certifikat finns [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].

#### <a name="create-a-certificate"></a>Skapa ett certifikat
toocreate egna självsignerade certifikat, öppna kommandokonsolen i operativsystemet och kör följande kommandon hello.

> **Obs:** hello dator där du kör det här kommandot måste ha hello JDK installerad. Hello sökvägen toohello keytool beror också på hello plats där du installerar hello JDK. Mer information finns i [nyckeln och certifikatet Management Tool (keytool)] [ Key and Certificate Management Tool (keytool)] i hello Java online dokumenten.
> 
> 

toocreate hello .pfx-fil:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

toocreate hello .cer-fil:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Där:

* `<java-install-dir>`är hello sökvägen toohello katalog där du installerade Java.
* `<keystore-id>`Hej keystore post identifierare (till exempel `AzureRemoteAccess`).
* `<cert-store-dir>`är hello sökvägen toohello katalog där du vill att toostore certifikat (till exempel `C:/Certificates`).
* `<cert-file-name>`är hello namnet på hello certifikatfil (till exempel `AzureWebDemoCert`).
* `<password>`hello-lösenord som du väljer tooprotect hello; Det måste vara minst 6 tecken. Du kan ange inga lösenord, men detta inte rekommenderas.
* `<dname>`är hello X.500 huvudnamnet toobe som är associerade med alias och används som hello utfärdare och ämne fält i hello självsignerat certifikat.

Mer information finns i [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].

#### <a name="upload-hello-certificate"></a>Överför hello-certifikat
tooupload ett självsignerat certifikat tooAzure gå toohello **inställningar** på hello klassiska portalen och klicka sedan på hello **Hanteringscertifikat** fliken. Klicka på **överför** längst hello hello sidan och navigera toohello platsen för hello CER-fil som du skapade.

#### <a name="convert-hello-pfx-file-into-jks"></a>Konvertera hello PFX-filen till JKS
I hello Windows kommandotolk (Kör som administratör), cd toohello katalog som innehåller hello certifikat och köra hello följande kommando, där `<java-install-dir>` är hello katalog där du installerade Java på datorn:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. När du uppmanas, ange hello mål keystore lösenord. Detta kan vara hello hello JKS filens lösenord.
2. När du uppmanas, ange hello källa keystore lösenord. Detta är hello lösenordet du angav för hello PFX-filen.

hello två lösenord har inte toobe hello samma. Du kan ange inga lösenord, men detta inte rekommenderas.

## <a name="build-a-web-app-creation-application"></a>Skapa ett program för att skapa webbprogram
### <a name="create-hello-eclipse-workspace-and-maven-project"></a>Skapa hello Eclipse arbetsytan och Maven-projekt
I det här avsnittet skapar du en arbetsyta och ett Maven-projekt för hello appen att skapa webbprogrammet, med namnet AzureWebDemo.

1. Skapa ett nytt Maven-projekt. Klicka på **Arkiv > Nytt > Maven-projekt**. I **nya Maven-projekt**väljer **skapa ett enkelt projekt** och **använda standardplatsen för arbetsytan**.
2. På hello andra sidan i **nya Maven-projekt**, anger hello följande:
   
   * Grupp-ID:`com.<username>.azure.webdemo`
   * Artefakt-ID: AzureWebDemo
   * Version: 0.0.1-SNAPSHOT
   * Paketera: jar
   * Namn: AzureWebDemo
     
     Klicka på **Slutför**.
3. Öppna filen för pom.xml av hello nytt projekt i Projektutforskaren. Välj hello **beroenden** fliken. Eftersom det är ett nytt projekt visas inga paket ännu.
4. Öppna hello Maven databaser visa. **Klicka på fönstret > Visa > andra > Maven > Maven databaser** och på **OK**. Hej **Maven databaser** vyn visas längst ned hello hello IDE.
5. Öppna **globala databaser**, högerklicka på hello **centrala** databasen och välj **Rebuild Index**.
   
    ![][1]
   
    Det här steget kan ta flera minuter beroende på anslutningen hello hastighet. När hello index återskapar du bör se hello Microsoft Azure-paket i hello **centrala** Maven-databasen.
6. I **beroenden**, klickar du på **Lägg till**. I **Ange grupp-ID....**  ange `azure-management`. Välj hello-paket för hanteringspaketdistributionen och hantering av App Service Web Apps:
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > **Obs:** om du uppdaterar hello beroenden när en ny version-version, måste toore-Lägg till var och en av hello beroenden i den här listan.
   > När du klickar på **Lägg till** och markera varje beroende, visas det med hello nytt versionsnummer i hello **beroenden** lista.
   > 
   > 

Klicka på **OK**. hello Azure-paket och sedan visas i hello **beroenden** lista.

### <a name="writing-java-code-toocreate-a-web-app-by-calling-hello-azure-sdk"></a>Skriva Java-kod tooCreate en Webbapp som anropar hello Azure SDK
Skriv sedan hello-kod som anropar API: er i hello Azure SDK för Java toocreate hello App Service webbapp.

1. Skapa en Java-klass toocontain hello huvudsakliga post punkt kod. Högerklicka på projektnoden hello i Projektutforskaren, och välj **New > klassen**.
2. I **ny Java-klass**, namnge hello klassen `WebCreator` och kontrollera hello **offentliga statiska void main** kryssrutan. hello val ska visas på följande sätt:
   
    ![][2]
3. Klicka på **Slutför**. Hej WebCreator.java filen visas i Projektutforskaren.

### <a name="calling-hello-azure-api-toocreate-an-app-service-web-app"></a>Anropar hello Azure API tooCreate en App Service Web App
#### <a name="add-necessary-imports"></a>Lägga till nödvändiga importer
WebCreator.java, lägga till hello efter importen. importen ge åtkomst tooclasses i hello bibliotek för förbrukning av Azure API: er:

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


#### <a name="define-hello-main-entry-point-class"></a>Definiera hello huvudsakliga post punkt klass
Eftersom hello syftet med hello AzureWebDemo program är toocreate en App Service Web App name hello huvudsakliga klass för det här programmet `WebAppCreator`. Den här klassen innehåller hello huvudsakliga post punkt kod som anropar hello Azure Service Management API toocreate hello webbprogrammet.

Lägg till följande parameterdefinitioner för hello webbapp och webbutrymmet hello. Du behöver tooprovide din egen Azure-prenumeration-ID och certifikat information.

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

* `<subscription-id>`är hello Azure prenumerations-ID som du vill toocreate hello resurs.
* `<certificate-store-path>`är hello sökvägen och filnamnet toohello JKS fil i katalogen lagra lokala certifikatarkivet. Till exempel `C:/Certificates/CertificateName.jks` för Linux och `C:\Certificates\CertificateName.jks` för Windows.
* `<certificate-password>`är hello-lösenord som du angav när du skapade ditt JKS certifikat.
* `webAppName`kan vara vilket namn som du väljer; den här proceduren använder hello namn `WebDemoWebApp`. hello fullständigt domännamn är hello `webAppName` med hello `domainName` läggs, så i detta fall hello fullständigt domännamn är `webdemowebapp.azurewebsites.net`.
* `domainName`ska anges som ovan.
* `webSpaceName`ska vara någon av hello värden som definieras i hello [WebSpaceNames] [ WebSpaceNames] klass.
* `appServicePlanName`ska anges som ovan.

> **Obs:** varje gång du kör det här programmet måste du toochange hello värdet för `webAppName` och `appServicePlanName` (eller ta bort hello webbprogram på hello Azure Portal) innan du kör hello programmet igen. Annars misslyckas körningen eftersom hello samma resursen finns redan på Azure.
> 
> 

#### <a name="define-hello-web-creation-method"></a>Definiera hello metod för skapande av webbprogram
Sedan definiera en metod toocreate hello-webbapp. Den här metoden `createWebApp`, anger hello parametrar av hello webbapp och hello webbutrymmet. Dessutom skapar och konfigurerar hello App Service Web Apps management-klienten, som definieras av hello [WebSiteManagementClient] [ WebSiteManagementClient] objekt. hello Hanteringsklient är viktiga toocreating Web Apps. Det ger RESTful webbtjänster som möjliggör program toomanage webbprogram (utför åtgärder som skapar, uppdatera och ta bort) genom att anropa hello service management API.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for hello App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path toohello JKS file
            keyStorePassword,  // Password for hello JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create hello App Service Web Apps management client toocall Azure APIs
        // and pass it hello App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for hello web app with hello specified parameters.
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
        // Note that hello server farm name takes hello Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define hello web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create hello web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output hello HTTP status code of hello response; 200 indicates hello request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output hello name of hello web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

hello koden kommer utdata hello HTTP-status för hello-svar som visar lyckad eller misslyckad och om det lyckas, kommer att bli hello namnet på hello skapa webbprogram.

#### <a name="define-hello-main-method"></a>Definiera hello main() metod
Ange hello main() metoden kod som anropar createWebApp() toocreate hello webbprogrammet.

Slutligen anropa `createWebApp` från `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-hello-application-and-verify-web-app-creation"></a>Kör programmet hello och kontrollera att skapa en webbapp
tooverify som programmet körs, klickar du på **kör > kör**. När programmet hello har slutförts körs, bör du se hello följande utdata i hello Eclipse-konsolen:

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

Logga in på hello klassiska Azure-portalen och på **Web Apps**. hello nytt webbprogram ska visas i listan för hello Web Apps inom några minuter.

## <a name="deploying-an-application-toohello-web-app"></a>Distribuera ett program toohello Web App
När du har kört AzureWebDemo och skapat hello nytt webbprogram, logga in på hello klassiska portalen klickar du på **Web Apps**, och välj **WebDemoWebApp** i hello **Web Apps** lista. På hello webbapp instrumentpanelen klickar du på **Bläddra** (eller klicka på hello URL `webdemowebapp.azurewebsites.net`) toonavigate tooit. En tom platshållare-sida visas eftersom inget innehåll har publicerade toohello webbprogrammet ännu.

Nästa du skapar ett ”Hello World”-program och distribuera den toohello webbprogram.

### <a name="create-a-jsp-hello-world-application"></a>Skapa ett JSP Hello World-program
#### <a name="create-hello-application"></a>Skapa hello program
I ordning toodemonstrate hur toodeploy en toohello web programmet hello följande procedur beskrivs hur du toocreate ett enkelt ”Hello World” Java-program och ladda upp den toohello App Service Web App som skapats av ditt program.

1. Klicka på **Arkiv > Nytt > dynamiskt webbprojekt**. Ge den namnet `JSPHello`. Du behöver inte toochange andra inställningar i den här dialogrutan. Klicka på **Slutför**.
   
    ![][3]
2. Expandera hello i Projektutforskaren **JSPHello** projekt, högerklicka på **webbinnehåll**, klicka på **New > JSP-fil**. I dialogrutan för ny JSP-fil hello namn hello nya filen `index.jsp`. Klicka på **Nästa**.
3. I hello **Välj JSP-mall** väljer **ny JSP-fil (html)** och på **Slutför**.
4. Lägg till följande kod i hello hello i index.jsp, `<head>` och `<body>` tagga avsnitt:
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, hello time is <%= date %> 
        </body>

#### <a name="run-hello-hello-world-application-in-localhost"></a>Kör hello World Hello program i localhost
Innan du kör det här programmet måste tooconfigure några egenskaper.

1. Högerklicka på hello **JSPHello** projektet och välj **egenskaper**.
2. I hello **egenskaper** dialogrutan: Välj **Java byggsökväg**väljer hello **ordning och exportera** markerar **JRE systembibliotek**, klicka på **In** toomove den toohello överkant hello-listan.
   
    ![][4]
3. Även i hello **egenskaper** dialogrutan: Välj **Körningsmål** och på **ny**.
4. I hello **nya Server-körningsmiljön** dialogrutan, Välj en server som **Apache Tomcat v7.0** och på **nästa**. I hello **Tomcat Server** dialogrutan, ange **namn** för`Apache Tomcat v7.0`, och ange **Tomcat installationskatalog** toohello katalog där du installerade hello-version Tomcat-server som du vill toouse.
   
    ![][5]
   
    Klicka på **Slutför**.
5. Toohello returnerar **Körningsmål** sidan hello **egenskaper** dialogrutan. Välj **Apache Tomcat v7.0**, klicka på **OK**.
   
    ![][6]
6. I hello Eclipse **kör** -menyn klickar du på **kör**. I hello **kör som** markerar **körs på servern**. I hello **körs på servern** markerar **Tomcat v7.0-Server**:
   
    ![][7]
   
    Klicka på **Slutför**.
7. Hej när programmet körs, bör du se hello **JSPHello** visas sidan i ett fönster för localhost i Eclipse (`http://localhost:8080/JSPHello/`), visa hello följande meddelande:
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-hello-application-as-a-war"></a>Exportera hello program som en WAR-fil
Exportera hello web project-filer som en webb-arkivfil (WAR) så att du kan distribuera den toohello webbprogram. hello finns följande web project-filer i hello webbinnehåll mapp:

    META-INF
    WEB-INF
    index.jsp

1. Högerklicka på mappen för hello webbinnehåll och välj **exportera**.
2. I hello **exportera Välj** dialogrutan klickar du på **Web > WAR** filen och klicka sedan på **nästa**.
3. I hello **WAR-Export** dialogrutan Välj hello src katalog i hello aktuella projektet och inkludera hello namnet på hello WAR-filen hello slutet. Exempel:
   
    `<project-path>/JSPHello/src/JSPHello.war`

Mer information om hur du distribuerar WAR-filerna finns [lägga till en Java-program tooAzure App Service Web Apps](web-sites-java-add-app.md).

### <a name="deploying-hello-hello-world-application-using-ftp"></a>Distribuera hello Hello World program med hjälp av FTP
Markera en FTP-klient toopublish hello tredjepartsprogram. Den här proceduren beskrivs två alternativ: hello Kudu-konsolen som är inbyggda i Azure och FileZilla, ett populära verktyg med en lämplig, grafiskt användargränssnitt.

> **Obs:** hello Azure Toolkit för Eclipse stöder distribution toostorage konton och cloud services, men stöder inte distribution tooweb appar. Du kan distribuera toostorage konton och molntjänster med ett projekt för distribution av Azure enligt beskrivningen i [skapar en Hello World program för Azure i Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), men inte tooweb appar. Använda andra metoder, till exempel FTP eller GitHub tootransfer filer tooyour webb-app.
> 
> **Obs:** rekommenderar vi inte med FTP från Kommandotolken för Windows hello (hello FTP.EXE kommandoradsverktyg som medföljer Windows). FTP-klienter som använder active FTP, till exempel FTP.EXE, växlar ofta toowork över brandväggar. Aktiva FTP anger en intern nätverksbaserade adress, toowhich en FTP-servern misslyckas sannolikt tooconnect.
> 
> 

Mer information om distribution tooan App Service webbapp med FTP finns hello följande avsnitt:

* [Distribuera med hjälp av ett FTP-verktyg](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a>Ställ in autentiseringsuppgifter för distribution
Kontrollera att du har kört hello **AzureWebDemo** programmet toocreate ett webbprogram. Du överför filer toothis plats.

1. Logga in hello klassiska portal och klicka på **Web Apps**. Kontrollera att **WebDemoWebApp** visas i hello lista över webbappar och se till att den körs. Klicka på **WebDemoWebApp** tooopen dess **instrumentpanelen** sidan.
2. På hello **instrumentpanelen** sidan under **snabb i korthet**, klickar du på **ställa in dina autentiseringsuppgifter för distribution** (om du redan har autentiseringsuppgifter för distribution läser detta  **Återställa dina autentiseringsuppgifter för distribution**).
   
    Autentiseringsuppgifter för distribution är associerade med ett Microsoft-konto. Du behöver toospecify ett användarnamn och lösenord som du kan använda toodeploy med Git och FTP. Du kan använda dessa autentiseringsuppgifter toodeploy tooany webbprogram i alla Azure-prenumerationer som är kopplade till ditt Microsoft-konto. Ange autentiseringsuppgifter för Git och FTP-distribution i hello dialogrutan och registrera hello användarnamn och lösenord för framtida användning.

#### <a name="get-ftp-connection-information"></a>Hämta information om FTP-anslutning
toouse FTP toodeploy programmet filer toohello nyskapad webbapp måste tooobtain anslutningsinformationen. Det finns två sätt tooobtain anslutningsinformationen. Ett sätt är toovisit hello webbappens **instrumentpanelen** sidan; hello annat sätt är toodownload hello webbprogram Publicera profil. hello publiceringsprofil är en XML-fil som innehåller information som FTP-värden namn och inloggning autentiseringsuppgifter för dina webbappar i Azure App Service. Du kan använda det här användarnamnet och lösenordet toodeploy tooany webbprogrammet i alla prenumerationer som är kopplade till hello Azure-konto, inte bara det här en.

tooobtain FTP anslutningsinformation från hello webbapps blad i hello [Azure Portal][Azure Portal]:

1. Under **Essentials**, hitta och kopiera hello **värdnamn för FTP-**. Detta är en liknande URI för`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.
2. Under **Essentials**, hitta och kopiera **användarnamn för FTP-/ Distributionsanvändare**. Detta innebär att hello formuläret *webappname\deployment-username*, till exempel `WebDemoWebApp\deployer77`.

tooobtain FTP anslutningsinformation från hello Publicera profil:

1. I hello webbapps blad klickar du på **Get publiceringsprofil**. Detta kommer att ladda ned en .publishsettings-fil tooyour lokal enhet.
2. Öppna hello .publishsettings-fil i en XML-redigerare eller textredigerare och hitta hello `<publishProfile>` element som innehåller `publishMethod="FTP"`. Det bör se ut hello följande:
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. Observera att hello webbprogrammet `publishProfile` inställningar mappa toohello FileZilla Platshanteraren inställningarna enligt följande:

* `publishUrl`är hello samma som **värdnamn för FTP-**, hello värdet i **värden**.
* `publishMethod="FTP"`innebär att du ställer in **protokollet** för**FTP - File Transfer Protocol**, och **kryptering** för**använder vanlig FTP**.
* `userName`och `userPWD` är nycklarna för hello faktiska användarnamn och lösenord för värden som du angav när du återställer hello autentiseringsuppgifter för distribution. `userName`är hello samma som **distribution / FTP-användare**. Mappa för**användare** och **lösenord** i FileZilla.
* `ftpPassiveMode="True"`innebär att hello FTP-platsen använder passiv FTP-överföringen. Välj **passiva** på hello **Överföringsinställningar** fliken.

#### <a name="configure-hello-web-app-toohost-a-java-application"></a>Konfigurera hello webbprogrammet toohost ett Java-program
Innan du publicerar hello program behöver du toochange några konfigurationsinställningarna så att hello webbprogrammet kan vara värd för ett Java-program.

1. Hello klassiska portal, gå toohello webbappens **instrumentpanelen** och klickar på **konfigurera**. På hello **konfigurera** anger hello följande inställningar.
2. I **Java version** hello standardvärdet är **ut**; Välj hello Java version program-mål, till exempel 1.7.0_51. När du gör det också se till att **webbehållaren** anges tooa version av Tomcat-Server.
3. I **standarddokument**, lägga till index.jsp och flytta upp toohello överkant hello-listan. (hello standardfilen för webbprogram är hostingstart.html.)
4. Klicka på **Spara**.

#### <a name="publish-your-application-using-kudu"></a>Publicera programmet med hjälp av Kudu
Enkelriktade toopublish hello program är toouse hello Kudu debug konsol som är inbyggda i Azure. Kudu kallas toobe stabil och konsekvent med App Service Web Apps och Tomcat-Server. Du har åtkomst till hello konsolen för hello webbprogrammet genom att bläddra tooa URL för hello följande format:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. För den här proceduren finns hello Kudu-konsolen på hello följande URL-Adressen. Bläddra toothis plats:
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. Välj hello översta menyn **Felsökningskonsolen > CMD**.
3. På kommandoraden för hello-konsolen, gå för`/site/wwwroot` (eller klicka på `site`, sedan `wwwroot` hello directory vyn i hello överst på sidan för hello):
   
    `cd /site/wwwroot`
4. När du har angett **Java version**, Tomcat-server ska skapa en katalog för webbappar. Navigera toohello webbappar directory på kommandoraden för hello-konsolen:
   
    `mkdir webapps`
   
    `cd webapps`
5. Dra JSPHello.war från `<project-path>/JSPHello/src/` och släpp hello Kudu directory vyn under `/site/wwwroot/webapps`. Dra inte den toohello ”dra hit tooupload och zip” område eftersom Tomcat kommer packa upp den.
   
   ![][8]

Första JSPHello.war visas i hello directory området ensamt:

  ![][9]

Under en kort tid (förmodligen mindre än 5 minuter) kommer Tomcat Server packa hello WAR-filen till en uppackade JSPHello katalog. Klicka på hello rot directory toosee om index.jsp har uppackade och kopieras dit. I så fall, gå tillbaka toohello webbappar directory toosee om hello packat JSPHello katalogen har skapats. Om du inte kan se dessa objekt vänta och upprepa.

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a>Publicera programmet med hjälp av FileZilla (valfritt)
Ett annat verktyg som du kan använda toopublish hello program är FileZilla, en populär från tredje part FTP-klient med en lämplig, grafiskt användargränssnitt. Du kan hämta och installera FileZilla från [http://filezilla-project.org/](http://filezilla-project.org/) om du inte redan har det. Mer information om hur du använder hello klienten finns hello [FileZilla dokumentationen](https://wiki.filezilla-project.org/Documentation) och det här blogginlägget på [FTP-klienter - del 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Klicka på FileZilla, **fil > Platshanteraren**.
2. I hello **Platshanteraren** dialogrutan klickar du på **ny plats**. En ny tom FTP-plats visas i **Välj posten** uppmaning tooprovide ett namn. Den här proceduren namnet `AzureWebDemo-FTP`.
   
    På hello **allmänna** anger hello följande inställningar:
   
   * **Värden:** RETUR hello **värdnamn för FTP-** som du kopierade från hello instrumentpanelen.
   * **Port:** (lämna det tomt eftersom det här är en passiv överföring och hello servern avgör hello port toouse.)
   * **Protokoll:** FTP File Transfer Protocol
   * **Kryptering:** använder vanlig FTP
   * **Inloggningstyp:** Normal
   * **Användare:** RETUR hello distribution / FTP-användare som du kopierade från hello instrumentpanelen. Detta är hello fullständig FTP-användarnamn, som har hello formuläret *webappname\username*.
   * **Lösenord:** ange hello lösenord som du angav när du anger autentiseringsuppgifter för distribution av hello.
     
     På hello **Överföringsinställningar** väljer **passiva**.
3. Klicka på **Anslut**. Om lyckas Filezillas konsolen visas en `Status: Connected` meddelande och utfärda en `LIST` kommandot toolist hello kataloginnehållet.
4. I hello **lokala** plats Kontrollpanelen, Välj hello källkatalogen i vilken hello JSPHello.war fil finns; hello åtkomstsökvägen liknande toohello följande:
   
    `<project-path>/JSPHello/src/`
5. I hello **Remote** plats Kontrollpanelen, Välj hello målmappen. Du ska distribuera hello WAR-filen toohello `webapps` katalog under roten för hello webbapp. Navigera för`/site/wwwroot`, högerklicka på `wwwroot`, och välj **skapa directory**. Katalog över hello `webapps` och ange den katalogen.
6. Överför JSPHello.war för`/site/wwwroot/webapps`. Välj JSPHello.war i hello **lokala** filen listan högerklickar du på den och väljer **överför**. Du bör se den visas i `/site/wwwroot/webapps`.
7. När du har kopierat JSPHello.war toohello webbappar directory Tomcat Server kommer automatiskt att packa upp (packa) hello filer i hello WAR-filen. Även om Tomcat servern börjar uppackning nästan omedelbart, kan det ta lång tid (möjligen timmar) för hello filer tooappear hello FTP-klienten.

#### <a name="run-hello-hello-world-application-on-hello-web-app"></a>Kör hello World Hello på hello Web App
1. När du har överfört hello WAR-filen och verifiera att Tomcat-server har skapat en uppackade `JSPHello` directory, bläddra för`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello program.
   
   > **Obs:** om du klickar på **Bläddra** från hello klassiska portalen kan du få hello standard webbsidan säger ”Java-baserad webbprogrammet har skapats”. Du kanske toorefresh hello webbsidan i ordning tooview hello programmet utdata i stället för hello standard webbsidan.
   > 
   > 
2. När programmet hello körs, bör du se en webbsida med hello följande utdata:
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a>Rensa Azure-resurser
Den här proceduren skapar en webbapp i Apptjänst. Du kommer att debiteras för hello resurs så länge den finns. Om du planerar toocontinue med hello webbapp för testning och utveckling, bör du stoppa eller ta bort den. Ett webbprogram som har stoppats medför fortfarande en liten kostnad, men du kan starta om den när som helst. Om du tar bort ett webbprogram raderas alla data som du har laddat upp tooit.

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
