---
title: "aaaCreate en arbets- eller skolkonto identitet i AAD för Linux | Microsoft Docs"
description: "Lär dig hur toocreate ett arbets- eller skolkonto identitet i Azure Active Directory toouse med Linux virtuella datorer."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: b0f86d77-c669-4aa1-a095-c2aa4d9105fe
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: 54c3d0b0e89fe1b2d6a9b58a46776fe446ed72b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-linux-vms"></a><span data-ttu-id="73c4f-103">Skapa en identitet för arbetet eller skolan i Azure Active Directory toouse med virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="73c4f-103">Creating a Work or School identity in Azure Active Directory toouse with Linux VMs</span></span>
<span data-ttu-id="73c4f-104">Om du har skapat en personlig Azure-konto eller har en personlig MSDN-prenumeration och skapa hello Azure-konto tootake nytta av hello MSDN Azure-krediter--du använde en *Microsoft-konto* identitet toocreate den.</span><span class="sxs-lookup"><span data-stu-id="73c4f-104">If you created a personal Azure account or have a personal MSDN subscription and created hello Azure account tootake advantage of hello MSDN Azure credits -- you used a *Microsoft account* identity toocreate it.</span></span> <span data-ttu-id="73c4f-105">Många av de viktigaste funktionerna i Azure-- [resurs mallar](../../azure-resource-manager/resource-group-overview.md) är ett exempel--kräver ett arbets- eller skolkonto konto (en identitet som hanteras av Azure Active Directory) toowork.</span><span class="sxs-lookup"><span data-stu-id="73c4f-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) toowork.</span></span> <span data-ttu-id="73c4f-106">Du kan följa hello instruktionerna nedan toocreate nya arbets- eller Skol-kontot eftersom Lyckligtvis något av bästa hello om ditt personliga Azure-konto är att det kommer en ny arbetet eller skolan med Azure Active Directory standarddomänen som du kan använda toocreate konto som du kan använda med Azure-funktioner som kräver detta.</span><span class="sxs-lookup"><span data-stu-id="73c4f-106">You can follow hello instructions below toocreate a new work or school account because fortunately, one of hello best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use toocreate a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="73c4f-107">Dock ändringar gör det möjligt toomanage din prenumeration med någon typ av Azure-konto med hjälp av hello `azure login` interaktiv inloggning metod som beskrivs [här](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="73c4f-107">However, recent changes make it possible toomanage your subscription with any type of Azure account using hello `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="73c4f-108">Du kan använda denna funktion eller du kan följa hello instruktionerna som följer.</span><span class="sxs-lookup"><span data-stu-id="73c4f-108">You can either use that mechanism, or you can follow hello instructions that follow.</span></span> <span data-ttu-id="73c4f-109">Du kan också [skapar en arbets- eller skolkonto identitet i Azure Active Directory toouse med virtuella Windows-datorer](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="73c4f-109">You can also [create a work or school identity in Azure Active Directory toouse with Windows VMs](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

