---
title: "aaaAzure virtuella säkerhetsmetoder | Microsoft Docs"
description: "Den här artikeln innehåller en mängd olika säkerhet bästa praxis toobe som används i virtuella datorer i Azure."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 5e757abe-16f6-41d5-b1be-e647100036d8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: b03bcc75fde6d49897f9a7f6f15aec87456edd0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-vm-security"></a>Metodtips för Virtuella Azure-säkerhet

I de flesta infrastruktur som en tjänst (IaaS)-scenarier [Azure virtuella datorer (VM)](https://docs.microsoft.com/en-us/azure/virtual-machines/) är computing hello huvudsakliga arbetsbelastningen för organisationer som använder molnet. Detta är särskilt tydligt i [hybridscenarion](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) där tooslowly för organisationer som vill migrera arbetsbelastningar toohello moln. I sådana fall följer hello [allmänna säkerhetsaspekter för IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx), och använda security best practices tooall dina virtuella datorer.

Den här artikeln beskrivs olika VM säkerhetsmetoder, var och en härledd från våra kunder och vår egen direkt upplevelser med virtuella datorer.

Metodtips för hello baseras på en konsensus meningar och de fungerar med den aktuella Azure-plattformen funktionerna och egenskapsuppsättningar. Eftersom åsikter och -tekniker kan förändras över tid, vi planerar tooupdate detta artikel regelbundet tooreflect ändringarna.

För varje bästa praxis artikeln hello:

* Vilka hello bästa praxis är.
* Därför är det en bra idé tooenable den.
* Hur du kan lära dig tooenable den.
* Vad som händer om du inte tooenable den.
* Möjliga alternativ toohello bästa praxis.

hello artikel undersöker hello följande säkerhetsmetoder för VM:

* VM-autentisering och åtkomstkontroll
* Åtkomst till VM tillgänglighet och nätverk
* Skydda data i vila i virtuella datorer genom att tillämpa kryptering
* Hantera dina VM-uppdateringar
* Hantera dina VM säkerhetstillståndet
* Övervaka prestanda för VM

## <a name="vm-authentication-and-access-control"></a>VM-autentisering och åtkomstkontroll

hello första steget för att skydda den virtuella datorn är tooensure att endast auktoriserade användare kan tooset in nya virtuella datorer. Du kan använda [Azure Resource Manager principer](../azure-resource-manager/resource-manager-policy.md) tooestablish konventioner för resurser i din organisation skapa anpassade principer och använda dessa principer tooresources som [resursgrupper](../azure-resource-manager/resource-group-overview.md).

Virtuella datorer som tillhör tooa resursgruppen naturligt ärver dess principer. Även om vi rekommenderar den här metoden toomanaging VMs, du kan också kontrollera åtkomst tooindividual VM principer med hjälp av [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md).

När du aktiverar Resource Manager och RBAC toocontrol VM kan förbättra du den övergripande VM säkerheten. Vi rekommenderar att du konsoliderar virtuella datorer med samma livslängd växla till hello hello samma resursgrupp. Med resursgrupper kan du distribuera, övervaka och dyker upp fakturering kostnaderna för dina resurser. tooenable användare tooaccess och konfigurera virtuella datorer kan använda en [minsta privilegium metoden](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models). Och när du tilldelar behörigheter toousers planera toouse hello följande inbyggda Azure roller:

- [Virtual Machine-deltagare](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor): kan hantera virtuella datorer, men inte hello virtuella nätverks- eller lagringsdrivrutiner konto toowhich-de är anslutna.
- [Klassiska Virtual Machine-deltagare](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor): hantera virtuella datorer som skapats med hjälp av hello klassiska distributionsmodellen, men inte hello virtuella nätverks- eller lagringsdrivrutiner konto toowhich hello virtuella datorer är anslutna.
- [Säkerhetshanteraren](../active-directory/role-based-access-built-in-roles.md#security-manager): hantera säkerhetskomponenter, säkerhetsprinciper och virtuella datorer.
- [DevTest Labs användaren](../active-directory/role-based-access-built-in-roles.md#devtest-labs-user): kan visa allt och ansluta, starta, starta om och stänga av virtuella datorer.

Dela inte konton och lösenord för administratörer och inte återanvända lösenord i flera användarkonton eller tjänster, särskilt lösenord för sociala medier eller andra icke-administrativa aktiviteter. Helst bör du använda [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) mallar tooset upp dina virtuella datorer på ett säkert sätt. Genom att använda den här metoden kan du förbättra dina val för distribution och tillämpa säkerhetsinställningar i hela hello distribution.

Organisationer som inte behöver använda data åtkomstkontroll genom att dra nytta av funktioner, till exempel RBAC kanske beviljar användarna fler behörigheter än vad som krävs. Olämpliga åtkomst toocertain användardata kan direkt påverka data.

## <a name="vm-availability-and-network-access"></a>Åtkomst till VM tillgänglighet och nätverk

Om den virtuella datorn kör kritiska program som behöver toohave hög tillgänglighet, rekommenderar vi starkt att du använder flera virtuella datorer. För bättre tillgänglighet, kan du skapa minst två virtuella datorer i hello [tillgänglighetsuppsättning](../virtual-machines/windows/tutorial-availability-sets.md).

[Azure belastningsutjämnare](../load-balancer/load-balancer-overview.md) kräver också att Utjämning av nätverksbelastning VMs hör toohello samma tillgänglighetsuppsättning. Om dessa virtuella datorer måste kunna nås från hello Internet, måste du konfigurera en [Internetriktade belastningsutjämnare](../load-balancer/load-balancer-internet-overview.md).

När virtuella datorer som är exponerade toohello Internet, är det viktigt att du [styr flödet i nätverkstrafiken med nätverkssäkerhetsgrupper (NSG: er)](../virtual-network/virtual-networks-nsg.md). Eftersom NSG: er kan vara tillämpade toosubnets, kan du minimera hello antalet NSG: er genom att gruppera dina resurser efter undernät och tillämpa NSG: er toohello undernät. hello avsikten är toocreate ett lager för isolering av nätverk, vilket du kan göra genom att konfigurera hello [nätverkssäkerhet](../best-practices-network-security.md) funktioner i Azure.

Du kan också använda funktionen för hello-in-time (JIT) VM-åtkomst från Azure Security Center toocontrol som har fjärråtkomst tooa specifik VM och hur länge.

Organisationer som inte framtvingar begränsningar för åtkomst till nätverket tooInternet riktade virtuella datorer är exponeras toosecurity risker, till exempel en Remote Desktop Protocol (RDP) Brute Force-attacker.

## <a name="protect-data-at-rest-in-your-vms-by-enforcing-encryption"></a>Skydda data i vila i dina virtuella datorer genom att tillämpa kryptering

[Datakryptering i viloläge](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) är ett obligatoriskt steg mot data sekretess-, efterlevnads- och suveränitet. [Azure Disk Encryption](../security/azure-security-disk-encryption.md) kan IT-administratörer tooencrypt Windows och Linux IaaS-VM-diskarna. Diskkryptering kombinerar hello standardiserade Windows BitLocker-funktionen och hello Linux dm-crypt funktionen tooprovide volymkryptering för hello OS och hello datadiskar.

Du kan använda Disk Encryption toohelp skydda dina data toomeet din organisations krav för säkerhet och efterlevnad. Företaget bör överväga att använda kryptering toohelp minimera risker relaterade toounauthorized dataåtkomst. Vi rekommenderar också att du krypterar enheter innan du skriver toothem känsliga data.

Vara säker på att tooencrypt din VM data volymer tooprotect dem i vila i Azure storage-konto. Skydda hello krypteringsnycklar och hemligheten med hjälp av [Azure Key Vault](https://azure.microsoft.com/en-us/documentation/articles/key-vault-whatis/).

Organisationer som inte behöver använda datakryptering är mer synliga toodata-problem. Till exempel obehöriga eller icke-registrerade användare stjäla data i avslöjade konton eller få obehörig åtkomst toodata kodad i ClearFormat. Förutom att på sådana risker, toocomply med industrins föreskrifter, måste du bekräfta att de utöva fordringar och använder rätt säkerhet kontroller tooenhance sina datasäkerhet företag.

toolearn mer om diskkryptering, se [Azure Disk Encryption för Windows och Linux IaaS-VM](azure-security-disk-encryption.md).


## <a name="manage-your-vm-updates"></a>Hantera dina VM-uppdateringar

Eftersom Azure virtuella datorer, som alla lokala virtuella datorer, är avsedda toobe användarhanterat, Azure push inte-toothem för Windows-uppdateringar. Du är dock uppmuntras tooleave hello automatisk Windows Update-inställningen är aktiverad. Ett annat alternativ är toodeploy [Windows Server Update Services (WSUS)](https://technet.microsoft.com/windowsserver/bb332157.aspx) eller en annan lämplig uppdateringshantering produkt antingen på en annan virtuell dator eller lokalt. Behåll virtuella datorer aktuella både WSUS och Windows Update. Vi rekommenderar också att du använder en sökning produkten tooverify som alla IaaS-VM är igång toodate.

Bilder som tillhandahålls av Azure är regelbundet uppdaterade tooinclude hello senaste avrunda av Windows-uppdateringar. Det finns dock ingen garanti för att hello avbildningar är aktuella vid tidpunkten för distribution. En liten fördröjning (av fler än några veckor) följande offentliga versioner är möjligt. Söker efter och installera alla Windows-uppdateringar ska vara hello första steget för varje distribution. Det här måttet är särskilt viktigt tooapply när du distribuerar avbildningar som kommer från dig eller dina egna bibliotek. Bilder som har angetts som en del av hello Azure Marketplace uppdateras automatiskt som standard.

Organisationer som inte tillämpa programuppdateringen principer är mer synliga toothreats som drar nytta av tidigare fast kända säkerhetsproblem. Företag måste bekräfta att de utöva fordringar och med rätt kontroller toohelp trygga hello arbetsbelastningen finns i molnet hello förutom riskera sådana hot toocomply med industrins föreskrifter.

Det är viktigt tooemphasize som programuppdateringen bästa praxis för vanliga datacenter och Azure IaaS har många likheter. Därför rekommenderar vi att du utvärderar din aktuella software update principer tooinclude virtuella datorer.

## <a name="manage-your-vm-security-posture"></a>Hantera dina VM säkerhetstillståndet

Cyber hot är under utveckling och skydda dina virtuella datorer kräver en omfattande övervakning kapaciteten som kan snabbt identifiera hot, förhindra obehörig åtkomst tooyour resurser, utlöser aviseringar och falsklarm. hello säkerhetstillståndet för sådana arbetsbelastning omfattar alla säkerhetsaspekterna av hello virtuell dator från update management toosecure nätverksåtkomst.

toomonitor hello säkerhetsläget i din [Windows](../security-center/security-center-virtual-machine.md) och [virtuella Linux-datorer](../security-center/security-center-linux-virtual-machine.md), använda [Azure Security Center](../security-center/security-center-intro.md). Skydda dina virtuella datorer i Azure Security Center genom att utnyttja hello följande funktioner:

* Tillämpa säkerhetsinställningar för OS med rekommenderade konfigurationsregler
* Identifiera och ladda ned system säkerhetsuppdateringar och viktiga uppdateringar som saknas
* Distribuera Endpoint program mot skadlig kod protection rekommendationer
* Validera diskkryptering
* Utvärdera och åtgärda problem
* Identifiera hot

Security Center kan övervaka aktivt för hot och potentiella hot som exponeras **säkerhetsaviseringar**. Korrelerade hot aggregeras i en enda vy som kallas **säkerhetsincident**.

toounderstand hur Security Center kan hjälpa dig att identifiera potentiella hot i dina virtuella datorer finns i Azure, se hello följande video:

<iframe src="https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Organisationer som inte framtvingar ett starkt säkerhetstillståndet för sina virtuella datorer vara medveten om eventuella försök av obehöriga användare toocircumvent upprätta säkerhetsåtgärder.

## <a name="monitor-vm-performance"></a>Övervaka prestanda för VM

Missbruk av resursen kan vara ett problem när VM processer använder mer resurser än de ska. Prestandaproblem med en virtuell dator kan leda tooservice avbrott, vilket överskrider hello säkerhetsprincipen för tillgänglighet. Därför det är absolut nödvändigt toomonitor VM åtkomst inte bara reaktivt, när ett problem uppstår, men även proaktivt mot baslinjeprestanda mätt under normal drift.

Genom att analysera [Azure diagnostikloggfiler](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), kan du övervaka dina VM-resurser och identifiera potentiella problem som kan påverka datorns prestanda och tillgänglighet. hello Azure Diagnostics tillägget innehåller övervakning och diagnostikfunktionerna för Windows-baserade virtuella datorer. Du kan aktivera dessa funktioner genom att inkludera hello-tillägget som en del av hello [Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md).

Du kan också använda [Azure-Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md) toogain inblick i dina resursens hälsotillstånd.

Organisationer som inte övervakar VM prestanda är toodetermine oavsett om vissa ändringar i prestandamönster är normal eller onormal. Om hello VM förbrukar mer resurser än normalt kan dessa avvikelser tyda på potentiella från en extern resurs eller en komprometterad process som körs i hello VM.
