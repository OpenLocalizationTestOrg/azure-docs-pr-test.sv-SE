---
title: "Skapa en webbapp från Azure Marketplace | Microsoft Docs"
description: "Läs om hur du skapar en ny WordPress-webbapp från Azure Marketplace med hjälp av Azure Portal."
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
ms.openlocfilehash: 16951ac0fcc350b7176747a7ad4e0bc8e186ab17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-from-the-azure-marketplace"></a>Skapa en webbapp i Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace innehåller en mängd olika populära webbappar som har utvecklats av öppen källkod programvaruföretag, till exempel WordPress och Umbraco CMS. I kursen får lära du att skapa WordPress-appen från Azure marketplace.
som skapar en Azure Web App och MySQL-databas. 

![Exempel WordPress web appinstrumentpanelen](./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png)

## <a name="before-you-begin"></a>Innan du börjar 

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="deploy-from-azure-marketplace"></a>Distribuera från Azure Marketplace
Följ stegen nedan för att distribuera WordPress från Azure Marketplace.

### <a name="sign-in-to-azure"></a>Logga in på Azure
Logga in på [Azure-portalen](https://portal.azure.com).

### <a name="deploy-wordpress-template"></a>Distribuera WordPress-mallen
Azure Marketplace innehåller mallar för att ställa in resurser, konfigurera den [WordPress](https://portal.azure.com/#create/WordPress.WordPress) mall för att komma igång.
   
Ange följande information för att distribuera WordPress-appen och dess resurser.

  ![Skapar du WordPress](./media/app-service-web-create-web-app-from-marketplace/wordpress-portal-create.png)


| Fält         | Föreslaget värde           | Beskrivning  |
| ------------- |-------------------------|-------------|
| Appnamn      | mywordpressapp          | Ange ett unikt namn för din **Webbprogramnamnet**. Det här namnet används som en del av standard-DNS-namn för din app `<app_name>.azurewebsites.net`, så det måste vara unikt över alla program i Azure. Du kan senare mappa ett anpassat domännamn i appen innan du ansluter till dina användare |
| Prenumeration  | Betala per användning             | Välj en **prenumeration**. Om du har flera prenumerationer kan välja lämplig prenumerationen. |
| Resursgrupp| mywordpressappgroup                 |    Ange en **resursgruppen**. En resursgrupp är en logisk behållare i vilka Azure-resurser som webbappar, databaser som distribueras och hanteras. Du kan skapa en resursgrupp eller använda en befintlig |
| App Service-plan | myappplan          | Programtjänstplaner representerar mängd fysiska resurser som används som värd för dina appar. Välj den **plats** och **prisnivå**. Mer information om priser finns [prisnivå](https://azure.microsoft.com/pricing/details/app-service/) |
| Databas      | mywordpressapp          | Välj lämplig databasprovidern för MySQL. Web Apps stöder **ClearDB**, **Azure-databas för MySQL** och **MySQL app**. Mer information finns i [databaskonfiguration](#database-config) nedan. |
| Application Insights | ON eller OFF          | Det här är valfritt. [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) tillhandahåller övervakningstjänster för webbappen genom att klicka på **på**.|

<a name="database-config"></a>

### <a name="database-configuration"></a>Konfiguration av databas
Följ stegen nedan baserat på ditt val av MySQL databasprovidern.  Du rekommenderas att både Web App och MySQL-databasen finnas på samma plats.

#### <a name="cleardb"></a>ClearDB 
[ClearDB](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview) är en fristående lösning för en helt integrerad tjänst MySQL på Azure. För att kunna använda ClearDB databaser, måste du associera ett kreditkort din [Azure-konto](http://account.windowsazure.com/subscriptions). Om du har valt ClearDB databasprovidern kan du visa en lista över befintliga databaser att välja bland eller klicka på **Skapa nytt** för att skapa en databas.

![Skapa ClearDB](./media/app-service-web-create-web-app-from-marketplace/mysqldbcreate.png)

#### <a name="azure-database-for-mysql-preview"></a>Azure-databas för MySQL (förhandsgranskning)
[Azure-databas för MySQL](https://azure.microsoft.com/en-us/services/mysql) innehåller en hanterad databastjänst för apputveckling och distribution som gör att du håller en MySQL-databas i minuter och skala direkt på molnet som du litar på de flesta. Med sammanlagt prisnivå modeller, får du alla funktioner som du vill ha hög tillgänglighet, säkerhet och återställning – inbyggt, utan extra kostnad. Klicka på **prisnivå** att välja en annan [prisnivån](https://azure.microsoft.com/pricing/details/mysql). Om du vill använda en befintlig databas eller en befintlig MySQL-server, använder du en befintlig resursgrupp som servern finns. 

![Konfigurera databasinställningarna för webbappen](./media/app-service-web-create-web-app-from-marketplace/wordpress-azure-database.PNG)

> [!NOTE]
>  Azure-databas för MySQL (förhandsversion) och Web App på Linux (förhandsversion) är inte tillgängliga i alla regioner. Mer information om [Azure-databas för MySQL (förhandsgranskning)](https://docs.microsoft.com/en-us/azure/mysql) och [webbprogrammet på Linux](./app-service-linux-intro.md) begränsningar. 

#### <a name="mysql-in-app"></a>MySQL i appen
[MySQL app](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app) är en funktion i App Service som gör kör MySql internt på plattformen. Grundläggande funktioner som stöds med lanseringen av funktionen:

- MySQL-server som körs på samma instans sida vid sida med webbservern som värd för webbplatsen. Detta ökar prestandan för din app.
- Lagringen delas mellan både MySQL och filerna web app. Observera med kostnadsfria och den delade planer som du kan träffa på vår kvotgränser när du använder webbplatsen baserat på åtgärderna som du utför. Checka ut [kvotgränsen](https://azure.microsoft.com/en-us/pricing/details/app-service/plans/) för kostnadsfria och den delade planer.
- Du kan aktivera loggning av långsam frågor och allmänna loggning för MySQL. Observera att detta kan påverka prestanda för platser och bör inte alltid är aktiverat. Loggningsfunktionen hjälper undersöker eventuella problem i programmet. 

Mer information finns på detta [artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/08/18/announcing-mysql-in-app-preview-for-web-apps/ )

![MySQL-apphantering](./media/app-service-web-create-web-app-from-marketplace/mysqlinappmanage.PNG)

Du kan se förloppet genom att klicka på klockikonen överst på portalsidan medan WordPress-appen distribueras.    
![Förloppsindikator](./media/app-service-web-create-web-app-from-marketplace/deploy-success.png)

## <a name="manage-your-new-azure-web-app"></a>Hantera din nya Azure-webbapp

Gå till Azure Portal och titta på webbappen du nyss skapade.

Logga in på [https://portal.azure.com](https://portal.azure.com).

Klicka på **Apptjänster** på menyn till vänster och klicka sedan på namnet på din Azure-webbapp.

![Navigera till webbappen på Azure Portal](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-list.png)


Nu visas webbappens _blad_ (en portalsida som öppnas vågrätt).

Sidan **Översikt** visas som standard på webbappens blad. På den här sidan får du en översikt över hur det går för appen. Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. På flikarna till vänster på bladet kan du se olika konfigurationssidor som du kan öppna.

![App Service-blad på Azure Portal](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-detail.png)

Flikarna på bladet innehåller många bra funktioner som du kan lägga till i webbappen. I listan nedan kan du se några av möjligheterna:

* Mappa ett anpassat DNS-namn
* Bind ett anpassat SSL-certifikat
* Konfigurera kontinuerlig distribution
* Skala upp
* Lägg till användarautentisering

Slutför installationsguiden för WordPress-appen igång 5 minuter WordPress. Checka ut [Wordpress dokumentationen](https://codex.WordPress.org/) att utveckla ditt webbprogram.

![Installationsguiden för WordPress](./media/app-service-web-create-web-app-from-marketplace/wplanguage.png)

## <a name="configuring-your-app"></a>Konfigurera appen 
Det finns flera steg ingår i att hantera din WordPress-appen innan den är klar för produktion. Följ dessa steg för att konfigurera och hantera WordPress-appen:

| Om du vill göra det här … | Använder du det här … |
| --- | --- |
| **Ladda upp eller lagra stora filer** |[WordPress-plugin-programmet för att använda Blob storage](https://wordpress.org/plugins/windows-azure-storage/)|
| **Skicka e-post** |Köp [SendGrid](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SendGrid.SendGrid?tab=Overview) e-tjänsten och använder den [WordPress-plugin-programmet för att använda SendGrid](https://wordpress.org/plugins/sendgrid-email-delivery-simplified/) konfigureras|
| **Anpassade domännamn** |[Konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md) |
| **HTTPS** |[Aktivera HTTPS för en webbapp i Azure App Service](app-service-web-tutorial-custom-ssl.md) |
| **Förproduktion verifiering** |[Ställ in mellanlagrings- och dev miljöer för web apps i Azure App Service](web-sites-staged-publishing.md)|
| **Övervakning och felsökning** |[Aktivera diagnostikloggning för web apps i Azure App Service](web-sites-enable-diagnostic-log.md) och [övervakaren Web Apps i Azure App Service](app-service-web-tutorial-monitoring.md) |
| **Distribuera din webbplats** |[Distribuera en webbapp i Azure App Service](app-service-deploy-local-git.md) |


## <a name="secure-your-app"></a>Skydda din app 
Det finns flera steg ingår i att hantera din WordPress-appen innan den är klar för produktion. Följ dessa steg för att konfigurera och hantera WordPress-appen:

| Om du vill göra det här … | Använder du det här … |
| --- | --- |
| **Stark användarnamn och lösenord**|  Ändra lösenord ofta. Vill inte använda vanliga användarnamn *admin* eller *wordpress* osv. Tvinga alla WordPress användare att använda unika användarnamn och lösenord. |
| **Hålla dig uppdaterad** | Kontinuerligt din WordPress-core, teman, plugin-program. Använd den senaste PHP-körningen i Azure App service |
| **Uppdatera nycklarna för WordPress-säkerhet** | Uppdatera [WordPress säkerhetsnyckeln](https://codex.wordpress.org/Editing_wp-config.php#Security_Keys) att förbättra kryptering som lagras i cookies|

## <a name="improve-performance"></a>Förbättra prestanda
Prestanda i molnet uppnås främst via cachelagring och skalbara. Dock bör minnet, bandbredd och andra attribut för värd för Web Apps övervägas.

| Om du vill göra det här … | Använder du det här … |
| --- | --- |
| **Förstå funktioner för App Service-instans** |[Prisinformation, inklusive funktionerna i appen tjänstnivåer](https://azure.microsoft.com/en-us/pricing/details/app-service/)|
| **Cache-resurser** |Använd [Azure Redis-cache](https://azure.microsoft.com/en-us/services/cache/), eller en av de andra erbjudandena för cachelagring i den [Azure Store](https://azuremarketplace.microsoft.com) |
| **Skala ditt program** |Du behöver skala [webbappen i Azure App Service](web-sites-scale.md) och/eller MySQL-databas. MySQL i appen inte stöd för skalbar, därför väljer ClearDB eller Azure-databas för MySQL (förhandsversion). [Skala Azure-databas för MySQL (förhandsgranskning)](https://azure.microsoft.com/en-us/pricing/details/mysql/) eller om du använder [ClearDB hög tillgänglighet routning](http://w2.cleardb.net/faqs/) att skala upp din databas |

## <a name="availability-and-disaster-recovery"></a>Tillgänglighet och katastrofåterställning
Hög tillgänglighet innehåller aspekt för katastrofåterställning för att upprätthålla kontinuitet för företag. Planera för fel och katastrofer i molnet måste du identifiera felen snabbt. De här lösningarna kan genomföra en strategi för hög tillgänglighet.

| Om du vill göra det här … | Använder du det här … |
| --- | --- |
| **Läsa in saldo platser** eller **geo-distribuera webbplatser** |[Vidarebefordra trafik med Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/) |
| **Säkerhetskopiera och återställa** |[Säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md) och [återställa en webbapp i Azure App Service](web-sites-restore.md) |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om olika funktioner i [Apptjänst att utveckla och skala](/app-service-web/).