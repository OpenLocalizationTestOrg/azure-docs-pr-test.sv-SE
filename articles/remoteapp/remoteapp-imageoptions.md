---
title: Skapa en Azure RemoteApp-avbildning | Microsoft Docs
description: "Lär dig mer om alternativen för att skapa avbildningar för Azure RemoteApp"
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
ms.openlocfilehash: 4b8ba6f264f982e03930c5ad4ccdb2d80f2c8665
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-remoteapp-image"></a>Skapa en Azure RemoteApp-avbildning
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Azure RemoteApp använder avbildningar för att lagra de appar som du delar med dina användare. (Vi tar avbildningen och använda den för att skapa virtuella datorer – det är vad användarna åtkomst när de loggar in på Azure RemoteApp.) För att skapa en Azure RemoteApp-samling med ditt val av program, om det är molnet eller hybrid, börja med att skapa en bild med de program som är installerade. Skapa sedan en samling som använder avbildningen, tilldela användare till samlingen och publicera appar till dessa användare.

Du har flera alternativ för att skapa eller använda avbildningar. Grundläggande [krav](remoteapp-imagereqs.md) för en bild är att den kör Windows Server 2012 R2 och har installerat fjärråtkomst värd för fjärrskrivbordssession (RDSH)-roll. Hur du får som är där det blir intressant.

Du har följande alternativ när det gäller bilder:

* Du kan importera och använda en [bilden baserat på en virtuell dator i Azure](remoteapp-image-on-azurevm.md). Det här är bra för av branschspecifika appar som kräver anpassade inställningar. Du kan anpassa avbildningen ska fungera för appen.
* Du kan [skapa och ladda upp en anpassad avbildning](remoteapp-create-custom-image.md). Det här är bra om du redan har en avbildning som du använder för din lokala Remote Desktop Services-distribution.
* Du kan använda en av de [mallavbildningarna](remoteapp-images.md) ingår i RemoteApp-prenumeration. Dessa avbildningar skapas och underhålls av RemoteApp-teamet och innehåller vissa standard program (till exempel Office-paket) som du kan göra tillgängliga för användarna. Observera att endast Office 365 Pro Plus bilden kan användas i en Produktionsinställning.

Oavsett var du får en avbildning eller hur du skapar du vill kontrollera att du förstår de [appkrav](remoteapp-appreqs.md) så att din app fungerar bra i RemoteApp. Nästa steg är att skapa en [moln](remoteapp-create-cloud-deployment.md) eller [hybrid](remoteapp-create-hybrid-deployment.md) samling.

