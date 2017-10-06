---
title: aaaCompare B2B-samarbete och B2C i Azure Active Directory | Microsoft Docs
description: "Vad är hello skillnaden mellan Azure Active Directory B2B-samarbete och Azure AD B2C?"
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
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 34d88b9a7d023e077568e8df3d5e1610ae05b361
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Jämför B2B-samarbete och B2C i Azure Active Directory

Både Azure Active Directory (AD Azure) B2B-samarbete och Azure AD B2C kan du du toowork med externa användare i Azure AD. Men hur jämför de?


Funktioner för B2B-samarbete |     Azure AD B2C fristående erbjudande
-------- | --------
Avsedd för: organisationer som vill toobe kan tooauthenticate användare från en partnerorganisation, oavsett identitetsleverantör. | Avsedd för: bjuda in kunder i din mobil- och webb-appar om individer, institutionella eller organisationens kunder i din Azure AD.
Identiteter som stöds: anställda med arbets- eller skolkonton, samarbetar med arbets-eller skolkonton eller en e-postadress. Snart toosupport direkt federation.  | Identiteter som stöds: konsumentanvändare med lokala program konton (alla e-adress eller användaren name) eller någon stöds sociala identitet med hjälp av direkt federation.
Som kataloganvändare hello partner: Partner användare från hello externa organisation hanteras i hello samma katalog som anställda, men kommenterade särskilt. De kan hanteras hello samma sätt som anställda, kan tillagda toohello samma grupper och så vidare  | Som directory hello kunden användaren entiteter: I hello programkatalogen. Hanteras separat från hello organisationens anställda och partners directory (om sådan finns.
Enkel inloggning (SSO) tooall Azure AD-anslutna appar stöds. Du kan exempelvis ange åtkomst tooOffice 365 eller lokala appar och tooother SaaS-appar, till exempel Salesforce eller Workday.  |  SSO toocustomer ägs appar i hello Azure AD B2C-klienter stöds. SSO tooOffice 365 eller tooother Microsoft och icke - Microsoft SaaS-appar som inte stöds.
Partner livscykel: hanteras av hello värd/bjuda in organisation.  | Kunden livscykel: självbetjäning eller hanteras av programmet hello.
Säkerhetsprinciper och efterlevnad: hanteras av hello värd/bjuda in organisation.  | Säkerhetsprinciper och efterlevnad: hanteras av programmet hello.
Anpassning: Värd/bjuda in organisation varumärken används.  |    Anpassning: Hanteras av programmet. Vanligtvis tenderar toobe produkten som är märkta med hello organisation Tona till hello bakgrund.
Mer information: [blogginlägget](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [dokumentation](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | Mer information: [produktsidan](https://azure.microsoft.com/en-us/services/active-directory-b2c/), [dokumentation](https://docs.microsoft.com/en-us/azure/active-directory-b2c/)


### <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Egenskaper för användare av B2B-samarbete](active-directory-b2b-user-properties.md)
* [Lägga till en B2B-samarbete tooa användarroll](active-directory-b2b-add-guest-to-role.md)
* [Delegera B2B-samarbete inbjudningar](active-directory-b2b-delegate-invitations.md)
* [Dynamiska grupper och B2B-samarbete](active-directory-b2b-dynamic-groups.md)
* [Konfigurera SaaS-appar för B2B-samarbete](active-directory-b2b-configure-saas-apps.md)
* [Användartoken för B2B-samarbete](active-directory-b2b-user-token.md)
* [B2B-samarbete användaranspråk mappning](active-directory-b2b-claims-mapping.md)
* [Extern delning av Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuella begränsningar för B2B-samarbete](active-directory-b2b-current-limitations.md)
* [Få support för B2B-samarbete](active-directory-b2b-support.md)
