---
title: "Snabbstart för aaaAzure moln Shell (förhandsversion) | Microsoft Docs"
description: "Snabbstart för hello Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: e60700b92c10c331910dd8bb3c627fe1a024091c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-for-using-hello-azure-cloud-shell"></a><span data-ttu-id="ad12a-103">Snabbstart för att använda hello Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="ad12a-103">Quickstart for using hello Azure Cloud Shell</span></span>

<span data-ttu-id="ad12a-104">Det här dokumentet beskriver hur toouse hello Azure Cloud Shell i hello [Azure-portalen](https://ms.portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ad12a-104">This document details how toouse hello Azure Cloud Shell in hello [Azure portal](https://ms.portal.azure.com/).</span></span>

## <a name="start-cloud-shell"></a><span data-ttu-id="ad12a-105">Starta molnet Shell</span><span class="sxs-lookup"><span data-stu-id="ad12a-105">Start Cloud Shell</span></span>
1. <span data-ttu-id="ad12a-106">Starta **moln Shell** från hello övre navigeringsfältet av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ad12a-106">Launch **Cloud Shell** from hello top navigation of hello Azure portal</span></span> <br>
![](media/shell-icon.png)
2. <span data-ttu-id="ad12a-107">Välj en prenumeration toocreate ett lagringskonto och Azure-filresursen</span><span class="sxs-lookup"><span data-stu-id="ad12a-107">Select a subscription toocreate a storage account and Azure file share</span></span>
3. <span data-ttu-id="ad12a-108">Välj ”Skapa lagring”</span><span class="sxs-lookup"><span data-stu-id="ad12a-108">Select "Create storage"</span></span>

> [!TIP]
> <span data-ttu-id="ad12a-109">Du autentiseras automatiskt i varje sesssion för Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="ad12a-109">You are automatically authenticated for Azure CLI 2.0 in every sesssion.</span></span>

### <a name="set-your-subscription"></a><span data-ttu-id="ad12a-110">Ange din prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad12a-110">Set your subscription</span></span>
1. <span data-ttu-id="ad12a-111">Lista över prenumerationer som du har åtkomst till:</span><span class="sxs-lookup"><span data-stu-id="ad12a-111">List subscriptions you have access to:</span></span> <br>
`az account list`
2. <span data-ttu-id="ad12a-112">Ange din önskade prenumeration:</span><span class="sxs-lookup"><span data-stu-id="ad12a-112">Set your preferred subscription:</span></span> <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> <span data-ttu-id="ad12a-113">Prenumerationen kommer att sparas för framtida sessioner med hjälp av `/home/<user>/.azure/azureProfile.json`.</span><span class="sxs-lookup"><span data-stu-id="ad12a-113">Your subscription will be remembered for future sessions using `/home/<user>/.azure/azureProfile.json`.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="ad12a-114">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="ad12a-114">Create a resource group</span></span>
<span data-ttu-id="ad12a-115">Skapa en ny resursgrupp i WestUS med namnet ”MyRG”:</span><span class="sxs-lookup"><span data-stu-id="ad12a-115">Create a new resource group in WestUS named "MyRG":</span></span> <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a><span data-ttu-id="ad12a-116">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="ad12a-116">Create a Linux VM</span></span>
<span data-ttu-id="ad12a-117">Skapa en Ubuntu VM i din nya resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ad12a-117">Create an Ubuntu VM in your new resource group.</span></span> <span data-ttu-id="ad12a-118">hello Azure CLI 2.0 skapar SSH-nycklar och installationsprogrammet hello VM med dem.</span><span class="sxs-lookup"><span data-stu-id="ad12a-118">hello Azure CLI 2.0 will create SSH keys and setup hello VM with them.</span></span> <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> <span data-ttu-id="ad12a-119">Hej tooauthenticate för offentliga och privata nycklar som används för den virtuella datorn placeras i `/User/.ssh/id_rsa` och `/User/.ssh/id_rsa.pub` av Azure CLI 2.0 som standard.</span><span class="sxs-lookup"><span data-stu-id="ad12a-119">hello public and private keys used tooauthenticate your VM are placed in `/User/.ssh/id_rsa` and `/User/.ssh/id_rsa.pub` by Azure CLI 2.0 by default.</span></span> <span data-ttu-id="ad12a-120">.Ssh-mappen sparas i dina anslutna Azure-filresursen 5 GB avbildningen.</span><span class="sxs-lookup"><span data-stu-id="ad12a-120">Your .ssh folder is persisted in your attached Azure file share's 5-GB image.</span></span>

<span data-ttu-id="ad12a-121">Ditt användarnamn på den här virtuella datorn kommer att ditt användarnamn som används i molnet Shell ($User@Azure:).</span><span class="sxs-lookup"><span data-stu-id="ad12a-121">Your username on this VM will be your username used in Cloud Shell ($User@Azure:).</span></span>

### <a name="ssh-into-your-linux-vm"></a><span data-ttu-id="ad12a-122">SSH till den virtuella Linux-datorn</span><span class="sxs-lookup"><span data-stu-id="ad12a-122">SSH into your Linux VM</span></span>
1. <span data-ttu-id="ad12a-123">Sök efter ditt VM-namn i hello Azure portal sökfältet</span><span class="sxs-lookup"><span data-stu-id="ad12a-123">Search for your VM name in hello Azure portal search bar</span></span>
2. <span data-ttu-id="ad12a-124">Klicka på ”Anslut” och kör:`ssh username@ipaddress`</span><span class="sxs-lookup"><span data-stu-id="ad12a-124">Click "Connect" and run: `ssh username@ipaddress`</span></span>

![](media/sshcmd-copy.png)

<span data-ttu-id="ad12a-125">Du bör se hello Ubuntu Välkommen prompt vid hello SSH anslutningen har upprättats.</span><span class="sxs-lookup"><span data-stu-id="ad12a-125">Upon establishing hello SSH connection, you should see hello Ubuntu welcome prompt.</span></span>
![](media/ubuntu-welcome.png)

## <a name="cleaning-up"></a><span data-ttu-id="ad12a-126">Rensa</span><span class="sxs-lookup"><span data-stu-id="ad12a-126">Cleaning up</span></span> 
<span data-ttu-id="ad12a-127">Ta bort resursgruppen och alla resurser i den:</span><span class="sxs-lookup"><span data-stu-id="ad12a-127">Delete your resource group and any resources within it:</span></span> <br>
<span data-ttu-id="ad12a-128">Kör `az group delete -n MyRG`</span><span class="sxs-lookup"><span data-stu-id="ad12a-128">Run `az group delete -n MyRG`</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad12a-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ad12a-129">Next Steps</span></span>
[<span data-ttu-id="ad12a-130">Lär dig mer om bestående lagring på molnet Shell</span><span class="sxs-lookup"><span data-stu-id="ad12a-130">Learn about persisting storage on Cloud Shell</span></span>](persisting-shell-storage.md) <br><span data-ttu-id="ad12a-131">
[Lär dig mer om Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span><span class="sxs-lookup"><span data-stu-id="ad12a-131">
[Learn about Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span></span> <br><span data-ttu-id="ad12a-132">
[Lär dig mer om Azure File storage](../storage/files/storage-files-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="ad12a-132">
[Learn about Azure File storage](../storage/files/storage-files-introduction.md)</span></span> <br>