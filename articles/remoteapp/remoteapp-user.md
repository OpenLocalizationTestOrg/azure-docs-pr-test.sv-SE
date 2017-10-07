---
title: "aaaAdd användaren-tooyour Azure RemoteApp-samlingen | Microsoft Docs"
description: "Lär dig hur tooadd användare tooyour Azure RemoteApp-samling"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 6b751fd2-2b11-499f-a2eb-12cb47f3129b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 0ae88e04c8bfc2ed55dc963945ed7e9ff687b603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-user-tooyour-azure-remoteapp-collection"></a>Hur tooadd användaren-tooyour Azure RemoteApp-samling
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Innan användarna kan se och använda dina appar i Azure RemoteApp, ha toogrant dem åtkomst till tooyour samling. Detta är en del enkelt hello: på hello **användaråtkomst** fliken ange hello kontoinformation för hello användaren och klicka sedan på kryssmarkeringen hello.

Information om vad behöver du? Det beror på hello typen av samling du skapade (moln eller hybrid) och om du använder Office 365 ProPlus i samlingen.

## <a name="supported-user-identities"></a>Användaridentiteter som stöds
hello olika samlingstyper (moln eller hybrid) stöd för användning av olika användaridentiteter för åtkomst tooapplications.  

För en hybridsamling av RemoteApp-du behöver tooset upp en Active Directory-domän infrastruktur på lokal och en Azure Active Directory-klient med katalogintegrering (och enkel alternativt inloggning). Dessutom måste toocreate vissa Active Directory-objekt i hello lokala katalog.  

För en molnsamling i RemoteApp-kan användare som har stöd för identiteter för Azure Active Directory beviljas användaren åtkomst tooRemoteApp tooinclude Microsoft Accounts.  Se hello tabellen nedan.

Office 365-användare är Azure Active Directory-användare. Om de har Azure Active Directory hybrid kan Directory synkroniseras konton, de beviljas användaråtkomst i en distribution med RemoteApp.   

Du kan använda den här tabellen som en snabbreferens som identitet stöds i din samling och hello Active Directory-krav är.

| Användarkonton | Molnet | Hybrid |
| --- | --- | --- |
| Microsoft-konto |Ja |Nej |
| Azure Active Directory (AD Azure) | | |
| Azure AD cloud endast |Ja |Nej |
| ADsync med Lösenordssynkronisering |Ja |Ja |
| ADsync utan Lösenordssynkronisering |Ja |Nej |
| ADsync med AD FS |Ja |Ja |
| [3-part Azure stöds identitetsleverantörer](https://msdn.microsoft.com/library/azure/jj679342.aspx) (exempel Ping) |Ja |Ja |
| Multi-Factor Authentication |Ja |Ja |

Checka ut [mer](remoteapp-ad.md) om att konfigurera Active Directory för RemoteApp.

> [!NOTE]
> hello Azure Active Directory-användare måste komma från hello-klient som är associerad med din prenumeration. (Du kan visa och ändra din prenumeration på hello **inställningar** fliken hello-portalen. Se [ändra hello Azure Active Directory-klient som används av RemoteApp](remoteapp-changetenant.md) för mer information.)
> 
> 

## <a name="office-365-proplus-user-account-information"></a>Office 365 ProPlus-kontoinformation
Om du använder Office 365 ProPlus hello-mallavbildningen i samlingen *eller* om du har skapat en anpassad avbildning som använder Office 365 är bara tillåtna tooadd Azure Active Directory-användare som har Office 365-prenumerationer för hello standarddomän för din prenumeration. Se [med hjälp av Office 365 med Azure RemoteApp](remoteapp-o365.md) för mer information.

