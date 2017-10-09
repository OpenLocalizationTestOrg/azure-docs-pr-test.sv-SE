---
title: "aaaMigrate en enterprise web app tooAzure Apptjänst"
description: Visar hur toouse Web Apps migrering Assistant tooquickly migrera befintliga IIS-webbplatser tooAzure App Service Web Apps
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: 
ms.assetid: 2e846fc0-37cc-42e6-ac57-ff442ef16e85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 7d66c5b799f0eefe85cbd9ba596ee0a05167f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-enterprise-web-app-tooazure-app-service"></a>Migrera en enterprise web app tooAzure Apptjänst
Du kan enkelt migrera dina befintliga webbplatser som körs på Internet Information Service (IIS) 6 eller senare för[App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

> [!IMPORTANT]
> Windows Server 2003 har nått slutet av stödet juli 14 2015. Om du för närvarande är värd för dina webbplatser på en IIS-server som är Windows Server 2003, är Web Apps låg risk, låg kostnad och låg friktion tookeep dina webbplatser och Web Apps migrering Assistant hjälper automatisera hello migreringsprocessen för dig. 
> 
> 

[Web Apps Migration Assistant](https://www.movemetothecloud.net/) kan analysera din installation av IIS-server, identifiera vilka platser kan migrerade tooApp Service, markera alla element som inte kan migreras eller stöds inte för hello plattform och sedan migrera dina webbplatser och associerade databaserna tooAzure.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="elements-verified-during-compatibility-analysis"></a>Element som verifieras vid analys av kompatibilitet
hello Migration Assistant skapar en beredskap rapporten tooidentify eventuella leder till problem eller allvarliga problem som kan förhindra en lyckad migrering från lokala IIS tooAzure App Service Web Apps. Vissa av hello huvudpunkter toobe medveten om är:

* Port bindningar – Web Apps stöder bara Port 80 för HTTP och Port 443 för HTTPS-trafik. Olika portkonfigurationer ignoreras och trafiken kommer att dirigeras too80 eller 443. 
* Autentisering – Web Apps stöder anonym autentisering som standard och formulär för autentisering där det anges av ett program. Windows-autentisering kan användas genom att integrera med Azure Active Directory och AD FS endast. Alla andra former av autentisering – till exempel grundläggande autentisering - stöds inte för närvarande. 
* Global Assembly Cache (GAC) – hello GAC inte stöds i Web Apps. Om ditt program referenser till sammansättningar som du normalt distribuerar toohello GAC, måste toodeploy toohello programmet bin-mappen i Web Apps. 
* IIS5-Kompatibelt läge – detta stöds inte i Web Apps. 
* Programpooler – i Web Apps, varje plats och dess underordnade program körs i hello samma programpool. Om din webbplats har flera underordnade program som använder flera programpooler, konsolidera dem tooa enda programpool med standardinställningarna eller migrera varje program tooa separat app.
* COM-komponenter – Web Apps tillåter inte hello registrering av COM-komponenter på hello-plattformen. Om dina webbplatser och program genom att utnyttja alla COM-komponenter, måste du skriva om dem i förvaltad kod och distribuerar dem med hello webbplatsen eller programmet.
* ISAPI-tillägg – Web Apps kan stödja hello användning av ISAPI-tillägg. Du behöver toodo hello följande:
  
  * distribuera hello DLL-filer med ditt webbprogram 
  * registrera hello DLL-filer med hjälp av [Web.config](http://www.iis.net/configreference/system.webserver/isapifilters)
  * Placera en applicationHost.xdt-fil i hello platsens rot med hello innehåll som beskrivs i ”så att arbitrart ISAPI-tillägg toobe inlästa” [i den här artikeln](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples) 
    
  
    
    Fler exempel på hur toouse omvandlingar för XML-dokumentet på din webbplats kan se [transformera webbplatsen Microsoft Azure](http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx).
* Andra komponenter som SharePoint, framsidan Servertillägg (Servertillägg för Frontpage), FTP, migreras inte SSL-certifikat.

## <a name="how-toouse-hello-web-apps-migration-assistant"></a>Hur toouse hello Web Apps migrering Assistant
Det här avsnittet går igenom ett exempel tootoomigrate några webbplatser som använder en SQL Server-databas och körs på en lokal dator för Windows Server 2003 R2 (IIS 6.0):

1. Hello IIS-servern eller klientdatorn gå för[https://www.movemetothecloud.net/](https://www.movemetothecloud.net/) 
   
   ![](./media/web-sites-migration-from-iis-server/migration-tool-homepage.png)
2. Installera Web Apps migrering Assistant genom att klicka på hello **dedikerad IIS-servern** knappen. Fler alternativ kommer att alternativen i hello nära framtid. 
3. Klicka på hello **installera verktyget** knappen tooinstall Web Apps migrering assistenten på din dator.
   
   ![](./media/web-sites-migration-from-iis-server/install-page.png)
   
   > [!NOTE]
   > Du kan också klicka på **hämta offline installation** toodownload en ZIP-fil för att installera på servrar inte ansluten toohello internet. Du kan också klicka på **överför en befintlig migreringen är klar rapport**, vilket är ett avancerat alternativ toowork med en befintlig migreringen är klar rapport som du skapade tidigare (beskrivs senare).
   > 
   > 
4. I hello **programmet installeras** klickar du på **installera** tooinstall på din dator. Den installeras också motsvarande beroenden webbdistribution, DacFX och IIS, om det behövs. 
   
   ![](./media/web-sites-migration-from-iis-server/install-progress.png)
   
   När du har installerat startar automatiskt Web Apps migrering Assistant.
5. Välj **migrera platser och databaser från en fjärrserver tooAzure**. Ange administrativa autentiseringsuppgifter för hello hello fjärrservern och klicka på **Fortsätt**. 
   
   ![](./media/web-sites-migration-from-iis-server/migrate-from-remote.png)
   
   Du kan självklart välja toomigrate från lokala hello-servern. hello remote alternativet är användbart när du vill toomigrate webbplatser från en webbserver i IIS.
   
   Nu hello Migreringsverktyget ska granska hello IIS-servern konfigurering, till exempel platser, program, programpooler och beroenden tooidentify kandidat webbplatser för migrering. 
6. hello skärmbilden nedan visar tre webbplatser – **standardwebbplats**, **TimeTracker**, och **CommerceNet4**. Alla har en associerad databas som vi vill toomigrate. Välj alla hello platser du vill tooassess och klicka sedan på **nästa**.
   
   ![](./media/web-sites-migration-from-iis-server/select-migration-candidates.png)
7. Klicka på **överför** tooupload hello beredskapsrapporten. Om du klickar på **spara filen lokalt**, du kan köra hello Migreringsverktyget igen senare och överför hello sparat beredskapsrapporten som tidigare nämnts.
   
   ![](./media/web-sites-migration-from-iis-server/upload-readiness-report.png)
   
   När du överför hello beredskapsrapporten utför Azure beredskap för analys och visar du hello resultat. Läs hello assessment information för varje webbplats och se till att du förstår eller att du har löst alla problem innan du fortsätter. 
   
   ![](./media/web-sites-migration-from-iis-server/readiness-assessment.png)
8. Klicka på **börjar migrering** toostart hello migrering. Du kommer nu att omdirigerade tooAzure toolog till ditt konto. Det är viktigt att du loggar in med ett konto som har en aktiv Azure-prenumeration. Om du inte har ett Azure-konto så att du kan registrera dig för en kostnadsfri utvärderingsversion [här](https://azure.microsoft.com/pricing/free-trial/?WT.srch=1&WT.mc_ID=SEM_). 
9. Välj hello innehavarkonto, Azure-prenumeration och region toouse för migrerade Azure-webbappar och databaser och klicka sedan på **starta migreringen**. Du kan välja hello webbplatser toomigrate senare.
   
   ![](./media/web-sites-migration-from-iis-server/choose-tenant-account.png)
10. På nästa skärm för hello du ändra toohello för migrering standardinställningarna som:
    
    * Använd en befintlig Azure SQL-databas eller skapa en ny Azure SQL Database och konfigurera autentiseringsuppgifterna
    * Välj hello webbplatser toomigrate
    * Definiera namn för hello Azure-webbappar och deras länkade SQL-databaser
    * Anpassa hello globala inställningar och inställningar på webbplatsnivå
    
    hello skärmbilden nedan visar alla hello webbplatser som valts för migrering med hello standardinställningar.
    
    ![](./media/web-sites-migration-from-iis-server/migration-settings.png)
    
    > [!NOTE]
    > Hej **aktivera Azure Active Directory** kryssrutan i anpassade inställningar integrerar hello Azure-webbapp med [Azure Active Directory](../active-directory/active-directory-whatis.md) (hello **standardkatalog**). Mer information om synkronisering av Azure Active Directory med din lokala Active Directory finns [katalogintegrering](http://msdn.microsoft.com/library/jj573653).
    > 
    > 
11. När du ändrar alla hello som önskas, klickar du på **skapa** toostart hello migreringsprocessen. hello Migreringsverktyget skapa hello Azure SQL Database och Azure webbapp och sedan publicera hello webbplatsens innehåll och databaser. hello migreringens förlopp framgår tydligt hello Migreringsverktyget och du ser översiktsskärm hello slutpunkt, vilka information hello platser migreras, om de lyckades, länkar toohello nyskapade Azure-webbappar. 
    
    Om något fel uppstår under migreringen, hello migrering verktyget kommer tydligt ange hello fel och återställning hello ändringar. Du kan också kan toosend hello felrapporten direkt toohello engineering team genom att klicka på hello **skicka felrapporten** knappen med hello avbildade fel anropsstack och skapa meddelandetexten. 
    
    ![](./media/web-sites-migration-from-iis-server/migration-error-report.png)
    
    Om migrera lyckas utan fel kan du också klicka på hello **ge Feedback** knappen tooprovide feedback direkt. 
12. Klicka på hello länkar toohello Azure-webbappar och verifiera att hello migreringen har slutförts.
13. Nu kan du hantera hello migreras web apps i Azure App Service. toodo detta, logga in på hello [Azure Portal](https://portal.azure.com).
14. Öppna hello Web Apps bladet toosee migrerade webbplatser (visas som webbprogram) i hello Azure-portalen, och sedan klicka på någon av dem toostart hantera hello webbprogram, till exempel konfigurera kontinuerlig publicera, hur du skapar säkerhetskopior, autoskalning, och övervaka användning eller prestanda.
    
    ![](./media/web-sites-migration-from-iis-server/TimeTrackerMigrated.png)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

