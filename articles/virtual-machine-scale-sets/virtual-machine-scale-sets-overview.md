---
title: "aaaAzure virtuella skalningsuppsättningarna översikt | Microsoft Docs"
description: "Mer information om skalningsuppsättningar för virtuella datorer i Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 788b2d1636e0bf4ef3fbf94aed9b3303c5fafa82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Vad är skalningsuppsättningar för virtuella datorer i Azure?
Skaluppsättningar för den virtuella datorn är en Azure compute-resurs som du kan använda toodeploy och hantera en uppsättning identiska virtuella datorer. Med alla virtuella datorer som konfigurerats hello samma, skala som är utformad toosupport SANT Autoskala och ingen före etablering av virtuella datorer krävs. Det är därför enklare toobuild storskaliga tjänster som är riktade till stor beräkning och stordata av arbetsbelastningar.

För program som behöver tooscale beräkningsresurser ut och i skala balanserade operations implicit fel- och update-domäner. För en ytterligare introduktion tooscale anger information finns i toohello [Azure blogg meddelande](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Titta igenom dessa videor för mer information om skalningsuppsättningar:

* [Mark Russinovich berättar om Azure-skalningsuppsättningar](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Skaluppsättningar för virtuell dator med Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Skapa och hantera skalningsuppsättningar
Du kan skapa en skala som angetts i hello [Azure-portalen](https://portal.azure.com) genom att välja **nya** och skriva **skala** på hello sökfältet. **Virtuella datorns skaluppsättning** visas i hello resultat. Därifrån kan du fyller i hello krävs fält toocustomize och distribuera din skaluppsättning. Du kan också ha alternativ tooset grundläggande Autoskala regler baserat på CPU-användning i hello-portalen.

Du kan definiera och distribuera skalningsuppsättningar med hjälp av JSON-mallar och [REST API:er](https://msdn.microsoft.com/library/mt589023.aspx) precis som enskilda virtuella Azure Resource Manager-datorer. Därför går det att använda alla standardmetoder för Azure Resource Manager-distribution. Mer information om mallar finns i [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

Du kan hitta en uppsättning exempel mallar för virtuella datorn anger i hello [Azure Quickstart mallar GitHub-lagringsplatsen](https://github.com/Azure/azure-quickstart-templates). (Leta efter mallar med **vmss** i hello avdelning.)

Hello Quickstart mallen exempel länkar en ”distribuera tooAzure”-knappen i hello viktig information för varje mall toohello distribution av funktionen. Ange toodeploy hello skala klickar hello-knappen och fyll sedan i alla parametrar som krävs i hello-portalen. 

## <a name="scaling-a-scale-set-out-and-in"></a>Skala ut eller skala in en skalningsuppsättning
Du kan ändra hello kapacitet på en skala som angetts i hello Azure-portalen genom att klicka på hello **skalning** avsnittet **inställningar**. 

toochange skaluppsättning kapacitet på kommandoraden för hello använder hello **skala** i [Azure CLI](https://github.com/Azure/azure-cli). Till exempel använda det här kommandot tooset en scale set tooa kapacitet på 10 virtuella datorer:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

tooset hello antal virtuella datorer i en skala in med hjälp av PowerShell använder hello **uppdatering AzureRmVmss** kommando:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

tooincrease eller minska hello antal virtuella datorer på en skala ange med hjälp av en Azure Resource Manager-mall, ändra hello **kapacitet** egenskapen och distribuera om hello-mallen. Den här enkelhet gör det enkelt toointegrate skalningsuppsättningar med Azure Autoskala eller toowrite din egen anpassade skalning nivå om du behöver toodefine anpassad skala händelser som Azure-Autoskalningsfunktionen inte har stöd för. 

Om du omdistribuera en Azure Resource Manager mallen toochange hello kapacitet, kan du definiera en mycket mindre mall som innehåller endast hello **SKU** egenskapen paket med hello uppdateras kapacitet. [Här är ett exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

## <a name="autoscale"></a>Automatisk skalning

En skaluppsättning kan alternativt konfigureras med Autoskala inställningarna när den skapas i hello Azure-portalen. hello antal virtuella datorer kan sedan ökas eller minskas baserat på Genomsnittlig CPU-användning. 

Många av hello skala ange mallar i hello [Azure Quickstart mallar](https://github.com/Azure/azure-quickstart-templates) definiera Autoskala inställningar. Du kan också lägga till automatiska inställningar tooan befintlig skaluppsättning. Den här Azure PowerShell-skript lägger exempelvis till processorbaserad autoskalning tooa skaluppsättning:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

Du hittar en lista över giltiga mått tooscale på i [stöds mått med Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) under hello rubriken ”Microsoft.Compute/virtualMachineScaleSets”. Mer avancerade Autoskala alternativ är också tillgängliga, inklusive schemabaserade Autoskala och webhooks toointegrate med aviseringen system.

## <a name="monitoring-your-scale-set"></a>Övervaka din skalningsuppsättning
Hej [Azure-portalen](https://portal.azure.com) listor skala anger och visar deras egenskaper. hello portal stöder också hantering. Du kan utföra hanteringsåtgärder på både skalningsuppsättningar och enskilda virtuella datorer i en skalningsuppsättning. hello-portalen innehåller också ett diagram över anpassningsbara Resursanvändning. 

Om du behöver toosee eller redigera hello underliggande JSON-definitionen för en Azure-resurs, kan du också använda [resursutforskaren Azure](https://resources.azure.com). Skaluppsättningar är en resurs under hello Microsoft.Compute Azure-resursprovidern. Du kan se dem genom att expandera hello följande länkar från den här platsen:

**Prenumerationer** > **din prenumeration** > **resursgrupper ** > **providrar** > **Microsoft.Compute** > ** virtualMachineScaleSets** > **din skalningsuppsättning** -> osv.

## <a name="scale-set-scenarios"></a>Scenarier för skalningsuppsättningar
Det här avsnittet innehåller några vanliga scenarier för skalningsuppsättningar. Vissa Azure-tjänster på högre nivåer (som Batch, Service Fabric och Container Service) använder dessa scenarier.

* **Använd RDP eller SSH tooconnect tooscale ange instanser**: en skalningsuppsättning skapas i ett virtuellt nätverk och enskilda virtuella datorer i hello skaluppsättning allokeras inte offentliga IP-adresser som standard. Den här principen undviker hello utgifter och hantering omkostnader för att fördela separat offentliga IP-adresser tooall hello noder i beräknings-rutnätet. Om du behöver för att dirigera externa anslutningar tooscale ange virtuella datorer, kan du konfigurera en scale set tooautomatically tilldela offentliga IP-adresser toonew virtuella datorer. Du kan också ansluta tooVMs från andra resurser i ditt virtuella nätverk som kan tilldelas den offentliga IP-adresser, till exempel belastningsutjämnare och fristående virtuella datorer. 
* **Ansluta tooVMs med hjälp av NAT-regler**: kan du skapa en offentlig IP-adress, tilldelar den belastningsutjämnaren tooa och definiera ett inkommande NAT-pool. Dessa åtgärder mappar portar på hello IP-adress tooa port på en virtuell dator i skaluppsättning för hello. Exempel:
  
  | Källa | Källport | Mål | Målport |
  | --- | --- | --- | --- |
  |  Offentlig IP-adress |Port 50000 |vmss\_0 |Port 22 |
  |  Offentlig IP-adress |Port 50001 |vmss\_1 |Port 22 |
  |  Offentlig IP-adress |Port 50002 |vmss\_2 |Port 22 |
  
   I [det här exemplet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)NAT-regler är definierade tooenable en SSH-anslutning tooevery VM i en skaluppsättning, med hjälp av en enda offentlig IP-adress.
  
   [Det här exemplet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat) hello samma med RDP och Windows.
* **Ansluta tooVMs med hjälp av en ”jumpbox”**: Om du skapar en skalningsuppsättning och en fristående virtuell dator i samma virtuella nätverk, hello hello fristående virtuella datorn och hello skaluppsättning för virtuell dator kan ansluta tooone ett annat med hjälp av deras interna IP-adresser, som definieras av hello virtuella nätverk eller undernät. Om du skapar en offentlig IP-adress och tilldela den toohello fristående VM, kan du använda RDP eller SSH tooconnect toohello fristående VM. Du kan sedan ansluta från att tooyour skaluppsättning dator instanser. Nu kanske du ser att en enkel skalningsuppsättning i sig är säkrare än en enkel fristående virtuell dator med en offentlig IP-adress i standardkonfigurationen.
  
   Till exempel distribuerar [den här mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox) en enkel skalningsuppsättning med en fristående virtuell dator. 
* **Belastningsutjämning tooscale uppsättning instanser**: Om du vill toodeliver arbete tooa beräkningsklustret för virtuella datorer med hjälp av en metod för resursallokering, du kan konfigurera en Azure belastningsutjämnare med regler för belastningsutjämning av lager 4 därefter. Du kan definiera avsökningar tooverify som programmet körs genom att skriva portarna med ett angivet protokoll, intervall och sökväg för begäran. [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) stöder även skalningsuppsättningar tillsammans med layer-7 och mer avancerade scenarier för belastningsutjämning.
  
   [Det här exemplet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) skapar en skalningsuppsättning som körs Apache-webbservrar och använder en toobalance hello belastningsutjämnare som varje virtuell dator tar emot. (Titta på hello Microsoft.Network/loadBalancers resurstyp och networkProfile och extensionProfile i virtualMachineScaleSet).

   För [det här Linux-exemplet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) och [Windows-exemplet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) används Application Gateway.  

* **Distribuera en skalningsuppsättning som ett beräkningskluster i en PaaS-klusterhanterare**: Skalningsuppsättningar beskrivs ibland som nästa generationens arbetsroll. Trots att en giltig beskrivning körs det hello risken för förvirrande scale set-funktioner med Azure Cloud Services-funktioner. I en viss mening ger skalningsuppsättningar en sann arbetsroll eller arbetsresurs. Detta är en generaliserad beräkningsresurs som är plattforms-/körtidsoberoende, kan anpassas och integreras i Azure Resource Manager IaaS.
  
   En Cloud Services-arbetsroll är begränsad i termer av plattforms-/körtidsstöd (endast Windows-plattformsavbildningar). Men även tjänster som VIP-växling, konfigurerbara inställningar för uppgradering och körtids-/appdistributionsspecifika inställningar finns med. Dessa tjänster är *ännu* inte tillgängliga i skalningsuppsättningar eller också levereras de av andra PaaS-tjänster på högre nivå som Azure Service Fabric. Du kan tänka på skalningsuppsättningar som en infrastruktur som stöder PaaS. PaaS-lösningar som [Service Fabric](https://azure.microsoft.com/services/service-fabric/) bygger på denna infrastruktur.
  
   I [det här exemplet](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) med den här metoden distribuerar [Azure Container Service](https://azure.microsoft.com/services/container-service/) ett kluster baserat på skalningsuppsättningar med en behållardirigerare.

## <a name="scale-set-performance-and-scale-guidance"></a>Prestanda för skalningsuppsättningar och riktlinjer för skalning
* Skaluppsättning stöder upp too1 000 virtuella datorer. Om du skapar och överföra din egen anpassade VM-avbildningar är hello gränsen 100. Faktorer att överväga när du använder stora skalningsuppsättningar finns i [Arbeta med stora skalningsuppsättningar för virtuella datorer](virtual-machine-scale-sets-placement-groups.md).
* Du har inte toopre-skapa Azure storage-konton toouse skala uppsättningar. Skala anger support Azure hanterade diskar som negera prestandaproblem om hello antalet diskar per lagringskonto. Mer information finns i [skalningsuppsättningar och hanterade diskar för virtuella datorer i Azure](virtual-machine-scale-sets-managed-disks.md).
* Överväg att använda Azure Premium Storage i stället för Azure Storage för snabbare, mer förutsägbara etableringstider för virtuella datorer samt förbättrade IO-prestanda.
* Hej kärnkvot i hello region där du distribuerar begränsar hello antal virtuella datorer som du kan skapa. Du kan behöva toocontact kundsupport tooincrease kvotgränsen beräkning, även om du har en hög antalet kärnor för användning med Azure Cloud Services idag. tooquery din kvot som kör det här kommandot Azure CLI: `azure vm list-usage`. Eller kör detta PowerShell-kommando: `Get-AzureRmVMUsage`.

## <a name="frequently-asked-questions-for-scale-sets"></a>Vanliga frågor och svar om skalningsuppsättningar
**F.** Hur många virtuella datorer kan man ha i en skalningsuppsättning?

**S.** En skalningsuppsättning kan ha 0 too1, 000 virtuella datorer baserat på plattformen bilder eller 0 too100 virtuella datorer baserat på anpassade avbildningar. 

**F.** Kan datadiskar användas i skalningsuppsättningar?

**S.** Ja. En skalningsuppsättning kan definiera en bifogade data diskar konfiguration som gäller tooall virtuella datorer i hello uppsättningen. Mer information finns i [Azure scale sets and attached data disks (Azure-skalningsuppsättningar och anslutna datadiskar)](virtual-machine-scale-sets-attached-disks.md). Andra alternativ för att lagra data är:

* Azure-filer (delade SMB-enheter)
* OS-enhet
* Temporär enhet (lokal, backas inte upp av Azure Storage)
* Azure-datatjänst (t.ex. Azure-tabeller, Azure-blobbar)
* Extern datatjänst (t.ex. fjärrdatabas)

**F.** Vilka Azure-regioner har stöd för skalningsuppsättningar?

**S.** Alla regioner stöder skalningsuppsättningar.

**F.** Hur skapar jag en skalningsuppsättning med en anpassad avbildning?

**S.** Skapa en hanterad disk baserat på den anpassade virtuella hårddiskavbildningen (VHD) och referera till den i din mall för skalningsuppsättningen. [Här är ett exempel](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**F.** Om jag minska min skaluppsättning kapacitet från 20 too15 som virtuella datorer tas bort?

**S.** Virtuella datorer tas bort från hello skaluppsättningen jämnt över domäner för uppdatering och fel domäner toomaximize tillgänglighet. Virtuella datorer med hello högsta-ID tas bort först.

**F.** Vad händer om jag öka sedan hello kapacitet från 15 too18?

**S.** Om du ökar kapaciteten too18 skapas 3 nya virtuella datorer. Varje gång ökas hello VM instans-ID från hello tidigare högsta värdet (till exempel 20, 21, 22). Virtuella datorer balanseras mellan feldomäner och uppdateringsdomäner.

**F.** Kan jag framtvinga en körning av sekvensen när jag använder flera tillägg i en skalningsuppsättning?

**S.** Inte direkt, men för hello customScript-tillägg i skriptet kan vänta på ett annat tillägg toofinish. Du kan visa ytterligare vägledning om tillägget ordningsföljd i hello blogginlägget [tillägget ordningsföljd i Azure Skalningsuppsättningar](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**F.** Fungerar skalningsuppsättningar med Azures tillgänglighetsuppsättningar?

**S.** Ja. En skalningsuppsättning är en implicit tillgänglighetsuppsättning med 5 feldomäner och 5 uppdateringsdomäner. Skala uppsättningar med fler än 100 virtuella datorer sträcker sig över flera *placering grupper*, som är likvärdiga toomultiple tillgänglighetsuppsättningar. Mer information om placeringsgrupper finns i [Arbeta med stora skalningsuppsättningar för virtuella datorer](virtual-machine-scale-sets-placement-groups.md). En tillgänglighetsuppsättning för virtuella datorer kan finnas i hello samma virtuella nätverk som en skaluppsättning för virtuella datorer. En vanlig konfiguration är tooput control-noden virtuella datorer (vilket kräver ofta unik konfiguration) i en tillgänglighetsgrupp anger och placera datanoder i hello skaluppsättning.

Du kan hitta mer svar tooquestions om skala anger i hello [skalningsuppsättningarna för virtuella Azure-datorn vanliga frågor och svar](virtual-machine-scale-sets-faq.md).
