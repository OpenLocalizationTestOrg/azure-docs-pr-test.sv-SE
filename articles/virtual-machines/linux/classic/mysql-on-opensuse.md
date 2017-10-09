---
title: "aaaInstall MySQL på en OpenSUSE VM | Microsoft Docs"
description: "Lär dig tooinstall MySQL på en OpenSUSE VMirtual för Linux-dator i Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
ms.openlocfilehash: 8f96d29f29cb9c466dd7fdaf92b378783fdaacd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a><span data-ttu-id="d06c8-103">Installera MySQL på en virtuell dator som kör OpenSUSE Linux i Azure</span><span class="sxs-lookup"><span data-stu-id="d06c8-103">Install MySQL on a virtual machine running OpenSUSE Linux in Azure</span></span>
<span data-ttu-id="d06c8-104">[MySQL] [ MySQL] är en populär, öppen källkod SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d06c8-104">[MySQL][MySQL] is a popular, open-source SQL database.</span></span> <span data-ttu-id="d06c8-105">De här självstudierna visar hur toocreate en virtuell dator som kör OpenSUSE Linux, installera MySQL.</span><span class="sxs-lookup"><span data-stu-id="d06c8-105">This tutorial shows you how toocreate a virtual machine running OpenSUSE Linux, then install MySQL.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="d06c8-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d06c8-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d06c8-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d06c8-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="d06c8-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d06c8-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a><span data-ttu-id="d06c8-109">Skapa en virtuell dator som kör OpenSUSE Linux</span><span class="sxs-lookup"><span data-stu-id="d06c8-109">Create a virtual machine running OpenSUSE Linux</span></span>
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-hello-virtual-machine"></a><span data-ttu-id="d06c8-110">Installera och köra MySQL på hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d06c8-110">Install and run MySQL on hello virtual machine</span></span>
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="d06c8-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d06c8-111">Next steps</span></span>
<span data-ttu-id="d06c8-112">Mer information om MySQL finns hello [MySQL dokumentationen][MySQLDocs].</span><span class="sxs-lookup"><span data-stu-id="d06c8-112">For details about MySQL, see hello [MySQL Documentation][MySQLDocs].</span></span>

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

