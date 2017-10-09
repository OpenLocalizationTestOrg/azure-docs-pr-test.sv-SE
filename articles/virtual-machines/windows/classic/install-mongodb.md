---
title: "aaaInstall MongoDB på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur du skapar tooinstall MongoDB på en virtuell dator i Azure med hello klassiska distributionsmodellen med Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: aafd86b4c49c6a97ae8a1a2191ae375b6c47483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>Installera MongoDB på en Windows-dator i Azure
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).  Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. tooinstall och konfigurera MongoDB med hello Resource Manager-modellen, se [i den här artikeln](../install-mongodb.md).

[MongoDB] [ MongoDB] är en populär öppen källkod, högpresterande NoSQL-databas. Den här artikeln hjälper dig att skapa en Windows Server virtuell dator (VM) med hello [Azure-portalen][AzurePortal]. Du kan sedan skapa och koppla en data disk toohello VM innan du installerar och konfigurerar MongoDB. Om du har en befintlig virtuell dator i Azure som du vill att toouse kan du hoppa direkt för[installera och konfigurera MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Skapa en virtuell dator som kör Windows Server
Följ dessa instruktioner toocreate en virtuell dator.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> Du kan lägga till en slutpunkt för MongoDB när du skapar hello virtuell dator och konfigurera den på följande sätt: namnet som **Mongo**, använda **TCP** som hello-protokollet och ange båda hello offentliga och privata portar för**27017**.
>
>

## <a name="attach-a-data-disk"></a>Anslut en datadisk
tooprovide lagring för hello virtuell dator kan ansluta en datadisk och initiera den så att Windows kan använda den. Om du redan har en datadisk, kan du koppla den befintliga disken eller du kan koppla en tom disk.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Anvisningar för att initiera hello disk, finns i ”så här: initierar en ny datadisk i Windows Server” i [hur tooattach data disk tooa Windows-dator](attach-disk.md).

## <a name="install-and-run-mongodb-on-hello-virtual-machine"></a>Installera och köra MongoDB på hello virtuell dator
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Sammanfattning
I kursen får du har lärt dig hur toocreate en virtuell dator som kör Windows Server fjärransluta tooit, och ansluta en datadisk.  Du också lära sig hur tooinstall och konfigurera MongoDB på hello Windows-baserad virtuell dator. Du kan nu komma åt MongoDB på hello Windows-baserade virtuella datorn genom följande hello avancerade avsnitt i hello [MongoDB-dokumentation][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

