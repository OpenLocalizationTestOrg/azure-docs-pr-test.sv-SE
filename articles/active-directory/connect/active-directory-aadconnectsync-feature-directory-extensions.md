---
title: "Azure AD Connect-synkronisering: katalogtillägg | Microsoft Docs"
description: "Det här avsnittet beskrivs hello directory tillägg-funktionen i Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 31525ae914aa4d9e047ea1515b460a8311d5c815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect-synkronisering: katalogtillägg
Katalogtillägg tillåter tooextend hello schemat i Azure AD med dina egna attribut från lokala Active Directory. Den här funktionen kan du använda attribut du fortsätter toomanage lokala toobuild LOB-appar. Attributen kan användas via [Azure AD Graph katalogtillägg](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) eller [Microsoft Graph](https://graph.microsoft.io/). Du kan se hello attribut tillgängliga med [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) och [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respektive.

För närvarande förbrukar inga Office 365-arbetsbelastning attributen.

Du konfigurerar vilka ytterligare attribut som du vill toosynchronize i hello anpassade inställningar sökväg i hello installationsguiden.
![Schemat tillägget guiden](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  
hello installationen visar hello följande attribut som är giltiga alternativ:

* Användare och grupp objekttyper
* Enkelvärdesattribut anges: String, Boolean, heltal, binär
* Flera värden attribut: sträng, binär

hello lista med attribut som läses från hello schema-cache skapas under installationen av Azure AD Connect. Om du har utökat hello Active Directory-schemat med ytterligare attribut, sedan hello [schemat måste uppdateras](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) innan dessa nya attribut visas.

Ett objekt i Azure AD kan ha upp too100 katalogattribut tillägg. hello maxlängden är 250 tecken. Om ett attributvärde är längre trunkeras det av hello Synkroniseringsmotorn.

Under installationen av Azure AD Connect registreras ett program när dessa attribut är tillgängliga. Du kan se det här programmet i hello Azure-portalen.  
![Schemat tillägget App](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Dessa attribut finns nu tillgängliga diagram:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

hello-attribut med tillägget prefixet\_{AppClientId}\_. Hej AppClientId har hello samma värde för alla attribut i Azure AD-klienten.

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
