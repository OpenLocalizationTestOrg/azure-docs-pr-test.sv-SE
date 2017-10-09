---
title: "aaaCreate en arbets- eller skolkonto identitet i AAD för Windows | Microsoft Docs"
description: "Lär dig hur toocreate ett arbets- eller skolkonto identitet i Azure Active Directory toouse med Windows-datorer."
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
ms.openlocfilehash: dd6e2381fd0aa503483aa786b36232e557729c4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-toouse-with-windows-vms"></a>Skapa en identitet för arbetet eller skolan i Azure Active Directory toouse med virtuella Windows-datorer
Om du har skapat en personlig Azure-konto eller har en personlig MSDN-prenumeration och skapa hello Azure-konto tootake nytta av hello MSDN Azure-krediter--du använde en *Microsoft-konto* identitet toocreate den. Många av de viktigaste funktionerna i Azure-- [resurs mallar](../../azure-resource-manager/resource-group-overview.md) är ett exempel--kräver ett arbets- eller skolkonto konto (en identitet som hanteras av Azure Active Directory) toowork. Du kan följa hello instruktionerna nedan toocreate nya arbets- eller Skol-kontot eftersom Lyckligtvis något av bästa hello om ditt personliga Azure-konto är att det kommer en ny arbetet eller skolan med Azure Active Directory standarddomänen som du kan använda toocreate konto som du kan använda med Azure-funktioner som kräver detta.

Dock ändringar gör det möjligt toomanage din prenumeration med någon typ av Azure-konto med hjälp av hello `azure login` interaktiv inloggning metod som beskrivs [här](../../xplat-cli-connect.md). Du kan använda denna funktion eller du kan följa hello instruktionerna som följer. Du kan också [skapar en arbets- eller skolkonto identitet i Azure Active Directory toouse med Linux virtuella datorer](../linux/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]

