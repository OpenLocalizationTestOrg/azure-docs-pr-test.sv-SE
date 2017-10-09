---
title: "molnsamlingar för aaaTroubleshoot RemoteApp - skapa | Microsoft Docs"
description: "Lär dig hur tootroubleshoot RemoteApp cloud fel vid skapande av samling"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: 95eb7797-7b5e-4781-ad23-f15dd85b213d
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9484ecbdb048ede8df725215b313e049cc7648f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Felsöka molnsamlingar för att skapa RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Om du har problem med att skapa en molnsamling kan du kolla hello följande information.

## <a name="your-image-is-invalid"></a>Bilden är ogiltig
Om du ser ett meddelande som ”GoldImageInvalid” när du väntar Azure tooprovision din samling, innebär det att mallavbildningen inte uppfyller hello [definierat avbildningen krav](remoteapp-imagereqs.md). Därför gå läsa de [krav](remoteapp-imagereqs.md), åtgärda avbildningen och försök toocreate samlingen igen.

## <a name="common-errors-seen-in-hello-azure-management-portal"></a>Vanliga fel som visas i hello Azure Management portal
    DNS server could not be reached
    ProvisioningTimeout

Molnet samlingar ofta misslyckas under skapande av på grund av du använder anpassade avbildningar.  Om du ser något av hello ovan fel och du använder en anpassad avbildning toocreate hello samling, kontrollera hello följande saker:

* Kontrollera att hello anpassad avbildning som du har överfört uppfyller kraven för avbildningen.
* Oftast hello vanligt problem är hello avbildningen inte var korrekt Sysprep.  
* Kontrollera hello avbildningen kan starta i Hyper-V eller försök att skapa ett IAAS VM direkt i din Azure-prenumeration med hjälp av hello avbildning. Om hello VM misslyckas tooboot och inte starta sedan indikerar detta vanligtvis den egna avbildningen hello inte förbereddes på rätt sätt.  Kontrollera hello anpassad avbildning har skapats följande hello hur toocreate en anpassad mall bild för RemoteApp

Om du använder en av hello Microsoft avbildningarna som ingår i din prenumeration, försök igen toocreate hello-samling. Hello problemet kvarstår sedan kontakta Microsoft support.

    PlatformImageTrialModeOnly

Om du ser felet detta innebär vanligen som du har uppgraderat tooa betalt konto, men du försöker toouse en avbildning av Microsoft tillhandahållna som är endast giltig vid hello utvärderingsversion läge hello-tjänsten. I det här fallet försök toocreate molnsamling igen, men vara säker på att toospecify hello rätt avbildning.

