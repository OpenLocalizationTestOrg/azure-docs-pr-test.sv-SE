---
title: "Uppgradera från DirSync och Azure AD Sync | Microsoft Docs"
description: "Beskriver hur du uppgraderar från DirSync och Azure AD Sync till Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
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
ms.openlocfilehash: 9e8faf365c0f47582b4abc3554e0bb6e1c3e7902
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2018
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Uppgradera Windows Azure Active Directory-synkronisering och Azure Active Directory Sync
Azure AD Connect är det bästa sättet att ansluta din lokala katalog till Azure AD och Office 365. Det här är en bra tidpunkt för att uppgradera till Azure AD Connect från Windows Azure Active Directory Sync (DirSync) eller Azure AD Sync eftersom dessa verktyg nu är föråldrade och stöds inte längre från och med 13 April 2017.

Synkroniseringsverktyg för två identitet är föråldrade har erbjuds för en enkel skog kunder (DirSync) och för flera skogar och andra avancerade kunder (Azure AD Sync). Dessa äldre verktyg har ersatts av en enda lösning som är tillgänglig för alla scenarier: Azure AD Connect. Det finns nya funktioner, funktionsförbättringar och stöd för nya scenarier. För att kunna fortsätta att synkronisera dina lokala identitetsdata till Azure AD och Office 365, rekommenderar vi starkt att du uppgraderar till Azure AD Connect. Microsoft garanterar inte dessa äldre versioner att fungera efter den 31 December 2017.

Den senaste versionen av DirSync gavs ut i juli 2014 och den senaste versionen av Azure AD Sync gavs ut i maj 2015.

## <a name="what-is-azure-ad-connect"></a>Vad är Azure AD Connect?
Azure AD Connect är efterföljaren till DirSync och Azure AD Sync. Alla scenarier kombinerar dessa två stöds. Du kan läsa mer om den i [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Utfasningen schema
| Date | Kommentera |
| --- | --- |
| Den 13 april 2016 |Windows Azure Active Directory Sync (”DirSync”) och Microsoft Azure Active Directory Sync (”Azure AD Sync”) tillkännages som föråldrade. |
| 13 april 2017 |Stöd för parterna. Kunder kommer inte längre att kunna öppna ett supportärende utan att först uppgradera till Azure AD Connect. |
|Den 31 december 2017|Azure AD kan inte längre att godta kommunikation från Windows Azure Active Directory Sync (”DirSync”) och Microsoft Azure Active Directory Sync (”Azure AD Sync”).

## <a name="how-to-transition-to-azure-ad-connect"></a>Hur du övergår till Azure AD Connect
Om du kör DirSync, det finns två sätt som du kan uppgradera: uppgradera, parallell distribution på plats. Uppgradering på plats rekommenderas för de flesta kunder och om du har ett senare operativsystem och mindre än 50 000 objekt. I annat fall rekommenderas att göra en parallell distribution där DirSync-konfigurationen har flyttats till en ny server som kör Azure AD Connect.

| Lösning | Scenario |
| --- | --- |
| [Uppgradera från DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Om du har en befintlig DirSync-server som redan körs.</li> |
| [Uppgradera från Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md) |<li>Om du flyttar från Azure AD Sync.</li> |

Om du vill se hur du gör en uppgradering på plats från DirSync till Azure AD Connect läser du den här videon på Channel 9:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
**F: Jag har fått ett e-postmeddelande från Azure-teamet och/eller ett meddelande från meddelandecenter för Office 365, men jag använder Connect.**  
Meddelandet skickades också för kunder med Azure AD Connect med ett versionsnummer 1.0. \*.0 (med en pre-1.1-version). Microsoft rekommenderar kunder att hålla dig uppdaterad med Azure AD Connect-versioner. Den [automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) funktion som introducerades i 1.1 gör det enkelt att alltid har den senaste versionen av Azure AD Connect installeras.

**F: kommer DirSync-/ Azure AD Sync sluta fungera på den 13 April 2017?**  
DirSync-/ Azure AD Sync fortsätter att fungera på den 13 April 2017.  Azure AD kan dock inte längre accepterar kommunikation från DirSync-/ Azure AD Sync efter den 31 December 2017.

**F: vilka versioner av DirSync kan uppgradera från?**  
Det går för att uppgradera från någon DirSync-version som för närvarande används. 

**F: Vad händer om Azure AD Connector för FIM/MIM?**  
Azure AD Connector för FIM/MIM har **inte** har meddelats som föråldrade. Det är på **funktionen låsa**; inga nya funktioner har lagts till och tas emot utan felkorrigeringar. Microsoft rekommenderar att kunder som använder du planerar att flytta från den till Azure AD Connect. Det rekommenderas att inte starta alla nya distributioner som använder den. Den här anslutningen kommer att utannonseras föråldrad i framtiden.

## <a name="additional-resources"></a>Ytterligare resurser
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
