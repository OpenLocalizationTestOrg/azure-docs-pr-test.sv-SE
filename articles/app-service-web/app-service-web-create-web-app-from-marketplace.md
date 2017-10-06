---
title: "aaaCreate en webbapp från hello Azure Marketplace | Microsoft Docs"
description: "Lär dig hur toocreate från hello Azure Marketplace med hjälp av en ny WordPress-webbapp hello Azure-portalen."
services: app-service\web
documentationcenter: 
author: sunbuild
manager: erikre
editor: 
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: sunbuild
ms.custom: mvc
ms.openlocfilehash: 5ad1ca2f3f7831d857c3e9b02738b6b34acf3649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-from-hello-azure-marketplace"></a>Skapa en webbapp från hello Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

hello Azure Marketplace innehåller en mängd olika populära webbappar som har utvecklats av öppen källkod programvaruföretag, till exempel WordPress och Umbraco CMS. I kursen får du lära dig hur toocreate WordPress-appen från Azure marketplace.
som skapar en Azure Web App och MySQL-databas. 

![Exempel WordPress web appinstrumentpanelen](./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png)

## <a name="before-you-begin"></a>Innan du börjar 

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="deploy-from-azure-marketplace"></a>Distribuera från Azure Marketplace
Åtgärderna hello nedan toodeploy WordPress från Azure Marketplace.

### <a name="sign-in-tooazure"></a>Logga in tooAzure
Logga in toohello [Azure-portalen](https://portal.azure.com).

### <a name="deploy-wordpress-template"></a>Distribuera WordPress-mallen
hello Azure Marketplace innehåller mallar för att skapa resurser, installationsprogrammet hello [WordPress](https://portal.azure.com/#create/WordPress.WordPress) mallen tooget igång.
   
Ange hello följande information toodeploy hello WordPress-appen och dess resurser.

  ![Skapar du WordPress](./media/app-service-web-create-web-app-from-marketplace/wordpress-portal-create.png)


| Fält         | Föreslaget värde           | Beskrivning  |
| ------------- |-------------------------|-------------|
| Appnamn      | mywordpressapp          | Ange ett unikt namn för din **Webbprogramnamnet**. Det här namnet används som en del av hello standard DNS-namn för din app `<app_name>.azurewebsites.net`, så den behöver toobe unikt över alla program i Azure. Du kan senare mappa en anpassad domän namn tooyour app innan exponera tooyour användare |
| Prenumeration  | Betala per användning             | Välj en **prenumeration**. Om du har flera prenumerationer, Välj hello lämpliga prenumeration. |
| Resursgrupp| mywordpressappgroup                 |    Ange en **resursgruppen**. En resursgrupp är en logisk behållare i vilka Azure-resurser som webbappar, databaser som distribueras och hanteras. Du kan skapa en resursgrupp eller använda en befintlig |
| App Service-plan | myappplan          | Programtjänstplaner representerar hello mängd fysiska resurser som används toohost dina appar. Välj hello **plats** och hello **prisnivå**. Mer information om priser finns [prisnivå](https://azure.microsoft.com/pricing/details/app-service/) |
| Databas      | mywordpressapp          | Välj hello lämpliga databasprovidern för MySQL. Web Apps stöder **ClearDB**, **Azure-databas för MySQL** och **MySQL app**. Mer information finns i [databaskonfiguration](#database-config) nedan. |
| Application Insights | ON eller OFF          | Det här är valfritt. [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) tillhandahåller övervakningstjänster för webbappen genom att klicka på **på**.|

<a name="database-config"></a>

### <a name="database-configuration"></a>Konfiguration av databas
Åtgärderna hello nedan baserat på ditt val av MySQL databasprovidern.  Vi rekommenderar att både Web App och MySQL-databasen är i hello samma plats.

#### <a name="cleardb"></a>ClearDB 
[ClearDB](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview) är en fristående lösning för en helt integrerad tjänst MySQL på Azure. I ordning toouse ClearDB databaser, behöver du tooassociate ett kreditkort tooyour [Azure-konto](http://account.windowsazure.com/subscriptions). Om du har valt ClearDB databasprovidern kan du visa en lista över befintliga databaser toochoose från eller klicka på **Skapa nytt** knappen toocreate en databas.

![Skapa ClearDB](./media/app-service-web-create-web-app-from-marketplace/mysqldbcreate.png)

#### <a name="azure-database-for-mysql-preview"></a>Azure-databas för MySQL (förhandsgranskning)
[Azure-databas för MySQL](https://azure.microsoft.com/en-us/services/mysql) innehåller en hanterad databastjänst för apputveckling och distribution som gör att du toostand upp en MySQL-databas i minuter och skala på hello föra på hello moln som du litar på mest. Med sammanlagt prisnivå modeller, får du alla hello-funktioner som du vill ha hög tillgänglighet, säkerhet och återställning – inbyggt, utan extra kostnad. Klicka på **prisnivå** toochoose en annan [prisnivån](https://azure.microsoft.com/pricing/details/mysql). toouse en befintlig databas eller befintlig MySQL-servern använder en befintlig resursgrupp i vilka hello-servern finns. 

![Konfigurera hello databasinställningarna för hello-webbprogram](./media/app-service-web-create-web-app-from-marketplace/wordpress-azure-database.PNG)

> [!NOTE]
>  Azure-databas för MySQL (förhandsversion) och Web App på Linux (förhandsversion) är inte tillgängliga i alla regioner. Mer om toolearn [Azure-databas för MySQL (förhandsgranskning)](https://docs.microsoft.com/en-us/azure/mysql) och [webbprogrammet på Linux](./app-service-linux-intro.md) begränsningar. 

#### <a name="mysql-in-app"></a>MySQL i appen
[MySQL app](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app) är en funktion i App Service som gör körs MySql internt på hello plattform. hello grundläggande funktioner som stöds med hello-versionen av hello funktionen:

- MySQL-server som körs på samma instans sida vid sida med webbservern som värd för webbplatsen hello hello. Detta ökar prestandan för din app.
- Lagringen delas mellan både MySQL och filerna web app. Observera med kostnadsfria och den delade planer som du kan träffa på vår kvotgränser när du använder hello plats baserat på hello åtgärder du utför. Checka ut [kvotgränsen](https://azure.microsoft.com/en-us/pricing/details/app-service/plans/) för kostnadsfria och den delade planer.
- Du kan aktivera loggning av långsam frågor och allmänna loggning för MySQL. Observera att detta kan påverka hello platsprestanda och bör inte alltid är aktiverat. hello loggningsfunktionen hjälper undersöker eventuella problem i programmet. 

Mer information finns på detta [artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/08/18/announcing-mysql-in-app-preview-for-web-apps/ )

![MySQL-apphantering](./media/app-service-web-create-web-app-from-marketplace/mysqlinappmanage.PNG)

Du kan titta på hello förloppet genom att klicka på klockikonen för hello hello överst i hello Portalsida när hello WordPress-appen distribueras.    
![Förloppsindikator](./media/app-service-web-create-web-app-from-marketplace/deploy-success.png)

## <a name="manage-your-new-azure-web-app"></a>Hantera din nya Azure-webbapp

Gå toohello Azure portal tootake en titt på hello webbprogram som du nyss skapade.

toodo, logga in för[https://portal.azure.com](https://portal.azure.com).

Hello vänstra menyn klickar du på **Apptjänster**, klicka på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-list.png)


Nu visas webbappens _blad_ (en portalsida som öppnas vågrätt).

Som standard visas din webbapps blad hello **översikt** sidan. På den här sidan får du en översikt över hur det går för appen. Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. hello flikar hello vänster på hello bladet visar hello annan konfigurationssidor som du kan öppna.

![App Service-blad på Azure Portal](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-detail.png)

Flikarna i hello bladet innehåller hello många bra funktioner som du kan lägga till tooyour webbprogram. hello följande lista innehåller några av hello möjligheter:

* Mappa ett anpassat DNS-namn
* Bind ett anpassat SSL-certifikat
* Konfigurera kontinuerlig distribution
* Skala upp
* Lägg till användarautentisering

Slutför hello 5 minuter WordPress installationen guiden toohave WordPress-appen igång. Checka ut [Wordpress dokumentationen](https://codex.WordPress.org/) toodevelop ditt webbprogram.

![Installationsguiden för WordPress](./media/app-service-web-create-web-app-from-marketplace/wplanguage.png)

## <a name="configuring-your-app"></a>Konfigurera appen 
Det finns flera steg ingår i att hantera din WordPress-appen innan den är klar för produktion. Följ dessa steg tooconfigure och hantera WordPress-appen:

| toodo detta... | Använder du det här … |
| --- | --- |
| **Ladda upp eller lagra stora filer** |[WordPress-plugin-programmet för att använda Blob storage](https://wordpress.org/plugins/windows-azure-storage/)|
| **Skicka e-post** |Köp [SendGrid](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SendGrid.SendGrid?tab=Overview) e-tjänsten och använda hello [WordPress-plugin-programmet för att använda SendGrid](https://wordpress.org/plugins/sendgrid-email-delivery-simplified/) tooconfigure den|
| **Anpassade domännamn** |[Konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md) |
| **HTTPS** |[Aktivera HTTPS för en webbapp i Azure App Service](app-service-web-tutorial-custom-ssl.md) |
| **Förproduktion verifiering** |[Ställ in mellanlagrings- och dev miljöer för web apps i Azure App Service](web-sites-staged-publishing.md)|
| **Övervakning och felsökning** |[Aktivera diagnostikloggning för web apps i Azure App Service](web-sites-enable-diagnostic-log.md) och [övervakaren Web Apps i Azure App Service](app-service-web-tutorial-monitoring.md) |
| **Distribuera din webbplats** |[Distribuera en webbapp i Azure App Service](app-service-deploy-local-git.md) |


## <a name="secure-your-app"></a>Skydda din app 
Det finns flera steg ingår i att hantera din WordPress-appen innan den är klar för produktion. Följ dessa steg tooconfigure och hantera WordPress-appen:

| toodo detta... | Använder du det här … |
| --- | --- |
| **Stark användarnamn och lösenord**|  Ändra lösenord ofta. Vill inte använda vanliga användarnamn *admin* eller *wordpress* osv. Tvinga alla WordPress användare toouse unikt användarnamn och lösenord. |
| **Hålla dig uppdaterad** | Behåll din WordPress-core, teman, plugin-program in toodate. Använd hello senaste PHP-körning tillgängligt i Azure App service |
| **Uppdatera nycklarna för WordPress-säkerhet** | Uppdatera [WordPress säkerhetsnyckeln](https://codex.wordpress.org/Editing_wp-config.php#Security_Keys) tooimprove kryptering lagras i cookies|

## <a name="improve-performance"></a>Förbättra prestanda
Prestanda i hello molnet uppnås främst via cachelagring och skalbara. Dock bör hello minne, bandbredd och andra attribut för värd för Web Apps övervägas.

| toodo detta... | Använder du det här … |
| --- | --- |
| **Förstå funktioner för App Service-instans** |[Prisinformation, inklusive funktionerna i appen tjänstnivåer](https://azure.microsoft.com/en-us/pricing/details/app-service/)|
| **Cache-resurser** |Använd [Azure Redis-cache](https://azure.microsoft.com/en-us/services/cache/), eller en av hello andra cachelagring erbjudanden i hello [Azure Store](https://azuremarketplace.microsoft.com) |
| **Skala ditt program** |Du behöver tooscale [hello webbapp i Azure App Service](web-sites-scale.md) och/eller MySQL-databas. MySQL i appen inte stöd för skalbar, därför väljer ClearDB eller Azure-databas för MySQL (förhandsversion). [Skala Azure-databas för MySQL (förhandsgranskning)](https://azure.microsoft.com/en-us/pricing/details/mysql/) eller om du använder [ClearDB hög tillgänglighet routning](http://w2.cleardb.net/faqs/) tooscale upp databasen |

## <a name="availability-and-disaster-recovery"></a>Tillgänglighet och katastrofåterställning
Hög tillgänglighet innehåller hello aspekt av disaster recovery toomaintain verksamhetskontinuitet. Planera för fel och katastrofer i hello moln måste du toorecognize hello fel snabbt. De här lösningarna kan genomföra en strategi för hög tillgänglighet.

| toodo detta... | Använder du det här … |
| --- | --- |
| **Läsa in saldo platser** eller **geo-distribuera webbplatser** |[Vidarebefordra trafik med Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/) |
| **Säkerhetskopiera och återställa** |[Säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md) och [återställa en webbapp i Azure App Service](web-sites-restore.md) |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om olika funktioner i [Apptjänst toodevelop och skala](/app-service-web/).
