---
title: "Azure AD Connect: Synkronisera tjänstinstanser | Microsoft Docs"
description: "Den här sidan dokument att tänka på för Azure AD-instanser."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2a0d8a599cf84cd6530bdbb24951156510d2cf3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Speciella överväganden vid instanser
Azure AD Connect används oftast med hello world wide instans av Azure AD och Office 365. Men det finns också andra instanser och de har olika krav för URL: er och andra saker.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Germany
Hej [Microsoft Cloud Tyskland](http://www.microsoft.de/cloud-deutschland) är ett suveräna moln som drivs av en förvaltare tyska data.

| URL: er tooopen i proxyserver |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Listor över återkallade certifikat |

När du loggar in tooyour Azure AD-klient måste du använda ett konto i hello onmicrosoft.de domän.

Funktioner som finns för närvarande inte i hello Microsoft Cloud Tyskland:

* **Azure AD Connect Health** är inte tillgänglig.
* **Automatiska uppdateringar** är inte tillgänglig.
* **Tillbakaskrivning av lösenord** är tillgängliga för förhandsgranskning med Azure AD Connect-version 1.1.570.0 och efter.
* Andra Azure AD Premium-tjänster är inte tillgängliga.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government-moln
Hej [Microsoft Azure Government molnet](https://azure.microsoft.com/features/gov/) är en molntjänst för USA: S regering.

Detta moln har stöd av tidigare versioner av DirSync. Från build 1.1.180 av Azure AD Connect stöds hello nästa generation av hello molnet. Den här generationen använder endast USA-baserade slutpunkter och har en lista över URL: er tooopen i din proxyserver.

| URL: er tooopen i proxyserver |
| --- |
| \*.microsoftonline.com |
| \*. microsoftonline.us |
| \*. gov.us.microsoftonline.com |
| + Listor över återkallade certifikat |

Azure AD Connect kan inte tooautomatically identifiera att Azure AD-klienten finns i hello offentliga moln. Du måste i stället tootake hello följande åtgärder när du installerar Azure AD Connect.

1. Starta hello Azure AD Connect-installationen.
2. När du ser hello första sidan där du bör tooaccept hello licensavtal, behöver du inte fortsätta men lämna hello installationsguiden körs.
3. Starta regedit och ändra hello registernyckeln `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello värdet `2`.
4. Gå tillbaka toohello Azure AD Connect-guiden för installation, acceptera hello LICENSAVTALET och fortsätt. Under installationen gör att toouse hello **anpassad konfiguration** installation sökväg (och inte snabbinstallationen). Fortsätt sedan hello installationen som vanligt.

Funktioner som finns för närvarande inte i hello Microsoft Azure Government molnet:

* **Azure AD Connect Health** är inte tillgänglig.
* **Automatiska uppdateringar** är inte tillgänglig.
* **Tillbakaskrivning av lösenord** är tillgängliga för förhandsgranskning med Azure AD Connect-version 1.1.570.0 och efter.
* Andra Azure AD Premium-tjänster är inte tillgängliga.

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
