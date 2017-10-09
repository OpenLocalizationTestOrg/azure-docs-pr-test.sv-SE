---
title: "aaaEnterprise tillstånd centrala översikt | Microsoft Docs"
description: "Innehåller information om Enterprise tillstånd centrala inställningar i Windows-enheter. Enterprise tillstånd centrala ger användarna en enhetlig miljö på sina Windows-enheter och minskar hello tid som behövs för att konfigurera en ny enhet."
services: active-directory
keywords: "Vad är Enterprise tillstånd växling enterprise synkronisering molnet för windows"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 83b3b58f-94c1-4ab0-be05-20e01f5ae3f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 436076383b70b7cd65a612e3928f62f26ac5fa84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-state-roaming-overview"></a>Översikt över enterprise tillståndsväxling
Med Windows 10 [Azure Active Directory (AD Azure)](active-directory-whatis.md) användare få hello möjlighet toosecurely synkronisera sina användarinställningar och programmet inställningar data toohello moln. Enterprise tillstånd centrala ger användarna en enhetlig miljö på sina Windows-enheter och minskar hello tid som behövs för att konfigurera en ny enhet. Enterprise tillstånd centrala fungerar liknande toohello standard [synkronisera inställningar för konsumenten](http://windows.microsoft.com/en-US/windows-8/sync-settings-pcs) som introducerades i Windows 8. Dessutom erbjuder Enterprise tillstånd centrala:

* **Uppdelning av företagets och konsumentdata** – organisationer har kontroll över sina data och det finns ingen blandning av företagets data i ett moln konsumentkonto eller konsumentdata i ett moln enterprise-konto.
* **Förbättrad säkerhet** – Data krypteras automatiskt innan de lämnar hello användarens Windows 10-enhet med hjälp av Azure Rights Management (Azure RMS) och data förblir krypterade i vila i hello molnet. Allt innehåll förblir krypterade i vila i hello moln, förutom hello namnområden, till exempel namn på inställningar och Windows-app.  
* **Bättre hantering och övervakning av** – ger kontroll och synlighet över som synkroniseras inställningarna i din organisation och på vilka enheter via portalen hello Azure AD-integrering. 

Enterprise tillstånd växling är tillgängliga i Azure-regioner. Du kan hitta hello uppdatera listan över tillgängliga regioner på hello [Azure-tjänster efter regioner](https://azure.microsoft.com/regions/#services) sidan under Azure Active Directory.

| Artikel | Beskrivning |
| --- | --- |
| [Aktivera Företagsroaming i Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md) |Enterprise tillstånd växling är tillgängliga tooany organisation med en prenumeration för Premium Azure Active Directory (AD Azure). Mer information om hur tooget en Azure AD-prenumeration finns hello [Azure AD-produkten](https://azure.microsoft.com/services/active-directory) sidan. |
| [Inställningar och dataroaming vanliga frågor och svar](active-directory-windows-enterprise-state-roaming-faqs.md) |Det här avsnittet besvarar några frågor som IT-administratörer kan ha om inställningar och data appsynkronisering. |
| [Gruppen principer och MDM-inställningar för synkronisering av inställningar](active-directory-windows-enterprise-state-roaming-group-policy-settings.md) |Windows 10 ger en Grupprincip och mobila enheter management (MDM) princip inställningar toolimit inställningar för synkronisering. |
| [Centrala referens för Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md) |hello följande är en fullständig lista över alla hello-inställningar som kommer att flyttade och/eller säkerhetskopierade i Windows 10. |
| [Felsökning](active-directory-windows-enterprise-state-roaming-troubleshooting.md) |Det här avsnittet går igenom några enkla steg för felsökning och innehåller en lista över kända problem. |

