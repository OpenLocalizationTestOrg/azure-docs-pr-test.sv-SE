---
title: Skydda appar och resurser i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur du spärra appar och resurser i Azure RemoteApp"
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
ms.openlocfilehash: 1c052906788f0f4fe4ca9fd6d3af63336245174a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Skydda appar och resurser i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Azure RemoteApp ger användare åtkomst till centralt hanterade Windows-appar som kan du styra vad användarna kan och inte kan göra.  Detta är särskilt användbart när användaren ansluter från en ohanterad enhet (till exempel sina personliga Macbook) och du vill styra användaråtkomst eller upplevelse.

Om du använder Active Directory för autentisering av användare och du vill att användarna inte att kopiera data från en app, kan du till exempel konfigurera en Remote Desktop-Grupprincip att blockera användare från att kopiera data.

Ett annat exempel är om du vill blockera internet-åtkomst för en viss app i samlingen. Du kan skapa en regel för Windows-brandväggen som blockerar åtkomst när du skapar avbildningen för samlingen.

## <a name="implementation-options"></a>Implementeringsalternativ för
  Här är implementeringsalternativen viktiga som kan användas enskilt eller tillsammans efter behov:

1. Om RemoteApp-samlingen är ansluten till en domän kan du använda någon [Grupprincip](https://technet.microsoft.com/library/cc725828.aspx) (med undantag för inaktivt och koppla från timeout principerna beskrivs [här](../azure-subscription-service-limits.md)).
2. Som ett alternativ till Grupprincip (om samlingen är inte ansluten till en domän eller om du inte har rätt privilegier i AD) kan du konfigurera [lokala principer](https://technet.microsoft.com/library/cc775702.aspx) i mallavbildningen.  Observera att gruppen principer trumf lokala principer när det finns en konflikt.
3. Vissa inställningar för OS-appen kan inte konfigureras via Grupprincip, men kan vara via registret med den [RegEdit verktyget](remoteapp-hybridtrouble.md) vid konfiguration av mallavbildningen.
4. Du kan använda [Windows-brandväggen](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) nätverket att styra åtkomsten till och från den dator där programmet körs. Kontrollera att du inte blockerar URL: er och portar som anges här.
5. Du kan använda [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) att styra vilka program och filer användare kan köra. Smarta användare kan till exempel ta reda på hur att köra program att du inte har publicerats men som är tillgängliga i bilden som du använde för att skapa samlingen - AppLocker kan blockera detta.

## <a name="detailed-information"></a>Detaljerad information
* Följande RDS-principer är troligt att det mest användbara:
  * [Enhet och resurs](https://technet.microsoft.com/library/ee791794.aspx)
  * [Omdirigering av skrivare](https://technet.microsoft.com/library/ee791784.aspx)
  * [Profiler](https://technet.microsoft.com/library/ee791865.aspx).
* Observera att konfigurera omdirigeringar via RemoteApp PowerShell-modul (som visas [här](remoteapp-redirection.md)) förlitar sig på klientdatorn att genomdriva principer, så om säkerhet är det primära målet ska du tillämpa principen via mallavbildningen lokal policy eller via en Grupprincip.
* [Principer för Windows Server 2012 R2](https://technet.microsoft.com/library/hh831791.aspx).
* [Office 2013 principerna](https://technet.microsoft.com/library/cc178969.aspx) (inklusive [hur du anpassar Office-verktygsfältet](https://technet.microsoft.com/library/cc179143.aspx)).

