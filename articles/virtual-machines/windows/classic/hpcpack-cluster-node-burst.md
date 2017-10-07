---
title: aaaAdd burst noder tooan HPC Pack klustret | Microsoft Docs
description: "Lär dig hur tooexpand ett HPC Pack kluster i Azure på begäran genom att lägga till rollen worker-instanser körs i en tjänst i molnet"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 7ec40ffe76485742c9e458ec49e11805990974e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-on-demand-burst-nodes-tooan-hpc-pack-cluster-in-azure"></a>Lägga till på begäran ”burst” noder tooan HPC Pack klustret i Azure
Om du ställer in en [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) kluster i Azure måste du kanske vill sätt tooquickly skala hello klustret kapacitet upp eller ned, utan att behålla en uppsättning fördefinierade beräkningsnod virtuella datorer. Den här artikeln lär du dig hur tooadd på begäran ”burst” noder (worker-rollinstanser som körs i en tjänst i molnet) som resurser tooa head beräkningsnod i Azure. 

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

![Burst-noder][burst]

hello åtgärderna i den här artikeln kan du lägga till Azure-noder snabbt tooa molnbaserade HPC Pack huvudnod VM för en test- eller konceptbevis-distribution. hello anvisningar är hello samtidigt som hello steg för ”burst tooAzure” tooadd moln beräkningskluster kapacitet tooan lokalt HPC Pack. En självstudiekurs finns [ställa in ett hybrid beräkningskluster with Microsoft HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Detaljerad information och överväganden för Produktionsdistribution finns [Burst tooAzure with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Krav
* **HPC Pack huvudnod distribueras i en Azure VM** -du kan använda en fristående huvudnod VM eller ett som är en del av en större kluster. toocreate en fristående huvudnod finns [distribuera ett HPC Pack huvudnod i en Azure VM](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Automatisk HPC Pack klustret distributionsalternativ finns [alternativ toocreate och hantera Windows HPC-kluster i Azure with Microsoft HPC Pack](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Om du använder hello [HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md) toocreate hello kluster i Azure kan du ta Azure burst-noder i distributionen automatiskt. Se hello exemplen i den artikeln.
  > 
  > 
* **Azure-prenumeration** -tooadd Azure noder, kan du välja hello samma prenumeration som används toodeploy hello huvudnod virtuell dator eller en annan prenumeration (eller prenumerationer).
* **Kärnor kvoten** – du kan behöva tooincrease hello kvoten för kärnor, särskilt om du väljer toodeploy flera Azure-noder med flera kärnor. tooincrease en kvot [öppna en supportbegäran online customer](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) utan kostnad.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-hello-azure-nodes"></a>Steg 1: Skapa en molntjänst och ett lagringskonto för hello Azure-noder
Använd hello klassiska Azure-portalen eller likvärdiga verktyg tooconfigure hello efter resurser som är nödvändiga toodeploy Azure-noder:

* En ny Azure-molntjänst
* Ett nytt Azure storage-konto

> [!NOTE]
> Inte återanvända en befintlig molntjänst i din prenumeration. 
> 
> 

**Överväganden**

* Konfigurera en separat molntjänst för varje nod Azure-mall som du planerar toocreate. Du kan dock använda hello samma lagringskonto för flera noden mallar.
* Vi rekommenderar att du har hittat hello Molntjänsten och hello storage-konto för distribution av hello i hello samma Azure-region.

## <a name="step-2-configure-an-azure-management-certificate"></a>Steg 2: Konfigurera ett Azure-hanteringscertifikat
tooadd Azure-noder beräkningsresurser, du behöver ett certifikat på hello huvudnod och ladda upp en motsvarande certifikat toohello används för distribution av hello Azure-prenumeration.

I det här scenariot kan du välja hello **standard HPC Azure-Hanteringscertifikatet** som HPC Pack installeras och konfigureras automatiskt på huvudnoden. Det här certifikatet är användbart för testning syfte och proof of concept distributioner. toouse detta certifikat, ladda upp filen C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer från hello huvudnod VM toothe prenumeration. tooupload hello certifikat i hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **inställningar** > **Hanteringscertifikat**.

Ytterligare alternativ tooconfigure hello certifikat, se [scenarier tooConfigure hello Azure-Hanteringscertifikat för Azure Burst distributioner](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-toohello-cluster"></a>Steg 3: Distribuera Azure-noder toohello kluster
Hej steg tooadd och starta Azure noder i det här scenariot är vanligtvis hello samma som hello steg med en lokal huvudnod. Mer information finns i följande avsnitt i hello [steg tooDeploy Azure noder med Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Skapa en mall för Azure nod
* Lägg till Azure-noder toohello Windows HPC-kluster
* Starta (tillhandahålla) hello Azure-noder

När du lägger till och starta hello noder, är de klara toouse toorun kluster-jobb.

Om det uppstår problem när du distribuerar Azure-noder kan se [felsöka distributioner av Azure-noder med Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Nästa steg
* toouse hello beräkningsintensiva instansstorleken burst noder, se hello överväganden i [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Om du vill öka eller minska automatiskt hello Azure bearbetningsresurser enligt hello klusterarbetsbelastning, se [automatiskt växa eller krympa Azure-beräkningsresurser i ett kluster med HPC Pack](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
