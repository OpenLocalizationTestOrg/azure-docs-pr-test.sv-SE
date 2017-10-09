---
title: "aaaPlanning för migrering av IaaS-resurser från klassiska tooAzure Resource Manager | Microsoft Docs"
description: "Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/01/2017
ms.author: kasing
ms.openlocfilehash: 7574122d951119db4991187945739b190ef14995
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-tooazure-resource-manager"></a>Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager
Azure Resource Manager erbjuder många häpnadsväckande funktioner, men det är kritiska tooplan ut din migrering resa toomake saker att gå smidigt. Utgifter tid om hur du planerar säkerställer att det inte uppstår problem vid körning av migreringsaktiviteter.

> [!NOTE]
> följande riktlinjer hello var kraftigt överförda tooby hello Azure Customer Advisory team och Molnlösning arkitekter som arbetar med kunder på migrera stora miljöer. Det här dokumentet fortsätter som sådana tooget uppdateras när nya mönster av lyckade framkommer, så kom tillbaka från tid tootime toosee om det inte finns några nya rekommendationer.

Det finns fyra allmänna faser av hello migrering resa:<br>

![Migrering faser](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Planera

### <a name="technical-considerations-and-tradeoffs"></a>Teknisk information och nackdelar

Du kanske vill tooconsider beroende på dina tekniska krav storlek, geografiska områden och operativa metoder:

1. Varför önskas Azure Resource Manager för din organisation?  Vad är hello affärsskäl för migrering?
2. Vad är hello tekniska skäl för Azure Resource Manager?  Vad (eventuella) ytterligare Azure-tjänster skulle du som tooleverage?
3. Vilka program (eller uppsättningar av virtuella datorer) ingår i hello migrering?
4. Vilka scenarier stöds med hello migrering API?  Granska hello [stöds inte funktioner och konfigurationer](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#unsupported-features-and-configurations).
5. Kommer dina operativa team nu stöd för program/virtuella datorer i Azure Resource Manager och klassisk?
6. Hur (om alls) Azure Resource Manager ändras din VM-distribution, hantering, övervakning och rapportering av processer?  Behöver din distribution skript toobe uppdateras?
7. Vad är hello kommunikation planera tooalert intressenter (användare, ägare och infrastrukturens ägare)?
8. Beroende på hello komplexitet hello miljö, ska det vara en underhållsperiod där programmet hello är inte tillgänglig tooend användare och tooapplication ägare?  I så fall, hur länge?
9. Vad är hello utbildning plan tooensure intressenter är kunniga och proficient i Azure Resource Manager?
10. Vad är hello programhantering eller hantering av projektplanen för hello migrering?
11. Vad är hello tidslinjer för migrering av hello Azure Resource Manager och andra relaterade teknik väg maps?  De optimalt justeras?

### <a name="patterns-of-success"></a>Mönster för att lyckas

Lyckad kunder ha detaljerade planer där hello föregående frågor diskuteras, dokumenteras och omfattas.  Kontrollera hello planer är brett meddelas toosponsors och intressenter.  Förse dig med kunskap om migreringsalternativ; Vi rekommenderar starkt att du läsa igenom det här dokumentet som nedan.

* [Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Använd PowerShell toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Använd CLI toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-verktyg för att hjälpa till med migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska de vanligaste migreringsfelen](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="pitfalls-tooavoid"></a>Fallgropar tooavoid

- Fel tooplan.  hello teknik steg som krävs för migreringen är beprövade och hello resultatet är förutsägbara.
- Antagandet att hello plattform som stöds migrering API tar hänsyn till alla scenarier. Läs hello [stöds inte funktioner och konfigurationer](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#unsupported-features-and-configurations) toounderstand vilka scenarier som stöds.
- Planera inte potentiella avbrott i programmet för slutanvändare.  Planera otillräckligt med buffert tooadequately Varna slutanvändare kan vara otillgänglig programmet tid.


## <a name="lab-test"></a>Laboratorietest

**Replikera din miljö och gör en testmigrering**
  > [!NOTE]
  > Exakt replikering av din befintliga miljö körs med hjälp av ett community-bidragit verktyg som inte stöds officiellt av Microsoft-supporten. Därför är det en **valfria** steg, men det är hello toofind på bästa sätt problem utan att röra din produktionsmiljöer. Om verktyget en community-bidragit inte är ett alternativ, sedan läsa om hello Abort-Validera/Förbered testsändning rekommendation nedan.
  >

  Utföra ett laboratorietest för ditt exakt scenario (beräkning, nätverk och lagring) är hello bästa sätt tooensure en smidig migrering. Detta bidrar till:

  - Ett helt separata labb eller en befintlig icke-produktionsmiljö tootest. Vi rekommenderar en helt separata labb som kan migreras flera gånger och förstörande kan ändras.  Skript toocollect/hydrat metadata från hello verkliga prenumerationer visas nedan.
  - Det är ett bra toocreate hello labb i en separat prenumeration. hello orsaken är att hello lab torn flera gånger och har en separat isolerade prenumeration minskar hello chansen att något riktigt att av misstag tas bort.

  Detta kan åstadkommas med hjälp av hello AsmMetadataParser verktyget. [Läs mer om det här verktyget här](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Mönster för att lyckas

hello följande fanns problem som identifieras i många av hello större migreringar. Detta är inte en fullständig förteckning och du bör se toohello [stöds inte funktioner och konfigurationer](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#unsupported-features-and-configurations) för mer information.  Du kan eller inte kan stöta på dessa tekniska problem, men om du vill lösa dessa innan du försöker migrera säkerställer en jämnare upplevelse.

- **Gör en Avbryt-Validera/Förbered testsändning** -detta är kanske hello viktigaste steget tooensure klassisk tooAzure Resource Manager migreringen lyckades. hello migreringen API har tre steg: verifiera, förbereda och genomförande. Verifiera att läsa hello tillstånd för klassiska miljön och returnera ett resultat av alla frågor. Men eftersom vissa problem det kan finnas i hello Azure Resource Manager-stacken, kommer verifiera inte fånga allt. hello nästa steg i migreringen, Förbered hjälper exponera dessa problem. Förbereda kommer flytta hello metadata från klassiskt tooAzure Resource Manager, men kommer inte commit hello flyttningen, och kommer inte ta bort eller ändra någonting på hello klassisk sida. hello testsändning innebär att förbereda hello migreringen och sedan avbryts (**inte genomför**) förbereda hello migrering. hello målet med Validera/förbereda/Avbryt testsändning är toosee alla hello metadata i hello Azure Resource Manager-stacken, undersöka den (*programmässigt eller i portalen*), och kontrollera att allt migreras korrekt och gå igenom tekniska problem.  Den också ger dig en uppfattning om migreringstid så att du kan planera för avbrottstid i enlighet med detta.  En Validera/förbereda/Avbryt medför några driftavbrott; Därför är det utan avbrott tooapplication användning.
  - hello objekten nedan måste lösas innan hello testsändning toobe, men ett testsändning test kommer också på ett säkert sätt att tömma ut dessa förberedelsesteg om de saknas. Under migreringen enterprise hittades hello testsändning toobe ett säkert och ovärderlig sätt tooensure migreringsberedskap.
  - När förbereder körs hello kontrollen plan (Azure hanteringsåtgärder) kommer att låsas för hello hela virtuella nätverk, så att inga ändringar kan göras tooVM metadata under Validera/förbereda/Avbryt.  Men annars eventuella program-funktionen (RD, VM användning, etc.) påverkas.  Användare av virtuella datorer hello vet inte att hello testsändning körs.

- **Express Route-kretsar och VPN-**. Express Route-Gateways med auktorisering länkar kan för närvarande inte migreras utan driftavbrott. Hello lösning finns [migrera ExpressRoute-kretsar och associerade virtuella nätverk från hello klassiska toohello Resource Manager-distributionsmodellen](../../expressroute/expressroute-migration-classic-resource-manager.md).

- **VM-tillägg** -tillägg för virtuell dator är potentiellt en hello största förbi vägspärrar toomigrating virtuella datorer som körs. Reparation av VM-tillägg höjningen 1 – 2 dagar kan ta så planera på lämpligt sätt.  Ett aktivt Azure-agenten är nödvändiga tooreport tillbaka VM-tillägget status för virtuella datorer som körs. Om hello status kommer tillbaka som felaktigt för en VM som körs, stoppas detta migrering. själva hello-agenten inte behöver toobe i fungerande skick tooenable migrering, men om tillägg finns på hello VM, både en fungerande agent och utgående internet-anslutning (med DNS) som behövs för migrering toomove framåt.
  - Om anslutningen tooa DNS-servern går förlorad under migreringen, alla VM-tillägg utom BGInfo version 1. \* måste toofirst tas bort från varje VM innan migreringen förbereder och nytt läggs till i efterhand tillbaka toohello VM efter migreringen för Azure Resource Manager.  **Detta är endast för virtuella datorer som körs.**  Om hello virtuella datorer har stoppats frigjorts, behöver inte toobe bort VM-tillägg.

  > [!NOTE]
  > Många tillägg som Azure-diagnostik och security center övervakning kan installera sig igen efter migreringen så att de är inte ett problem.

  - Kontrollera dessutom Nätverkssäkerhetsgrupper inte är att begränsa utgående Internetåtkomst. Detta kan inträffa för vissa konfigurationer för Nätverkssäkerhetsgrupper. Utgående Internetåtkomst (och DNS) krävs för VM-tillägg toobe migreras tooAzure Resource Manager.
  - Finns två versioner av hello BGInfo-tillägget och kallas version 1 och 2.  

      - Om hello VM använder hello BGInfo-tillägget för version 1, kan du lämna det här tillägget är. hello migrering API hoppar du över det här tillägget. Hej BGInfo-tillägget kan läggas till efter migreringen.
      - Om hello VM använder hello JSON-baserade BGInfo version 2-tillägget, skapades hello VM med hello Azure-portalen. hello migrering API innehåller det här tillägget i hello migrering tooAzure hanteraren för filserverresurser, som hello agenten fungerar och har utgående Internetåtkomst (och DNS).

  - **Reparation alternativ 1**. Om du vet att dina virtuella datorer inte har utgående Internetåtkomst, en fungerande DNS-tjänsten och arbetar Azure-agenter på hello virtuella datorer, avinstallera alla VM-tillägg som en del av hello migrering innan Förbered kan du sedan installera om hello VM-tillägg efter migreringen.
  - **Reparation alternativ 2**. Om VM-tillägg är för stort för en överskrida ett annat alternativ är tooshutdown/Frigör alla virtuella datorer innan migreringen. Migrera hello frigjorts virtuella datorer och starta om dem på hello Azure Resource Manager-sida. här hello fördelen är att VM-tillägg ska migreras. hello Nackdelen är att alla externa virtuella IP-adresser kommer att försvinna (det kan vara en icke-starter), och naturligtvis hello VMs stängs orsakar en mycket större inverkan på fungerande program.

    > [!NOTE]
    > Om en princip för Azure Security Center konfigureras mot hello som kör virtuella datorer som migreras, hello säkerhetsprincip måste stoppas innan du tar bort tillägg toobe, annars hello säkerhet övervakning tillägget installeras automatiskt på hello VM efter ta bort den.

- **Tillgänglighetsuppsättningar** - för ett virtuellt nätverk (vNet) toobe migreras tooAzure Resource Manager, hello klassisk distribution (d.v.s. Molntjänsten) som finns virtuella datorer måste vara i en tillgänglighetsuppsättning eller hello virtuella datorer måste alla inte i någon tillgänglighetsuppsättning. Med mer än en tillgänglighetsuppsättning i hello molntjänst är inte kompatibel med Azure Resource Manager och stoppas migrering.  Dessutom kan kan det finnas vissa virtuella datorer i en tillgänglighetsuppsättning och vissa virtuella datorer inte i en tillgänglighetsuppsättning. tooresolve, du behöver tooremediate eller blandar Molntjänsten.  Planera därför detta kan ta lång tid.

- **Web/Worker-Rollsdistributioner** -molntjänster som innehåller webb-och arbetsroller kan inte migrera tooAzure Resource Manager. hello web/worker-roller måste först tas bort från hello virtuella nätverket innan migreringen kan starta.  En typisk lösning är toojust flytta web/worker rollen instanser tooa separat klassiskt virtuellt nätverk som även är länkade tooan ExpressRoute-krets eller toomigrate hello kod toonewer PaaS App Services (den här diskussionen är utanför hello i det här dokumentet). Hello f.d. omdistribuera fallet, skapa ett nytt klassiskt virtuellt nätverk, flytta/Omdistributionen hello web/worker roller toothat nytt virtuellt nätverk och ta bort hello distributioner från hello virtuella nätverk som flyttas. Inga kodändringar. hello nya [virtuella nätverk Peering](../../virtual-network/virtual-network-peering-overview.md) kapaciteten kan använda toopeer tillsammans hello klassiska virtuella nätverk som innehåller hello web/worker-roller och andra virtuella nätverk i hello samma Azure-region som hello virtuella nätverk som Migrera (**när virtuella nätverk migreringen är klar som peerkoppla virtuella nätverk som inte kan migreras**), därför att erbjuda hello samma funktioner varken prestanda och ingen fördröjning/bandbredd påföljder. Eftersom hello [virtuella nätverk Peering](../../virtual-network/virtual-network-peering-overview.md), web/worker-rollsdistributioner kan nu enkelt begränsas och inte blockerar hello migrering tooAzure Resource Manager.

- **Azure Resource Manager-kvoter** -Azure-regioner har separata kvoter/gränser för både klassiska och Azure Resource Manager. Även om ny maskinvara inte används i ett Migreringsscenario *(vi växling befintliga virtuella datorer från klassiskt tooAzure Resource Manager)*, Azure Resource Manager kvoter måste fortfarande toobe på plats med tillräckligt med kapacitet innan migreringen kan starta. Nedan visas hello större gränser som vi har sett orsaka problem.  Öppna en kvot stöd biljett tooraise hello begränsar.

    > [!NOTE]
    > Dessa gränser måste aktiveras i toobe hello samma region som din aktuella miljö toobe migreras.
    >

    - Nätverksgränssnitt
    - Belastningsutjämnare
    - Offentliga IP-adresser
    - Statiska offentliga IP-adresser
    - Kärnor
    - Nätverkssäkerhetsgrupper
    - Routningstabeller

    Du kan kontrollera din aktuella Azure Resource Manager-kvoter med hello följande kommandon med hello senaste versionen av Azure PowerShell.

    **Beräkna** *(kärnor, Tillgänglighetsuppsättningar)*

    ```powershell
    Get-AzureRmVMUsage -Location <azure-region>
    ```

    **Nätverket** *(virtuella nätverk, statiska offentliga IP-adresser, offentliga IP-adresser, säkerhetsgrupper för nätverk, nätverket gränssnitt, belastningsutjämnare, vägtabeller)*

    ```powershell
    Get-AzureRmUsage /subscriptions/<subscription-id>/providers/Microsoft.Network/locations/<azure-region> -ApiVersion 2016-03-30 | Format-Table
    ```

    **Lagring** *(Lagringskonto)*

    ```powershell
    Get-AzureRmStorageUsage
    ```

- **Azure Resource Manager API throttling-begränsning** - om du har en tillräckligt stor miljö (t.ex. > 400 virtuella datorer i ett virtuellt nätverk), du kan träffa hello standard API begränsning begränsningar för skrivning (för närvarande `1200 writes/hour`) i Azure Resource Manager. Innan du börjar migreringen måste bör du höja ett stöd biljett tooincrease gränsen för din prenumeration.


- **Etableringsstatus tidsgränsen ut VM** – om någon virtuell dator har hello statusen `provisioning timed out`använder den här toobe måste matcha före migrering. hello bara toodo detta är avbrott som avetablering/reprovisioning hello VM (ta bort det, hålla hello disk och återskapa hello VM).

- **RoleStateUnknown VM-statusen** - om migrering stoppas på grund av tooa `role state unknown` fel visas, inspektera hello VM med hjälp av hello portal och se till att den körs. Det här felet försvinner vanligtvis dess äger (ingen reparation krävs) efter några minuter och ofta övergående typen ofta visas under en virtuell dator `start`, `stop`, `restart` åtgärder. **Rekommenderad praxis:** nytt försök migrera igen efter några minuter.

- **Fabric-kluster finns inte** – i vissa fall kan vissa virtuella datorer som inte kan migreras för olika udda skäl. En av dessa kända fall är om hello VM skapades nyligen (inom hello förra veckan eller liknande) och har hänt tooland ett Azure-kluster som inte ännu är utrustade för Azure Resource Manager-arbetsbelastningar.  Du får ett felmeddelande som säger `fabric cluster does not exist` och hello VM inte kan migreras. Väntar på ett par dagar kommer vanligtvis Lös det här problemet enligt hello klustret kommer snart Azure Resource Manager är aktiverad. Dock är en omedelbar lösning för`stop-deallocate` hello VM fortsätta vidarebefordra migreringen och starta hello VM säkerhetskopiera i Azure Resource Manager när du har migrerat.

### <a name="pitfalls-tooavoid"></a>Fallgropar tooavoid

- Inte ta genvägar och utelämna hello Validera/förbereda/Avbryt testsändning migreringar.
- De flesta, om inte alla, kommer av din potentiella problem yta under hello Validera/förbereda/Avbryt steg.

## <a name="migration"></a>Migrering

### <a name="technical-considerations-and-tradeoffs"></a>Teknisk information och nackdelar

Nu är du redo eftersom du har arbetat via hello kända problem med din miljö.

Du kanske vill tooconsider för verkliga hello-migreringar:

1. Planera och schemalägga hello virtuella nätverk (minsta enheten av migrering) med ökande prioritet.  Enkel virtuella nätverk hello först och förloppet med hello mer komplicerade virtuella nätverk.
2. De flesta kunder har icke-produktion och produktionsmiljöer.  Schemalägga produktion senast.
3. **(VALFRITT)**  Schemalägga ett Underhåll driftstopp med mycket buffert ifall oväntade problem uppstå.
4. Kommunicerar med och justera med din supportteam om problem uppstår.

### <a name="patterns-of-success"></a>Mönster för att lyckas

Hej teknisk information från hello _Test Lab_ avsnitt bör övervägas och hindra tidigare tooa verkliga migrering.  Med lämpliga tester är hello migreringen faktiskt en händelsebaserad.  För produktionsmiljöer, kan det vara bra toohave ytterligare stöd, till exempel en betrodd Microsoft-partner eller Microsoft Premier-tjänster.

### <a name="pitfalls-tooavoid"></a>Fallgropar tooavoid

Testa inte helt kan orsaka problem och fördröjning i hello migrering.  

## <a name="beyond-migration"></a>Utöver migreringen

### <a name="technical-considerations-and-tradeoffs"></a>Teknisk information och nackdelar

Nu när du är i Azure Resource Manager maximera hello-plattformen.  Läs hello [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) toofind ut om ytterligare fördelar.

Tooconsider saker:

- Paketering hello migrering med andra aktiviteter.  Merparten av kunderna välja en underhållsperiod för programmet.  Om så kanske du vill toouse detta driftstopp tooenable andra Azure Resource Manager-funktioner som kryptering och migrering tooManaged diskar.
- Kontrollera igen hello tekniska och affärsskäl för Azure Resource Manager; Aktivera hello ytterligare tjänster som är tillgängliga bara på Azure Resource Manager som gäller tooyour miljö.
- Modernisera din miljö med PaaS-tjänster.

### <a name="patterns-of-success"></a>Mönster för att lyckas

Vara ändamålsenlig på vilka tjänster du nu vill tooenable i Azure Resource Manager.  Många kunder hitta hello nedan intressant för sina miljöer i Azure:

- [Rollbaserad åtkomstkontroll](../../azure-resource-manager/resource-group-overview.md#access-control).
- [Azure Resource Manager-mallar för distribution av enklare och mer kontrollerad](../../azure-resource-manager/resource-group-overview.md#template-deployment).
- [Taggar](../../azure-resource-manager/resource-group-using-tags.md).
- [Kontroll av aktiviteten](../../azure-resource-manager/resource-group-audit.md)
- [Resursprinciper](../../azure-resource-manager/resource-manager-policy.md)

### <a name="pitfalls-tooavoid"></a>Fallgropar tooavoid

Kom ihåg varför du startade den här klassisk tooAzure Resource Manager migrering resa.  Vilka är hello ursprungliga affärsskäl? Har du uppnått hello affärsskäl?


## <a name="next-steps"></a>Nästa steg

* [Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Använd PowerShell toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Använd CLI toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-verktyg för att hjälpa till med migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska de vanligaste migreringsfelen](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
