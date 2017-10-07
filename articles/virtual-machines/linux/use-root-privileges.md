---
title: "aaaUse rotprivilegier på Linux-datorer | Microsoft Docs"
description: "Lär dig hur toouse rot behörighet på en virtuell Linux-dator i Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: a2c106a2-dceb-43a3-9dd1-50ed77685952
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 9411588c5fd0c86c4c73b3e44fbb56ab150013d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="81611-103">Använd rotprivilegier på virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="81611-103">Using root privileges on Linux virtual machines in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="81611-104">Som standard hello `root` användaren är inaktiverad på Linux-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="81611-104">By default, hello `root` user is disabled on Linux virtual machines in Azure.</span></span> <span data-ttu-id="81611-105">Användare kan köra kommandon med utökade privilegier med hjälp av hello `sudo` kommando.</span><span class="sxs-lookup"><span data-stu-id="81611-105">Users can run commands with elevated privileges by using hello `sudo` command.</span></span> <span data-ttu-id="81611-106">Hello upplevelse kan dock variera beroende på hur hello systemet har etablerats.</span><span class="sxs-lookup"><span data-stu-id="81611-106">However, hello experience may vary depending on how hello system was provisioned.</span></span>

1. <span data-ttu-id="81611-107">**SSH-nyckel och lösenord eller lösenord endast** -hello virtuella datorn har etablerats med antingen ett certifikat (`.CER` fil) eller SSH-nyckeln som ett lösenord, eller bara ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="81611-107">**SSH key and password OR password only** - hello virtual machine was provisioned with either a certificate (`.CER` file) or SSH key as well as a password, or just a user name and password.</span></span> <span data-ttu-id="81611-108">I det här fallet `sudo` ombeds du att hello användarens lösenord innan hello kommando körs.</span><span class="sxs-lookup"><span data-stu-id="81611-108">In this case `sudo` will prompt for hello user's password before executing hello command.</span></span>
2. <span data-ttu-id="81611-109">**SSH-nyckeln** -hello virtuella datorn har etablerats med ett certifikat (`.cer`, `.pem`, eller `.pub` filen) eller SSH-nyckel, men inget lösenord.</span><span class="sxs-lookup"><span data-stu-id="81611-109">**SSH key only** - hello virtual machine was provisioned with a certificate (`.cer`, `.pem`, or `.pub` file) or SSH key, but no password.</span></span>  <span data-ttu-id="81611-110">I det här fallet `sudo` **inte** fråga efter hello användarens lösenord innan hello kommando körs.</span><span class="sxs-lookup"><span data-stu-id="81611-110">In this case `sudo` **will not** prompt for hello user's password before executing hello command.</span></span>

## <a name="ssh-key-and-password-or-password-only"></a><span data-ttu-id="81611-111">SSH-nyckel och lösenord eller lösenord endast</span><span class="sxs-lookup"><span data-stu-id="81611-111">SSH Key and Password, or Password Only</span></span>
<span data-ttu-id="81611-112">Logga in på hello virtuell Linux-dator med hjälp av SSH-nyckeln eller lösenordet autentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="81611-112">Log into hello Linux virtual machine using SSH key or password authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>
    [sudo] password for azureuser:

<span data-ttu-id="81611-113">I det här fallet uppmanas hello användaren att ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="81611-113">In this case hello user will be prompted for a password.</span></span> <span data-ttu-id="81611-114">När du har angett hello lösenord `sudo` körs hello-kommando med `root` privilegier.</span><span class="sxs-lookup"><span data-stu-id="81611-114">After entering hello password `sudo` will run hello command with `root` privileges.</span></span>

<span data-ttu-id="81611-115">Du kan också aktivera passwordless sudo genom att redigera hello `/etc/sudoers.d/waagent` fil, till exempel:</span><span class="sxs-lookup"><span data-stu-id="81611-115">You can also enable passwordless sudo by editing hello `/etc/sudoers.d/waagent` file, for example:</span></span>

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

<span data-ttu-id="81611-116">Detta innebär att för passwordless sudo av hello användare ”azureuser”.</span><span class="sxs-lookup"><span data-stu-id="81611-116">This change will allow for passwordless sudo by hello user "azureuser".</span></span>

## <a name="ssh-key-only"></a><span data-ttu-id="81611-117">SSH nyckeln endast</span><span class="sxs-lookup"><span data-stu-id="81611-117">SSH Key Only</span></span>
<span data-ttu-id="81611-118">Logga in på hello virtuell Linux-dator med hjälp av SSH-nyckelautentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="81611-118">Log into hello Linux virtual machine using SSH key authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>

<span data-ttu-id="81611-119">I det här fallet hello användaren kommer **inte** uppmanas att ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="81611-119">In this case hello user will **not** be prompted for a password.</span></span> <span data-ttu-id="81611-120">När du trycker på `<enter>`, `sudo` körs hello-kommando med `root` privilegier.</span><span class="sxs-lookup"><span data-stu-id="81611-120">After pressing `<enter>`, `sudo` will run hello command with `root` privileges.</span></span>

