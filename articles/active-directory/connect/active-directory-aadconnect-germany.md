---
title: aaaAzure AD Connect i Microsoft Cloud Tyskland
description: "Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. Detta gör att du tooprovide en gemensam identitet för Office 365 och Azure SaaS-program som är integrerade med Azure AD."
keywords: "Introduktion tooAzure AD Connect, Azure AD Connect översikt över vad är Azure AD Connect, installera active directory, Tyskland svart skog"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: f32194fa6c365614f68e5d1ddcf0dac44d223292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect i Microsoft Cloud Tyskland –offentlig förhandsversion
## <a name="introduction"></a>Introduktion
Azure AD Connect tillhandahåller synkronisering mellan din lokala Active Directory och Azure Active Directory.
För närvarande många hello scenarier i [Microsoft Cloud Tyskland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) måste utföras av hello-operator. När du använder Microsoft Cloud Tyskland måste du vara medveten om följande hello:

* hello måste följande URL: er vara öppna på en proxyserver för synkronisering toooccur har:
  
  * *.microsoftonline.de
  * *.windows.net
  * * Listor över återkallade certifikat
* När du loggar in tooyour Azure AD-katalog måste du använda ett konto i hello onmicrosoft.de domän.
* hello följande funktioner är inte tillgängliga:
  * Azure AD Connect Health
  * Automatiska uppdateringar
 
## <a name="download"></a>Ladda ned
Du kan hämta Azure AD Connect från hello Azure AD Connect-bladet i hello-portalen.  Använd hello instruktionerna nedan toolocate hello Azure AD Connect-bladet.

### <a name="hello-azure-ad-connect-blade"></a>hello Azure AD Connect-bladet
När du har loggat in toohello Azure-portalen, hello följande:

1. Gå tooBrowse
2. Välj Azure Active Directory
3. Välj sedan Azure AD Connect

Du bör se hello följande:

![Azure AD Connect-bladet](media/active-directory-aadconnect-germany/germany1.png)

hello följande tabell beskrivs hello-funktioner som visas i hello-bladet.

| Rubrik | Beskrivning |
| --- | --- |
| SYNKRONISERINGSSTATUS |Visar om synkronisering är aktiverat eller inaktiverat. |
| SENASTE SYNKRONISERING |Hej senast en lyckad synkronisering har slutförts. |
| FEDERERADE DOMÄNER |Visar hello antal externa domäner som har konfigurerats. |

## <a name="installation"></a>Installation
tooinstall Azure AD Connect, kan du använda hello dokumentationen [här](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Avancerade funktioner och ytterligare information
För ytterligare information och riktlinjer för anpassade inställningar eller avancerade konfigurationer kan du börja med [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).  Den här sidan innehåller information och länkar tooadditional vägledning.

