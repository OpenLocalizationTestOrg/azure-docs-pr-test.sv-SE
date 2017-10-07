---
title: "aaaRequire säker överföring i Azure Storage | Microsoft Docs"
description: "Läs mer om hello ”kräver säker överföring” funktionen för Azure Storage, och hur tooenable den."
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.openlocfilehash: 27f745c5e771b50213c1dbb39dee081947be1f39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="require-secure-transfer"></a><span data-ttu-id="68c6a-103">Kräv säker överföring</span><span class="sxs-lookup"><span data-stu-id="68c6a-103">Require secure transfer</span></span>

<span data-ttu-id="68c6a-104">Hej ”överföringen säker krävs” alternativet ökar hello säkerheten för ditt lagringskonto genom att bara tillåta begäranden toohello storage-konto från säkra anslutningar.</span><span class="sxs-lookup"><span data-stu-id="68c6a-104">hello "Secure transfer required" option enhances hello security of your storage account by only allowing requests toohello storage account from secure connections.</span></span> <span data-ttu-id="68c6a-105">Till exempel vid anrop av REST API: er tooaccess storage-konto, måste du ansluta med hjälp av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="68c6a-105">For example, when calling REST APIs tooaccess your storage account, you must connect using HTTPS.</span></span> <span data-ttu-id="68c6a-106">Alla begäranden som använder HTTP avvisas när ”säker överföring krävs” är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="68c6a-106">Any requests using HTTP are rejected when "Secure transfer required" is enabled.</span></span>

<span data-ttu-id="68c6a-107">När du använder hello Azure Files tjänsten misslyckas en anslutning utan kryptering när ”säker överföring krävs” är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="68c6a-107">When you are using hello Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="68c6a-108">Detta inkluderar scenarier med hjälp av SMB 2.1 och SMB 3.0 utan kryptering vissa varianter av hello Linux SMB-klienten.</span><span class="sxs-lookup"><span data-stu-id="68c6a-108">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of hello Linux SMB client.</span></span> 

<span data-ttu-id="68c6a-109">Hej ”överföringen säker krävs” alternativet är inaktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="68c6a-109">By default, hello "Secure transfer required" option is disabled.</span></span>

> [!NOTE]
> <span data-ttu-id="68c6a-110">Eftersom Azure Storage stöder HTTPS för anpassade domännamn, tillämpas inte det här alternativet när du använder ett anpassat domännamn.</span><span class="sxs-lookup"><span data-stu-id="68c6a-110">Because Azure Storage doesn't support HTTPS for custom domain names, this option is not applied when using a custom domain name.</span></span>

## <a name="enable-secure-transfer-required-in-hello-azure-portal"></a><span data-ttu-id="68c6a-111">Aktivera ”säker överföring krävs” i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="68c6a-111">Enable "Secure transfer required" in hello Azure portal</span></span>

<span data-ttu-id="68c6a-112">Du kan aktivera hello ”säker överföring krävs” inställningen både när du skapar ett lagringskonto i hello [Azure-portalen](https://portal.azure.com), och för befintliga lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="68c6a-112">You can enable hello "Secure transfer required" setting both when you create a storage account in hello [Azure portal](https://portal.azure.com), and for existing storage accounts.</span></span>

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a><span data-ttu-id="68c6a-113">Kräv säker överföring när du skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="68c6a-113">Require secure transfer when you create a storage account</span></span>

1. <span data-ttu-id="68c6a-114">Öppna hello **skapa lagringskonto** bladet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="68c6a-114">Open hello **Create storage account** blade in hello Azure portal.</span></span>
1. <span data-ttu-id="68c6a-115">Under **säker överföring krävs**väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="68c6a-115">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![Skärmbild](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a><span data-ttu-id="68c6a-117">Kräv säker överföring för ett befintligt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="68c6a-117">Require secure transfer for an existing storage account</span></span>

1. <span data-ttu-id="68c6a-118">Välj ett befintligt lagringskonto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="68c6a-118">Select an existing storage account in hello Azure portal.</span></span>
1. <span data-ttu-id="68c6a-119">Välj **Configuration** under **inställningar** i hello lagring kontoblad-menyn.</span><span class="sxs-lookup"><span data-stu-id="68c6a-119">Select **Configuration** under **SETTINGS** in hello storage account menu blade.</span></span>
1. <span data-ttu-id="68c6a-120">Under **säker överföring krävs**väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="68c6a-120">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![Skärmbild](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a><span data-ttu-id="68c6a-122">Aktivera ”säker överföring krävs” programmässigt</span><span class="sxs-lookup"><span data-stu-id="68c6a-122">Enable "Secure transfer required" programmatically</span></span>

<span data-ttu-id="68c6a-123">hello Inställningens namn är _supportsHttpsTrafficOnly_ i lagringskontoegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="68c6a-123">hello setting name is _supportsHttpsTrafficOnly_ in storage account properties.</span></span> <span data-ttu-id="68c6a-124">Du kan aktivera ”säker överföring krävs” med REST API eller verktyg, bibliotek:</span><span class="sxs-lookup"><span data-stu-id="68c6a-124">You can enable "Secure transfer required" setting with REST API, tools or libraries:</span></span>

* <span data-ttu-id="68c6a-125">**REST API** (Version: 2016-12-01): [versionspaket](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span><span class="sxs-lookup"><span data-stu-id="68c6a-125">**REST API** (Version: 2016-12-01): [release package](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span></span>
* <span data-ttu-id="68c6a-126">**PowerShell** (Version: 4.1.0): [versionspaket](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span><span class="sxs-lookup"><span data-stu-id="68c6a-126">**PowerShell** (Version: 4.1.0): [release package](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span></span>
* <span data-ttu-id="68c6a-127">**CLI** (Version: 2.0.11): [versionspaket](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span><span class="sxs-lookup"><span data-stu-id="68c6a-127">**CLI** (Version: 2.0.11): [release package](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span></span>
* <span data-ttu-id="68c6a-128">**NodeJS** (Version: 1.1.0): [versionspaket](https://www.npmjs.com/package/azure-arm-storage/)</span><span class="sxs-lookup"><span data-stu-id="68c6a-128">**NodeJS** (Version: 1.1.0): [release package](https://www.npmjs.com/package/azure-arm-storage/)</span></span>
* <span data-ttu-id="68c6a-129">**.NET SDK** (Version: 6.3.0): [versionspaket](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span><span class="sxs-lookup"><span data-stu-id="68c6a-129">**.NET SDK** (Version: 6.3.0): [release package](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span></span>
* <span data-ttu-id="68c6a-130">**Python SDK** (Version: 1.1.0): [versionspaket](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span><span class="sxs-lookup"><span data-stu-id="68c6a-130">**Python SDK** (Version: 1.1.0): [release package](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span></span>
* <span data-ttu-id="68c6a-131">**Ruby SDK** (Version: 0.11.0): [versionspaket](https://rubygems.org/gems/azure_mgmt_storage)</span><span class="sxs-lookup"><span data-stu-id="68c6a-131">**Ruby SDK** (Version: 0.11.0): [release package](https://rubygems.org/gems/azure_mgmt_storage)</span></span>

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a><span data-ttu-id="68c6a-132">Aktivera ”säker överföring krävs” med REST API</span><span class="sxs-lookup"><span data-stu-id="68c6a-132">Enable "Secure transfer required" setting with REST API</span></span>

<span data-ttu-id="68c6a-133">toosimplify testning med REST API, som du kan använda [ArmClient](https://github.com/projectkudu/ARMClient) toocall från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="68c6a-133">toosimplify testing with REST API, you can use [ArmClient](https://github.com/projectkudu/ARMClient) toocall from command line.</span></span>

 <span data-ttu-id="68c6a-134">Du kan använda under kommandoraden toocheck hello med hello REST-API:</span><span class="sxs-lookup"><span data-stu-id="68c6a-134">You can use below command line toocheck hello setting with hello REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

<span data-ttu-id="68c6a-135">Hello svar hittar du _supportsHttpsTrafficOnly_ inställningen.</span><span class="sxs-lookup"><span data-stu-id="68c6a-135">In hello response, you can find _supportsHttpsTrafficOnly_ setting.</span></span> <span data-ttu-id="68c6a-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="68c6a-136">Sample:</span></span>

```Json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}",
  "kind": "Storage",
  ...
  "properties": {
    ...
    "supportsHttpsTrafficOnly": false
  },
  "type": "Microsoft.Storage/storageAccounts"
}
```

<span data-ttu-id="68c6a-137">Du kan använda under kommandoraden tooenable hello med hello REST-API:</span><span class="sxs-lookup"><span data-stu-id="68c6a-137">You can use below command line tooenable hello setting with hello REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json
```
<span data-ttu-id="68c6a-138">Exempel på Input.json:</span><span class="sxs-lookup"><span data-stu-id="68c6a-138">Sample of Input.json:</span></span>
```Json
{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="68c6a-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68c6a-139">Next steps</span></span>
<span data-ttu-id="68c6a-140">Azure Storage tillhandahåller en omfattande uppsättning säkerhetsfunktioner för, vilket tillsammans ger utvecklare möjligheten toobuild säkra program.</span><span class="sxs-lookup"><span data-stu-id="68c6a-140">Azure Storage provides a comprehensive set of security capabilities, which together enable developers toobuild secure applications.</span></span> <span data-ttu-id="68c6a-141">Mer information finns i hello [säkerhetsguiden för lagring](storage-security-guide.md).</span><span class="sxs-lookup"><span data-stu-id="68c6a-141">For more details, visit hello [Storage Security Guide](storage-security-guide.md).</span></span>
