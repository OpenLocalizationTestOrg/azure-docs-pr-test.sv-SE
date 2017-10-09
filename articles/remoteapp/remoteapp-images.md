---
title: aaaWhat finns i hello Azure RemoteApp-mallavbildningarna? | Microsoft Docs
description: "Läs mer om hello mallavbildningarna som ingår i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ea012cec8dc581a8bd4a5a138ce302de19d5c6af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-in-hello-azure-remoteapp-template-images"></a>Vad är hello Azure RemoteApp-mallavbildningarna?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Din Azure RemoteApp-prenumeration innehåller tre mallavbildningar:

* Windows Server 2012
* Microsoft Office 365 ProPlus (Office 365-prenumeration krävs)
* Microsoft Office 2013 Professional Plus (endast utvärderingsversion)

> [!IMPORTANT]
> Azure RemoteApp-prenumeration ger dig åtkomst toohello programvara i hello bilder med hello undantag av Office 365 ProPlus, som kräver en separat prenumeration och Office 2013, som inte kan användas i produktion. Det innebär att du kan dela hello programmen eller apparna i hello mallavbildningarna med dina användare. Om du skapar en samling som använder hello Windows Server 2012 R2-avbildningen kan publicera du System Center Endpoint Protection för användare tooaccess via RemoteApp.
> 
> Kolla in hello [RemoteApp-licensieringsuppgifterna](remoteapp-licensing.md) för mer information. Och [använda Office med Azure RemoteApp](remoteapp-o365.md) för hello Office-licensiering information.
> 
> 

I nedanstående avsnitt finns information om vad varje avbildning innehåller.

## <a name="windows-server-2012-r2--hello-vanilla-image"></a>Windows Server 2012 R2 (”Hej vanliga avbildningen)
Den här avbildningen baseras på operativsystemet Microsoft Windows Server 2012 R2 Datacenter och hello följande roller och funktioner installerade toomeet hello krav för Azure RemoteApp-mallavbildningar:

* .NET Framework 4.5, 3.5.1, 3.5
* Skrivbordsmiljö
* Handskriftstjänster
* Media Foundation
* Värd för fjärrskrivbordssession
* Windows PowerShell 4.0
* Windows PowerShell ISE
* WoW64-stöd

Den här avbildningen innehåller även hello följande program:

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (prenumeration krävs)
Office 365 är hello mest efterfrågade programmet så vi har skapat en ”anpassad” avbildning du toowork med.

Den här avbildningen är en utökning av hello vanliga avbildningen och har hello följande komponenter från Microsoft Office 365 ProPlus installerat dessutom toohello komponenter som beskrivs i hello Windows Server 2012 R2-avbildningen:

* Åtkomst
* Excel
* Lync
* OneNote
* OneDrive för företag (Observera att hello sync-agenten inte stöds för användning med Azure RemoteApp)
* Outlook
* PowerPoint
* Word
* Språkverktyg för Microsoft Office

hello avbildningen ingår även Visio Pro och Project Pro.

Och hello följande program:

* SQL Native Client
* ODBC-drivrutin
* SQL Server Data Mining-klient
* MasterDataServices-klient
* Microsoft Publisher
* PowerQuery
* PowerMap

Alla funktioner i Office 365 ProPlus-apparna är bara tillgängliga för användare som har en Office 365 ProPlus-plan. Mer information om hello Office 365-prenumeration planer finns [Office 365-tjänstplaner](http://technet.microsoft.com/library/office-365-plan-options.aspx). Har du fler frågor? Kolla in hello [Office 365 + RemoteApp](remoteapp-o365.md) information. Se även hello ny artikel, [hur toouse din Office 365-prenumeration med Azure RemoteApp](remoteapp-officesubscription.md).

Observera att du toolicense Office 365 ProPlus, Visio Pro och Project Pro separat – alla har en egen licens.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (endast utvärderingsversion)
Du kan testa hello service med hello Office 2013-avbildningen under hello utvärderingsperioden.

Den här avbildningen är en utökning av hello vanliga avbildningen och har hello följande komponenter från Microsoft Office 2013 Professional Plus installerat dessutom toohello komponenter som beskrivs i hello Windows Server 2012 R2-avbildningen:

* Åtkomst
* Excel
* Lync
* OneNote
* OneDrive för företag (Observera att hello sync-agenten inte stöds för användning med Azure RemoteApp)
* Outlook
* PowerPoint
* Project
* Visio
* Word
* Språkverktyg för Microsoft Office

> [!IMPORTANT]
> **Juridisk information:** Den här avbildningen innehåller inte en licens för Microsoft Office och *kan inte användas för produktion*. hello Office 2013 Professional Plus avbildningen är avsedd för Utvärderingsanvändning endast. Om du vill toouse Office-appar i Azure RemoteApp för produktion, måste toouse hello Office 365 ProPlus-avbildningen. Mer info om Office-licensiering finns i artikeln om att [använda Office 365 med Azure RemoteApp](remoteapp-o365.md).
> 
> 

