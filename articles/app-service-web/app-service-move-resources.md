---
title: Flytta Web App resurser till en annan resursgrupp
description: "Beskriver scenarier där du kan flytta webbprogram och Apptjänster från en resursgrupp till en annan."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 1b5059dc052005b6079f70ecf6771a3771df8d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="supported-move-configurations"></a><span data-ttu-id="190f4-103">Konfigurationer som stöds flytta</span><span class="sxs-lookup"><span data-stu-id="190f4-103">Supported Move Configurations</span></span>
<span data-ttu-id="190f4-104">Du kan flytta Azure Web App resurser med hjälp av den [Resource Manager flytta resurser API](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="190f4-104">You can move Azure Web App resources using the [Resource Manager Move Resources API](../azure-resource-manager/resource-group-move-resources.md).</span></span>

<span data-ttu-id="190f4-105">Azure Web Apps stöder för närvarande följande move-scenarier:</span><span class="sxs-lookup"><span data-stu-id="190f4-105">Azure Web Apps currently supports the following move scenarios:</span></span>

* <span data-ttu-id="190f4-106">Flytta hela innehållet i en resursgrupp (webbappar, apptjänstplaner och certifikat) till en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="190f4-106">Move the entire contents of a resource group (web apps, app service plans, and certificates) to another resource group.</span></span> 
   > [!Note]
   > <span data-ttu-id="190f4-107">Mål resursgruppens namn får inte innehålla några Microsoft.Web resurser i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="190f4-107">The destination resource group can not contain any Microsoft.Web resources in this scenario.</span></span>

* <span data-ttu-id="190f4-108">Flytta enskilda webbprogram till en annan resursgrupp fortfarande värd dem i deras aktuella apptjänstplan (apptjänstplan förblir i den gamla resursgruppen).</span><span class="sxs-lookup"><span data-stu-id="190f4-108">Move individual web apps to a different resource group, while still hosting them in their current app service plan (the app service plan stays in the old resource group).</span></span>


