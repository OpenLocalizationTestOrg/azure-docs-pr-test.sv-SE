---
title: "aaaDeploy Skalningsuppsättning virtuell dator med hjälp av Visual Studio | Microsoft Docs"
description: "Distribuera Skalningsuppsättningar i virtuella datorer med hjälp av Visual Studio och Resource Manager-mall"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c89a9f2478ccc3d22989aea604a4273bcc46df82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-visual-studio"></a>Hur toocreate en Virtual Machine Scale Set med Visual Studio
Den här artikeln visar hur toodeploy ett Azure Virtual Machine Scale Set med hjälp av en Visual Studio resurs distribution.

[Azure Virtual Machine-Skalningsuppsättningar](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) är en Azure Compute resurs toodeploy och hantera en samling liknande virtuella datorer med Autoskala och belastningsutjämning. Du kan etablera och distribuera Skalningsuppsättningar i virtuella datorer med hjälp av [Azure Resource Manager-mallar](https://github.com/Azure/azure-quickstart-templates). Azure Resource Manager-mallar som kan distribueras med hjälp av Azure CLI, PowerShell, REST och även direkt från Visual Studio. Visual Studio tillhandahåller en uppsättning exempel mallar, som kan distribueras som en del av en grupp distributionsprojekt för Azure-resurs.

Azure-resursgrupp distributioner är ett sätt toogroup och publicera en uppsättning relaterade Azure-resurser i en enskild distribution-åtgärd. Du kan lära dig mer om de här: [skapa och distribuera Azure-resursgrupper via Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Förutsättningar
tooget igång med att distribuera Skalningsuppsättningar i virtuella datorer i Visual Studio, behöver du hello följande:

* Visual Studio 2013 eller senare
* Azure SDK 2.7, 2.8 eller 2.9

>[!NOTE]
>Dessa instruktioner förutsätter att du använder Visual Studio med [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Skapa ett projekt
1. Skapa ett nytt projekt i Visual Studio genom att välja **filen | Nya | Projektet**.
   
    ![Ny fil][file_new]

2. Under **Visual C# | Molnet**, Välj **Azure Resource Manager** toocreate ett projekt för distribution av en Azure Resource Manager-mall.
   
    ![Skapa projekt][create_project]

3. Hello listan över mallar och väljer du hello Linux eller Windows Virtual Machine Scale ange mall.
   
   ![Välj mall][select_Template]

4. När projektet har skapats visas PowerShell-skript för distribution, en Azure Resource Manager-mall och en parameterfil för hello Skaluppsättning för virtuell dator.
   
    ![Solution Explorer][solution_explorer]

## <a name="customize-your-project"></a>Anpassa ditt projekt
Nu kan du redigera hello mallen toocustomize ladda den för programmets behov, till exempel lägga till egenskaperna för VM-tillägget eller redigera regler för belastningsutjämning. Ange hello Virtual Machine Scale-mallar är som standard konfigurerade toodeploy hello AzureDiagnostics tillägg, vilket gör det enkelt tooadd Autoskala regler. Den distribuerar också en belastningsutjämnare med en offentlig IP-adress som konfigurerats med ingående NAT-regler. 

hello belastningsutjämnare kan du ansluta toohello VM-instanser med SSH (Linux) eller RDP (Windows). hello frontend portintervall startar vid 50000. För linux det innebär att om du SSH tooport 50000, är routade tooport 22 av hello första VM i hello skalan. Ansluta tooport 50001 är routade tooport 22 av hello andra VM och så vidare.

 Ett bra sätt tooedit mallarna med Visual Studio är toouse hello JSON-disposition tooorganize hello parametrar, variabler och resurser. Förstå hello peka schemat Visual Studio ut fel i mallen innan du distribuerar den.

![JSON-Explorer][json_explorer]

## <a name="deploy-hello-project"></a>Distribuera hello-projekt
1. Distribuera hello Azure Resource Manager-mall toocreate hello Skaluppsättning för virtuell dator resurs. Högerklicka på projektnoden hello och välj **distribuera | Ny distribution**.
   
    ![Distribuera mallen][5deploy_Template]
    
2. Välj prenumerationen i dialogrutan för hello ”distribuera tooResource grupp”.
   
    ![Distribuera mallen][6deploy_Template]

3. Härifrån kan skapa du en Azure-resursgrupp toodeploy din mall.
   
    ![Ny resursgrupp][new_resource]

4. Klicka sedan på **redigera parametrar** tooenter parametrar som skickas tooyour mall. Ange hello användarnamn och lösenord för hello OS, som är nödvändiga toocreate hello distribution. Om du inte har PowerShell-verktyg för Visual Studio installerat, bör toocheck **spara lösenord** tooavoid en dold PowerShell kommandoradsverktyget fråga eller Använd [keyvault support](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Redigera parametrar][edit_parameters]

5. Klicka på **distribuera**. Hej **utdata** visar hello distribution pågår. Observera att hello-åtgärden körs hello **distribuera AzureResourceGroup.ps1** skript.
   
   ![Utdatafönstret][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a>Utforska dina virtuella datorns Skaluppsättning
När hello distributionen är klar kan du visa hello nya Virtual Machine Scale Set i hello Visual Studio **Cloud Explorer** (uppdatera hello lista). Cloud Explorer kan du hantera Azure-resurser i Visual Studio när du utvecklar program. Du kan också visa Skalningsuppsättning i virtuell dator i hello [Azure-portalen](https://portal.azure.com) och [resursutforskaren Azure](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 hello portal innehåller hello bästa sättet toovisually hanterar din Azure-infrastrukturen med en webbläsare, medan resursutforskaren Azure tillhandahåller ett enkelt sätt tooexplore och felsöka Azure-resurser, ger ett fönster i hello ”instans visa” och också med PowerShell kommandon för hello resurser du tittar på.

## <a name="next-steps"></a>Nästa steg
När du har distribuerats Skalningsuppsättningar i virtuella datorer via Visual Studio, kan du anpassa din project toosuit kraven för application. Till exempel konfigurera Autoskala genom att lägga till en **Insights** resurs, lägga till infrastruktur tooyour mall (t.ex. fristående virtuella datorer) eller distribuera program med hjälp av hello tillägget för anpassat skript. Bra exempel mallar finns i hello [Azure Quickstart mallar](https://github.com/Azure/azure-quickstart-templates) GitHub-lagringsplatsen (Sök efter ”vmss”).

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
