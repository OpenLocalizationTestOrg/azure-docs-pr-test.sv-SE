---
title: "Skapa FQDN för en Linux-VM i Azure portal | Microsoft Docs"
description: "Lär dig hur du skapar ett fullständigt domännamn eller FQDN för Resource Manager-baserad virtuell dator i Azure-portalen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49bfec791fcca3feabc4eb280cefd7faada0ea31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a><span data-ttu-id="4f9a7-103">Skapa ett fullständigt kvalificerat domännamn i Azure-portalen för en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="4f9a7-103">Create a fully qualified domain name in the Azure portal for a Linux VM</span></span>

<span data-ttu-id="4f9a7-104">När du skapar en virtuell dator (VM) i den [Azure-portalen](https://portal.azure.com), en offentlig IP-resurs för den virtuella datorn skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-104">When you create a virtual machine (VM) in the [Azure portal](https://portal.azure.com), a public IP resource for the virtual machine is automatically created.</span></span> <span data-ttu-id="4f9a7-105">Du kan använda denna IP-adress för att komma åt den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-105">You use this IP address to remotely access the VM.</span></span> <span data-ttu-id="4f9a7-106">Även om portalen inte skapar en [fullständigt kvalificerade domännamnet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), eller FQDN, du kan lägga till en när den virtuella datorn har skapats.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-106">Although the portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can add one once the VM is created.</span></span> <span data-ttu-id="4f9a7-107">Den här artikeln visar hur du skapar en DNS-namn eller ett FQDN.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-107">This article demonstrates the steps to create a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="4f9a7-108">Skapa ett fullständigt domännamn</span><span class="sxs-lookup"><span data-stu-id="4f9a7-108">Create a FQDN</span></span>
<span data-ttu-id="4f9a7-109">Den här artikeln förutsätter att du redan har skapat en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="4f9a7-110">Om det behövs kan du [skapa en virtuell dator i portalen](quick-create-portal.md) eller [med Azure CLI](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="4f9a7-110">If needed, you can [create a VM in the portal](quick-create-portal.md) or [with the Azure CLI](quick-create-cli.md).</span></span> <span data-ttu-id="4f9a7-111">Följ dessa steg när den virtuella datorn är igång:</span><span class="sxs-lookup"><span data-stu-id="4f9a7-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="4f9a7-112">Du kan nu fjärransluta till den virtuella datorn använder den här DNS-namn som med `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-112">You can now connect remotely to the VM using this DNS name such as with `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f9a7-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f9a7-113">Next steps</span></span>
<span data-ttu-id="4f9a7-114">Nu när den virtuella datorn har ett offentligt IP-adress och DNS-namn, du kan distribuera gemensamt ramverk för programmet eller tjänster, till exempel nginx, MongoDB, Docker, osv.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as nginx, MongoDB, Docker, etc.</span></span>

<span data-ttu-id="4f9a7-115">Du kan också läsa mer om [med Resource Manager](../../azure-resource-manager/resource-group-overview.md) tips om hur du skapar dina Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="4f9a7-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

