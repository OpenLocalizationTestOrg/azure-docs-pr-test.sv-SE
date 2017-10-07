---
title: aaaResource Manager och klassisk distribution | Microsoft Docs
description: "Beskriver distributionsmodell för hello skillnaderna mellan hello Resource Manager-modellen och klassiska hello (eller Service Management)."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7ae0ffa3-c8da-4151-bdcc-8f4f69290fb4
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: tomfitz
ms.openlocfilehash: fbf1959991b100547a459bf88a29c0afbc8592e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-hello-state-of-your-resources"></a>Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och hello för dina resurser
I det här avsnittet får du lära dig om Azure Resource Manager och klassiska distributionsmodeller hello tillståndet för dina resurser och varför dina resurser har distribuerats med en eller hello andra. hello Resource Manager och klassiska distributionsmodeller representerar två olika sätt att distribuera och hantera dina Azure-lösningar. Du arbetar med dem via två uppsättningar API och hello distribuerade resurser kan innehålla viktiga skillnader. hello två modeller är inte helt kompatibla med varandra. Det här avsnittet beskrivs dessa skillnader.

toosimplify hello distribution och hantering av resurser, Microsoft rekommenderar att du använder Resource Manager för alla nya resurser. Om möjligt rekommenderar Microsoft att du distribuerar befintliga resurser via Resource Manager.

Om du är ny tooResource Manager kan du behöva toofirst granska hello-terminologi som definierats i hello [översikt över Azure Resource Manager](resource-group-overview.md).

## <a name="history-of-hello-deployment-models"></a>Historik över hello distributionsmodeller
Azure som ursprungligen endast hello som klassiska distributionsmodellen. I den här modellen fanns varje resurs självständigt. Det gick inte toogroup relaterade resurser tillsammans. I stället du hade toomanually spåra vilka resurser som består av din lösning eller ditt program och Kom ihåg toomanage dem i ett samordnat sätt. toodeploy en lösning, hade tooeither skapa varje resurs individuellt via hello klassiska portalen eller skapa ett skript som distribuerats alla hello resurser i hello rätt ordning. toodelete en lösning var du tvungen toodelete varje resurs individuellt. Du kan inte enkelt tillämpa och uppdatera principer för åtkomstkontroll för relaterade resurser. Slutligen kan du inte använda taggar tooresources toolabel dem med villkor som hjälper dig övervaka dina resurser och hantera fakturering.

I 2014 introducerade Azure Resource Manager som läggs till hello konceptet för en resursgrupp. En resursgrupp är en behållare för resurser som delar en gemensam livscykel. hello Resource Manager-modellen har flera fördelar:

* Du kan distribuera, hantera och övervaka alla hello-tjänster för din lösning som en grupp i stället för att hantera dessa tjänster individuellt.
* Du kan upprepade gånger distribuera lösningen under hela dess livscykel och vara säker på att dina resurser distribueras i ett konsekvent tillstånd.
* Du kan använda access control tooall resurser i resursgruppen och dessa principer tillämpas automatiskt när nya resurser läggs toohello resursgruppen.
* Du kan lägga till taggar tooresources toologically ordna alla hello resurser i din prenumeration.
* Du kan använda JavaScript Object Notation (JSON) toodefine hello infrastrukturen för lösningen. hello JSON-fil kallas en Resource Manager-mall.
* Du kan definiera hello beroenden mellan resurser så att de distribueras i rätt ordning för hello.

När Resource Manager lades till i alla resurser lades retroaktivt toodefault resursgrupper. Om du skapar en resurs via nu klassisk distribution, skapas automatiskt hello resurs inom en standard resursgrupp för tjänsten, även om du inte anger den resursgruppen i distributionen. Dock innebär bara befintliga inom en resursgrupp inte att hello resurs har konverterade toohello Resource Manager-modellen. Vi ska titta på hur varje tjänst hanterar hello två distributionsmodeller i hello nästa avsnitt. 

## <a name="understand-support-for-hello-models"></a>Förstå stöd för hello modeller
När du bestämmer vilka distribution modellen toouse för dina resurser, finns det tre scenarier toobe medveten om:

1. hello tjänsten stöder Resource Manager och innehåller bara en enda typ.
2. hello tjänsten stöder Resource Manager men innehåller två typer - en för hanteraren för filserverresurser och en för klassisk. Det här scenariot gäller endast toovirtual datorer, lagringskonton och virtuella nätverk.
3. hello-tjänsten har inte stöd för hanteraren för filserverresurser.

toodiscover om en tjänst stöder hanteraren för filserverresurser, se [resursproviders och typer](resource-manager-supported-services.md).

Om hello-tjänsten som du vill toouse inte stöder Resource Manager, måste du fortsätta använda klassisk distribution.

Om hello tjänsten stöder Resource Manager och **är inte** en virtuell dator, storage-konto eller virtuella nätverk kan du använda Resource Manager utan några problem.

För virtuella datorer, lagringskonton och virtuella nätverk, om hello resursen har skapats via klassisk distribution fortsätta du toooperate på den via klassiska åtgärder. Om hello virtuell dator, storage-konto eller virtuella nätverket har skapats via Resource Manager-distribution, måste du fortsätta att använda Resource Manager-åtgärder. Denna skillnad kan få förvirrande när prenumerationen innehåller en blandning av resurser som skapats via Resource Manager och klassisk distribution. Den här kombinationen av resurser kan skapa oväntade resultat eftersom hello resurser inte har stöd för hello samma åtgärder.

I vissa fall kan en Resource Manager-kommandot kan hämta information om en resurs som skapats via klassisk distribution eller kan utföra administrativa åtgärder, som att flytta en klassisk resurs tooanother resursgrupp. Men dessa fall ger inte hello intryck av att hello stöder Resource Manager-åtgärder. Anta att du har en resursgrupp som innehåller en virtuell dator som har skapats med klassisk distribution. Om du kör hello följande Resource Manager PowerShell-kommando:

```powershell
Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines
```

Den returnerar hello virtuell dator:

```powershell
Name              : ExampleClassicVM
ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
ResourceName      : ExampleClassicVM
ResourceType      : Microsoft.ClassicCompute/virtualMachines
ResourceGroupName : ExampleGroup
Location          : westus
SubscriptionId    : {guid}
```

Dock hello Resource Manager cmdlet **Get-AzureRmVM** returnerar endast virtuella datorer distribueras via Resource Manager. hello returnerar följande kommando inte hello virtuell dator som skapats via klassisk distribution.

```powershell
Get-AzureRmVM -ResourceGroupName ExampleGroup
```

Endast resurser som skapats via Hanteraren för filserverresurser stöd taggar. Du kan inte använda taggar tooclassic resurser.

## <a name="resource-manager-characteristics"></a>Resource Manager-egenskaper
toohelp du förstår hello två modeller, nu ska vi se hello egenskaperna för Resource Manager-typer:

* Skapats via hello [Azure-portalen](https://portal.azure.com/).
  
     ![Azure Portal](./media/resource-manager-deployment-model/portal.png)
  
     Du har hello möjlighet att använda Resource Manager eller klassisk distribution för bearbetning, lagring och nätverksresurser. Välj **Resource Manager**.
  
     ![Resource Manager-distribution](./media/resource-manager-deployment-model/select-resource-manager.png)
* Skapa hello Resource Manager version av hello Azure PowerShell-cmdlets. Dessa kommandon har hello format *Verb AzureRmNoun*.

  ```powershell
  New-AzureRmResourceGroupDeployment
  ```

* Skapats via hello [Azure Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) för REST-åtgärder.
* Skapats via Azure CLI-kommandon som körs i hello **arm** läge.
  
  ```azurecli
  azure config mode arm
  azure group deployment create
  ```

* hello resurstypen omfattar inte **(klassiskt)** i hello namn. hello följande bild visar hello typ som **lagringskonto**.
  
    ![webbapp](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Klassisk distribution egenskaper
Du kan också känna hello klassiska distributionsmodellen som hello Service Management-modell.

Resurser som skapats i hello klassisk distribution modellen resursen hello följande egenskaper:

* Skapats via hello [klassiska portalen](https://manage.windowsazure.com)
  
     ![Klassisk portal](./media/resource-manager-deployment-model/classic-portal.png)
  
     Eller, hello Azure-portalen och du anger **klassiska** distribution (för bearbetning, lagring och nätverk).
  
     ![Klassisk distribution](./media/resource-manager-deployment-model/select-classic.png)
* Skapats via hello Service Management-versionen av hello Azure PowerShell-cmdlets. Dessa kommandonamn har hello format *Verb AzureNoun*.

  ```powershell
  New-AzureVM
  ```

* Skapats via hello [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) för REST-åtgärder.
* Skapats via Azure CLI-kommandon som körs i **asm** läge.

  ```azurecli
  azure config mode asm
  azure vm create
  ```
   
* hello resurstypen innehåller **(klassiskt)** i hello namn. hello följande bild visar hello typ som **Storage-konto (klassisk)**.
  
    ![Klassisk typ](./media/resource-manager-deployment-model/classic-type.png)

Du kan använda hello Azure portal toomanage resurser som har skapats via klassisk distribution.

## <a name="changes-for-compute-network-and-storage"></a>Ändringar för beräkning, nätverk och lagring
hello visar följande diagram beräkning, nätverk och lagring resurser har distribuerats via Resource Manager.

![Resurshanterarens arkitektur](./media/resource-manager-deployment-model/arm_arch3.png)

Observera hello efter relationer mellan hello resurser:

* Alla hello resurser finns inom en resursgrupp.
* hello virtuell dator beror på en specifik storage-konto som definierats i hello Storage resource provider toostore diskarna i blob storage (krävs).
* hello virtuella refererar till ett specifikt nätverkskort som definierats i hello nätverksresursprovidern (krävs) och en tillgänglighetsuppsättning definierade i hello Compute-resursprovidern (valfritt).
* hello NIC referenser hello virtuella datorns tilldelade IP-adress (obligatoriskt), hello undernät av hello virtuellt nätverk för hello virtuella datorn (krävs) och tooa Nätverkssäkerhetsgruppen (valfritt).
* hello undernät inom ett virtuellt nätverk refererar till en Nätverkssäkerhetsgrupp (valfritt).
* hello belastningen belastningsutjämnaren instansen refererar till hello serverdelspool IP-adresser som inkluderar hello nätverkskort på en virtuell dator (valfritt) och refererar till en offentlig eller privat IP-belastningsutjämnaradress (valfritt).

Här följer hello komponenter och deras relationer för klassisk distribution:

![klassiska arkitektur](./media/resource-manager-deployment-model/arm_arch1.png)

klassiska hello-lösningen som värd för en virtuell dator innehåller:

* En nödvändig molntjänst som fungerar som en behållare för värd för virtuella datorer (beräkning). Virtuella datorer tillhandahålls automatiskt med ett nätverkskort (NIC) och en IP-adress som tilldelats av Azure. Dessutom hello tjänst i molnet innehåller en instans för externa belastningsutjämnaren, en offentlig IP-adress och standard slutpunkter tooallow remote fjärrskrivbord och PowerShell för Windows-baserade virtuella datorer och SSH (Secure Shell) trafik för Linux-baserade virtuella datorer.
* Ett obligatoriskt lagringskonto som lagrar hello virtuella hårddiskar för en virtuell dator, inklusive hello operativsystem, tillfällig och ytterligare datadiskar (lagring).
* Ett valfritt virtuellt nätverk som fungerar som en ytterligare behållare som du kan skapa en uppdelade struktur och ange hello undernätet på vilka hello virtuella datorn finns (nätverk).

hello i den följande tabellen beskrivs ändringar i hur beräknings-, nätverks- och Storage-resursprovidrar samverkar:

| Objekt | Klassisk | Resource Manager |
| --- | --- | --- |
| Molntjänst för Virtual Machines |Molntjänst var en behållare för hello virtuella datorer som krävde tillgänglighet från hello plattformen och belastningsutjämning. |Molntjänst är inte längre ett objekt som krävs för att skapa en virtuell dator med en ny modell för hello. |
| Virtuella nätverk |Ett virtuellt nätverk är valfri för hello virtuell dator. Om ingår, kan hello virtuellt nätverk distribueras med Resource Manager. |Virtuell dator kräver ett virtuellt nätverk som har distribuerats med Resource Manager. |
| Lagringskonton |hello virtuell dator kräver ett lagringskonto som lagrar hello virtuella hårddiskar för hello operativsystem, tillfällig och ytterligare datadiskar. |hello virtuella datorn kräver en storage-konto toostore diskarna i blob storage. |
| Tillgänglighetsuppsättningar |Tillgänglighet toohello plattformen indikerades genom att konfigurera hello samma ”AvailabilitySetName” på hello virtuella datorer. hello maximala antalet feldomäner var 2. |Tillgänglighetsuppsättning är en resurs som exponeras av Microsoft.Compute-providern. Virtuella datorer som kräver hög tillgänglighet måste inkluderas i hello Tillgänglighetsuppsättning. hello maximala antalet feldomäner är nu 3. |
| Tillhörighetsgrupper |Tillhörighetsgrupper krävdes för virtuella nätverk. Men med hello introduktionen av regionala virtuella nätverk, som behövs inte längre. |toosimplify, hello Tillhörighetsgrupper konceptet finns inte i hello API: er som exponeras via Azure Resource Manager. |
| Belastningsutjämning |Skapa en molntjänst ger en implicit belastningsutjämnare för hello virtuella datorer distribueras. |hello belastningsutjämnare är en resurs som exponeras av Microsoft.Network-providern för hello. hello primära nätverksgränssnittet för hello virtuella datorer som behöver toobe belastningsutjämnas ska referera till hello belastningsutjämnaren. Belastningsutjämnare kan vara interna eller externa. En belastningen belastningsutjämnaren instans refererar hello serverdelspool IP-adresser som inkluderar hello nätverkskort på en virtuell dator (valfritt) och refererar till en offentlig eller privat IP-belastningsutjämnaradress (valfritt). [Läs mer.](../virtual-network/resource-groups-networking.md) |
| Virtuell IP-adress |Cloud Services får en standard-VIP (virtuell IP-adress) när en virtuell dator läggs tooa Molntjänsten. hello virtuella IP-adressen är hello-adress kopplad till hello implicita belastningsutjämnaren. |Offentliga IP-adressen är en resurs som exponeras av Microsoft.Network-providern för hello. Den offentliga IP-adressen kan vara statisk (reserverad) eller dynamisk. Dynamiska offentliga IP-adresser kan tilldelas tooa belastningsutjämnaren. Offentliga IP-adresser kan skyddas med hjälp av säkerhetsgrupper. |
| Reserverad IP-adress |Du kan reservera en IP-adress i Azure och associera den med en molnbaserad tjänst tooensure som hello IP-adressen är fäst. |Offentlig IP-adress kan skapas i ”statiskt” läge och det erbjudanden hello samma funktion som en ”reserverad IP-adress”. Statiska offentliga IP-adresser kan endast tilldelas tooa belastningsutjämnaren just nu. |
| Offentlig IP-adress (PIP) per VM |Offentliga IP-adresser kan också vara associerade tooa VM direkt. |Offentliga IP-adressen är en resurs som exponeras av Microsoft.Network-providern för hello. Den offentliga IP-adressen kan vara statisk (reserverad) eller dynamisk. Endast dynamiska offentliga IP-adresser kan dock vara tilldelade tooa nätverksgränssnittet tooget en offentlig IP-adress per VM just nu. |
| Slutpunkter |Ange slutpunkter behövs toobe som konfigurerats på en virtuell dator toobe öppna upp anslutningar på vissa portar. En av hello vanligt sätt att ansluta toovirtual datorer är genom att konfigurera inkommande slutpunkter. |Inkommande NAT-regler kan konfigureras på belastningsutjämnare tooachieve hello samma funktion för att aktivera slutpunkter på specifika portar för att ansluta toohello virtuella datorer. |
| DNS-namn |En molntjänst får ett implicit, globalt-unikt DNS-namn. Till exempel: `mycoffeeshop.cloudapp.net`. |DNS-namn är valfria parametrar som kan anges för en offentlig IP-adressresurs. hello FQDN är i hello följande format: `<domainlabel>.<region>.cloudapp.azure.com`. |
| Nätverksgränssnitt |Primära och sekundära nätverksgränssnitt och dess egenskaper har definierats som nätverkskonfigurationen för en virtuell dator. |Nätverksgränssnittet är en resurs som exponeras av Microsoft.Network-providern. hello livscykeln för hello nätverksgränssnittet inte är kopplad tooa virtuella datorn. Den refererar till hello virtuella datorns tilldelade IP-adress (obligatoriskt), hello undernät av hello virtuellt nätverk för hello virtuella datorn (krävs) och tooa Nätverkssäkerhetsgruppen (valfritt). |

toolearn om att ansluta virtuella nätverk från olika distributionsmodeller finns [ansluta virtuella nätverk från olika distributionsmodeller i hello portal](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-tooresource-manager"></a>Migrera från klassiska tooResource Manager
Om du är klar toomigrate se dina resurser från klassiska distributionen tooResource Manager-distribution:

1. [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-deep-dive.md)
2. [Migrering av plattform som stöds av IaaS-resurser från klassiska tooAzure Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-overview.md)
3. [Migrera IaaS-resurser från klassiska tooAzure Resource Manager med hjälp av Azure PowerShell](../virtual-machines/windows/migration-classic-resource-manager-ps.md)
4. [Migrera IaaS-resurser från klassiska tooAzure Resource Manager med hjälp av Azure CLI](../virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
**Kan jag skapa en virtuell dator med Azure Resource Manager toodeploy i ett virtuellt nätverk som skapats med hjälp av klassisk distribution?**

Detta stöds inte. Du kan inte använda Azure Resource Manager toodeploy en virtuell dator till ett virtuellt nätverk som skapades med klassisk distribution.

**Kan jag skapa en virtuell dator med hello Azure Resource Manager från en användaravbildning som skapades med hello Azure Service Management API: er?**

Detta stöds inte. Du kan dock kopiera hello VHD-filer från ett lagringskonto som skapades med hello Service Management API: er och lägga till dem tooa nytt konto via Azure Resource Manager.

**Vad är hello inverkan på hello kvoten för min prenumeration?**

hello kvoter för hello virtuella datorer, virtuella nätverk och lagringskonton som skapats via hello Azure Resource Manager skiljer sig från andra kvoter. Varje prenumeration hämtar kvoter toocreate hello resurser med hjälp av hello nya API: er. Du kan läsa mer om hello ytterligare kvoter [här](../azure-subscription-service-limits.md).

**Kan jag fortsätta toouse Mina automatiserade skript för etablering av virtuella datorer, virtuella nätverk och lagringskonton via hello Resource Manager API: er?**

Alla hello automatisering och skript som du har skapat fortsätta toowork för hello befintliga virtuella datorer, virtuella nätverk som skapats under hello Azure Service Management-läge. Hello skript har dock toobe uppdaterade toouse hello nytt schema för att skapa hello samma resurser via hello Resource Manager-läget.

**Var hittar jag exempel på Azure Resource Manager-mallar**

En omfattande uppsättning startmallar finns på [Azure Resource Manager-Snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Nästa steg
* toowalk genom hello skapa mallen som definierar en virtuell dator, storage-konto och virtuella nätverk finns [genomgång av Resource Manager-mall](resource-manager-template-walkthrough.md).
* toosee hello kommandon för att distribuera en mall finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).

