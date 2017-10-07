---
title: "lägen för aaaResource Manager och Service Management (klassisk) distribution | Microsoft Docs"
description: "Lär dig hello skillnaderna mellan Resource Manager och klassiska distributionsmodeller."
services: virtual-network
documentationcenter: 
author: telmosampaio
manager: carmonm
editor: 
tags: azure-resource-manager,azure-service-management
redirect_url: ./azure-resource-manager/resource-manager-deployment-model
ms.assetid: 18a235d8-38ac-4886-9e56-b3855c73ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: telmos
ms.openlocfilehash: e14f815ba9d79d9dd8f83b45bda78d0eba0bec56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-deployment-models"></a>Azures distributionsmodeller
hello Azure-plattformen är i ett övergångsstadium.  Om du nya tooAzure eller har använt den för år, det är viktigt toounderstand några av hello viktiga ändringar gör vi toohello plattform under den här övergången.

Alla Azure-resurser stöd för en eller båda av följande distributionsmodeller hello:

* **Resource Manager:** detta är hello senaste distributionsmodell för Azure-resurser. De flesta nyare resurser stöder redan denna distributionsmodell och så småningom kommer alla resurser.   
* **Klassisk:** den här modellen har stöd för de flesta befintliga Azure-resurser i dag. Nya resurser läggs tooAzure inte stöder den här modellen.

hello Info i dokumentation för varje Azure-resurs modeller som den kan skapas med.

## <a name="why-does-this-matter"></a>Varför har den här frågan?
Det gäller för hello följande orsaker:

* hello Azure-plattformsfunktioner som du använder skiljer sig mellan dessa två modeller.  Till exempel resurser som skapades med hello Resource Manager-modellen (eller bara Resource Manager) kan skapas med [Azure Resource Manager-mallar](azure-resource-manager/resource-group-overview.md#template-deployment), medan resurserna som skapats med hello klassiska distributionsmodellen kan inte.
* hello Azure-resurs enskilda funktioner eller beteenden kan skilja mellan hello två modeller eller endast finnas i en modell eller hello andra.  Belastningsutjämning trafik mellan virtuella datorer som skapats med hello klassiska distributionsmodellen är till exempel *implicit* eftersom virtuella datorer är medlemmar i en Azure-molntjänst och är automatiskt den belastningsutjämnade över virtuella datorer i en tjänst i molnet. Virtuella datorer som skapats med Resource Manager inte är medlemmar i en molntjänst och en separat Azure belastningsutjämnare resursen måste vara *explicit* skapade tooload Utjämna trafiken över flera virtuella datorer.  
* Hur du skapar, konfigurerar och hanterar Azure-resurser skiljer sig mellan dessa två modeller.
* Resurser som har skapats med en distributionsmodell kan inte nödvändigtvis samverka med resurser som har skapats med en annan distributionsmodell. Azure virtuella datorer som skapats med hjälp av en distributionsmodell kan till exempel bara vara ansluten tooAzure virtuella nätverk som skapats med hello samma distributionsmodell.    

Underliggande varje hello distribution är ett programmeringsgränssnitt (API) för varje resurs.  Det finns en [Resource Manager API](https://msdn.microsoft.com/library/azure/dn948464.aspx) för hello Resource Manager-modellen och en [Service Management API](https://msdn.microsoft.com/library/azure/ee460799.aspx) för hello klassiska distributionsmodellen. Utvecklare kan skriva kod toointeract med API: erna *direkt*.  

IT-proffs dock vanligtvis interagera med API: erna *indirekt* genom att använda en grafisk portal i en webbläsare med hjälp av Azure PowerShell cmdlets på en Windows-dator eller genom att använda hello Azure-kommandoradsgränssnittet (CLI) på antingen en Windows-, OS X- eller Linux-dator. Alla tre metoderna indirekt används av hello IT-teknikern interagera direkt med hello API: er. Detta innebär att när nya funktioner har införts toohello Azure-plattformen eller resurser den alltid är direkt tillgängligt via hello API först med hello indirekta metoder får stöd för hello nya resurser och funktioner efter hello API har gjorts tillgängligt.  

hello avsnitten nedan beskrivs hur Azure-resurser konfigureras med olika distributionsmodeller för hello via hello tre indirekta metoder.

## <a name="portals"></a>Portaler
Azure har två portaler:

* **[Azure-portalen](https://manage.windowsazure.com):** om du har använt Azure på ett tag, du har använt den här portalen. Det är används toocreate och konfigurera äldre Azure-resurser som stöder hello klassiska distributionsmodellen. Du kan inte använda den toocreate eller konfigurera resurser som har endast stöd för hanteraren för filserverresurser. 
* **[Azure preview portal](https://azure.microsoft.com/overview/preview-portal/):** om du använder en nyare Azure resurs du troligen har använt den här portalen. Den kan använda toocreate och konfigurera vissa Azure-resurser. Du kan slutligen att kan toocreate och konfigurera alla Azure-resurser med den. För vissa resurser som stöder båda distributionsmodellerna kan portalen använda toocreate och konfigurerar en resurs med hjälp av antingen distributionsmodell. 

Vissa funktioner och resurser kan endast skapas och konfigureras i en portal eller hello andra. Vissa resurser eller funktioner (ännu) kan inte skapas eller konfigureras i antingen portal och kan bara konfigureras med PowerShell, hello CLI eller båda. vilken metod som du kan skapa med detaljerad information om hello dokumentationen för varje Azure-resurs. 

## <a name="powershell"></a>PowerShell
Med [PowerShell](/powershell/azureps-cmdlets-docs) du kan använda en kommandorad eller författare skript toocreate och konfigurera Azure-resurser från en Windows-dator.  Enskilda Azure resurser har [Resource Manager cmdlets](/powershell/azure/overview), [Service Management-cmdlets](/powershell/azure/overview?view=azuresmps-3.7.0), eller båda.  Vissa funktioner och resurser kan bara skapas och konfigureras med PowerShell eller hello CLI. Beroende på hello resurs när du använder Resource Manager PowerShell-cmdlets kan du ha två alternativ för att skapa och konfigurera Azure-resurser:

* **PowerShell-cmdlets:** kan du skapa och konfigurera varje Azure-resurs sig med hello-cmdlets för varje resurs. Du kan göra detta från en kommandorad eller genom att inkludera flera kommandon i ett PowerShell-skript som du kan lagra och version.
* **PowerShell-cmdlets med en Azure Resource Manager-mall:** kan du använda PowerShell toocreate Azure resurser med hjälp av en Azure Resource Manager-mall. Mallar kan vara sparade och en ny version. Lär dig mer genom att läsa hello [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md) artikel. Flera [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/) finns för vanliga lösningar som kan hämtas och ändras för.

## <a name="cli"></a>CLI
Du kan skapa och konfigurera Azure-resurser från Windows-, OS X- eller Linux-datorer med hjälp av hello CLI.  Läs hello [installera hello Azure CLI](cli-install-nodejs.md) artikel tooinstall hello CLI på din operativsystemet. Som PowerShell, det finns olika kommandon som måste användas, beroende på om du skapar resurser med hjälp av [Resource Manager](xplat-cli-azure-resource-manager.md) eller hello [klassisk (servicehantering)](virtual-machines/linux/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) distributionsmodeller.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [Resource Manager](azure-resource-manager/resource-group-overview.md).
* Förstå hur för[utforma mallar](best-practices-resource-manager-design-templates.md).

