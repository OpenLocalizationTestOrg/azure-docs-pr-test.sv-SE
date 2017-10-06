---
title: aaaOffice 365 extern delning och Azure Active Directory B2B-samarbete | Microsoft Docs
description: "anspråk mappning referens för Azure Active Directory B2B-samarbete"
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 60452b27b328453eda729bd839c982b479cb6f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Extern delning av Office 365 och Azure Active Directory B2B-samarbete

Extern delning i Office 365 (OneDrive, SharePoint Online, enhetlig grupper, etc.) och Azure Active Directory (AD Azure) B2B collaboration är tekniskt hello samma sak. Alla extern delning (utom OneDrive/SharePoint Online), inklusive gäster i Office 365-grupper, redan använder hello Azure AD B2B-samarbetsinbjudan API: er för att delas.

OneDrive/SharePoint Online finns en separat inbjudan manager. Stöd för extern delning i OneDrive/SharePoint Online igång innan Azure AD utvecklats stödet. Över tiden, OneDrive/SharePoint Online extern delning har uppstått flera funktioner och många miljoner användare som använder hello produkten har inbyggda delning mönster. Det finns emellertid vissa skillnader mellan hur OneDrive/SharePoint Online extern delning fungerar och hur fungerar Azure AD B2B-samarbete:

- OneDrive/SharePoint Online lägger till användare toohello katalog när användare har lösts in deras inbjudan. Så innan åtgärd visas hello användare i Azure AD-portalen. Om en användare i hello inbjuder tiden i en annan plats, skapas en ny inbjudan. Men när du använder Azure AD B2B-samarbete läggs användare omedelbart på inbjudan så att de visas överallt.

- Hej inlösning upplevelse i OneDrive/SharePoint Online ser ut hello upplevelse i Azure AD B2B-samarbete. När en användare redeems en inbjudan, hello upplevelser ser likadana ut.

- Azure AD B2B collaboration uppmanas användare kan nå från OneDrive/SharePoint Online delning av dialogrutor. OneDrive/SharePoint Online uppmanas användare visas också i Azure AD när de lösa deras inbjudan.

- toomanage extern delning i OneDrive/SharePoint Online med Azure AD B2B-samarbete ange hello OneDrive/SharePoint Online extern delning inställning för**Tillåt endast att dela med externa användare redan i hello directory**. Användarna kan gå tooexternally delade platser och välj från externa samarbetspartners som hello admin har lagts till. Hej administratör kan lägga till hello externa samarbetspartners via hello B2B-samarbetsinbjudan API: er.

![Hej OneDrive/SharePoint Online extern delning](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Egenskaper för användare av B2B-samarbete](active-directory-b2b-user-properties.md)
* [Lägga till en B2B-samarbete tooa användarroll](active-directory-b2b-add-guest-to-role.md)
* [Delegera B2B-samarbete inbjudningar](active-directory-b2b-delegate-invitations.md)
* [Dynamiska grupper och B2B-samarbete](active-directory-b2b-dynamic-groups.md)
* [B2B-samarbete kod och PowerShell-exempel](active-directory-b2b-code-samples.md)
* [Konfigurera SaaS-appar för B2B-samarbete](active-directory-b2b-configure-saas-apps.md)
* [Användartoken för B2B-samarbete](active-directory-b2b-user-token.md)
* [B2B-samarbete användaranspråk mappning](active-directory-b2b-claims-mapping.md)
* [Aktuella begränsningar för B2B-samarbete](active-directory-b2b-current-limitations.md)
