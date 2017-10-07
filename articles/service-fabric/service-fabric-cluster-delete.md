---
title: aaaDelete en Azure-kluster och dess resurser | Microsoft Docs
description: "Lär dig hur toocompletely ta bort ett Service Fabric-kluster antingen ta bort hello resursgruppen som innehåller hello klustret eller selektivt ta bort resurser."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 5c15a4184644da715cd69397f2150de86ab433ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-hello-resources-it-uses"></a>Ta bort ett Service Fabric-kluster på Azure och hello resurser som används
Service Fabric-klustret består av många andra Azure-resurser förutom toohello klusterresurs sig själv. Så toocompletely ta bort Service Fabric-kluster behöver du också toodelete alla hello av resurser.
Har du två alternativ: antingen ta bort hello resursgruppen som hello klustret finns i (som tar bort hello klusterresurs och andra resurser i resursgruppen hello) eller uttryckligen ta bort hello klusterresurs och dess tillhörande resurser (men inte andra resurser i resursgruppen hello).

> [!NOTE]
> Om du tar bort hello klusterresurs **inte** ta bort alla hello andra resurser som består av Service Fabric-klustret.
> 
> 

## <a name="delete-hello-entire-resource-group-rg-that-hello-service-fabric-cluster-is-in"></a>Ta bort hello hela resursgruppen (RG) som hello Service Fabric-klustret är i
Detta är hello enklaste sättet tooensure du ta bort alla hello resurser som är associerade med klustret, inklusive hello resursgruppen. Du kan ta bort hello resursgrupp med hjälp av PowerShell eller via hello Azure-portalen. Om resursgruppen har resurser som inte är relaterade tooService fabric-kluster, kan du ta bort specifika resurser.

### <a name="delete-hello-resource-group-using-azure-powershell"></a>Ta bort hello resursgruppen med hjälp av Azure PowerShell
Du kan också ta bort hello resursgruppen genom att köra hello följande Azure PowerShell-cmdlets. Kontrollera att Azure PowerShell 1.0 eller högre är installerad på datorn. Om du inte har gjort det innan du följer hello steg som beskrivs i [hur tooinstall och konfigurera Azure PowerShell.](/powershell/azure/overview)

Öppna ett PowerShell-fönster och kör hello följande PS-cmdlets:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Du får en fråga tooconfirm hello borttagning om du inte använde hello *-Force* alternativet. Hej RG vid bekräftelse och alla hello-resurser som den innehåller tas bort.

### <a name="delete-a-resource-group-in-hello-azure-portal"></a>Ta bort en resursgrupp i hello Azure-portalen
1. Inloggningen toohello [Azure-portalen](https://portal.azure.com).
2. Navigera toohello Service Fabric-kluster som du vill toodelete.
3. Klicka på hello resursgruppens namn på hello klustret essentials sida.
4. Detta öppnar hello **resurs grupp Essentials** sidan.
5. Klicka på **Ta bort**.
6. Följ instruktionerna för hello på sidan toocomplete hello borttagningen av hello resursgruppen.

![Ta bort resursgruppen.][ResourceGroupDelete]

## <a name="delete-hello-cluster-resource-and-hello-resources-it-uses-but-not-other-resources-in-hello-resource-group"></a>Ta bort hello klusterresurs och hello resurser används, men inte andra resurser i hello resursgrupp
Om resursgruppen har resurser som är relaterade toohello Service Fabric-kluster du vill toodelete och det är enklare toodelete hello hela resursgruppen. Följ dessa steg om du vill tooselectively delete hello resurser i taget i resursgruppen.

Om du har distribuerat ditt kluster med hjälp av hello-portalen eller något av hello Service Fabric Resource Manager-mallar från hello mallgalleriet märks alla hello-resurser som hello klustret använder med hello följande två taggar. Du kan använda dem toodecide vilka resurser du vill toodelete.

***Taggen nr 1:*** nyckel = clusterName, värde = 'namn på hello klustret'

***Taggen nr 2:*** nyckel = resourceName, värde = ServiceFabric

### <a name="delete-specific-resources-in-hello-azure-portal"></a>Ta bort specifika resurser i hello Azure-portalen
1. Inloggningen toohello [Azure-portalen](https://portal.azure.com).
2. Navigera toohello Service Fabric-kluster som du vill toodelete.
3. Gå för**alla inställningar** på hello essentials bladet.
4. Klicka på **taggar** under **resurshantering** hello inställningar-bladet.
5. Klicka på någon av hello **taggar** i hello taggar bladet tooget en lista över alla hello resurser med taggen.
   
    ![Resurstaggar][ResourceTags]
6. När du har hello lista med märkta resurser klickar du på var och en av hello resurser och ta bort dem.
   
    ![Taggade resurser][TaggedResources]

### <a name="delete-hello-resources-using-azure-powershell"></a>Ta bort hello resurser med hjälp av Azure PowerShell
Du kan ta bort hello resurser i taget genom att köra hello följande Azure PowerShell-cmdlets. Kontrollera att Azure PowerShell 1.0 eller högre är installerad på datorn. Om du inte har gjort det innan du följer hello steg som beskrivs i [hur tooinstall och konfigurera Azure PowerShell.](/powershell/azure/overview)

Öppna ett PowerShell-fönster och kör hello följande PS-cmdlets:

```powershell
Login-AzureRmAccount
```
För varje hello resurser du vill toodelete, kör hello följande:

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of hello resource group>" -Force
```

toodelete hello-klusterresursen kör hello följande:

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of hello resource group>" -Force
```

## <a name="next-steps"></a>Nästa steg
Läs hello följande tooalso Lär dig om att uppgradera ett kluster och partitionering tjänster:

* [Lär dig mer om klusteruppgradering](service-fabric-cluster-upgrade.md)
* [Lär dig mer om partitionering tillståndskänsliga tjänster för maximal skala](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
