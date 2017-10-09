---
title: "aaaGet igång med Azure Search i Node.js | Microsoft Docs"
description: "Se hur du bygger ett sökprogram på en värd- och molnbaserad söktjänst i Azure med Node.js som programmeringsspråk."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 0625dc1b-9db6-40d5-ba9a-4738b75cbe19
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: evboyle
ms.openlocfilehash: e9c7d756c2ea191ee2a285485c90439b96aa73b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a>Komma igång med Azure Search i Node.js
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Lär dig hur toobuild anpassade Node.js söka program som använder Azure Search för dess sökinställningar. Den här kursen använder hello [Azure Söktjänsts-REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekt och åtgärder som används i den här övningen.

Vi använde [Node.js](https://Nodejs.org) och NPM, [Sublime Text 3](http://www.sublimetext.com/3), och Windows PowerShell på Windows 8.1 toodevelop och testa den här koden.

toorun det här exemplet måste du ha en Azure Search-tjänst som du kan registrera dig för i hello [Azure-portalen](https://portal.azure.com). Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) stegvisa instruktioner.

## <a name="about-hello-data"></a>Om hello data
Det här exempelprogrammet använder data från hello [USA geologiska tjänster (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrerad på hello tillstånd Rhode dataö tooreduce hello dataset storlek. Vi använder den här data toobuild ett sökprogram som returnerar Landmärke byggnader, till exempel sjukhus och skolorna samt geologiska funktioner som dataströmmar, sjöar och toppmöten.

I det här programmet hello **DataIndexer** programmet skapar och belastningar hello index med hjälp av en [indexeraren](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstruktion, hämtar hello filtrerade USGS datamängd från en offentlig Azure SQL-databas. Autentiseringsuppgifter och anslutningen information toohello online datakällan har angetts i hello programkod. Ingen ytterligare konfiguration krävs.

> [!NOTE]
> Vi har använt ett filter på den här datauppsättningen toostay under hello 10 000 dokumentet gränsen på hello kostnadsfria prisnivån. Den här begränsningen gäller inte om du använder hello standardnivån. Mer information om kapaciteten för varje prisnivå finns i [Tjänstbegränsningar för Search](search-limits-quotas-capacity.md).
> 
> 

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a>Hitta hello tjänstnamn och api-nyckel för Azure Search-tjänst
När du har skapat hello service Retur-URL för toohello portal tooget hello eller `api-key`. Anslutningar tooyour Search-tjänsten kräver att du har båda hello-URL: en och en `api-key` tooauthenticate hello anrop.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på hello hopp-fältet **söktjänsten** toolist alla Azure Search-tjänster som har etablerats för din prenumeration.
3. Välj hello-tjänster som du vill toouse.
4. Du bör se paneler viktig information, till exempel hello nyckelikonen för att komma åt hello admin nycklar på hello service instrumentpanelen.
5. Kopiera hello-tjänstens URL, admin-nyckel och en nyckel för frågan. Du måste alla tre senare när du lägger till dem toohello config.js-fil.

## <a name="download-hello-sample-files"></a>Hämta hello exempelfilerna
Använd någon av följande metoder toodownload hello exempel hello.

1. Gå för[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).
2. Klicka på **hämta ZIP**, spara hello ZIP-fil och sedan extrahera alla hello filer den innehåller.

Alla efterföljande filändringar och körningsinstruktioner görs mot filer i den här mappen.

## <a name="update-hello-configjs-with-your-search-service-url-and-api-key"></a>Uppdatera hello config.js. med Search-tjänstens URL och API-nyckel
Med hjälp av hello URL: en och api-nyckel som du kopierade tidigare, ange hello URL, admin-nyckel och fråga nyckel i konfigurationsfilen.

Administratörsnycklar beviljar fullständig kontroll över tjänståtgärder, inklusive att skapa eller ta bort ett index och inläsning av dokument. Frågan nycklar är däremot för skrivskyddade åtgärder, som vanligtvis används av klientprogram som ansluter tooAzure sökning.

I det här exemplet innehåller vi hello fråga viktiga toohelp förstärka hello bästa praxis för att använda hello Frågenyckeln i klientprogram.

följande skärmbild visar hello **config.js** öppna i en textredigerare, med hello relevanta poster avgränsat så att du kan se där tooupdate hello-fil med hello värden som är giltiga för din söktjänst.

![][5]

## <a name="host-a-runtime-environment-for-hello-sample"></a>Värd för en körningsmiljö hello-exempel
hello urvalet kräver en HTTP-server som du kan installera globalt med npm.

Använda ett PowerShell-fönster för hello följande kommandon.

1. Navigera toohello mapp som innehåller hello **package.json** fil.
2. Skriv `npm install`.
3. Skriv `npm install -g http-server`.

## <a name="build-hello-index-and-run-hello-application"></a>Skapa hello indexet och kör programmet hello
1. Skriv `npm run indexDocuments`.
2. Skriv `npm run build`.
3. Skriv `npm run start_server`.
4. Dirigera webbläsaren till `http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Söka i USGS-data
Hej USGS datauppsättningen innehåller poster som är relevanta toohello tillstånd Rhode dataö. Om du klickar på **Sök** på en tom sökrutan visas hello upp 50 poster som är hello standard.

Anger en sökterm blir hello sökmotor något toogo på. Prova att skriva namnet på någon från regionen. ”Roger Williams” var hello första resursstyrningen av Rhode dataö. Många parker, byggnader och skolor bär hans namn.

![][9]

Du kan också prova någon av dessa söktermer:

* Pawtucket
* Pembroke
* goose +cape

## <a name="next-steps"></a>Nästa steg
Detta är hello första Azure Search-självstudierna baserat på Node.js och hello USGS dataset. Över tiden, kommer vi utöka den här självstudiekursen toodemonstrate ytterligare sökfunktioner som du kanske vill toouse i din anpassade lösningar.

Om du redan har viss erfarenhet av Azure Search kan du använda det här exemplet som utgångspunkt för att prova förslagsställare (frågeifyllningsförslag eller Komplettera automatiskt), filter och aspektbaserad navigering. Du kan också förbättra vid hello sökresultatsidan genom att lägga till antal och batchbearbetning dokument så att användare kan bläddra igenom hello resultat.

Nya tooAzure sökningen? Vi rekommenderar att du försöker andra självstudiekurser toodevelop förstå vad du kan skapa. Besök vår [dokumentationssidan](https://azure.microsoft.com/documentation/services/search/) toofind mer resurser. Du kan också visa hello länkar i vår [Video-och kursen](search-video-demo-tutorial-list.md) tooaccess mer information.

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
