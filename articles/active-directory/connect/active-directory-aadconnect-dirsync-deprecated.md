---
title: "aaaUpgrade från DirSync och Azure AD Sync | Microsoft Docs"
description: "Beskriver hur tooupgrade från DirSync och Azure AD Sync tooAzure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d465b137fb54f0c6e28ccf73429c4bd3aafd8698
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Uppgradera Windows Azure Active Directory-synkronisering och Azure Active Directory Sync
Azure AD Connect är hello bästa sätt tooconnect din lokala katalog med Azure AD och Office 365. Det här är en bra tidpunkt tooupgrade tooAzure AD Connect från Windows Azure Active Directory Sync (DirSync) eller Azure AD Sync eftersom dessa verktyg är nu föråldrade och slutet av stödet för 13 April 2017.

hello två identitet synkroniseringsverktyg som är föråldrade erbjöds för enkel skog kunder (DirSync) och för flera skogar och andra avancerade kunder (Azure AD Sync). Dessa äldre verktyg har ersatts av en enda lösning som är tillgänglig för alla scenarier: Azure AD Connect. Det finns nya funktioner, funktionsförbättringar och stöd för nya scenarier. toobe kan toocontinue toosynchronize din lokala identitet data tooAzure AD och Office 365, rekommenderar vi starkt att du uppgraderar tooAzure AD Connect.

hello senaste versionen av DirSync gavs ut i juli 2014 och hello senaste versionen av Azure AD Sync gavs ut i maj 2015.

## <a name="what-is-azure-ad-connect"></a>Vad är Azure AD Connect?
Azure AD Connect är hello efterföljande tooDirSync och Azure AD Sync. Alla scenarier kombinerar dessa två stöds. Du kan läsa mer om den i [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Utfasningen schema
| Date | Kommentar |
| --- | --- |
| Den 13 april 2016 |Windows Azure Active Directory Sync (”DirSync”) och Microsoft Azure Active Directory Sync (”Azure AD Sync”) tillkännages som föråldrade. |
| 13 april 2017 |Stöd för parterna. Kunder kommer inte längre att kunna tooopen ett supportärende utan att uppgradera tooAzure AD Anslut först. |
|Den 31 december 2017|Azure AD kommer inte längre att acceptera kommunikation från Windows Azure Active Directory Sync (”DirSync”) och Microsoft Azure Active Directory Sync (”Azure AD Sync”).

## <a name="how-tootransition-tooazure-ad-connect"></a>Hur tootransition tooAzure AD Connect
Om du kör DirSync, det finns två sätt som du kan uppgradera: uppgradera, parallell distribution på plats. Uppgradering på plats rekommenderas för de flesta kunder och om du har ett senare operativsystem och mindre än 50 000 objekt. I annat fall bör toodo en parallell distribution där DirSync-konfigurationen är flyttas tooa nya server som kör Azure AD Connect.

Om du använder Azure AD Sync rekommenderar en uppgradering på plats. Om du vill, det är möjligt tooinstall en ny Azure AD Connect-server parallellt och gör en öppning migrering från din Azure AD Sync server tooAzure AD Connect.

| Lösning | Scenario |
| --- | --- |
| [Uppgradera från DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Om du har en befintlig DirSync-server som redan körs.</li> |
| [Uppgradera från Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md) |<li>Om du flyttar från Azure AD Sync.</li> |

Om du vill toosee hur toodo ett lokalt uppgradera från DirSync tooAzure AD Connect och se den här videon på Channel 9:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
**F: Jag har fått ett e-postmeddelande från hello Azure-teamet och/eller ett meddelande från hello meddelandecenter för Office 365, men jag använder Connect.**  
hello-meddelande skickades också toocustomers med Azure AD Connect med ett versionsnummer 1.0. \*.0 (med en pre-1.1-version). Microsoft rekommenderar kunder toostay aktuella med Azure AD Connect-versioner. Hej [automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) funktion som introducerades i 1.1 gör det enkelt tooalways har den senaste versionen av Azure AD Connect installeras.

**F: kommer DirSync-/ Azure AD Sync sluta fungera på den 13 April 2017?**  
DirSync-/ Azure AD Sync fortsätter toowork April 13 2017.  Azure AD kommer dock inte längre accepterar kommunikation från DirSync-/ Azure AD Sync på December 31 2017.

**F: vilka versioner av DirSync kan uppgradera från?**  
Det är stöds tooupgrade från någon DirSync-version som för närvarande används.

**F: Vad händer om hello Azure AD Connector för FIM/MIM?**  
hello Azure AD Connector för FIM/MIM har **inte** har meddelats som föråldrade. Det är på **funktionen låsa**; inga nya funktioner har lagts till och tas emot utan felkorrigeringar. Microsoft rekommenderar att kunder som använder den tooplan toomove från den tooAzure AD Connect. Det är starkt rekommenderat toonot starta alla nya distributioner som använder den. Den här anslutningen kommer att utannonseras inte längre i hello framtida.

## <a name="additional-resources"></a>Ytterligare resurser
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
