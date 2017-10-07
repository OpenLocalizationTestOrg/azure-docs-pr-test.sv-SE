---
title: aaaMove App webbresurser tooanother resursgruppen.
description: "Beskriver hello scenarier där du kan flytta webbprogram och Apptjänster från en resursgrupp tooanother."
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
ms.openlocfilehash: 931012fee656b7f8a4b2c225c38739a9171d3b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="supported-move-configurations"></a><span data-ttu-id="e7d85-103">Konfigurationer som stöds flytta</span><span class="sxs-lookup"><span data-stu-id="e7d85-103">Supported Move Configurations</span></span>
<span data-ttu-id="e7d85-104">Du kan flytta Azure Web App resurser med hjälp av hello [Resource Manager flytta resurser API](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="e7d85-104">You can move Azure Web App resources using hello [Resource Manager Move Resources API](../azure-resource-manager/resource-group-move-resources.md).</span></span>

<span data-ttu-id="e7d85-105">Azure Web Apps stöder för närvarande hello efter flytten scenarier:</span><span class="sxs-lookup"><span data-stu-id="e7d85-105">Azure Web Apps currently supports hello following move scenarios:</span></span>

* <span data-ttu-id="e7d85-106">Flytta hello hela innehållet i en resursgrupp (webbappar, apptjänstplaner och certifikat) tooanother resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e7d85-106">Move hello entire contents of a resource group (web apps, app service plans, and certificates) tooanother resource group.</span></span> 
   > [!Note]
   > <span data-ttu-id="e7d85-107">hello mål resursgruppens namn får inte innehålla några Microsoft.Web resurser i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="e7d85-107">hello destination resource group can not contain any Microsoft.Web resources in this scenario.</span></span>

* <span data-ttu-id="e7d85-108">Flytta enskilda web apps tooa annan resursgrupp, medan du fortfarande värd dem i deras aktuella apptjänstplan (hello app service-plan förblir i hello gamla resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="e7d85-108">Move individual web apps tooa different resource group, while still hosting them in their current app service plan (hello app service plan stays in hello old resource group).</span></span>


