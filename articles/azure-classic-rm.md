---
title: "Resource Manager och Service Management (klassiskt) distributionslägen | Microsoft Docs"
description: "Lär dig skillnaderna mellan Resource Manager och klassiska distributionsmodeller."
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
ms.openlocfilehash: 0f451efa239dd36d82229f01cc385d6df46654f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-deployment-models"></a>Azures distributionsmodeller
Azure-plattformen är i ett övergångsstadium.  Om du inte har använt Azure eller har använt den för år, är det viktigt att förstå några av de viktigaste förändringarna gör vi för plattformen under den här övergången.

Alla Azure-resurser stöd för en eller båda av följande distributionsmodeller:

* **Resource Manager:** detta är den senaste distributionsmodellen för Azure-resurser. De flesta nyare resurser stöder redan denna distributionsmodell och så småningom kommer alla resurser.   
* **Klassisk:** den här modellen har stöd för de flesta befintliga Azure-resurser i dag. Nya resurser läggs till Azure stöder inte den här modellen.

I dokumentationen för varje Azure resursinformation modeller som den kan skapas med.

## <a name="why-does-this-matter"></a>Varför har den här frågan?
Det gäller av följande skäl:

* Azure-plattformsfunktioner som du använder skiljer sig mellan dessa två modeller.  Exempelvis resurser som har skapats med Resource Manager-distributionsmodellen (eller bara Resource Manager) kan skapas med [Azure Resource Manager-mallar](azure-resource-manager/resource-group-overview.md#template-deployment), medan resurserna som skapats med den klassiska distributionsmodellen kan inte.
* Funktioner för enskilda Azure-resurs eller beteenden kan skilja mellan de två modellerna eller endast finnas i en modell eller den andra.  Belastningsutjämning trafik mellan virtuella datorer som skapats med den klassiska distributionsmodellen är till exempel *implicit* eftersom virtuella datorer är medlemmar i en Azure-molntjänst och är automatiskt den belastningsutjämnade över virtuella datorer i en tjänst i molnet. Virtuella datorer som skapats med Resource Manager inte är medlemmar i en molntjänst och en separat Azure belastningsutjämnare resursen måste vara *explicit* skapade för att läsa in Utjämna trafiken över flera virtuella datorer.  
* Hur du skapar, konfigurerar och hanterar Azure-resurser skiljer sig mellan dessa två modeller.
* Resurser som har skapats med en distributionsmodell kan inte nödvändigtvis samverka med resurser som har skapats med en annan distributionsmodell. Azure virtuella datorer som skapats med hjälp av en distributionsmodell kan till exempel bara vara ansluten till Azure-nätverk som skapats med hjälp av samma distributionsmodell.    

Underliggande var och en av distributionsmodellerna är ett programmeringsgränssnitt (API) för varje resurs.  Det finns en [Resource Manager API](https://msdn.microsoft.com/library/azure/dn948464.aspx) för Resource Manager-distributionsmodellen och en [Service Management API](https://msdn.microsoft.com/library/azure/ee460799.aspx) för den klassiska distributionsmodellen. Utvecklare kan skriva kod för att interagera med API: erna *direkt*.  

IT-proffs dock vanligtvis interagera med API: erna *indirekt* genom att använda en grafisk portal i en webbläsare, med hjälp av Azure PowerShell-cmdlets på en Windows-dator eller med hjälp av Azure-kommandoradsgränssnittet (CLI) på antingen Windows , OS X eller Linux-dator. Alla tre metoderna indirekt används av IT-teknikern interagera direkt med API: erna. Det innebär att när nya funktioner introduceras i Azure-plattformen eller resurser bör det alltid är direkt tillgänglig via API först med indirekta metoder får stöd för nya resurser och funktioner efter API: et har gjorts tillgängligt.  

I avsnitten nedan beskrivs hur Azure-resurser konfigureras med olika distributionsmodeller via tre indirekta metoder.

## <a name="portals"></a>Portaler
Azure har två portaler:

* **[Azure-portalen](https://manage.windowsazure.com):** om du har använt Azure på ett tag, du har använt den här portalen. Den används för att skapa och konfigurera äldre Azure-resurser som stöder den klassiska distributionsmodellen. Du kan inte använda den för att skapa eller konfigurera resurser som bara stöder Resource Manager. 
* **[Azure preview portal](https://azure.microsoft.com/overview/preview-portal/):** om du använder en nyare Azure resurs du troligen har använt den här portalen. Det kan användas för att skapa och konfigurera vissa Azure-resurser. Du kommer slutligen att kunna skapa och konfigurera alla Azure-resurser med den. För vissa resurser som stöder båda distributionsmodellerna kan portalen användas för att skapa och konfigurera en resurs med hjälp av antingen distributionsmodell. 

Vissa funktioner och resurser kan endast skapas och konfigureras i en portal eller den andra. Vissa resurser eller funktioner (ännu) får inte vara skapas eller konfigureras i antingen portal och kan bara konfigureras med PowerShell, CLI eller båda. I dokumentationen för varje Azure-resurs detaljerad information om vilken metod som du kan skapa med. 

## <a name="powershell"></a>PowerShell
Med [PowerShell](/powershell/azureps-cmdlets-docs) du kan använda kommandoraden eller skapa skript för att skapa och konfigurera Azure-resurser från en Windows-dator.  Enskilda Azure resurser har [Resource Manager cmdlets](/powershell/azure/overview), [Service Management-cmdlets](/powershell/azure/overview?view=azuresmps-3.7.0), eller båda.  Vissa resurser och funktioner kan bara skapas och konfigureras med PowerShell eller CLI. Beroende på resursen, när du använder Resource Manager PowerShell-cmdlets kan du ha två alternativ för att skapa och konfigurera Azure-resurser:

* **PowerShell-cmdlets:** kan du skapa och konfigurera varje Azure-resurs individuellt med hjälp av cmdlets för varje resurs. Du kan göra detta från en kommandorad eller genom att inkludera flera kommandon i ett PowerShell-skript som du kan lagra och version.
* **PowerShell-cmdlets med en Azure Resource Manager-mall:** du kan använda PowerShell för att skapa Azure-resurser med hjälp av en Azure Resource Manager-mall. Mallar kan vara sparade och en ny version. Lär dig mer genom att läsa den [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md) artikel. Flera [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/) finns för vanliga lösningar som kan hämtas och ändras för.

## <a name="cli"></a>CLI
Du kan skapa och konfigurera Azure-resurser från Windows-, OS X- eller Linux-datorer med hjälp av CLI.  Läs den [installerar Azure CLI](cli-install-nodejs.md) artikeln om du vill installera CLI på din operativsystemet. Som PowerShell, det finns olika kommandon som måste användas, beroende på om du skapar resurser med hjälp av [Resource Manager](xplat-cli-azure-resource-manager.md) eller [klassisk (servicehantering)](virtual-machines/linux/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) distributionsmodeller.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [Resource Manager](azure-resource-manager/resource-group-overview.md).
* Förstå hur du [utforma mallar](best-practices-resource-manager-design-templates.md).

