---
title: "aaaAzure App lokala cacheminne översikt | Microsoft Docs"
description: "Den här artikeln beskriver hur tooenable, storlek och fråga hello status för hello Azure App Service lokal Cache-funktion"
services: app-service
documentationcenter: app-service
author: SyntaxC4
manager: yochayk
editor: 
tags: optional
keywords: 
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2016
ms.author: cfowler
ms.openlocfilehash: 220331ac7e15352a434d63266701071024d868c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-local-cache-overview"></a>Översikt över Azure App Service lokalt cacheminne
Azure web appinnehåll lagras på Azure Storage och är anslutna upp en beständig sätt som ett delat innehåll. Den här designen är avsedd toowork med en rad olika appar och har hello följande attribut:  

* hello innehållet delas mellan instanser av virtuell dator (VM) av hello webbprogrammet.
* hello innehåll skyddas och kan ändras genom att köra webbappar.
* Loggfiler och diagnostikdata filer är tillgängliga under hello samma delade innehållsmappen.
* Publicering av nytt innehåll direkt uppdateringar hello innehållsmappen. Du kan genast visa hello samma innehåll via hello SCM webbplats och hello körs webbprogram (vanligtvis vissa tekniker, till exempel ASP.NET initiera en web app omstart på vissa ändringar tooget hello senaste innehåll).

När många webbappar som använder en eller flera av de här funktionerna, måste vissa webbprogram bara en hög prestanda, skrivskyddad innehållslagring som de kan köras från med hög tillgänglighet. De här apparna kan dra nytta av en VM-instans av en viss lokal cache.

hello Azure App Service lokalt cacheminne funktion tillhandahåller en webbvy roll av ditt innehåll. Det här innehållet är en write-men-ta bort cache lagring innehåll som har skapats asynkront vid start av platsen. När hello cache är klar, är hello platsen avstängda toorun mot hello cachelagrat innehåll. Webbprogram som körs på den lokala cachen har hello följande fördelar:

* De är immun toolatencies som uppstår när de har åtkomst till innehållet på Azure-lagring.
* De är immun toohello planerade uppgraderingar eller oplanerade stillestånd och eventuella andra avbrott med Azure Storage som uppstår på servrar som betjänar hello innehållsdelen.
* De har färre omstarter av appen på grund av toostorage resursen ändringar.

## <a name="how-local-cache-changes-hello-behavior-of-app-service"></a>Hur hello beteendet för Apptjänst ändras i lokalt cacheminne
* hello lokal cache är en kopia av hello /site och /siteextensions mappar hello webbprogrammet. Den har skapats på hello lokala VM-instans på web app start. hello hello lokala cachens storlek per webbprogrammet är begränsad too300 MB som standard, men du kan öka upp too2 GB.
* hello lokal cache är skrivskyddad. Eventuella ändringar kommer att ignoreras när hello webbprogrammet flyttar virtuella datorer eller hämtar startas om. Du bör inte använda lokalt cacheminne för appar som lagrar viktiga data i hello innehållslagring.
* Webbprogram kan fortsätta toowrite loggfiler och diagnostikdata som för närvarande. Loggfiler och data, men lagras lokalt på hello VM. Sedan kopieras de över regelbundet toohello delad innehållslagring. hello kopiera toohello delad innehållslagring är en bästa möjliga ansträngning--skrivåtgärder ryggstöd kan gå förlorade på grund av tooa oväntade krascher för en VM-instans.
* Det finns en ändring i hello mappstruktur för mappar för webbprogram som använder lokal Cache och hello loggfiler. Det finns nu undermappar i hello lagring loggfiler och Data som följer mönstret för hello ”UID” + tidsstämpel. Var och en av hello undermappar motsvarar tooa VM-instans där hello webbapp körs eller har körts.  
* Publicera ändringar toohello webb-app genom någon av hello publishing mekanismer publicerar toohello delad innehållslagring. Detta är avsiktligt eftersom vi vill hello publicerat innehåll toobe beständig. toorefresh hello lokala cachen av hello webbprogram, måste toobe startas om. Verkar det som ett steg i mycket? toomake hello livscykel sömlös, se hello information senare i den här artikeln.
* D:\Home peka toohello lokalt cacheminne. D:\Local fortsätter pekar toohello VM specifika temporärt.
* hello-standardvyn innehåll för hello SCM platsen fortsätter toobe som hello delad innehållslagring.

## <a name="enable-local-cache-in-app-service"></a>Aktivera lokal Cache i App Service
Du kan konfigurera lokal Cache med hjälp av en kombination av reserverade app-inställningar. Du kan konfigurera dessa app-inställningar med hjälp av hello följande metoder:

* [Azure Portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-hello-azure-portal"></a>Konfigurera lokal Cache med hjälp av hello Azure-portalen
<a name="Configure-Local-Cache-Portal"></a>

Du kan aktivera lokalt cacheminne på grundval av per webbprogram med hjälp av appinställningen:`WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Azure portal appinställningar: lokalt cacheminne](media/app-service-local-cache/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Konfigurera lokal Cache med hjälp av Azure Resource Manager
<a name="Configure-Local-Cache-ARM"></a>

```

...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],

"properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-hello-size-setting-in-local-cache"></a>Ändra inställningen för hello i det lokala cacheminnet
Som standard hello lokal cache-storleken är **300 MB**. Detta inkluderar hello /site och /siteextensions mappar som kopieras från hello content store, samt alla lokalt skapade loggar och data. tooincrease detta begränsar, Använd hello appinställningen `WEBSITE_LOCAL_CACHE_SIZEINMB`. Du kan öka hello storleken upp för**2 GB** (2 000 MB) per webbprogrammet.

## <a name="best-practices-for-using-app-service-local-cache"></a>Metodtips för App Service lokalt cacheminne
Vi rekommenderar att du använder lokal Cache tillsammans med hello [Mellanlagringsmiljöer](../app-service-web/web-sites-staged-publishing.md) funktion.

* Lägg till hello *Fäst* appinställningen `WEBSITE_LOCAL_CACHE_OPTION` med hello värdet `Always` tooyour **produktion** plats. Om du använder `WEBSITE_LOCAL_CACHE_SIZEINMB`, Lägg också till den som en Fäst inställningen tooyour produktionsplatsen.
* Skapa en **mellanlagring** fack och publicera tooyour mellanlagring plats. Du normalt anger inte hello mellanlagring fack toouse lokalt cacheminne tooenable en sömlös build distribuera test livscykel för mellanlagring om du får hello fördelarna med lokalt cacheminne för hello produktionsplatsen.
* Testa platsen mot mellanlagrings-plats.  
* När du är klar kan du utfärda ett [växlingsåtgärden](../app-service-web/web-sites-staged-publishing.md#Swap) mellan mellanlagring och produktion fack.  
* Fäst inställningar inkluderar namn och Fäst tooa plats. Så ärver hello mellanlagring fack hämtar växlas över till produktion, den hello lokalt cacheminne app-inställningar. hello växlas nyligen produktion ska köras mot hello lokalt cacheminne efter några minuter och kommer att varmkörts som en del av fack upp efter växlingen. När hello fack växlingen har slutförts, kör så din produktionsplatsen mot hello lokalt cacheminne.

## <a name="frequently-asked-questions-faq"></a>Vanliga frågor och svar (FAQ)
### <a name="how-can-i-tell-if-local-cache-applies-toomy-web-app"></a>Hur vet jag om lokalt cacheminne gäller toomy webbprogram?
Om ditt webbprogram måste en högpresterande, tillförlitlig innehållslagring använder inte hello innehållslagring toowrite kritiska data vid körning och är mindre än 2 GB i total storlek, är hello svaret ”Ja”! tooget hello sammanlagda storleken på mapparna /site och /siteextensions, kan du använda hello webbplatstillägg ”Azure Web Apps diskanvändning”.  

### <a name="how-can-i-tell-if-my-site-has-switched-toousing-local-cache"></a>Hur vet jag om min webbplats har bytt toousing lokal Cache?
Om du använder hello lokal Cache-funktionen med Mellanlagringsmiljöer slutförs inte hello växlingen tills det lokala cacheminnet är varmkörts. toocheck om webbplatsen körs mot lokalt cacheminne, kan du kontrollera hello worker process miljövariabeln `WEBSITE_LOCALCACHE_READY`. Använd hello instruktioner på hello [worker process miljövariabeln](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) sidan tooaccess hello worker process-miljövariabeln på flera instanser.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-toohave-them-why"></a>Jag bara publicerade nya ändringar, men webbappen verkar inte toohave dem. Varför?
Om ditt webbprogram använder lokalt cacheminne, måste toorestart plats tooget hello de senaste ändringarna. Inte vill toopublish ändringar tooa produktionsplatsen? Se hello fack alternativen i hello bästa praxis ovan.

### <a name="where-are-my-logs"></a>Var finns Mina loggar?
Med lokalt cacheminne ser dina loggar och mappar lite annorlunda ut. Dock hello strukturen för undermappar-förblir Hej samma, förutom att hello undermappar nestled under en undermapp med hello formatera ”VM-UID” + tidsstämpel.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Jag har lokal Cache aktiverad, men webbappen fortfarande hämtar startas om. Varför är? Jag tror lokalt cacheminne hjälpt med frekventa appen startas om.
Lokal Cache förhindra omstarter av lagringsrelaterade web app. Ditt webbprogram kan dock fortfarande genomgå omstarter under planerade infrastruktur uppgraderingar av hello VM. hello ska övergripande app omstarter som uppstår på lokal Cache aktiverad vara färre.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-toohello-faster-local-drive"></a>Lokal Cache utesluter alla kataloger från att kopieras toohello snabbare lokal enhet?
En mapp som heter databasen kommer att uteslutas hello steget som kopierar hello lagring innehåll. Detta hjälper med scenarier där webbplatsens innehåll kan innehålla en källkontroll som inte kanske behövs i dag tooday åtgärd av hello webbprogram. 
