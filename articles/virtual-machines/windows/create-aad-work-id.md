---
title: "Skapa en arbets- eller skolkonto identitet i AAD för Windows | Microsoft Docs"
description: "Lär dig hur du skapar en arbets- eller skolkonto identitet i Azure Active Directory för användning med Windows-datorer."
services: virtual-machines-windows
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d07dca34-618a-48aa-9971-03d9c1210f4a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: 7694b959a384aaed213adc31e02debca31b7c131
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a><span data-ttu-id="22412-103">Skapa en identitet för arbetet eller skolan i Azure Active Directory för användning med virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="22412-103">Creating a Work or School identity in Azure Active Directory to use with Windows VMs</span></span>
<span data-ttu-id="22412-104">Om du har skapat en personlig Azure-konto eller har en personlig MSDN-prenumeration och skapa Azure-konto för att kunna utnyttja MSDN Azure-krediter--du använde en *Microsoft-konto* identitet om du vill skapa den.</span><span class="sxs-lookup"><span data-stu-id="22412-104">If you created a personal Azure account or have a personal MSDN subscription and created the Azure account to take advantage of the MSDN Azure credits -- you used a *Microsoft account* identity to create it.</span></span> <span data-ttu-id="22412-105">Många av de viktigaste funktionerna i Azure-- [resurs mallar](../../azure-resource-manager/resource-group-overview.md) är ett exempel--kräver ett arbets- eller skolkonto konto (en identitet som hanteras av Azure Active Directory) ska fungera.</span><span class="sxs-lookup"><span data-stu-id="22412-105">Many great features of Azure -- [resource group templates](../../azure-resource-manager/resource-group-overview.md) is one example -- require a work or school account (an identity managed by Azure Active Directory) to work.</span></span> <span data-ttu-id="22412-106">Du kan följa anvisningarna nedan för att skapa en ny arbets- eller skolkonto eftersom Lyckligtvis något av de bästa om ditt personliga Azure-konto är att det kommer med en standard-Azure Active Directory-domän som du kan använda för att skapa en ny arbets- eller skolkonto som du kan använda med Azure-funktioner som kräver detta.</span><span class="sxs-lookup"><span data-stu-id="22412-106">You can follow the instructions below to create a new work or school account because fortunately, one of the best things about your personal Azure account is that it comes with a default Azure Active Directory domain that you can use to create a new work or school account that you can use with Azure features that require it.</span></span>

<span data-ttu-id="22412-107">Dock ändringar gör det möjligt att hantera din prenumeration med någon typ av Azure-konto med hjälp av den `azure login` interaktiv inloggning metod som beskrivs [här](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="22412-107">However, recent changes make it possible to manage your subscription with any type of Azure account using the `azure login` interactive login method described [here](../../xplat-cli-connect.md).</span></span> <span data-ttu-id="22412-108">Du kan använda denna funktion eller du kan följa instruktionerna som följer.</span><span class="sxs-lookup"><span data-stu-id="22412-108">You can either use that mechanism, or you can follow the instructions that follow.</span></span> <span data-ttu-id="22412-109">Du kan också [skapar en arbets- eller skolkonto identitet i Azure Active Directory för användning med virtuella Linux-datorer](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="22412-109">You can also [create a work or school identity in Azure Active Directory to use with Linux VMs](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

