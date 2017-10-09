---
title: aaaAzure RemoteApp-avbildning krav | Microsoft Docs
description: "Lär dig mer om hello kraven för att skapa avbildningar toobe används med Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7cbb90f4-6dc9-462c-a429-088cdb57414e
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 4e35203eb93a866d4e0bd591d42b34746c7ffa4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a>Krav för Azure RemoteApp-bilder
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp använder en Windows Server 2012 R2-bilden toohost alla hello program som du vill tooshare med dina användare. toocreate en anpassad avbildning som du kan börja med en befintlig avbildning eller [skapa en ny](remoteapp-create-custom-image.md).

> [!TIP]
> Visste du att din Azure RemoteApp-prenumeration du får åtkomst tooa Windows Server 2012 R2-avbildningen i hello Virtuella Azure-galleriet som du kan använda toocreate mallavbildningen? [Checka ut](remoteapp-image-on-azurevm.md).  
> 
> 

hello kraven för hello-avbildning som kan överföras för användning med Azure RemoteApp är:

* Anpassade program lagra inte data lokalt på hello avbildningen. Dessa avbildningar är tillståndslösa och får endast innehålla program.
* hello bilden innehåller inte data som kan gå förlorade.
* hello avbildningens storlek bör vara en multipel av MB. Om du försöker tooupload en avbildning som inte är en exakt multipel misslyckas hello överföringen.
* hello bildstorleken måste vara 127 GB eller mindre.
* Det måste finnas i en VHD-fil (VHDX-filer för närvarande stöds inte).
* hello VHD får inte vara en virtuell dator i generation 2.
* hello VHD kan vara fast storlek eller dynamiskt expanderande. En dynamiskt expanderande virtuell Hårddisk rekommenderas eftersom det tar mindre tid tooupload tooAzure än en VHD-fil med fast storlek.
* hello disken måste initieras med hello Master Boot Record (MBR) partitionering format. hello partitionstyp för GUID partition table (GPT) stöds inte.
* hello VHD måste innehålla en enkel installation av Windows Server 2012 R2. Den kan innehålla flera volymer, men bara en som innehåller en installation av Windows.
* hello Remote värd för fjärrskrivbordssession (RDSH)-rollen och hello Skrivbordsmiljö måste vara installerad.
* hello Anslutningsutjämning för fjärrskrivbord roll måste *inte* installeras.
* hello Krypterande filsystem (EFS) måste vara inaktiverad.
* hello bilden måste vara Sysprep med hello parametrar **/oobe / generalize/shutdown** (Använd inte hello **/mode:vm** parametern).
* Överför den virtuella Hårddisken från en ögonblicksbild kedja stöds inte.

Se [skapa en Azure RemoteApp-avbildning](remoteapp-imageoptions.md) för mer information om hur du skapar bilder för Azure RemoteApp.

