---
title: aaaCreate en Azure RemoteApp-avbildning | Microsoft Docs
description: "Lär dig mer om hello alternativen för att skapa avbildningar för Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: cb0f9424-6185-45a1-abe9-c23f1edf34f2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 54e63b6fa13addfcda96ce581910e1ac48d91e70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-remoteapp-image"></a>Skapa en Azure RemoteApp-avbildning
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp använder avbildningar toohold hello appar som du delar med dina användare. (Vi ta avbildningen och använda den toocreate VMs - som har vad hello användare komma åt när de loggar in på Azure RemoteApp.) toocreate en Azure RemoteApp-samling med ditt val av program, om det är molnet eller hybrid, du genom att skapa en bild med de program som är installerade. Skapa sedan en samling som använder avbildningen, och tilldela användare toohello samling och publicera appar toothose användare.

Du har flera alternativ för att skapa eller använda avbildningar. grundläggande hello [krav](remoteapp-imagereqs.md) för en bild är att den kör Windows Server 2012 R2 och ha hello Remote värd för fjärrskrivbordssession (RDSH)-rollen installerad. Hur du får som är där det blir intressant.

Har du följande alternativ när det gäller tooimages hello:

* Du kan importera och använda en [bilden baserat på en virtuell dator i Azure](remoteapp-image-on-azurevm.md). Det här är bra för av branschspecifika appar som kräver anpassade inställningar. Du kan anpassa hello avbildningen toowork för hello app.
* Du kan [skapa och ladda upp en anpassad avbildning](remoteapp-create-custom-image.md). Det här är bra om du redan har en avbildning som du använder för din lokala Remote Desktop Services-distribution.
* Du kan använda någon av hello [mallavbildningarna](remoteapp-images.md) ingår i RemoteApp-prenumeration. Dessa avbildningar skapas och underhålls av hello RemoteApp-teamet och innehåller vissa standard program (till exempel hello Office-paket) som du kan göra tillgängliga tooyour användare. Observera att endast hello Office 365 Pro Plus avbildningen kan användas i en Produktionsinställning.

Oavsett var du får en avbildning eller hur du skapar, kommer du att du förstår hello toomake [appkrav](remoteapp-appreqs.md) tooensure som din app fungerar bra i RemoteApp. Hello nästa steg är toocreate en [moln](remoteapp-create-cloud-deployment.md) eller [hybrid](remoteapp-create-hybrid-deployment.md) samling.

