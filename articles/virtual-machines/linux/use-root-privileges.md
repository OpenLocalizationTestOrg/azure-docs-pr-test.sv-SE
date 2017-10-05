---
title: "Använda rotprivilegier på Linux-datorer | Microsoft Docs"
description: "Lär dig hur du använder rotprivilegier på en Linux-dator i Azure."
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
ms.openlocfilehash: dc39db1f5fecffb60499a5420bfe72850e2fffd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="285ea-103">Använd rotprivilegier på virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="285ea-103">Using root privileges on Linux virtual machines in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="285ea-104">Som standard den `root` användaren är inaktiverad på Linux-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="285ea-104">By default, the `root` user is disabled on Linux virtual machines in Azure.</span></span> <span data-ttu-id="285ea-105">Användare kan köra kommandon med utökade privilegier med hjälp av den `sudo` kommando.</span><span class="sxs-lookup"><span data-stu-id="285ea-105">Users can run commands with elevated privileges by using the `sudo` command.</span></span> <span data-ttu-id="285ea-106">Upplevelsen kan dock variera beroende på hur systemet har etablerats.</span><span class="sxs-lookup"><span data-stu-id="285ea-106">However, the experience may vary depending on how the system was provisioned.</span></span>

1. <span data-ttu-id="285ea-107">**SSH-nyckel och lösenord eller lösenord endast** -den virtuella datorn har etablerats med antingen ett certifikat (`.CER` fil) eller SSH-nyckeln som ett lösenord, eller bara ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="285ea-107">**SSH key and password OR password only** - the virtual machine was provisioned with either a certificate (`.CER` file) or SSH key as well as a password, or just a user name and password.</span></span> <span data-ttu-id="285ea-108">I det här fallet `sudo` ombeds du att lösenordet innan kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="285ea-108">In this case `sudo` will prompt for the user's password before executing the command.</span></span>
2. <span data-ttu-id="285ea-109">**SSH-nyckeln** -den virtuella datorn har etablerats med ett certifikat (`.cer`, `.pem`, eller `.pub` filen) eller SSH-nyckel, men inget lösenord.</span><span class="sxs-lookup"><span data-stu-id="285ea-109">**SSH key only** - the virtual machine was provisioned with a certificate (`.cer`, `.pem`, or `.pub` file) or SSH key, but no password.</span></span>  <span data-ttu-id="285ea-110">I det här fallet `sudo` **inte** fråga efter användarens lösenord innan kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="285ea-110">In this case `sudo` **will not** prompt for the user's password before executing the command.</span></span>

## <a name="ssh-key-and-password-or-password-only"></a><span data-ttu-id="285ea-111">SSH-nyckel och lösenord eller lösenord endast</span><span class="sxs-lookup"><span data-stu-id="285ea-111">SSH Key and Password, or Password Only</span></span>
<span data-ttu-id="285ea-112">Logga in på Linux virtuella datorer som använder SSH-nyckeln eller lösenordet autentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="285ea-112">Log into the Linux virtual machine using SSH key or password authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>
    [sudo] password for azureuser:

<span data-ttu-id="285ea-113">I det här fallet uppmanas användaren att ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="285ea-113">In this case the user will be prompted for a password.</span></span> <span data-ttu-id="285ea-114">När du har angett lösenordet `sudo` körs kommandot med `root` privilegier.</span><span class="sxs-lookup"><span data-stu-id="285ea-114">After entering the password `sudo` will run the command with `root` privileges.</span></span>

<span data-ttu-id="285ea-115">Du kan också aktivera passwordless sudo genom att redigera den `/etc/sudoers.d/waagent` -fil, till exempel:</span><span class="sxs-lookup"><span data-stu-id="285ea-115">You can also enable passwordless sudo by editing the `/etc/sudoers.d/waagent` file, for example:</span></span>

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

<span data-ttu-id="285ea-116">Detta innebär att för passwordless sudo av användaren ”azureuser”.</span><span class="sxs-lookup"><span data-stu-id="285ea-116">This change will allow for passwordless sudo by the user "azureuser".</span></span>

## <a name="ssh-key-only"></a><span data-ttu-id="285ea-117">SSH nyckeln endast</span><span class="sxs-lookup"><span data-stu-id="285ea-117">SSH Key Only</span></span>
<span data-ttu-id="285ea-118">Logga in på Linux virtuella datorer som använder SSH-nyckelautentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:</span><span class="sxs-lookup"><span data-stu-id="285ea-118">Log into the Linux virtual machine using SSH key authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>

<span data-ttu-id="285ea-119">I det här fallet kommer användaren **inte** uppmanas att ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="285ea-119">In this case the user will **not** be prompted for a password.</span></span> <span data-ttu-id="285ea-120">När du trycker på `<enter>`, `sudo` körs kommandot med `root` privilegier.</span><span class="sxs-lookup"><span data-stu-id="285ea-120">After pressing `<enter>`, `sudo` will run the command with `root` privileges.</span></span>

