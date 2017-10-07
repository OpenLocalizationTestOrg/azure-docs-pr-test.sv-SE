---
title: aaaDynamic grupper och Azure Active Directory B2B-samarbete | Microsoft Docs
description: "Azure Active Directory B2B-samarbete kan användas med dynamiska grupper i Azure AD"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: b011298de5fd2c851c6d9caaf5c2b257807ef0a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamiska grupper och Azure Active Directory B2B-samarbete

## <a name="what-are-dynamic-groups"></a>Vad är dynamiska grupper?
Dynamisk konfiguration av medlemskap i säkerhetsgrupper för Azure Active Directory (Azure AD) finns i [hello Azure-portalen](https://portal.azure.com). Administratörer kan ange regler toopopulate grupper som har skapats i Azure Active Directory baserat på användarattribut (till exempel userType, avdelning eller land). Medlemmar kan läggas till automatiskt tooor tas bort från en säkerhetsgrupp baserat på deras attribut. Dessa grupper kan ge åtkomst tooapplications eller molnbaserade resurser (SharePoint-webbplatser, dokument) och tooassign licenser toomembers. Läs mer om dynamiska grupper i [särskilda grupper i Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).

hello lämpliga [licensiering Azure AD Premium P1 eller P2](https://azure.microsoft.com/pricing/details/active-directory/) är nödvändiga toocreate och Använd dynamiska grupper. Läs mer i hello artikel [skapa attributbaserade regler för dynamiska gruppmedlemskap i Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-hello-built-in-dynamic-groups"></a>Vad är hello inbyggda dynamiska grupper?
Hej **alla användare** dynamisk grupp gör det möjligt för innehavare administratörer toocreate klickar du på en grupp som innehåller alla användare i hello-klient med en enda. Som standard hello **alla användare** gruppen innehåller alla användare i hello-katalogen, inklusive medlemmar och gäster.
Hello nya Azure Active Directory-administratörsportalen, kan du välja tooenable hello **alla användare** i hello gruppinställningar visa.

![inbyggda grupper](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-hello-all-users-dynamic-group"></a>Härdning hello dynamiska gruppen för alla användare
Som standard hello **alla användare** innehåller gruppen användare samt dina B2B-samarbete (Gäst). Du kan ytterligare skydda din **alla användare** gruppen med hjälp av en regel tooremove gästanvändare. hello följande bild visar hello **alla användare** gruppen ändras tooexclude gäster.

![Aktivera gruppen för alla användare](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

Det kan också vara användbara toocreate en dynamisk grupp som innehåller endast gästanvändare, så att du kan tillämpa principer (till exempel Azure AD villkorliga åtkomstprinciper) toothem.
Hur sådan grupp kan se ut:

![Exkludera gästanvändare](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Egenskaper för användare av B2B-samarbete](active-directory-b2b-user-properties.md)
* [Lägga till en B2B-samarbete tooa användarroll](active-directory-b2b-add-guest-to-role.md)
* [Delegera B2B-samarbete inbjudningar](active-directory-b2b-delegate-invitations.md)
* [B2B-samarbete kod och PowerShell-exempel](active-directory-b2b-code-samples.md)
* [Konfigurera SaaS-appar för B2B-samarbete](active-directory-b2b-configure-saas-apps.md)
* [Användartoken för B2B-samarbete](active-directory-b2b-user-token.md)
* [B2B-samarbete användaranspråk mappning](active-directory-b2b-claims-mapping.md)
* [Extern delning av Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuella begränsningar för B2B-samarbete](active-directory-b2b-current-limitations.md)
