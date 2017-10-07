---
title: "aaaBest praxis för Azure App Service"
description: "Läs om bästa praxis och felsöka problem med Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: dariagrigoriu
ms.openlocfilehash: a1d3127f5a746aa43f7b56b45f17c45df9087bee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-app-service"></a>Metodtips för Azure App Service
Den här artikeln sammanfattar Metodtips för [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Samordning
När Azure-resurser genom att skriva en lösning, till exempel ett webbprogram och en databas finns i olika regioner hello effekter kan inkludera hello följande:

* Ökad latens kommunikationen mellan resurser
* Monetära kostnader för utgående data transfer cross-region som anges på hello [Azure sida med priser](https://azure.microsoft.com/pricing/details/data-transfers).

Samordning i hello samma region som är bäst för Azure-resurser genom att skriva en lösning, till exempel ett webbprogram och en databas eller storage-konto används toohold innehåll eller. När hello skapar resurser bör du kontrollera de finns i samma Azure-region om du inte har särskilda business eller utforma orsaken till dem inte toobe. Du kan flytta en Apptjänst-app toohello samma region som din databas genom att utnyttja hello [Apptjänst funktionen kloning](app-service-web-app-cloning-portal.md) tillgängliga för Premium Apptjänstplan appar.   

## <a name="memoryresources"></a>När appar förbruka mer minne än väntat
När du ser en app förbrukar mer minne än förväntat som anges via övervakning eller tjänsterekommendationer Tänk hello [App Service automatisk återställning funktionen](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Ett av hello alternativ för automatisk återställning hello-funktionen tar anpassade åtgärder baserat på ett tröskelvärde för minne. Åtgärder span hello spektrumet från e-postaviseringar tooinvestigation via minne dump tooon plats minskning av återvinning hello arbetsprocessen. Automatisk återställning kan konfigureras via web.config och via ett eget användargränssnitt enligt beskrivningen i i det här blogginlägget för hello [App webbtjänsttillägget Support webbplats](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>När appar konsumera mycket mer Processorkraft än väntat
När du upptäcker att en app förbrukar mer CPU än förväntat eller inträffar upprepas processoranvändning som anges via övervakning eller tjänsterekommendationer Överväg att skala upp eller skala ut hello App Service-plan. Om ditt program är statefull, är skala upp hello endast alternativet, även om ditt program är tillståndslös, skala ut får du större flexibilitet och högre risk för skalan. 

Mer information om ”statefull” eller ”tillståndslösa” program kan du titta på den här videon: [planera en skalbar slutpunkt till slutpunkt Flernivåapp på Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Mer information om alternativ för skalning och autoskalning i Apptjänst: [skala en Webbapp i Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>När socketen resurserna är uttömda
En vanlig orsak till lång körningstid förbrukar utgående TCP-anslutningar är hello användningen av klientbibliotek som inte är implementerade tooreuse TCP-anslutningar eller i hello fallet protokoll på en högre nivå, till exempel HTTP - Keep-Alive inte utnyttjas. Granska hello dokumentationen för varje hello bibliotek som refereras av hello appar i din App Service-Plan tooensure konfigureras eller komma åt i koden för effektiv användning av utgående anslutningar. Du kan även följa hello biblioteket dokumentationen vägledning för rätt skapandet och versionen eller rensa tooavoid läcka anslutningar. Medan sådana klienten bibliotek utredningar kan förlopp påverkan undvikas genom att skala ut toomultiple instanser.  

## <a name="appbackup"></a>När en app Säkerhetskopiera startar misslyckas
Hej två vanligaste orsakerna till varför app säkerhetskopieringen misslyckas är: Ogiltig lagringsinställningarna och konfiguration av ogiltig databas. Dessa fel inträffa vanligtvis när det finns ändringar toostorage eller databasresurser eller ändringar i hur tooaccess dessa resurser (t.ex. autentiseringsuppgifter som uppdateras för markerade i hello säkerhetskopieringsinställningar hello-databasen). Säkerhetskopieringar normalt körs enligt ett schema och kräver åtkomst toostorage (för att mata ut hello säkerhetskopierade filer) och databaser (för kopiera och läsa innehållet toobe ingår i hello säkerhetskopiering). Hej resultatet inte tooaccess någon av dessa resurser skulle vara konsekvent säkerhetskopieringen har misslyckats. 

Granska den senaste resultat toounderstand vilken typ av fel som händer när Säkerhetskopieringsfel uppstår. Hello gäller lagringsfel åtkomst, granska och uppdatera hello Lagringsinställningar som används i konfigurationen för säkerhetskopiering av hello. Hello gäller databasfel åtkomst, granska och uppdatera din anslutningar strängar som en del av appinställningar. Gå sedan vidare tooupdate konfigurationen för säkerhetskopiering-tooproperly inkluderar hello krävs databaser. Mer information om app-säkerhetskopiering finns hello [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md) dokumentation.

## <a name="nodejs"></a>När nya Node.js-appar är distribuerade tooAzure Apptjänst
Azure Apptjänst standardkonfigurationen för Node.js-appar är avsedda toobest färg hello behoven hos de vanligaste appar. Om konfigurationen för Node.js-appen skulle dra nytta av anpassade prestandajustering tooimprove prestanda eller Optimera resursanvändningen för CPU/minne/nätverksresurser, kan du granska våra metodtips och felsökning. Dokumentationen beskrivs hello iisnode-inställningar som du kan behöva tooconfigure för din Node.js-app beskriver hello olika scenarier eller problem som kan hantera din app och visar hur tooaddress problemen: [bästa praxis och felsökningsguide för noden program för Azure App Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   

