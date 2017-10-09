---
title: "aaaCreate FQDN för en Linux-VM i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate ett fullständigt domännamn eller FQDN för Resource Manager baserad virtuell dator i hello Azure-portalen."
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
ms.openlocfilehash: 1494a0cb1caa62069c72096a739aee111ac8b383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-linux-vm"></a><span data-ttu-id="701bf-103">Skapa ett fullständigt kvalificerat domännamn i hello Azure-portalen för en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="701bf-103">Create a fully qualified domain name in hello Azure portal for a Linux VM</span></span>

<span data-ttu-id="701bf-104">När du skapar en virtuell dator (VM) i hello [Azure-portalen](https://portal.azure.com), en offentlig IP-resurs för hello virtuell dator skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="701bf-104">When you create a virtual machine (VM) in hello [Azure portal](https://portal.azure.com), a public IP resource for hello virtual machine is automatically created.</span></span> <span data-ttu-id="701bf-105">Du kan använda den här IP-adressen tooremotely åtkomst hello VM.</span><span class="sxs-lookup"><span data-stu-id="701bf-105">You use this IP address tooremotely access hello VM.</span></span> <span data-ttu-id="701bf-106">Även om hello portal inte skapar en [fullständigt kvalificerade domännamnet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), eller FQDN, du kan lägga till en när hello VM har skapats.</span><span class="sxs-lookup"><span data-stu-id="701bf-106">Although hello portal does not create a [fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), or FQDN, you can add one once hello VM is created.</span></span> <span data-ttu-id="701bf-107">Den här artikeln visar hello steg toocreate ett DNS-namn eller FQDN.</span><span class="sxs-lookup"><span data-stu-id="701bf-107">This article demonstrates hello steps toocreate a DNS name or FQDN.</span></span>

## <a name="create-a-fqdn"></a><span data-ttu-id="701bf-108">Skapa ett fullständigt domännamn</span><span class="sxs-lookup"><span data-stu-id="701bf-108">Create a FQDN</span></span>
<span data-ttu-id="701bf-109">Den här artikeln förutsätter att du redan har skapat en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="701bf-109">This article assumes that you have already created a VM.</span></span> <span data-ttu-id="701bf-110">Om det behövs kan du [skapa en virtuell dator i hello portal](quick-create-portal.md) eller [med hello Azure CLI](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="701bf-110">If needed, you can [create a VM in hello portal](quick-create-portal.md) or [with hello Azure CLI](quick-create-cli.md).</span></span> <span data-ttu-id="701bf-111">Följ dessa steg när den virtuella datorn är igång:</span><span class="sxs-lookup"><span data-stu-id="701bf-111">Follow these steps once your VM is up and running:</span></span>

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

<span data-ttu-id="701bf-112">Du kan nu fjärransluta toohello VM som använder den här DNS-namn som med `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="701bf-112">You can now connect remotely toohello VM using this DNS name such as with `ssh azureuser@mydns.westus.cloudapp.azure.com`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="701bf-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="701bf-113">Next steps</span></span>
<span data-ttu-id="701bf-114">Nu när den virtuella datorn har ett offentligt IP-adress och DNS-namn, du kan distribuera gemensamt ramverk för programmet eller tjänster, till exempel nginx, MongoDB, Docker, osv.</span><span class="sxs-lookup"><span data-stu-id="701bf-114">Now that your VM has a public IP and DNS name, you can deploy common application frameworks or services such as nginx, MongoDB, Docker, etc.</span></span>

<span data-ttu-id="701bf-115">Du kan också läsa mer om [med Resource Manager](../../azure-resource-manager/resource-group-overview.md) tips om hur du skapar dina Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="701bf-115">You can also read more about [using Resource Manager](../../azure-resource-manager/resource-group-overview.md) for tips on building your Azure deployments.</span></span>

