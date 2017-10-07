---
title: "aaaHow tooupdate en molnbaserad tjänst | Microsoft Docs"
description: "Lär dig hur tooupdate cloud services i Azure. Lär dig hur en uppdatering på en tjänst i molnet fortsätter tooensure tillgänglighet."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c6a8b5e6-5c99-454c-9911-5c7ae8d1af63
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 7e4c8bd46e51a555b4309ea8927d120e8efcf0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-a-cloud-service"></a>Hur tooupdate en tjänst i molnet

Uppdatera en tjänst i molnet, inklusive dess roller och ett operativsystem, är tre steg. Först hello binärfiler och konfigurationsfiler för hello ny molntjänst eller OS-version måste överföras. Därefter reserverar Azure beräknings- och nätverksresurser för hello Molntjänsten baserat på hello krav hello nya cloud service-versionen. Slutligen utför Azure en löpande uppgradering tooincrementally hello klient toohello ny Uppdateringsversion eller operativsystem, samtidigt som din tillgänglighet. Den här artikeln beskrivs hello information om det sista steget – hello löpande uppgradering.

## <a name="update-an-azure-service"></a>Uppdatera en Azure-tjänst
Azure ordnar dina rollinstanser i logiska grupper kallas uppgraderingsdomäner (UD). Uppgraderingsdomäner (UD) är en logisk uppsättning rollinstanser som uppdateras som en grupp.  Azure-uppdateringar en cloud service en UD samtidigt, vilket ger instanser i andra UDs toocontinue trafik.

hello Standardantal uppgraderingsdomäner är 5. Du kan ange ett annat antal uppgraderingsdomäner genom att inkludera hello upgradeDomainCount attribut i definitionsfilen för hello-tjänsten (.csdef). Läs mer om hello upgradeDomainCount attributet [WebRole schemat](https://msdn.microsoft.com/library/azure/gg557553.aspx) eller [WorkerRole schemat](https://msdn.microsoft.com/library/azure/gg557552.aspx).

När du utför en uppdatering på plats av en eller flera roller i din service uppdaterar Azure uppsättningar av rollinstanser enligt toohello uppgraderingsdomänen toowhich de tillhör. Azure-uppdateringar för hello instanser i en viss uppgradera domän – stoppar dem, uppdatera dem, gör dem tillbaka online – flyttar till nästa hello-domän. Genom att stoppa endast hello-instanser som körs i hello aktuell uppgraderingsdomän, Azure ser till att det sker en uppdatering med hello minsta möjliga påverkan toohello som kör tjänsten. Mer information finns i [hur hello uppdateringen fortsätter](#howanupgradeproceeds) senare i den här artikeln.

> [!NOTE]
> När villkoren hello **uppdatera** och **uppgradera** ha något annan betydelse i kontexten hello Azure, de kan användas synonymt för hello processer och beskrivningar av hello funktioner i det här dokumentet.
>
>

Din tjänst måste definiera minst två instanser av en roll för att rollen toobe uppdateras på plats utan driftavbrott. Om tjänsten hello består av endast en instans av en roll, blir tjänsten otillgänglig tills hello på plats är klar.

Det här avsnittet innehåller följande information om Azure uppdateringar hello:

* [Tillåtna-tjänsten ändras under en uppdatering](#AllowedChanges)
* [Hur en uppgradering pågår](#howanupgradeproceeds)
* [Återställning av en uppdatering](#RollbackofanUpdate)
* [Initierar flera mutating åtgärder på ett pågående distribution](#multiplemutatingoperations)
* [Distributionen av roller uppgradera domäner](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>Tillåtna-tjänsten ändras under en uppdatering
hello följande tabell visar hello tillåtna ändringar tooa tjänsten under en uppdatering:

| Ändringar tillåts toohosting, tjänster och roller | Uppdatering på plats | Stegvis (VIP swap) | Ta bort och omdistribuera |
| --- | --- | --- | --- |
| Operativsystemversion |Ja |Ja |Ja |
| .NET-förtroendenivåer |Ja |Ja |Ja |
| Storlek på virtuell dator<sup>1</sup> |Ja<sup>2</sup> |Ja |Ja |
| Inställningar för lokal lagring |Öka endast<sup>2</sup> |Ja |Ja |
| Lägg till eller ta bort roller i en tjänst |Ja |Ja |Ja |
| Antal instanser av en viss roll |Ja |Ja |Ja |
| Antalet eller typen av slutpunkter för en tjänst |Ja<sup>2</sup> |Nej |Ja |
| Namn och värden för konfigurationsinställningar |Ja |Ja |Ja |
| Värden (men inte namn) för konfigurationsinställningar |Ja |Ja |Ja |
| Lägga till nya certifikat |Ja |Ja |Ja |
| Ändra befintliga certifikat |Ja |Ja |Ja |
| Distribuera nya kod |Ja |Ja |Ja |

<sup>1</sup> storlek ändras begränsad toohello delmängd av storlekar som finns tillgängliga för hello-Molntjänsten.

<sup>2</sup> kräver Azure SDK 1.5 eller senare versioner.

> [!WARNING]
> Ändra storlek på virtuell dator hello förstör lokala data.
>
>

följande objekt hello stöds inte under en uppdatering:

* Ändra hello namnet på en roll. Ta bort och Lägg sedan till hello-serverrollen med hello nytt namn.
* Ändring av hello uppgradera antal.
* Minska hello storleken på hello lokala resurser.

Om du gör andra uppdateringar tooyour service definition, till exempel minska hello storleken på lokal resurs, måste du utföra en VIP-växling uppdatering i stället. Mer information finns i [växla distribution](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Hur en uppgradering pågår
Du kan bestämma om du vill tooupdate alla hello roller i din tjänst eller en roll i hello-tjänsten. I båda fallen är alla instanser av varje roll som uppgraderas och tillhör toohello första uppgraderingsdomänen Stoppad, uppgraderas och online igen. När de är tillbaka online är hello instanser i hello andra uppgraderingsdomänen Stoppad, uppgraderas och online igen. En tjänst i molnet kan ha högst en uppgradering aktiva samtidigt. hello uppgraderingen utförs alltid mot hello senaste versionen av hello-Molntjänsten.

hello följande diagram illustrerar hur hello uppgraderingen fortsätter om du uppgraderar alla hello roller i hello-tjänsten:

![Uppgradera tjänsten](media/cloud-services-update-azure-service/IC345879.png "uppgradera tjänsten")

Den här nästa diagram illustrerar hur hello uppdatering fortsätter när du uppgraderar bara en enda roll:

![Uppgradera rollen](media/cloud-services-update-azure-service/IC345880.png "uppgradera roll")  

Under en automatisk uppdatering utvärderar hello Azure-Infrastrukturkontrollanten med jämna mellanrum hello hälsotillstånd hello cloud service toodetermine när säker toowalk hello nästa UD. Hälsotillstånd utvärderingen utförs på grundval av per roll och anser endast instanser i hello senaste versionen (d.v.s. instanser från UDs har redan gått). Den verifierar att ett minsta antal rollinstanser för varje roll har uppnått ett tillfredsställande avslutat tillstånd.

### <a name="role-instance-start-timeout"></a>Rollen instans Start Timeout
Hej Infrastrukturkontrollanten väntar 30 minuter för varje roll instans tooreach tillståndet igång. Om det ska gå att hello timeout-varaktighet, fortsätter hello Infrastrukturkontrollanten går toohello nästa rollinstans.

### <a name="impact-toodrive-data-during-cloud-service-upgrades"></a>Påverkan toodrive data under Molntjänsten uppgraderingar

När du uppgraderar en tjänst från en enda instans toomultiple instanser kommer din tjänst medan hello uppgraderingen utförs på grund av toohello sätt Azure uppgraderar tjänster. hello servicenivåavtal garanterande service tjänsttillgänglighet gäller endast tooservices som distribueras med mer än en instans. hello följande lista beskrivs hur hello data på varje enhet påverkas av uppgraderingsscenario varje Azure-tjänsten:

|Scenario|C-enheten|Enhet D|E-enheten|
|--------|-------|-------|-------|
|VM-omstart|Bevaras|Bevaras|Bevaras|
|Portalen omstart|Bevaras|Bevaras|Förstörs|
|Portalen avbildningsåterställning|Bevaras|Förstörs|Förstörs|
|Uppgradering på plats|Bevaras|Bevaras|Förstörs|
|Noden migrering|Förstörs|Förstörs|Förstörs|

Observera att, i hello ovanför listan hello E:-enheten representerar hello rollen rotenhet och får inte vara hårdkodade. Använd i stället hello **RoleRoot %** miljö variabeln toorepresent hello enhet.

toominimize hello avbrottstid när du uppgraderar en eninstanskategori tjänst distribuera en ny flera instanser toohello fristående server och utföra en VIP-växling.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Återställning av en uppdatering
Azure tillhandahåller flexibilitet för att hantera tjänster under en uppdatering genom att du kan initiera ytterligare åtgärder för en tjänst när hello inledande uppdateringsbegäran har godkänts av hello Azure-Infrastrukturkontrollanten. En återställning kan endast utföras när en uppdatering (konfigurationsändring) eller uppgraderingen är i hello **pågår** tillstånd hello-distribution. En uppdatering eller uppgradering av anses toobe pågående så länge det finns minst en instans av hello-tjänsten som ännu inte har uppdaterats toohello ny version. tootest om en återställning tillåts, kontrollera hello värdet av hello RollbackAllowed flagga som returneras av [hämta distribution](https://msdn.microsoft.com/library/azure/ee460804.aspx) och [hämta egenskaper för moln](https://msdn.microsoft.com/library/azure/ee460806.aspx) åtgärder, anges tootrue.

> [!NOTE]
> Den gör bara meningsfullt toocall återställning på en **lokalt** uppdatera eller uppgradera eftersom VIP-växling uppgraderingar inkluderar ersätta en hel körs instans av din tjänst med en annan.
>
>

Återställning av en pågående uppdatering har följande effekter på hello distribution hello:

* Alla rollinstanser som inte hade ännu har uppdaterats eller uppgraderade toohello ny version är inte uppdaterade eller uppgraderas eftersom dessa instanser körs redan hello Målversionen av hello-tjänsten.
* Någon roll instanser som redan har uppdaterats eller uppgraderade toohello ny version av hello-tjänsten (\*.cspkg) fil eller hello tjänstkonfiguration (\*.cscfg) är återställts toohello förberedande version av filen (eller båda) de här filerna.

Detta är funktionellt tillhandahålls av hello följande funktioner:

* Hej [återställning uppdatera eller uppgradera](https://msdn.microsoft.com/library/azure/hh403977.aspx) åtgärd som kan anropas på en uppdatering för configuration (utlöses genom att anropa [ändra distributionskonfiguration](https://msdn.microsoft.com/library/azure/ee460809.aspx)) eller en uppgradering (utlöses genom att anropa [ Uppgradera distributionen](https://msdn.microsoft.com/library/azure/ee460793.aspx)) så länge det finns minst en instans i hello tjänsten som ännu inte har uppdaterats toohello ny version.
* Hej låst och hello RollbackAllowed element, som returneras som en del av hello svarstexten av hello [hämta distribution](https://msdn.microsoft.com/library/azure/ee460804.aspx) och [hämta egenskaper för moln](https://msdn.microsoft.com/library/azure/ee460806.aspx) åtgärder:

  1. hello låst-elementet kan toodetect när en mutating åtgärden kan anropas på en viss distribution.
  2. Hej RollbackAllowed-elementet kan du toodetect när hello [återställning uppdatering eller uppgradera](https://msdn.microsoft.com/library/azure/hh403977.aspx) åtgärden kan anropas på en viss distribution.

  I ordning tooperform en återställning har inte toocheck både hello låst och hello RollbackAllowed element. Det tillräckligt tooconfirm RollbackAllowed anges tootrue. Dessa element returneras bara om de här metoderna anropas med hello begärandehuvudet inställt för ”x-ms-version: 2011-10-01” eller en senare version. Mer information om versionshantering huvuden finns [Management Versionhantering](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Det finns vissa situationer där en återställning av en uppdatering eller uppgradering stöds inte, de är följande:

* Minskad lokala resurser - tillåter om hello uppdateringen ökar hello lokala resurser för en roll hello Azure-plattformen inte att återställa.
* Kvotgränsen - ha om hello uppdatering var en skala ned åtgärden som du kanske inte längre tillräckligt beräkning kvoten toocomplete hello återställningsåtgärd. Varje Azure-prenumeration har en kvot som är associerade med den som anger hello högsta antalet kärnor som kan användas av alla värdbaserade tjänster som tillhör toothat prenumeration. Om en återställning av en viss uppdatering får din prenumeration via kvoten sedan som aktiveras en återställning inte.
* Konkurrenstillstånd - om hello ursprungliga uppdateringen har slutförts, en återställning är inte möjlig.

Är ett exempel på när hello återställningen av en uppdatering kan vara användbar om du använder hello [Uppgraderingsdistribution](https://msdn.microsoft.com/library/azure/ee460793.aspx) åtgärd i manuellt läge toocontrol hello hastighet med vilken en större på plats uppgradera tooyour Azure-värdtjänsten är distribuerat.

Under hello distributionen av hello uppgraderingen av du anropa [Uppgraderingsdistribution](https://msdn.microsoft.com/library/azure/ee460793.aspx) i manuellt läge och börja toowalk uppgraderingsdomäner. Om du någon gång när du övervakar hello uppgraderingen kan du Observera att vissa rollinstanser i hello första uppgradera domäner som du undersöker inte har svarar du anropa hello [återställning uppdatering eller uppgradera](https://msdn.microsoft.com/library/azure/hh403977.aspx) åtgärden hello-distribution som lämnar orörd hello-instanser som inte hade har uppgraderats och rollback-instanser som hade uppgraderat toohello tidigare servicepaket och konfiguration.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Initierar flera mutating åtgärder på ett pågående distribution
I vissa fall kanske du vill tooinitiate flera samtidiga mutating åtgärder på ett pågående distribution. Exempelvis kan du utföra en uppdatering på tjänsten och medan uppdateringen samlas över din tjänst du vill toomake har ändrats, t.ex. tooroll hello uppdateringen igen, använda en annan uppdatering eller ta bort hello-distribution. Om en Serviceuppgradering av innehåller buggy koden som orsakar en uppgraderad rollen instans toorepeatedly krasch är ett ärende där du kan behöva. I det här fallet hello Azure-Infrastrukturkontrollanten inte kan toomake förlopp vid tillämpning av uppgradera eftersom ett otillräckligt antal instanser i hello uppgraderade domän är felfria. Det här tillståndet är refererad tooas en *fastnat distribution*. Du kan få loss hello distributionen genom att återställa hello uppdatering eller använda en ny uppdatering över överkant hello misslyckas en.

När hello första begäran tooupdate eller uppgradering hello-tjänsten har tagits emot av hello Azure-Infrastrukturkontrollanten, kan du börja efterföljande mutera åtgärder. Det vill säga har du inte toowait för hello inledande åtgärden toocomplete innan du kan starta en annan mutating åtgärd.

Initierar en andra uppdateringsåtgärd medan hello första uppdateringen pågår utför liknande toohello rollback-åtgärd. Om hello andra uppdateringen i automatiskt läge hello första uppgraderingsdomänen uppgraderas omedelbart, eventuellt inledande tooinstances från flera uppgraderingsdomäner är offline på hello samma punkt i tid.

hello mutera operations är följande: [ändra distributionskonfiguration](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Uppgraderingsdistribution](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Distributionsstatus](https://msdn.microsoft.com/library/azure/ee460808.aspx), [ta bort Distribution](https://msdn.microsoft.com/library/azure/ee460815.aspx), och [återställning uppdatering eller uppgradering av](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Två åtgärder [hämta distribution](https://msdn.microsoft.com/library/azure/ee460804.aspx) och [hämta egenskaper för moln](https://msdn.microsoft.com/library/azure/ee460806.aspx), returnera hello låst flagga som kan vara undersökas toodetermine om en mutating åtgärden kan anropas på en viss distribution.

I ordning toocall hello versionen av dessa metoder som returnerar hello låst flaggan, måste du ange huvudet i begäran för ”x-ms-version: 2011-10-01” eller en senare. Mer information om versionshantering huvuden finns [Management Versionhantering](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Distributionen av roller uppgradera domäner
Azure fördelar instanser av en roll jämnt över ett antal uppgraderingsdomäner som kan konfigureras som en del av hello tjänstdefinitionsfilen (.csdef). Hej max antal uppgraderingsdomäner är 20 hello standardvärdet är 5. Läs mer om hur toomodify hello tjänstdefinitionsfilen [Azure Service Definition schemat (.csdef-fil)](cloud-services-model-and-package.md#csdef).

Till exempel om din roll har tio instanser kan innehåller som standard varje uppgraderingsdomänen två instanser. Om din roll har 14 instanser fyra hello uppgraderingsdomäner innehåller tre instanser och en femte domän innehåller två.

Uppgraderingsdomäner identifieras med ett Nollbaserat index: hello första uppgraderingsdomänen har ett ID 0 och hello andra uppgraderingsdomänen har ID 1 och så vidare.

hello följande diagram illustrerar hur en tjänst än innehåller två roller ska distribueras när hello service definierar två uppgraderingsdomäner. hello-tjänsten körs åtta instanser av hello-webbroll och nio instanser av hello worker-rollen.

![Distribution av Uppgraderingsdomäner](media/cloud-services-update-azure-service/IC345533.png "Distribution av Uppgraderingsdomäner")

> [!NOTE]
> Observera att Azure styr hur instanser fördelade över uppgraderingsdomäner. Det är inte möjligt toospecify vilka instanser som är allokerade toowhich domän.
>
>

## <a name="next-steps"></a>Nästa steg
[Hur tooManage Cloud Services](cloud-services-how-to-manage.md)  
[Hur tooMonitor Cloud Services](cloud-services-how-to-monitor.md)  
[Hur tooConfigure Cloud Services](cloud-services-how-to-configure.md)  
