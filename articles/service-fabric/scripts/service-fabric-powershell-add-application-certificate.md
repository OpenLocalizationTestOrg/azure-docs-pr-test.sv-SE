---
title: "aaaAzure PowerShell skriptexempel - lägga till programmet cert tooa klustret | Microsoft Docs"
description: "Azure PowerShell-skript exempel – Lägg till ett program certifikat tooa Service Fabric-kluster."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: aba62598e2e4775012f89b5070bef5e61aec64f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-an-application-certificate-tooa-service-fabric-cluster"></a><span data-ttu-id="7ee18-103">Lägg till ett program certifikat tooa Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="7ee18-103">Add an application certificate tooa Service Fabric cluster</span></span>

<span data-ttu-id="7ee18-104">Det här exempelskriptet skapar ett självsignerat certifikat i hello angivna Azure key vault och installerar den tooall noder i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7ee18-104">This sample script creates a self-signed certificate in hello specified Azure key vault and installs it tooall nodes of hello Service Fabric cluster.</span></span> <span data-ttu-id="7ee18-105">hello certifikat hämtas även tooa lokal mapp.</span><span class="sxs-lookup"><span data-stu-id="7ee18-105">hello certificate also downloads tooa local folder.</span></span> <span data-ttu-id="7ee18-106">hello namnet på hello hämtat certifikat är hello samma som hello namnet på hello certifikat i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7ee18-106">hello name of hello downloaded certificate is hello same as hello name of hello certificate in hello key vault.</span></span> <span data-ttu-id="7ee18-107">Anpassa hello parametrarna efter behov.</span><span class="sxs-lookup"><span data-stu-id="7ee18-107">Customize hello parameters as needed.</span></span>

<span data-ttu-id="7ee18-108">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview) och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="7ee18-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="7ee18-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="7ee18-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate tooa cluster")]

## <a name="script-explanation"></a><span data-ttu-id="7ee18-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="7ee18-110">Script explanation</span></span>

<span data-ttu-id="7ee18-111">Det här skriptet använder hello följande kommandon: varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7ee18-111">This script uses hello following commands: Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7ee18-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="7ee18-112">Command</span></span> | <span data-ttu-id="7ee18-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7ee18-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7ee18-114">Lägg till AzureRmServiceFabricApplicationCertificate</span><span class="sxs-lookup"><span data-stu-id="7ee18-114">Add-AzureRmServiceFabricApplicationCertificate</span></span>](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | <span data-ttu-id="7ee18-115">Lägga till en ny virtuell dator skala program certifikat toohello som utgör hello klustret.</span><span class="sxs-lookup"><span data-stu-id="7ee18-115">Add a new application certificate toohello virtual machine scale set that make up hello cluster.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="7ee18-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ee18-116">Next steps</span></span>

<span data-ttu-id="7ee18-117">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7ee18-117">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7ee18-118">Ytterligare Azure Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7ee18-118">Additional Azure Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
