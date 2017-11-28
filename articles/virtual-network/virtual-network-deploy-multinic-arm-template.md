---
title: "aaaCreate en virtuell dator med flera nätverkskort – Azure Resource Manager-mall | Microsoft Docs"
description: "Skapa en virtuell dator med flera nätverkskort med en Azure Resource Manager-mall."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5d9ffcbd40c72dc18ae6de38e739eb5e45bf669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a><span data-ttu-id="7d66b-103">Skapa en virtuell dator med flera nätverkskort med en mall</span><span class="sxs-lookup"><span data-stu-id="7d66b-103">Create a VM with multiple NICs using a template</span></span>
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="7d66b-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7d66b-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="7d66b-105">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="7d66b-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="7d66b-106">hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.</span><span class="sxs-lookup"><span data-stu-id="7d66b-106">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d66b-107">Krav</span><span class="sxs-lookup"><span data-stu-id="7d66b-107">Prerequisites</span></span>
<span data-ttu-id="7d66b-108">Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="7d66b-108">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="7d66b-109">toocreate dessa resurser, Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7d66b-109">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="7d66b-110">Navigera för[hello mallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="7d66b-110">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="7d66b-111">Hello mallen sidan toohello höger i **överordnade resursgruppen**, klickar du på **distribuera tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="7d66b-111">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="7d66b-112">Om det behövs, ändra hello parametervärden sedan gör hello i hello Azure preview portal toodeploy hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7d66b-112">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d66b-113">Kontrollera att din lagringskontonamn är unika.</span><span class="sxs-lookup"><span data-stu-id="7d66b-113">Make sure your storage account names are unique.</span></span> <span data-ttu-id="7d66b-114">Du kan inte ha dubbla lagringskontonamn i Azure.</span><span class="sxs-lookup"><span data-stu-id="7d66b-114">You cannot have duplicate storage account names in Azure.</span></span>
> 

## <a name="understand-hello-deployment-template"></a><span data-ttu-id="7d66b-115">Förstå hello Distributionsmall</span><span class="sxs-lookup"><span data-stu-id="7d66b-115">Understand hello deployment template</span></span>
<span data-ttu-id="7d66b-116">Innan du distribuerar den här dokumentationen hello-mall, kontrollera att du förstår hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="7d66b-116">Before you deploy hello template provided with this documentation, make sure you understand what it does.</span></span> <span data-ttu-id="7d66b-117">hello följande ger en bra översikt över hello mallen:</span><span class="sxs-lookup"><span data-stu-id="7d66b-117">hello following steps provide a good overview of hello template:</span></span>

1. <span data-ttu-id="7d66b-118">Navigera för[hello mallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="7d66b-118">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="7d66b-119">Klicka på **azuredeploy.json** tooopen hello mallfilen.</span><span class="sxs-lookup"><span data-stu-id="7d66b-119">Click **azuredeploy.json** tooopen hello template file.</span></span>
3. <span data-ttu-id="7d66b-120">Meddelande hello *osType* parametrar i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-120">Notice hello *osType* parameter listed below.</span></span> <span data-ttu-id="7d66b-121">Den här parametern är används tooselect toouse vilka VM-avbildning för hello databasserver, tillsammans med flera operativsystem relaterade inställningar.</span><span class="sxs-lookup"><span data-stu-id="7d66b-121">This parameter is used tooselect what VM image toouse for hello database server, along with multiple operating system related settings.</span></span>

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS toouse for VMs: Windows or Ubuntu."
      }
    },
    ```

4. <span data-ttu-id="7d66b-122">Bläddra nedåt i toohello lista över variabler och kontrollera hello definition för hello **dbVMSetting** variabler, som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-122">Scroll down toohello list of variables, and check hello definition for hello **dbVMSetting** variables, listed below.</span></span> <span data-ttu-id="7d66b-123">Den tar emot en hello matriselement i hello **dbVMSettings** variabeln.</span><span class="sxs-lookup"><span data-stu-id="7d66b-123">It receives one of hello array elements contained in hello **dbVMSettings** variable.</span></span> <span data-ttu-id="7d66b-124">Om du är bekant med termer som software development, kan du visa hello **dbVMSettings** variabeln som en hashtabell eller en ordlista.</span><span class="sxs-lookup"><span data-stu-id="7d66b-124">If you are familiar with software development terminology, you can view hello **dbVMSettings** variable as a hash table, or a dictionary.</span></span>

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. <span data-ttu-id="7d66b-125">Anta att du bestämmer dig för toodeploy Windows virtuella datorer som kör SQL i hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="7d66b-125">Suppose you decide toodeploy Windows VMs running SQL in hello back-end.</span></span> <span data-ttu-id="7d66b-126">Hej värde för **osType** skulle vara *Windows*, och hello **dbVMSetting** variabeln skulle innehålla hello-element som anges nedan, och som motsvarar hello första värdet i hello **dbVMSettings** variabeln.</span><span class="sxs-lookup"><span data-stu-id="7d66b-126">Then hello value for **osType** would be *Windows*, and hello **dbVMSetting** variable would contain hello element listed below, which represents hello first value in hello **dbVMSettings** variable.</span></span>

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. <span data-ttu-id="7d66b-127">Meddelande hello **vmSize** innehåller hello värdet *Standard_DS3*.</span><span class="sxs-lookup"><span data-stu-id="7d66b-127">Notice hello **vmSize** contains hello value *Standard_DS3*.</span></span> <span data-ttu-id="7d66b-128">Endast vissa VM-storlekar tillåta hello användning av flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="7d66b-128">Only certain VM sizes allow for hello use of multiple NICs.</span></span> <span data-ttu-id="7d66b-129">Du kan kontrollera vilka VM-storlekar stöd för flera nätverkskort genom att läsa hello [Windows VM-storlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [Linux VM-storlekar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artiklar.</span><span class="sxs-lookup"><span data-stu-id="7d66b-129">You can verify which VM sizes support multiple NICs by reading hello [Windows VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux VM sizes](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) articles.</span></span>

7. <span data-ttu-id="7d66b-130">Rulla nedåt för**resurser** och meddelande hello första elementet.</span><span class="sxs-lookup"><span data-stu-id="7d66b-130">Scroll down too**resources** and notice hello first element.</span></span> <span data-ttu-id="7d66b-131">Beskriver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7d66b-131">It describes a storage account.</span></span> <span data-ttu-id="7d66b-132">Det här lagringskontot kommer att använda toomaintain hello datadiskar som används av varje VM-databas.</span><span class="sxs-lookup"><span data-stu-id="7d66b-132">This storage account will be used toomaintain hello data disks used by each database VM.</span></span> <span data-ttu-id="7d66b-133">I det här scenariot har varje databas VM en OS-disk som lagras i reguljära storage och två datadiskar för som lagras i SSD (premium) lagring.</span><span class="sxs-lookup"><span data-stu-id="7d66b-133">In this scenario, each database VM has an OS disk stored in regular storage, and two data disks stored in SSD (premium) storage.</span></span>

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. <span data-ttu-id="7d66b-134">Rulla ned toohello nästa resurs, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-134">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="7d66b-135">Denna resurs representerar hello nätverkskort används för åtkomst till databasen i varje VM-databasen.</span><span class="sxs-lookup"><span data-stu-id="7d66b-135">This resource represents hello NIC used for database access in each database VM.</span></span> <span data-ttu-id="7d66b-136">Meddelande hello användning av hello **kopiera** funktion i den här resursen.</span><span class="sxs-lookup"><span data-stu-id="7d66b-136">Notice hello use of hello **copy** function in this resource.</span></span> <span data-ttu-id="7d66b-137">hello mall kan du toodeploy många virtuella datorer som du vill, som baseras på hello **dbCount** parameter.</span><span class="sxs-lookup"><span data-stu-id="7d66b-137">hello template allows you toodeploy as many VMs as you want, based on hello **dbCount** parameter.</span></span> <span data-ttu-id="7d66b-138">Du måste därför toocreate hello samma mängd nätverkskort för åtkomst till databasen, en för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d66b-138">Therefore you need toocreate hello same amount of NICs for database access, one for each VM.</span></span>

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. <span data-ttu-id="7d66b-139">Rulla ned toohello nästa resurs, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-139">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="7d66b-140">Denna resurs representerar hello nätverkskort används för hantering i varje databas VM.</span><span class="sxs-lookup"><span data-stu-id="7d66b-140">This resource represents hello NIC used for management in each database VM.</span></span> <span data-ttu-id="7d66b-141">Återigen behöver du något av dessa nätverkskort för varje databas VM.</span><span class="sxs-lookup"><span data-stu-id="7d66b-141">Once again, you need one of these NICs for each database VM.</span></span> <span data-ttu-id="7d66b-142">Meddelande hello **networkSecurityGroup** element, länka en NSG som tillåter åtkomst tooRDP/SSH toothis NIC endast.</span><span class="sxs-lookup"><span data-stu-id="7d66b-142">Notice hello **networkSecurityGroup** element, linking an NSG that allows access tooRDP/SSH toothis NIC only.</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. <span data-ttu-id="7d66b-143">Rulla ned toohello nästa resurs, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-143">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="7d66b-144">Denna resurs representerar en tillgänglighet set toobe som delas av alla virtuella datorer som databas.</span><span class="sxs-lookup"><span data-stu-id="7d66b-144">This resource represents an availability set toobe shared by all database VMs.</span></span> <span data-ttu-id="7d66b-145">På så sätt kan du garantera att det alltid är en virtuell dator i hello ange körs under underhåll.</span><span class="sxs-lookup"><span data-stu-id="7d66b-145">That way, you guarantee that there will always be one VM in hello set running during maintenance.</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. <span data-ttu-id="7d66b-146">Rulla ned toohello nästa resurs.</span><span class="sxs-lookup"><span data-stu-id="7d66b-146">Scroll down toohello next resource.</span></span> <span data-ttu-id="7d66b-147">Denna resurs representerar hello databasen virtuella datorer, som visas först i hello raderna nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-147">This resource represents hello database VMs, as seen in hello first few lines listed below.</span></span> <span data-ttu-id="7d66b-148">Meddelande hello användning av hello **kopiera** fungera igen, se till att flera virtuella datorer skapas baserat på hello **dbCount** parameter.</span><span class="sxs-lookup"><span data-stu-id="7d66b-148">Notice hello use of hello **copy** function again, ensuring that multiple VMs are created based on hello **dbCount** parameter.</span></span> <span data-ttu-id="7d66b-149">Observera också hello **dependsOn** samling.</span><span class="sxs-lookup"><span data-stu-id="7d66b-149">Also notice hello **dependsOn** collection.</span></span> <span data-ttu-id="7d66b-150">Visas en lista med två nätverkskort som behövs toobe som skapats före hello VM distribueras tillsammans med hello tillgänglighetsuppsättning och hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7d66b-150">It lists two NICs being necessary toobe created before hello VM is deployed, along with hello availability set, and hello storage account.</span></span>

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. <span data-ttu-id="7d66b-151">Rulla nedåt i hello VM resurs toohello **networkProfile** element, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-151">Scroll down in hello VM resource toohello **networkProfile** element, as listed below.</span></span> <span data-ttu-id="7d66b-152">Observera att det finns två nätverkskort som referens för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d66b-152">Notice that there are two NICs being reference for each VM.</span></span> <span data-ttu-id="7d66b-153">När du skapar flera nätverkskort för en virtuell dator måste du ange hello **primära** -egenskapen för en av hello nätverkskort för*SANT*, och hello rest för*FALSKT*.</span><span class="sxs-lookup"><span data-stu-id="7d66b-153">When you create multiple NICs for a VM, you must set hello **primary** property of one of hello NICs too*true*, and hello rest too*false*.</span></span>

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a><span data-ttu-id="7d66b-154">Distribuera hello ARM-mallen med hjälp av klickar du på toodeploy</span><span class="sxs-lookup"><span data-stu-id="7d66b-154">Deploy hello ARM template by using click toodeploy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d66b-155">Se till att du följer hello [förutsättningar](#Pre-requisites) steg innan du följer hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-155">Make sure you follow hello [pre-requisites](#Pre-requisites) steps before following hello instructions below.</span></span>
> 

<span data-ttu-id="7d66b-156">hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-156">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="7d66b-157">toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello höger i **Backend resursgrupp (se dokumentationen)** klickar du på **distribuera tooAzure**, Ersätt Hej Standardparametervärden om det behövs, och följ anvisningarna för hello hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d66b-157">toodeploy this template using click toodeploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello right of **Backend resource group (see documentation)** click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

<span data-ttu-id="7d66b-158">hello bilden nedan visar hello innehållet i hello ny resursgrupp efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="7d66b-158">hello figure below shows hello contents of hello new resource group, after deployment.</span></span>

![Backend-resursgrupp](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="7d66b-160">Distribuera hello-mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7d66b-160">Deploy hello template by using PowerShell</span></span>
<span data-ttu-id="7d66b-161">toodeploy hello mallen som du hämtade med PowerShell, installerar och konfigurerar PowerShell genom att slutföra hello stegen i hello [installerar och konfigurerar PowerShell](/powershell/azure/overview) artikel och slutför sedan hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7d66b-161">toodeploy hello template you downloaded by using PowerShell, install and configure PowerShell by completing hello steps in hello [Install and configure PowerShell](/powershell/azure/overview) article and then complete hello following steps:</span></span>

<span data-ttu-id="7d66b-162">Kör hello  **`New-AzureRmResourceGroup`**  cmdlet toocreate en resurs med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="7d66b-162">Run hello **`New-AzureRmResourceGroup`** cmdlet toocreate a resource group using hello template.</span></span>

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

<span data-ttu-id="7d66b-163">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="7d66b-163">Expected output:</span></span>

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="7d66b-164">Distribuera hello mallen med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7d66b-164">Deploy hello template by using hello Azure CLI</span></span>
<span data-ttu-id="7d66b-165">toodeploy hello mall med hjälp av hello Azure CLI, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-165">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="7d66b-166">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7d66b-166">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="7d66b-167">Kör hello  **`azure config mode`**  kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-167">Run hello **`azure config mode`** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="7d66b-168">hello förväntas följande utdata:</span><span class="sxs-lookup"><span data-stu-id="7d66b-168">hello expected output follows:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="7d66b-169">Öppna hello [parameterfilen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), Välj dess innehåll och spara den tooa filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7d66b-169">Open hello [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), select its contents, and save it tooa file in your computer.</span></span> <span data-ttu-id="7d66b-170">Det här exemplet, vi sparat hello parameterfilen för*parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="7d66b-170">For this example, we saved hello parameters file too*parameters.json*.</span></span>
4. <span data-ttu-id="7d66b-171">Kör hello  **`azure group deployment create`**  cmdlet toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="7d66b-171">Run hello **`azure group deployment create`** cmdlet toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="7d66b-172">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="7d66b-172">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    <span data-ttu-id="7d66b-173">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="7d66b-173">Expected output:</span></span>
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

