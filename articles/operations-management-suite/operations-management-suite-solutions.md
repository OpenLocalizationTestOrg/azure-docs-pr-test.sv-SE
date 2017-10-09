---
title: aaaSolutions i Operations Management Suite (OMS) | Microsoft Docs
description: "Lösningar utöka hello funktioner i Operations Management Suite (OMS) genom att tillhandahålla paketerade hanteringsscenarier som kunder kan lägga till tootheir OMS-arbetsyta.  Den här artikeln innehåller information om hur anpassade lösningar som skapats av kunder och partner."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5b5a538f1bc4b5577bec94db08bd43668bc6584a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-management-solutions-in-operations-management-suite-oms-preview"></a>Arbeta med hanteringslösningar i Operations Management Suite (OMS) (förhandsgranskning)
> [!NOTE]
> Den här är dokumentationen preliminär för hanteringslösningar i OMS som för närvarande finns i förhandsgranskningen.    
> 
> 

Hanteringslösningar utöka hello funktioner i Operations Management Suite (OMS) genom att tillhandahålla paketerade hanteringsscenarier som kunder kan lägga till tootheir miljö.  Dessutom för[lösningar som tillhandahålls av Microsoft](../log-analytics/log-analytics-add-solutions.md), partner och kunder kan skapa lösningar för hantering av toobe används i sina egna miljö eller göras tillgänglig toocustomers via hello community.

## <a name="finding-and-installing-management-solutions"></a>Söka efter och installera lösningar för hantering
Det finns flera metoder för att hitta och installera lösningar som beskrivs i följande avsnitt hello.

### <a name="azure-marketplace"></a>Azure Marketplace
Lösningar som tillhandahålls av Microsoft och tillförlitliga partners kan installeras från hello Azure Marketplace i hello Azure-portalen.

1. Logga in toohello Azure-portalen.
2. I hello vänster och välj **fler tjänster**.
3. Antingen rulla nedåt för**lösningar** eller typ *lösningar* till hello **Filter** dialogrutan.
4. Klicka på hello **+ Lägg till** knappen.
5. Sök efter lösningar som du är intresserad av antingen genom att bläddra, klicka på hello **Filter** knappen eller skriva i hello **Sök Everthing** rutan.
6. Klicka på en marketplace-objektet tooview detaljerad information.
7. Klicka på **skapa** tooopen hello **Lägg till lösning** fönstret.
8. Du kan ange toorequired information, till exempel hello [OMS arbetsytan och Automation-konto](#oms-workspace-and-automation-account) dessutom toovalues för alla parametrar i hello lösning.
9. Klicka på **skapa** tooinstall hello lösning.

### <a name="oms-portal"></a>OMS-portalen
Lösningar som tillhandahålls av Microsoft kan installeras från hello lösningar galleriet i hello OMS-portalen.

1. Logga in toohello OMS-portalen.
2. Klicka på hello **lösningar galleriet** panelen.
3. Läs mer om varje tillgänglig lösning på sidan för hello OMS lösningar galleri. Klicka på hello lösning som du vill tooadd tooOMS hello namn.
4. Hello sidan för hello-lösningen som du har valt visas detaljerad information om hello lösning. Klicka på **Lägg till**.
5. En ny panel för hello-lösning som du har lagt till visas på översiktssidan i OMS hello och du kan börja använda den när hello OMS-tjänsten bearbetar dina data.

### <a name="azure-quickstart-templates"></a>Azure-snabbstartmallar
Medlemmar i hello gemenskapen kan skicka management lösningar tooAzure Snabbstartsmallar.  Du kan hämta dessa mallar för senare installation eller inspektera dem toolearn hur för[skapa egna lösningar](#creating-a-solution).

1. Följ hello process som beskrivs i [OMS arbetsytan och Automation-konto](#oms-workspace-and-automation-account) toolink en arbetsyta och kontot.
2. Gå för[Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/).  
3. Sök efter en lösning som du är intresserad av.
4. Välj hello lösning hello resultat tooview information.
5. Klicka på hello **distribuera tooAzure** knappen.
6. Du kan ange tooprovide information, till exempel hello resursgrupp och plats i tillägget toovalues för alla parametrar i hello-lösning.
7. Klicka på **inköp** tooinstall hello lösning.

### <a name="deploy-azure-resource-manager-template"></a>Distribuera Azure Resource Manager-mall
Lösningar som du får från hello community eller som du [skapar själv](#creating-a-solution) implementeras som en Resource Manager-mall, och du kan använda någon av hello standardmetoder för [distribuerar en mall](../azure-resource-manager/resource-group-template-deploy-portal.md).  Observera att innan du installerar hello lösningen måste du skapa och länka hello [OMS arbetsytan och Automation-konto](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>OMS-arbetsytan och Automation-konto
De flesta lösningar kräver en [OMS-arbetsytan](../log-analytics/log-analytics-manage-access.md) toocontain vyer och en [Automation-konto](../automation/automation-security-overview.md#automation-account-overview) toocontain runbooks och relaterade resurser. Hej arbetsytan och kontot måste uppfylla följande krav hello.

* En lösning kan bara använda en OMS-arbetsytan och en Automation-konto.  
* hello OMS-arbetsytan och Automation-konto som används av en lösning måste vara länkade tooone en annan. En OMS-arbetsyta kanske bara länkade tooone Automation-konto och ett Automation-konto kan bara vara länkade tooone OMS-arbetsyta.
* toobe länkade hello OMS-arbetsytan och hello i Automation-kontot måste finnas i samma resursgrupp och region.  hello undantaget är en OMS-arbetsyta i östra USA och och Automation-konto i östra USA 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Skapa en länk mellan en OMS-arbetsytan och Automation-konto
Hur du anger hello OMS-arbetsytan och Automation-konto är beroende av hello installationsmetoden för lösningen.

* När du installerar en Microsoft-lösning via hello OMS-portalen är installerad hello aktuella OMS-arbetsytan och inga Automation-konto krävs.
* När du installerar en lösning via hello Azure Marketplace du tillfrågas om en OMS-arbetsytan och Automation-konto och hello länken mellan dem skapas åt dig.  
* Lösningar utanför hello Azure Marketplace kan länka du hello OMS-arbetsytan och Automation-konto innan du installerar hello lösning.  Du kan göra detta genom att välja en lösning i hello Azure Marketplace och välja hello OMS-arbetsytan och Automation-konto.  Du har inte tooactually installera hello lösning eftersom hello länk skapas när hello OMS-arbetsytan och Automation-konto har valts.  När hello länken har skapats kan sedan du använda den OMS-arbetsytan och Automation-kontot för alla lösningar. 

### <a name="verifying-hello-link-between-an-oms-workspace-and-automation-account"></a>Verifiera hello länken mellan en OMS-arbetsytan och Automation-konto
Du kan kontrollera hello länken mellan en OMS-arbetsyta och ett Automation-konto med hjälp av hello nedan.

1. Välj hello Automation-konto i hello Azure-portalen.
2. Rulla toohello längst ned på hello **inställningar** fönstret.
3. Om det finns ett avsnitt som heter **OMS-resurser** i hello **inställningar** fönstret och sedan det här kontot är anslutna tooan OMS-arbetsyta.
4. Välj **arbetsytan** tooview hello information om hello OMS-arbetsytan länkade toothis Automation-konto.

## <a name="listing-management-solutions"></a>Visar en lista över lösningar för hantering
Använd hello följa proceduren tootooview hello hanteringslösningar hello arbetsytor länkade tooyour Azure-prenumeration.

1. Logga in toohello Azure-portalen.
2. I hello vänster och välj **fler tjänster**.
3. Antingen rulla nedåt för**lösningar** eller typ *lösningar* till hello **Filter** dialogrutan.
4. Lösningar som installerats i alla arbetsytor visas.

Observera att du kan visa endast hello Microsoft solutions installerat hello aktuella arbetsytan med hello OMS-portalen.

## <a name="removing-a-management-solution"></a>Ta bort en lösning
När en hanteringslösning tas bort alla resurser i hello-lösning också att tas bort.  

1. Hitta hello lösning i hello Azure-portalen med hello proceduren i [lista lösningar](#listing-solutions).
2. Välj hello-lösning som du vill tooremove.
3. Klicka på hello **ta bort** knappen.

## <a name="creating-a-management-solution"></a>Skapa en lösning
Fullständig information om att skapa lösningar finns på [och skapa lösningar i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md). 

## <a name="next-steps"></a>Nästa steg
* Sök [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates) exempel på olika Resource Manager-mallar.
* Skapa en egen [hanteringslösningar](operations-management-suite-solutions-creating.md).

