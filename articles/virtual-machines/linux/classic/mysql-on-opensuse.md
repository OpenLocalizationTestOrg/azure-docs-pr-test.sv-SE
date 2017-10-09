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
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Installera MySQL på en virtuell dator som kör OpenSUSE Linux i Azure
[MySQL] [ MySQL] är en populär, öppen källkod SQL-databas. De här självstudierna visar hur toocreate en virtuell dator som kör OpenSUSE Linux, installera MySQL.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Skapa en virtuell dator som kör OpenSUSE Linux
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-hello-virtual-machine"></a>Installera och köra MySQL på hello virtuell dator
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Nästa steg
Mer information om MySQL finns hello [MySQL dokumentationen][MySQLDocs].

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

