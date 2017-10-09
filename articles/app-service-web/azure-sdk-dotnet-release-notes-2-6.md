---
title: "aaaAzure SDK för .NET 2.6 viktig information"
description: "Azure SDK för .NET 2.6 viktig information"
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: ac613cf20da4f731fab6f35ccbf6dbeaf18c0ec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK för .NET 2.6 viktig information
Det här dokumentet innehåller hello viktig information för hello Azure SDK för .NET 2.6-versionen. 

Du kan utveckla tjänsten molnprogram (PaaS) på .NET 4.5.2 eller .NET 4.6 under förutsättning att du manuellt installera hello Målversionen av .NET Framework på hello rolltjänst för molnet med Azure SDK 2.6. Se [installera .NET på en rolltjänst för molnet](http://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>Service Bus-uppdateringar
* Händelsehubbar: 
  
  * Kan nu riktade åtkomstkontroll när du skickar händelser genom att exponera ytterligare publisher slutpunkt för Händelsehubbar.
  * Lägga till tooEvent hubbar funktionen ytterligare stabilitet och förbättring.
  * Lägger till stöd för Amqp protokoll över WebSocket för meddelanden och Händelsehubbar.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight Tools för Visual Studio-uppdateringar
* **IntelliSense förbättring**: fjärrmetadata förslag
  
    HDInsight Tools för Visual Studio stöder nu hämtning fjärrmetadata när du redigerar ditt Hive-skript. Skriv till exempel **Välj * FROM** och alla hello tabellnamn kommer att visas. Hello kolumnnamn visas när du har angett en tabell.
* **Stöd för HDInsight-emulator**
  
    Nu kör HDInsight Tools för Visual Studio support ansluter tooHDInsight emulator, så du kan utveckla dina Hive-skript lokalt utan att några kostnader dessa skript mot dina HDInsight-kluster. 
  
    Mer information finns för[handboken](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **HDInsight Tools för Visual Studio-stöd för Allmänt Hadoop-kluster** (förhandsgranskning)
  
    HDInsight Tools för Visual Studio stöder nu allmänt Hadoop-kluster, så du kan använda HDInsight Tools för Visual Studio toodo hello följande:
  
  * ansluta tooyour kluster 
  * skriva Hive-fråga med förbättrad IntelliSense-autokomplettering stöd 
  * Visa alla hello i klustret med ett intuitivt användargränssnitt. 
    
    Mer information finns för[handboken](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>Uppdateringar för cachelagring i Rollinstanser
* **Cachelagring i Rollinstanser** har uppdaterats toouse **Microsoft Azure Storage SDK: N** version 4.3. Fram till nu hello **cachelagring i Rollinstanser** med Azure Storage SDK version 1.7.
  
    Kunder med hjälp av Azure SDK 2.5 eller nedan bör uppdatera tooAzure SDK 2.6 och flytta toohello ny version av Azure Storage SDK: N. 
  
    På den här gången Azure Storage-versionen är 2011-08-18 schemalagda toobe bort 1 augusti 2016. Alla migreringar av cachelagring i Rollinstanser från Azure SDK 2.5 eller under too2.6 måste ha slutförts vid den tidpunkten. Mer information om hello tillbakadragningen av Azure Storage version 2011-08-18 finns [Microsoft Azure Storage Service borttagning versionsuppdatering: tillägget too2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Presenterar vi hello 30 November 2016, pensionering för Azure Managed Cache Service och cachelagring i Rollinstanser för Azure. Vi rekommenderar att du migrerar tooAzure Redis-Cache som förberedelse för den här ur bruk. Mer information om datum och vägledning för migrering finns [som Azure Cache erbjudande är rätt för mig?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Azure Apptjänst-verktyg
hello följande objekt har uppdaterats i hello Azure SDK 2.6-versionen.

* Azure-publicering förbättrad tooinclude Azure API Apps som mål för en distribution.
* API Apps etablering funktioner tooenable användare med API-App för att skapa och etablera funktioner.
* Server Explorer ändras tooreflect nya App Service-noden med webb-, mobil- och API apps grupperade efter resursgrupp.
* Lägg till Azure API-App klient gest tillagd toomost C#-projekt som aktiverar automatisk generering av Swagger-aktiverade API Apps körs i Azure-prenumeration för en användare.
* Verktygsuppsättning för API Apps och Apptjänst noder i Server Explorer är tillgängliga i Visual Studio 2013 endast. 

## <a name="azure-resource-manager-tools-updates"></a>Uppdaterar Azure Resource Manager-verktyg
Verktyg för hello Azure resource manager har uppdaterat tooinclude mallar för virtuella datorer, nätverk och lagring. hello JSON redigera experience har uppdaterats tooinclude en ny disposition vy för mallar och hello möjlighet tooedit hello mallar med hjälp av JSON kodavsnitt. Mallar som distribuerats från Visual Studio använder ett PowerShell-skript som medföljer hello-projektet så att ändringar som görs toohello skript kommer att användas av Visual Studio.

## <a name="diagnostics-improvements-for-cloud-services"></a>Förbättringar av diagnostik för molntjänster
Azure SDK 2.6 ger tillbaka stöd för att samla in diagnostik loggar i hello Azure-beräkningsemulatorn och överföra dem toodevelopment lagring. Alla diagnostik loggar (inklusive application trace händelsespårning för Windows (ETW)-loggar, prestandaräknare, infrastruktur händelseloggarna loggar och windows-loggar) genereras när hello programmet körs i hello-emulatorn kan vara överförda toodevelopment lagring tooverify som din diagnostikloggning fungerar på din lokala dator. 

Hej diagnostiklagringskonto kan nu anges i hello (.cscfg) tjänstkonfigurationsfilen vilket gör det enklare toouse olika diagnostik lagringskonton för olika miljöer. Det finns några viktiga skillnader mellan hur hello anslutningssträngen fungerade i Azure SDK 2.4 och Azure SDK 2.6. Mer information om hur toouse hello diagnostik lagringsanslutningssträng och hur den påverkar dina projekt Se [konfigurera diagnostik för Azure Cloud Services](http://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Gör ändringar
### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-verktyg
* Hej **projekt för distribution av molnet** projekttyp som är tillgängliga i hello Azure SDK 2.5 har bytt namn för**Azure-resursgrupp**.
* **Molnet projekt för distribution av** typ av projekt som skapats i hello Azure SDK 2.5 kan användas i 2.6 men distribuera hello mallen från Visual Studio kommer att misslyckas. Dock distribuerar med hello PowerShell-skript kommer att fungera som det gjorde tidigare.  Mer information om hur toouse **projekt för distribution av molnet** i 2.6 läsa det här [efter](http://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Kända problem
* Insamling av loggar för diagnostik i hello-emulatorn kräver ett 64-bitars operativsystem. Diagnostik loggar samlas inte när du kör ett 32-bitars operativsystem. Detta påverkar inte andra funktioner i emulatorn. 
* Azure SDK 2.6 släpptes 4/29/2015 hade två problem: 
  
  * Universell App kunde inte läsas i Visual Studio 2015 när Azure SDK 2.6 installerades på hello-datorn.
  * Misslyckas felsökning Cloud Service-projekt i Visual Studio 2013 och Visual Studio 2015 där Visual Studio svarar inte och kraschar när visas en dialogruta med hälsningsmeddelande ”konfigurera diagnostik för emulatorn”.
    
    En uppdatering tooAzure SDK 2.6 gavs ut 5/18/2015. hello uppdaterade versionen är 2.6.30508.1601; den innehåller korrigeringar för två problem som beskrivs ovan. Du kan identifiera hello version av hello SDK från Kontrollpanelen -> program och funktioner -> Microsoft Azure-verktyg för Microsoft Visual Studio 2013 – v 2.6. hello versionskolumnen visar hello build-nummer.
    
    Om du fortfarande inför hello ovan problem, installera hello senaste versionen av hello Azure 2.6 SDK för [VS 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) eller [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Se även
[Support och tillbakadragning Information för hello Azure SDK för .NET och API: er](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

