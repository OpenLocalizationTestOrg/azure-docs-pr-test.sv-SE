---
title: aaaSecure appar och resurser i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toolock ned appar och resurser i Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7fbade87-a453-426d-bfa5-c72227ea83cd
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 26325826e92855a12a0087f19a3e32cbe1116449
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Skydda appar och resurser i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp ger användarna åtkomst toocentrally-hanterade Windows-appar, vilket gör att du kan styra vad användarna kan och inte kan göra.  Detta är särskilt användbart när hello användare ansluter från en ohanterad enhet (till exempel sina personliga Macbook) och du vill toocontrol hello användaråtkomst eller upplevelse.

Om du använder Active Directory för autentisering av användare och du vill att tooprevent användarna från att kopiera data från en app, kan du konfigurera en grupprincip för Remote Desktop tooblock användarna att kopiera data.

Ett annat exempel är om du vill tooblock internet-åtkomst för en viss app i samlingen. Du kan skapa en regel för Windows-brandväggen att block hello åtkomst när du skapar hello avbildning för samlingen.

## <a name="implementation-options"></a>Implementeringsalternativ för
  Här följer hello viktiga implementeringsalternativ som kan användas enskilt eller tillsammans efter behov:

1. Om RemoteApp-samlingen är ansluten till en domän kan du använda någon [Grupprincip](https://technet.microsoft.com/library/cc725828.aspx) (med undantag för hello av hello inaktivt och koppla från timeout principer beskrivs [här](../azure-subscription-service-limits.md)).
2. Som ett alternativt tooGroup principen (om samlingen är inte ansluten till en domän eller om du inte har rätt behörighet för hello i AD) kan du konfigurera [lokala principer](https://technet.microsoft.com/library/cc775702.aspx) i mallavbildningen.  Observera att gruppen principer trumf lokala principer när det finns en konflikt.
3. Vissa inställningar för OS-appen kan inte konfigureras via Grupprincip, men kan vara via registernyckeln hello [RegEdit verktyget](remoteapp-hybridtrouble.md) vid konfiguration av mallavbildningen.
4. Du kan använda [Windows-brandväggen](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) toocontrol network access tooand från hello datorn där hello appen körs. Kontrollera att du inte blockerar hello URL: er och portar som anges här.
5. Du kan använda [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) toocontrol vilka program och filer användare kan köra. Till exempel smarta användare kan ta reda på hur toorun program som du inte har publicerats men som är tillgängliga i hello avbildning som du använde toocreate hello samling - AppLocker kan blockera detta.

## <a name="detailed-information"></a>Detaljerad information
* hello följande RDS principer är sannolikt toobe som är mest användbara:
  * [Enhet och resurs](https://technet.microsoft.com/library/ee791794.aspx)
  * [Omdirigering av skrivare](https://technet.microsoft.com/library/ee791784.aspx)
  * [Profiler](https://technet.microsoft.com/library/ee791865.aspx).
* Observera att konfigurera omdirigeringar via hello RemoteApp PowerShell-modulen (som visas [här](remoteapp-redirection.md)) förlitar sig på hello datorn tooenforce hello klientprinciper, så om säkerheten är hello primära mål ska du tooenforce hello principen via Hej mallen bild lokala principen eller via en Grupprincip.
* [Principer för Windows Server 2012 R2](https://technet.microsoft.com/library/hh831791.aspx).
* [Office 2013 principerna](https://technet.microsoft.com/library/cc178969.aspx) (inklusive [hur toocustomize hello Office-verktygsfältet](https://technet.microsoft.com/library/cc179143.aspx)).

