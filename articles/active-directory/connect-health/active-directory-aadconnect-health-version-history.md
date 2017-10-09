---
title: aaaAzure AD Connect Health versionshistorik
description: "Det här dokumentet beskriver hello versioner för Azure AD Connect Health och vad som har inkluderats i de versionerna."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: a583263e412f5da9af75947f3431de2494042388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-health-version-release-history"></a>Versionshistorik för Azure AD Connect Health
hello Azure Active Directory-teamet uppdaterar regelbundet Azure AD Connect Health med nya funktioner. Den här artikeln visar hello versioner och funktioner som har släppts.

## <a name="october-2016"></a>Oktober 2016
**Agentuppdatering:**

* Azure AD Connect Health agent för AD FS \(version 2.6.408.0\)
  1. Förbättringar för identifiering av klienternas IP-adresser i autentiseringsbegäranden
  2. Felkorrigeringar relaterade tooAlerts
* Azure AD Connect Health agent för AD DS (version 2.6.408.0)
  1. Felkorrigeringar relaterade tooAlerts.
* Azure AD Connect Health agent för synkronisering (version 2.6.353.0) lanseras med Azure AD Connect version 1.1.281.0
  1. Ange hello krävs data för hello synkronisering felrapporter
  2. Felkorrigeringar relaterade tooAlerts

**Nya förhandsgranskningsfunktioner:**

* Synkronisering felrapporter för Azure AD Connect

**Nya funktioner:**

* Azure AD Connect Health för AD FS - fältet för IP-adress är tillgänglig i hello rapport över de 50 användarna med felaktigt användarnamn/lösenord.

## <a name="july-2016"></a>Juli 2016
**Nya förhandsgranskningsfunktioner:**

* [Azure AD Connect Health för AD DS](active-directory-aadconnect-health-adds.md).

## <a name="january-2016"></a>Januari 2016
**Agentuppdatering:**

* Azure AD Connect Health agent för AD FS (version 2.6.91.1512)

**Nya funktioner:**

* [Testa anslutningsverktyget för Azure AD Connect Health-agenter](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>November 2015
**Nya funktioner:**

* Stöd för [rollbaserad åtkomstkontroll](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)

**Nya förhandsgranskningsfunktioner:**

* [Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md).

**Fast problem:**

* Felkorrigeringar för fel som visas vid registreringar för agenten.

## <a name="september-2015"></a>September 2015
**Nya funktioner:**

* Felaktigt användarnamn lösenord rapport för AD FS
* Stöd för tooconfigure Unauthenticated HTTP-Proxy
* Stöd tooconfigure agenten på Server core
* Förbättringar av tooAlerts för AD FS
* Förbättringar i Azure AD Connect Health Agent för AD FS för anslutning och data överför.

**Fast problem:**

* Felkorrigeringar i Användningsinsikter för AD FS feltyper.

## <a name="june-2015"></a>Juni 2015
**Första versionen av Azure AD Connect Health för AD FS och AD FS-Proxy.**

**Nya funktioner:**

* Aviseringar för övervakning av AD FS och AD FS-Proxy-servrar med e-postmeddelanden.
* Enkel åtkomst tooAD FS topologi och mönster i AD FS-prestandaräknare.
* Utvecklingen av lyckade token-förfrågningar på AD FS-servrarna som är grupperade efter program, autentiseringsmetoder, begär etc för platsen.
* Trender för misslyckade begäranden på AD FS-servrarna som är grupperade efter program, fel typer osv.
* Enklare Agentdistributionen med autentiseringsuppgifter som Global administratör för Azure AD.  

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [övervaka din lokala identitet infrastruktur och synkroniseringstjänster i molnet hello](active-directory-aadconnect-health.md).

