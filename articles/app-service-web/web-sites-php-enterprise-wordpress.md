---
title: aaaEnterprise-klass WordPress i Azure | Microsoft Docs
description: "Lär dig hur toohost en företagsanpassad WordPress webbplatser i Azure App Service"
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 22d68588-2511-4600-8527-c518fede8978
ms.service: app-service-web
ms.devlang: php
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
ms.openlocfilehash: 4347eddb31d622d1189dc5db4d81b0f3745d6e69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-class-wordpress-on-azure"></a>Företagsklass WordPress på Azure
Azure Apptjänst är en skalbar, säker och enkla att använda miljö för verksamhetskritiska, storskaliga [WordPress] [ wordpress] platser. Microsoft själva körs företagsklass platser, till exempel hello [Office] [ officeblog] och [Bing] [ bingblog] bloggar. Den här artikeln visar hur toouse hello Web Apps-funktionen i Microsoft Azure App Service tooestablish och underhålla en företagsanpassad, molnbaserade WordPress-webbplats som kan hantera ett stort antal besökare.

## <a name="architecture-and-planning"></a>Arkitektur och planering
En grundläggande WordPress-installation har bara två krav:

* **MySQL-databas**: det här kravet är tillgängliga via [ClearDB i hello Azure Marketplace][cdbnstore]. Alternativt kan du hantera egna MySQL-installation på virtuella datorer i Azure genom att använda antingen [Windows] [ mysqlwindows] eller [Linux][mysqllinux].

  > [!NOTE]
  > ClearDB innehåller flera MySQL-konfigurationer. Varje konfiguration har olika prestandaegenskaper. Se hello [Azure Store] [ cdbnstore] information om erbjudanden som tillhandahålls via hello Azure Store eller direkt som det visas på hello [ClearDB webbplats](http://www.cleardb.com/pricing.view).
  >
  >
* **PHP 5.2.4 eller större**: Azure App Service för närvarande tillhandahåller [versioner av PHP 5.4, 5.5 och 5.6][phpwebsite].

  > [!NOTE]
  > Vi rekommenderar att du alltid köra hello senaste versionen av PHP så att du har hello senaste säkerhetskorrigeringar.
  >
  >

### <a name="basic-deployment"></a>Enkel distribution
Om du använder bara hello grundläggande krav, kan du skapa en grundläggande lösning i en Azure-region.

![Ett Azure webbapp och MySQL-databas finns i en enda Azure region][basic-diagram]

Även om detta skulle kan du skapa flera Web Apps instanser av hello plats tooscale ut programmet, finns allt inom hello Datacenter på ett visst geografiskt område. Från utanför den här regionen som kan visas långa svarstider när de använder hello plats. Om hello datacenter i den här regionen kraschar ändras ditt program.

### <a name="multi-region-deployment"></a>Distribution av flera regioner
Med hjälp av Azure [Traffic Manager][trafficmanager], du kan skala din WordPress-sajt över flera geografiska regioner och ge hello samma URL för alla användare. Alla besökare kommer in via Traffic Manager och är routade tooa region som baseras på hello belastnings-utjämnande konfiguration.

![En Azure webbapp, som finns i flera områden med hög tillgänglighet för CDBR router tooroute tooMySQL över regioner][multi-region-diagram]

Hello WordPress-webbplatsen fortfarande att skala över flera Web Apps instanser inom varje region, men storleksändringen är särskilda tooa region. Hög trafik och låg trafik regioner kan skalas på olika sätt.

tooreplicate och väg trafik toomultiple MySQL-databaser, du kan använda [ClearDB hög tillgänglighet routrar (CDBRs)] [ cleardbscale] (visas vänstra) eller [MySQL klustret operatör klass Edition (CGE)] [cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Flera regioner distribution med Medialagring och cachelagring
Om hello plats tar emot överföringar eller värdar mediefiler, kan du använda Azure Blob storage. Om du behöver cachelagring av du [Redis-cache][rediscache], [Memcache moln](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/), eller en av hello andra cachelagring erbjudanden i hello [Azure Store](http://azure.microsoft.com/gallery/store/).

![En Azure webbapp, som finns i flera områden med hög tillgänglighet för CDBR router för MySQL med hanterade Cache, Blob storage och innehållsleveransnätverk][performance-diagram]

BLOB storage är fördelade över regioner som standard, så du inte behöver tooworry om replikering av filer på alla platser. Du kan också aktivera hello Azure [innehållsleveransnätverk] [ cdn] för Blob storage som distribuerar filer tooend noder som är närmare tooyour besökare.

### <a name="planning"></a>Planering
#### <a name="additional-requirements"></a>Ytterligare krav
| toodo detta... | Använder du det här … |
| --- | --- |
| **Ladda upp eller lagra stora filer** |[WordPress-plugin-programmet för att använda Blob storage][storageplugin] |
| **Skicka e-post** |[SendGrid] [ storesendgrid] och hello [WordPress-plugin-programmet för att använda SendGrid][sendgridplugin] |
| **Anpassade domännamn** |[Konfigurera ett anpassat domännamn i Azure App Service][customdomain] |
| **HTTPS** |[Aktivera HTTPS för en webbapp i Azure App Service][httpscustomdomain] |
| **Förproduktion verifiering** |[Skapa mellanlagringsmiljöer för web apps i Azure App Service][staging] <p>När du flyttar en webbapp från mellanlagring tooproduction kan flytta du även hello WordPress-konfiguration. Kontrollera att alla inställningar är uppdaterade toohello kraven för din produktionsprogrammet innan du flyttar hello mellanlagrad app tooproduction.</p> |
| **Övervakning och felsökning** |[Aktivera diagnostikloggning för web apps i Azure App Service] [ log] och [övervakaren Web Apps i Azure App Service][monitor] |
| **Distribuera din webbplats** |[Distribuera en webbapp i Azure App Service][deploy] |

#### <a name="availability-and-disaster-recovery"></a>Tillgänglighet och katastrofåterställning
| toodo detta... | Använder du det här … |
| --- | --- |
| **Läsa in saldo platser** eller **geo-distribuera webbplatser** |[Vidarebefordra trafik med Azure Traffic Manager][trafficmanager] |
| **Säkerhetskopiera och återställa** |[Säkerhetskopiera en webbapp i Azure App Service] [ backup] och [återställa en webbapp i Azure App Service][restore] |

#### <a name="performance"></a>Prestanda
Prestanda i hello molnet uppnås främst via cachelagring och skalbara. Dock bör hello minne, bandbredd och andra attribut för värd för Web Apps övervägas.

| toodo detta... | Använder du det här … |
| --- | --- |
| **Förstå funktioner för App Service-instans** |[Prisinformation, inklusive funktionerna i appen tjänstnivåer][websitepricing] |
| **Cache-resurser** |[Redis-cache][rediscache], [Memcache moln](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/), eller en av hello andra cachelagring erbjudanden i hello [Azure Store](/gallery/store/) |
| **Skala ditt program** |[Skala en webbapp i Azure App Service] [ websitescale] och [ClearDB hög tillgänglighet routning][cleardbscale]. Om du väljer toohost och hantera MySQL installationen, bör du [MySQL-kluster CGE] [ cge] för skalbar. |

#### <a name="migration"></a>Migrering
Det finns två metoder toomigrate en befintlig WordPress plats tooAzure Apptjänst:

* **[WordPress exportera][export]**: den här metoden exporterar hello innehållet i din blogg. Du kan sedan importera hello innehåll tooa ny WordPress-webbplats på Azure App Service med hjälp av hello [Importverktyget för WordPress-plugin-programmet][import].

  > [!NOTE]
  > När den här processen kan du migrera ditt innehåll, migrerar inte alla plugin-program, teman eller andra anpassningar. Du måste installera komponenterna igen manuellt.
  >
  >
* **Manuell migrering**: [säkerhetskopiera din webbplats] [ wordpressbackup] och [databasen][wordpressdbbackup], och sedan manuellt återställa den tooa webbapp i Azure App Service och associerade MySQL-databas. Den här metoden är användbar toomigrate hög anpassade webbplatser eftersom den förhindrar hello undvika manuellt installera plugin-program, teman och andra anpassningar.

## <a name="step-by-step-instructions"></a>Stegvisa instruktioner
### <a name="create-a-wordpress-site"></a>Skapa en WordPress-webbplats
1. Använd hello [Azure Marketplace] [ cdbnstore] toocreate en MySQL-databas med hello storlek som du identifierade i hello [arkitektur och planering](#planning) avsnitt i hello region eller regioner där du ska placera webbplatsen.
2. Gör så hello i [skapa en WordPress-webbapp i Azure App Service] [ createwordpress] toocreate WordPress-webbapp. När du skapar hello webbapp väljer **Använd en befintlig MySQL-databas**, och välj hello-databasen som du skapade i steg 1.

Om du migrerar en befintlig WordPress-webbplats, se [migrera en befintlig WordPress plats tooAzure](#Migrate-an-existing-WordPress-site-to-Azure) när du har skapat ett nytt webbprogram.

### <a name="migrate-an-existing-wordpress-site-tooazure"></a>Migrera en befintlig tooAzure för WordPress-webbplats
Som anges i hello [arkitektur och planering](#planning) avsnittet finns det två sätt toomigrate en WordPress-webbplats:

* **Använd exportera och importera** för platser som inte har mycket anpassning eller där du bara vill toomove hello innehåll.
* **Använd säkerhetskopiering och återställning** för platser som har mycket anpassning där du vill att toomove allt.

Använd någon av följande avsnitt toomigrate hello din plats.

#### <a name="hello-export-and-import-method"></a>Hej export och import, metod
1. Använd [WordPress exportera] [ export] tooexport din befintliga platsen.
2. Skapa ett webbprogram med hjälp av hello steg i hello [skapa en WordPress-webbplats](#Create-a-new-WordPress-site) avsnitt.
3. Logga in tooyour WordPress-webbplats på hello [Azure-portalen][mgmtportal], och klicka sedan på **plugin-program** > **Lägg till ny**. Sök efter och installera hello **WordPress Importverktyget** plugin-programmet.
4. När du installerar hello Importverktyget för WordPress-plugin-programmet, klickar du på **verktyg** > **importera**, och klicka sedan på **WordPress** toouse hello Importverktyget för WordPress-plugin-programmet.
5. På hello **importera WordPress** klickar du på **Välj fil**. Hitta hello WXR filen som exporterades från den befintliga WordPress-webbplatsen och klicka sedan på **överför filen och importera**.
6. Klicka på **skicka**. Du uppmanas att hello importen lyckades.
7. När du har slutfört de här stegen kan du starta om webbplatsen från dess **Apptjänster** bladet i hello [Azure-portalen][mgmtportal].

När du har importerat hello plats, kanske du behöver tooperform hello följande steg tooenable inställningar som inte är i hello importfil.

| Om du använder det här... | Gör detta... |
| --- | --- |
| **Unika webbadresser** |Hello WordPress instrumentpanel hello nya platsen, klicka på **inställningar** > **unika webbadresser**, och sedan uppdatera hello unika webbadresser struktur. |
| **bild/media länkar** |tooupdate länkar toohello nya platsen använder hello [sammet blått uppdatera webbadresser plugin][velvet], en Sök och Ersätt-verktyget eller manuellt uppdatera hello länkar i databasen. |
| **Teman** |Gå för**utseende** > **tema**, och sedan uppdatera hello webbplatstema efter behov. |
| **Menyer** |Om temat stöder menyer, kanske länkar tooyour startsidan hello gamla underkatalog inbäddade i dessa. Gå för**utseende** > **menyer**, och sedan uppdatera dem. |

#### <a name="hello-backup-and-restore-method"></a>Hej backup och restore, metod
1. Säkerhetskopiera dina befintliga WordPress platsen med hjälp av hello information [WordPress säkerhetskopieringar][wordpressbackup].
2. Säkerhetskopiera den befintliga databasen med hjälp av hello information [säkerhetskopiera databasen][wordpressdbbackup].
3. Skapa en databas och återställer hello säkerhetskopia.

   1. Köp en ny databas från hello [Azure Marketplace][cdbnstore], eller konfigurera en MySQL-databas på en [Windows] [ mysqlwindows] eller [Linux ] [ mysqllinux] virtuella datorn.
   2. Använda en MySQL-klient som [MySQL arbetsstationen] [ workbench] tooconnect toohello ny databas och importera din WordPress-databas.
   3. Uppdatera hello databasen toochange hello domän poster tooyour nya Azure App Service domän, till exempel mywordpress.azurewebsites.net. Använd hello [Sök och Ersätt för WordPress databaser skriptet] [ searchandreplace] toosafely ändra alla instanser.
4. Skapa en webbapp i hello Azure-portalen och publicera hello WordPress-säkerhetskopiering.

   1. toocreate ett webbprogram som har en databas i hello [Azure-portalen][mgmtportal], klickar du på **ny** > **webb + mobilt**  >  **Azure Marketplace** > **Webbappar** > **Webbapp + SQL** (eller **Webbapp + MySQL**) > **Skapa**. Konfigurera alla hello krävs inställningar toocreate ett tomt webbprogram.
   2. Leta upp hello i säkerhetskopian WordPress **wp config.php** filen och öppna den i en textredigerare. Ersätt hello efter poster med hello information för den nya MySQL-databasen:

      * **%{Db_name/**: hello användarnamn i hello-databasen.
      * **DB_USER**: hello användaren namn som används för tooaccess hello databasen.
      * **DB_PASSWORD**: hello användarens lösenord.

        När du ändrar de här posterna, spara och Stäng hello **wp config.php** fil.
   3. Använd hello [distribuera en webbapp i Azure App Service] [ deploy] information tooenable hello distributionsmetod du vill toouse och sedan distribuera WordPress-säkerhetskopiering tooyour webbappen i Azure App Service.
5. Efter hello WordPress-webbplats har distribuerats, du ska kunna tooaccess hello ny plats (som en Apptjänst-webbapp) med hjälp av hello *. azurewebsite.net hello webbplatsens URL.

### <a name="configure-your-site"></a>Konfigurera din webbplats
När hello WordPress-webbplatsen har skapats eller migreras, Använd följande information tooimprove prestanda hello eller aktivera ytterligare funktioner.

| toodo detta... | Använder du det här … |
| --- | --- |
| **Ange App Service-plan läge, storlek och aktivera skalning** |[Skala en webbapp i Azure App Service][websitescale]. |
| **Aktivera beständiga databasanslutningar** |Som standard använder inte WordPress beständiga databasanslutningar, vilket kan orsaka din anslutning toohello databasen toobecome begränsas efter flera anslutningar. tooenable beständiga anslutningar, installera hello [beständiga anslutningar kortet plugin](https://wordpress.org/plugins/persistent-database-connection-updater/installation/). |
| **Förbättra prestanda** |<ul><li><p><a href="https://azure.microsoft.com/en-us/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/">Inaktivera hello ARR cookie</a>, vilket kan förbättra prestanda när WordPress körs på flera instanser av Web Apps.</p></li><li><p>Aktivera cachelagring. Du kan använda <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis-cache</a> (förhandsgranskning) med hello <a href="https://wordpress.org/plugins/redis-object-cache/">Redis cache WordPress-plugin för objektet</a>, eller kan du använda en av hello andra cachelagring erbjudanden från hello <a href="/gallery/store/">Azure Store</a>.</p></li><li><p>[Se WordPress snabbare med Wincache](https://wordpress.org/plugins/w3-total-cache/). Wincache är aktiverad som standard för webbprogram. När du använder WinCache och dynamiska Cache tillsammans, stänga av Wincaches fil-cache, men lämna hello användar- och sessionscachen aktiverad. tooturn av fil-cache i en systemnivå ini-fil, ange hello följande värde:<br/><code>wincache.fcenabled = 0</code></p></li><li><p>[Skala en webbapp i Azure App Service] [ websitescale] och använda <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB hög tillgänglighet routning</a> eller <a href="http://www.mysql.com/products/cluster/">MySQL-kluster CGE</a>.</p></li></ul> |
| **Använda BLOB för lagring** |<ol><li><p>[Skapa ett Azure storage-konto](../storage/common/storage-create-storage-account.md).</p></li><li><p>Lär dig hur för[Använd hello innehåll Distribution nätverket](../cdn/cdn-create-new-endpoint.md) toogeo-distribuera data lagrade i blobar.</p></li><li><p>Installera och konfigurera hello <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure Storage för WordPress-plugin-programmet</a>.</p><p>Detaljerad installation och konfigurationsinformation för hello plugin-programmet finns hello <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">användarhandboken</a>.</p> </li></ol> |
| **Aktivera e-post** |Aktivera <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> med hjälp av hello Azure Store. Installera hello <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid plugin</a> för WordPress. |
| **Konfigurera ett anpassat domännamn** |[Konfigurera ett anpassat domännamn i Azure App Service][customdomain]. |
| **Aktivera HTTPS för ett anpassat domännamn** |[Aktivera HTTPS för en webbapp i Azure App Service][httpscustomdomain]. |
| **Läsa in saldo eller geo-distribuera din webbplats** |[Vidarebefordra trafik med Azure Traffic Manager][trafficmanager]. Om du använder en anpassad domän, se [konfigurera ett anpassat domännamn i Azure App Service] [ customdomain] information om hur toouse Traffic Manager med anpassade domännamn. |
| **Aktivera automatisk säkerhetskopiering** |[Säkerhetskopiera en webbapp i Azure App Service][backup]. |
| **Aktivera diagnostikloggning** |[Aktivera diagnostikloggning för web apps i Azure App Service][log]. |

## <a name="next-steps"></a>Nästa steg
* [WordPress-optimering](http://codex.wordpress.org/WordPress_Optimization)
* [Konvertera WordPress toomultisite i Azure App Service](web-sites-php-convert-wordpress-multisite.md)
* [ClearDB Uppgraderingsguide för Azure](http://www.cleardb.com/store/azure/upgrade)
* [Värd för WordPress i en undermapp i webbappen i Azure App Service](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)
* [Steg för steg: Skapa en WordPress-webbplats med hjälp av Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)
* [Värd för din befintliga WordPress-blogg på Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)
* [Aktivera snyggt unika webbadresser i WordPress](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)
* [Hur toomigrate och kör din WordPress-blogg i Azure App Service](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)
* [Hur toorun WordPress på Azure App Service för kostnadsfri](http://architects.dzone.com/articles/how-run-wordpress-azure)
* [WordPress på Azure på två minuter eller mindre](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)
* [Flytta en WordPress-blogg tooAzure - del 1: skapa en WordPress-blogg på Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)
* [Flytta en WordPress-blogg tooAzure - del 2: Överför ditt innehåll](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)
* [Flytta en WordPress-blogg tooAzure - del 3: konfigurera den anpassade domänen](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)
* [Flytta en WordPress-blogg tooAzure - del 4: snyggt unika webbadresser och URL-omskrivning om regler](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)
* [Flytta en WordPress-blogg tooAzure - del 5: flytta från en undermapp toohello rot](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)
* [Hur tooset upp en WordPress webbapp i Azure-konto](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)
* [Propping in WordPress på Azure](http://www.johnpapa.net/wordpress-on-azure/)
* [Tips för WordPress på Azure](http://www.johnpapa.net/azurecleardbmysql/)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs och gör inga åtaganden.
>
>

## <a name="whats-changed"></a>Nyheter
En guide toohello ändring från webbplatser tooApp tjänsten finns [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714).

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: app-service-web-tutorial-custom-domain.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: app-service-web-tutorial-custom-ssl.md
[mysqlwindows]:../virtual-machines/windows/classic/mysql-2008r2.md
[mysqllinux]:../virtual-machines/linux/classic/mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: /powershell/azureps-cmdlets-docs
[Azure CLI]:../cli-install-nodejs.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
