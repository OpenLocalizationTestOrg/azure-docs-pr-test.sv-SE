---
title: "aaaCreate FQDN för en virtuell Windows-dator i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate ett fullständigt domännamn eller FQDN för Resource Manager baserad virtuell dator i hello Azure-portalen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 67c817ec97073803e513bc41ebde67b75ced565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-windows-vm"></a><span data-ttu-id="e37ef-103">Skapa ett fullständigt kvalificerat domännamn i hello Azure-portalen för en virtuell Windows-dator</span><span class="sxs-lookup"><span data-stu-id="e37ef-103">Create a fully qualified domain name in hello Azure portal for a Windows VM</span></span>

<span data-ttu-id="e37ef-104">När du skapar en virtuell dator (VM) i hello [Azure-portalen](https://portal.azure.com), en offentlig IP-resurs för hello virtuell dator skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e37ef-104">When you create a virtual machine (VM) in hello [Azure portal](https://portal.azure.com), a public IP resource for hello virtual machine is automatically created.</span></span> <span data-ttu-id="e37ef-105">Du kan använda den här IP-adressen tooremotely åtkomst hello VM.</span><span class="sxs-lookup"><span data-stu-id="e37ef-105">You use this IP address tooremotely access hello VM.</span></span> <span data-ttu-id="e37ef-106">Även om hello portal inte skapar en [fullständigt kvalificerade domännamnet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), eller FQDN, kan du skapa en när hello VM har skapats.</span><span class="sxs-lookup"><span data-stu-id="e37ef-106">Although hello portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can create one once hello VM is created.</span></span> <span data-ttu-id="e37ef-107">Den här artikeln visar hello steg toocreate ett DNS-namn eller FQDN.</span><span class="sxs-lookup"><span data-stu-id="e37ef-107">This article demonstrates hello steps toocreate a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="e37ef-108">Skapa ett fullständigt domännamn</span><span class="sxs-lookup"><span data-stu-id="e37ef-108">Create a FQDN</span></span>
<span data-ttu-id="e37ef-109">Den här artikeln förutsätter att du redan har skapat en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e37ef-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="e37ef-110">Om det behövs kan du [skapa en virtuell dator i hello portal](quick-create-portal.md) eller [med Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e37ef-110">If needed, you can [create a VM in hello portal](quick-create-portal.md) or [with Azure PowerShell](quick-create-powershell.md).</span></span> <span data-ttu-id="e37ef-111">Följ dessa steg när den virtuella datorn är igång:</span><span class="sxs-lookup"><span data-stu-id="e37ef-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="e37ef-112">Du kan nu fjärransluta toohello VM som använder den här DNS-namn som för Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="e37ef-112">You can now connect remotely toohello VM using this DNS name such as for Remote Desktop Protocol (RDP).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e37ef-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e37ef-113">Next steps</span></span>
<span data-ttu-id="e37ef-114">Nu när den virtuella datorn har ett offentligt IP-adress och DNS-namn, kan du distribuera gemensamt ramverk för programmet eller tjänster som IIS, SQL och SharePoint.</span><span class="sxs-lookup"><span data-stu-id="e37ef-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as IIS, SQL, or SharePoint.</span></span>

<span data-ttu-id="e37ef-115">Du kan också läsa mer om [med Resource Manager](../../azure-resource-manager/resource-group-overview.md) tips om hur du skapar dina Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="e37ef-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

