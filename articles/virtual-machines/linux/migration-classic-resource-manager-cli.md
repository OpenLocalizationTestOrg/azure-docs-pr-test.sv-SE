---
title: aaaMigrate VMs tooResource Manager med Azure CLI | Microsoft Docs
description: "Den här artikeln innehåller stegvisa hello stöds av plattformen migrering av resurser från klassiska tooAzure Resource Manager med hjälp av Azure CLI"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 0b11f4bb1b4658b3e88bf7629147ed953b678556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-cli"></a>Migrera IaaS-resurser från klassiska tooAzure Resource Manager med hjälp av Azure CLI
Dessa steg visar hur toouse Azure-kommandoradsgränssnittet (CLI) kommandon toomigrate infrastruktur som en tjänst (IaaS)-resurser från hello klassisk distribution modellen toohello Azure Resource Manager-distributionsmodellen. hello artikel kräver hello [Azure CLI](../../cli-install-nodejs.md).

> [!NOTE]
> Alla hello-åtgärder som beskrivs här är idempotent. Om du har problem med än en funktion som inte stöds eller ett konfigurationsfel rekommenderar vi att du gör hello förbereda, avbrytas eller genomföras igen. hello plattform försök sedan hello åtgärden igen.
> 
> 

<br>
Här är en flödesschema tooidentify hello ordning där steg måste toobe körs under en migreringsprocessen

![Skärmbild som visar hello migreringssteg](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>Steg 1: Förbered för migrering
Här följer några metodtips som vi rekommenderar medan du utvärderar migrera IaaS-resurser från klassiska tooResource Manager:

* Läs igenom hello [lista över konfigurationer som inte stöds eller funktioner](../windows/migration-classic-resource-manager-overview.md). Om du har virtuella datorer som använder icke-stödda konfigurationer eller funktioner, rekommenderar vi att du väntar hello/egenskapskonfiguration stöd toobe meddelats. Du kan också ta bort funktionen eller flytta från att konfigurationen tooenable migreringen om den passar dina behov.
* Om du har automatiserat skript som distribuerar din infrastruktur och dina program idag, försök toocreate en liknande inställningar med hjälp av dessa skript för migrering. Du kan också ställa in exempel miljöer med hjälp av hello Azure-portalen.

> [!IMPORTANT]
> Programgatewayer stöds inte för migrering från klassiska tooResource Manager. toomigrate ett klassiskt virtuellt nätverk med en Programgateway ta bort hello gateway innan du kör ett förbereda åtgärden toomove hello nätverk. När du har slutfört migreringen hello återansluta hello gateway i Azure Resource Manager. 
>
>ExpressRoute-gatewayer som ansluter tooExpressRoute kretsar i en annan prenumeration som inte kan migreras automatiskt. I sådana fall kan ta bort hello ExpressRoute-gateway, migrera hello virtuella nätverk och återskapa hello gateway. Se [migrera ExpressRoute-kretsar och associerade virtuella nätverk från hello klassiska toohello Resource Manager-distributionsmodellen](../../expressroute/expressroute-migration-classic-resource-manager.md) för mer information.
> 
> 

## <a name="step-2-set-your-subscription-and-register-hello-provider"></a>Steg 2: Ange din prenumeration och registrera hello-providern
För Migreringsscenarier, behöver du tooset upp din miljö för både klassiska och Resource Manager. [Installera Azure CLI](../../cli-install-nodejs.md) och [väljer din prenumeration](../../xplat-cli-connect.md).

Logga in tooyour konto.

    azure login

Välj hello Azure-prenumeration med hjälp av följande kommando hello.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Registreringen är en gång steg men behov toobe gjort en gång innan du försöker migrera. Utan att registrera visas följande felmeddelande hello 
> 
> *BadRequest: Prenumerationen har inte registrerats för migrering.* 
> 
> 

Registrera med resursprovidern för hello migrering med hjälp av följande kommando hello. Observera att i vissa fall kan det här kommandot på grund av timeout. Dock har hello registreringen lyckats.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Vänta 5 minuter för hello registrering toofinish. Du kan kontrollera hello status för hello godkännandet med hjälp av följande kommando hello. Se till att RegistrationState `Registered` innan du fortsätter.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Växla CLI toohello `asm` läge.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a>Steg 3: Kontrollera att du har tillräckligt med Azure Resource Manager-dator kärnor i hello Azure-regionen för din aktuella distributionen eller virtuella nätverk
För det här steget behöver du tooswitch för`arm` läge. Du kan göra detta med följande kommando hello.

```
azure config mode arm
```

Du kan använda hello följande CLI kommandot toocheck hello aktuell mängd kärnor som du har i Azure Resource Manager. toolearn mer om grundläggande kvoter finns [gränser och hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

När du har verifierat det här steget, kan du växla tillbaka för`asm` läge.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Steg 4: Alternativ 1 – migrera virtuella datorer i en tjänst i molnet
Hämta hello lista över molntjänster med hjälp av följande kommando hello och välj hello molnbaserad tjänst som du vill toomigrate. Observera att om hello virtuella datorer i hello molntjänst är i ett virtuellt nätverk eller om de har web/worker-roller, du får ett felmeddelande.

    azure service list

Kör hello följande tooget hello distribution kommandonamnet för hello molnbaserad tjänst från hello utförlig utdata. I de flesta fall är hello distributionsnamnet hello samma som hello molntjänstnamnet.

    azure service show <serviceName> -vv

Verifiera först om du kan migrera hello Molntjänsten med hello följande kommandon:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Förbered hello virtuella datorer i hello Molntjänsten för migrering. Du har två alternativ toochoose från.

Om du vill toomigrate hello VMs tooa plattform skapade virtuella nätverk använder du följande kommando hello.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Om du vill toomigrate tooan befintliga virtuella nätverk i hello Resource Manager-distributionsmodellen använder du följande kommando hello.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

När hello förberett lyckas, kan du titta igenom hello utförlig utdata tooget hello migrering tillstånd hello virtuella datorer och så att de är i hello `Prepared` tillstånd.

    azure vm show <vmName> -vv

Kontrollera hello-konfigurationen för hello förberedd resurser med hjälp av CLI eller hello Azure-portalen. Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando.

    azure service deployment abort-migration <serviceName> <deploymentName>

Om hello förberedda konfigurationen ser bra ut kan du gå vidare och genomför hello resurser med hjälp av följande kommando hello.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Steg 4: Alternativ 2 – migrera virtuella datorer i ett virtuellt nätverk
Välj hello virtuella nätverk som du vill toomigrate. Observera att om hello virtuellt nätverk innehåller web/worker-roller eller virtuella datorer med konfigurationer som inte stöds, visas ett felmeddelande för verifiering.

Hämta alla hello virtuella nätverk i hello prenumeration med hjälp av följande kommando hello.

    azure network vnet list

hello utdata ser ut ungefär så här:

![Skärmbild av hello kommandoraden med hello hela virtuella nätverksnamnet markerat.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

I ovanstående exempel hello, hello **virtualNetworkName** är hello hela namnet **”grupp classicubuntu16 classicubuntu16”**.

Verifiera först om du kan migrera hello virtuellt nätverk med hello följande kommando:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Förbered hello virtuellt nätverk önskat för migrering med hjälp av följande kommando hello.

    azure network vnet prepare-migration <virtualNetworkName>

Kontrollera hello-konfigurationen för hello förbereda virtuella datorer med hjälp av CLI eller hello Azure-portalen. Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando.

    azure network vnet abort-migration <virtualNetworkName>

Om hello förberedda konfigurationen ser bra ut kan du gå vidare och genomför hello resurser med hjälp av följande kommando hello.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Steg 5: Migrera ett lagringskonto
När du är klar migrera hello virtuella datorer, rekommenderar vi att du migrerar hello storage-konto.

Förbereda hello storage-konto för migrering med hjälp av följande kommando hello

    azure storage account prepare-migration <storageAccountName>

Kontrollera hello-konfigurationen för hello förberedd storage-konto med hjälp av CLI eller hello Azure-portalen. Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando.

    azure storage account abort-migration <storageAccountName>

Om hello förberedda konfigurationen ser bra ut kan du gå vidare och genomför hello resurser med hjälp av följande kommando hello.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Nästa steg

* [Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Använd PowerShell toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-verktyg för att hjälpa till med migrering av IaaS-resurser från klassiska tooAzure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska de vanligaste migreringsfelen](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
