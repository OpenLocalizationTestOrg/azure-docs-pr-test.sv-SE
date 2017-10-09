---
title: aaaPrivileged Identitetshantering prenumerationer - Azure | Microsoft Docs
description: "Beskriver hello prenumeration och Licensieringskrav för att hantera och använda Azure AD Privileged Identity Management i din klient"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: mwahl
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 2639d13c250a582fdcf0b277c9bab37fdfcabcb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Krav för Azure Active Directory Privileged Identity Management-prenumeration

Azure AD Privileged Identity Management är tillgänglig som en del av hello Premium P2-versionen av Azure AD. Mer information om hello andra funktioner i P2 och hur de förhåller tooPremium P1, se [Azure Active Directory-versioner](../active-directory-editions.md).

>[!NOTE]
När Azure Active Directory (Azure AD) Privileged Identity Management i förhandsgranskningen, fanns det ingen licens kontroller för en klient tootry hello-tjänst.  Nu när Azure AD Privileged Identity Management har nått allmän tillgänglighet, måste en utvärderingsversion eller betald prenumeration vara tillgänglig för hello klient toocontinue med Privileged Identity Management efter December 2016.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Bekräfta din utvärderingsversion eller betald prenumeration

Om du inte är säker på om din organisation har en utvärderingsversion eller köpt prenumeration kan du kontrollera om det finns en prenumeration i din klient genom att använda hello-kommandon som ingår i Azure Active Directory-modulen för Windows PowerShell V1. 
1. Öppna ett PowerShell-fönster.
2. Ange `Connect-MsolService` tooauthenticate som en användare i din klient.
3. Ange `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Detta kommando hämtar en lista över hello prenumerationer i din klient. Om det finns inga rader som returneras genom att du behöver tooobtain en Azure AD Premium P2 utvärderingsversion, köpa en prenumeration på Azure AD Premium P2 eller EMS E5 prenumeration toouse Azure AD Privileged Identity Management.  tooget en utvärderingsversion och börja använda Azure AD Privileged Identity Management läsa [Kom igång med Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

Om det här kommandot returnerar en rad i vilka SkuPartNumber är ”AAD_PREMIUM_P2” eller ”EMSPREMIUM” och IsTrial är ”True”, innebär en utvärderingsversion av Azure AD Premium P2 finns i hello-klient.  Om hello prenumerationsstatus inte har aktiverats och du inte har en Azure AD Premium P2- eller EMS E5 prenumeration inköp, måste sedan du köpa en prenumeration på Azure AD Premium P2 eller EMS E5 prenumeration toocontinue med hjälp av Azure AD Privileged Identity Management.

Azure AD Premium P2 är tillgänglig via en [Microsoft Enterprise-avtal](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), hello [öppna volymlicensprogram](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), och hello [Cloud Solution Providers programmet](https://partner.microsoft.com/en-US/cloud-solution-provider). Azure och Office 365-prenumeranter kan också köpa Azure AD Premium P2 online.  Mer information om priser för Azure AD Premium och hur tooorder online finns på [Azure Active Directory-priser](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Azure AD Privileged Identity Management är inte tillgänglig i klient

Azure AD Privileged Identity Management kommer inte längre tillgängliga i din klientorganisation om:
- Din organisation använde Azure AD Privileged Identity Management när den är i förhandsvisning och inte köpa Azure AD Premium P2-prenumeration eller EMS E5-prenumeration.
- Din organisation har en Azure AD Premium P2- eller EMS E5 utvärderingsversion ämne som upphört att gälla.
- Din organisation har en prenumeration du köpt som upphört att gälla.

När en Azure AD Premium P2-prenumeration eller EMS E5 prenumeration upphör att gälla eller en organisation som använder Azure AD Privileged Identity Management i förhandsgranskningen inte att hämta Azure AD Premium P2- eller EMS E5 prenumeration:

- Permanent roll tilldelningar tooAzure AD roller påverkas.
- hello Azure AD Privileged Identity Management – tillägget i hello Azure-portalen samt hello Graph API-cmdlets och PowerShell-gränssnitt för Azure AD Privileged Identity Management inte längre är tillgängliga för användare tooactivate Privilegierade roller, hantera privilegierad åtkomst eller utföra åtkomst granskning av Privilegierade roller.
- Berättigad rolltilldelningar för Azure AD-roller ska tas bort, och användare kommer inte längre att kunna tooactivate Privilegierade roller.
- Alla pågående åtkomst granskningar av Azure AD-roller avslutas och Azure AD Privileged Identity Management konfigurationsinställningar kommer att tas bort.
- Azure AD Privileged Identity Management kommer inte längre att skicka e-postmeddelanden om ändringarna för tilldelningen av rollen.

## <a name="next-steps"></a>Nästa steg

- [Kom igång med Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md)
- [Roller i Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-roles.md)
