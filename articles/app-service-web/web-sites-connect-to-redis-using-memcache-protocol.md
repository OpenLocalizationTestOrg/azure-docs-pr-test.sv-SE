---
title: aaaConnect en App Service web app tooRedis via hello Memcache - protokollet i Azure | Microsoft Docs
description: "Ansluta en webbapp i Azure App service tooRedis Cache med hjälp av hello Memcache-protokollet"
services: app-service\web
documentationcenter: php
author: SyntaxC4
manager: erikre
editor: riande
ms.assetid: 0fcdf9fa-2995-4839-ba4d-cfa389c4ba06
ms.service: app-service-web
ms.devlang: php
ms.topic: get-started-article
ms.tgt_pltfrm: windows
ms.workload: na
ms.date: 02/29/2016
ms.author: cfowler
ms.openlocfilehash: 48036d60fbbced59eb1e37584f507fffffff753d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Ansluta en webbapp i Azure App Service tooRedis Cache via hello Memcache-protokollet
I den här artikeln lär du dig hur tooconnect en WordPress web app i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) för[Azure Redis-Cache] [ 12] med hello [Memcache] [ 13] protokoll. Om du har en befintlig webbapp som använder en memcache-lagrad server för att cachelagra i minnet kan migrera du den tooAzure Apptjänst och Använd hello från första part cachelagring lösning i Microsoft Azure med liten eller ingen ändring tooyour programkod. Du kan dessutom använda dina befintliga Memcache kunskaper toocreate skalbara, distribuerade appar i Azure App Service med Azure Redis-Cache för cachelagring i minnet när du använder populära programramverk som .NET, PHP, Node.js, Java och Python.  

App Service Web Apps gör det här programscenariot med hello Web Apps Memcache-shim, det är en lokal memcache-lagrad server som fungerar som en Memcache-proxy för cachelagring av anrop tooAzure Redis-Cache. Detta gör att alla appar som kommunicerar med hjälp av hello Memcache-protokollet toocache data med Redis-Cache. Detta Memcache-shim fungerar på protokollnivå hello, så den kan användas av alla program och programramverk som kommunicerar med hjälp hello Memcache-protokollet.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## Krav
hello Web Apps Memcache-shim kan användas med alla program som kommunicerar med hello Memcache-protokollet. För det här exemplet är hello referensprogrammet en skalbar WordPress-webbplats som kan etableras från hello Azure Marketplace.

Följ hello steg som beskrivs i följande artiklar:

* [Etablera en instans av hello Azure Redis-Cache-tjänst][0]
* [Distribuera en skalbar WordPress-webbplats i Azure][1]

När du har distribuerat hello skalbar WordPress-webbplats och en Redis-Cache-instans som har etablerats blir redo tooproceed med att aktivera hello Memcache-shim i Azure App Service Web Apps.

## Aktivera hello Web Apps Memcache-shim
Du måste skapa tre appinställningar i ordning tooconfigure Memcache-shim. Detta kan göras med ett antal olika sätt, bland annat hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), hello [klassiska portalen][3], hello [Azure PowerShell-Cmdlets] [ 5] eller hello [Azure-kommandoradsgränssnittet][5]. Hello enligt det här exemplet ska vi toouse hello [Azure Portal] [ 4] tooset hello app-inställningar. hello följande värden kan hämtas från **inställningar** bladet för din Redis-Cache-instans.

![Inställningsblad för Azure Redis-cache](./media/web-sites-connect-to-redis-using-memcache-protocol/1-azure-redis-cache-settings.png)

### Lägga till appinställningen REDIS_HOST
Hej första appinställning du behöver toocreate är hello **REDIS\_värden** appinställningen. Den här inställningen anger hello mål toowhich hello shim vidarebefordrar hello cacheinformationen. Hej värdet som krävs för hello appinställningen redis_host kan hämtas från hello **egenskaper** bladet för din Redis-Cache-instans.

![Värdnamn för Azure Redis-cache](./media/web-sites-connect-to-redis-using-memcache-protocol/2-azure-redis-cache-hostname.png)

Ange hello nyckeln hello app för att**REDIS\_värden** och hello värdet för hello app inställningen toohello **värdnamn** av hello Redis-Cache-instansen.

![Appinställningen REDIS_HOST för Webbappar](./media/web-sites-connect-to-redis-using-memcache-protocol/3-azure-website-appsettings-redis-host.png)

### Lägga till appinställningen REDIS_KEY
Hej andra appinställning du behöver toocreate är hello **REDIS\_NYCKELN** appinställningen. Den här inställningen ger hello autentisering token krävs toosecurely åtkomst hello Redis-cacheinstansen. Du kan hämta hello-värde som krävs för hello appinställningen redis_key från hello **åtkomstnycklar** bladet för hello Redis-cacheinstansen.

![Primär nyckel för Azure Redis-cache](./media/web-sites-connect-to-redis-using-memcache-protocol/4-azure-redis-cache-primarykey.png)

Ange hello nyckeln hello app för att**REDIS\_NYCKELN** och hello värdet för hello app inställningen toohello **primärnyckel** av hello Redis-Cache-instansen.

![Azure-webbplats, AppSetting REDIS_KEY](./media/web-sites-connect-to-redis-using-memcache-protocol/5-azure-website-appsettings-redis-primarykey.png)

### Lägga till appinställningen MEMCACHESHIM_REDIS_ENABLE
hello sista appinställningen är används tooenable hello Memcache-Shim i Web Apps som använder hello REDIS_HOST och REDIS_KEY tooconnect toohello Azure Redis-Cache och vidarebefordra hello cache-anrop. Ange hello nyckeln hello app för att**MEMCACHESHIM\_REDIS\_aktivera** och hello värdet för**SANT**.

![Webbapp, AppSetting MEMCACHESHIM_REDIS_ENABLE](./media/web-sites-connect-to-redis-using-memcache-protocol/6-azure-website-appsettings-enable-shim.png)

När du är klar att lägga till app-inställningar för hello tre (3), klickar du på **spara**.

## Aktivera Memcache-tillägget för PHP
För hello programmet toospeak hello Memcache-protokollet är det nödvändigt tooinstall hello Memcache-tillägget tooPHP--hello språkramverket för WordPress-webbplatsen.

### Hämta hello php_memcache tillägg
Bläddra för[PECL][6]. Under hello cachelagring kategori, klickar du på [memcache][7]. Klicka på hello DLL-länken under hello kolumnen för nedladdningar.

![PHP PECL-webbplats](./media/web-sites-connect-to-redis-using-memcache-protocol/7-php-pecl-website.png)

Hämta hello Non-Thread Safe (NTS) x86 länk för hello-versionen av PHP aktiverad i Web Apps. (Standard är PHP 5.4)

![PHP PECL-webbplats, Memcache-paket](./media/web-sites-connect-to-redis-using-memcache-protocol/8-php-pecl-memcache-package.png)

### Aktivera tillägget php_memcache för hello
När du har hämtat hello filen packa upp och ladda upp hello **php\_memcache.dll** till hello **d:\\hem\\plats\\wwwroot\\bin\\ext\\**  directory. När hello php_memcache.dll har överförts till hello webbprogram, måste tooenable hello tillägget toohello PHP-körning. tooenable hello Memcache-tillägget i hello Azure-portalen, öppna hello **programinställningar** bladet för hello webbprogrammet sedan lägga till en ny appinställning med hello nyckeln för **PHP\_tillägg** och hello värdet **bin\\ext\\php_memcache.dll**.

> [!NOTE]
> Om hello webbprogrammet måste tooload flera PHP-tillägg, vara hello värdet php_extensions en komma-avgränsad lista med relativa sökvägar tooDLL filer.
> 
> 

![Webbapp, AppSetting PHP_EXTENSIONS](./media/web-sites-connect-to-redis-using-memcache-protocol/9-azure-website-appsettings-php-extensions.png)

När du är klar klickar du på **Spara**.

## Installera WordPress-plugin-programmet för Memcache
> [!NOTE]
> Du kan också hämta hello [Memcached Object Cache Plugin](https://wordpress.org/plugins/memcached/) från WordPress.org.
> 
> 

På hello WordPress-plugin-program klickar du på **Lägg till ny**.

![WordPress – plugin-sida](./media/web-sites-connect-to-redis-using-memcache-protocol/10-wordpress-plugin.png)

Skriv i sökrutan hello **memcached** och tryck på **RETUR**.

![WordPress – lägga till nytt plugin-program](./media/web-sites-connect-to-redis-using-memcache-protocol/11-wordpress-add-new-plugin.png)

Hitta **Memcached Object Cache** i hello sedan klickar du på **installera nu**.

![WordPress – installera plugin-programmet för Memcache ](./media/web-sites-connect-to-redis-using-memcache-protocol/12-wordpress-install-memcache-plugin.png)

### Aktivera hello Memcache WordPress-plugin-programmet
> [!NOTE]
> Följ hello instruktionerna i den här bloggen på [hur tooenable ett Webbplatstillägg i Web Apps] [ 8] tooinstall Visual Studio Team Services.
> 
> 

I hello `wp-config.php` lägger du till följande kod över hello sluta Redigera kommentar nära hello hello filens slut hello.

```php
$memcached_servers = array(
    'default' => array('localhost:' . getenv("MEMCACHESHIM_PORT"))
);
```

När den här koden har klistrats in, spara monaco automatiskt hello dokumentet.

hello nästa steg är tooenable hello objektcache plugin-programmet. Detta görs genom att dra och släppa **object-cache.php** från **wp-innehåll/plugins/memcached** mappen toohello **wp-content** mappen tooenable hello Memcache objekt Cache-funktionalitet.

![Leta upp hello memcache object-cache.php plugin-program](./media/web-sites-connect-to-redis-using-memcache-protocol/13-locate-memcache-object-cache-plugin.png)

Nu att hello **object-cache.php** filen har hello **wp-content** mappen hello Memcached Object Cache har nu aktiverats.

![Aktivera hello memcache object-cache.php plugin-program](./media/web-sites-connect-to-redis-using-memcache-protocol/14-enable-memcache-object-cache-plugin.png)

## Kontrollera hello Memcache Object Cache fungerar plugin-program
Alla hello steg tooenable hello Web Apps Memcache-shim är nu klar. hello enda som återstår är tooverify hello data fylls Redis-Cache-instans.

### Aktivera stöd för hello icke-SSL-port i Azure Redis-Cache
> [!NOTE]
> Hej Redis CLI har inte stöd för SSL-anslutning, därför hello följande steg är nödvändiga samtidigt hello för att skriva den här artikeln.
> 
> 

Hello Azure-portalen, bläddra toohello Redis-cacheinstansen som du skapade för det här webbprogrammet. När hello cachens blad är öppet klickar du på hello **inställningar** ikon.

![Inställningsknapp för Azure Redis-cache](./media/web-sites-connect-to-redis-using-memcache-protocol/15-azure-redis-cache-settings-button.png)

Välj **Åtkomstportar** hello-listan.

![Åtkomstport för Azure Redis-cache](./media/web-sites-connect-to-redis-using-memcache-protocol/16-azure-redis-cache-access-port.png)

Klicka på **Nej** för **Tillåt åtkomst endast via SSL**.

![Åtkomstport för Azure Redis-cache, endast SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/17-azure-redis-cache-access-port-ssl-only.png)

Du ser att hello icke-SSL-porten har angetts. Klicka på **Spara**.

![Azure Redis-cache, Redis-åtkomstportal, icke-SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/18-azure-redis-cache-access-port-non-ssl.png)

### Ansluta tooAzure Redis-Cache från redis-cli
> [!NOTE]
> Det här steget förutsätter att Redis har installerats lokalt på utvecklingsdatorn. [Installera Redis lokalt med hjälp av de här anvisningarna][9].
> 
> 

Öppna en kommandoradskonsol val och typen hello följande kommando:

```shell
redis-cli –h <hostname-for-redis-cache> –a <primary-key-for-redis-cache> –p 6379
```

Ersätt hello  **&lt;värdnamn för redis-cache&gt;**  med hello faktiska värdnamnet för xxxxx.redis.cache.Windows.NET och hello  **&lt;primary-key-för-redis-cache&gt;**  med hello snabbtangent för hello-cache, tryck på **RETUR**. När hello CLI har anslutit toohello Redis-cacheinstansen, utfärda ett redis-kommando. Jag har valt hello nycklar toolist i hello skärmbilden nedan.

![Ansluta tooAzure Redis-Cache från Redis-CLI i Terminal](./media/web-sites-connect-to-redis-using-memcache-protocol/19-redis-cli-terminal.png)

hello anropet toolist hello nycklar ska returnera ett värde. Om inte, försök navigera toohello webbapp och försök igen.

## Slutsats
Grattis! hello WordPress-appen har nu en centraliserad minnescache tooaid ökat genomflöde. Kom ihåg att hello Web Apps Memcache-Shim kan användas med alla Memcache-klienter oavsett programmera språk eller application framework. Bokför tooprovide feedback eller tooask frågor om hello Web Apps Memcache-shim för[MSDN-forum] [ 10] eller [Stackoverflow][11].

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

[0]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache
[1]: http://bit.ly/1t0KxBQ
[2]: http://manage.windowsazure.com
[3]: http://portal.azure.com
[4]: /powershell/azureps-cmdlets-docs
[5]: /downloads
[6]: http://pecl.php.net
[7]: http://pecl.php.net/package/memcache
[8]: http://blog.syntaxc4.net/post/2015/02/05/how-to-enable-a-site-extension-in-azure-websites.aspx
[9]: http://redis.io/download#installation
[10]: https://social.msdn.microsoft.com/Forums/home?forum=windowsazurewebsitespreview
[11]: http://stackoverflow.com/questions/tagged/azure-web-sites
[12]: /services/cache/
[13]: http://memcached.org
