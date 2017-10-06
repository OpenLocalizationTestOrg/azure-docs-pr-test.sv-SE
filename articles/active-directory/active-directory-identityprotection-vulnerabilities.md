---
title: aaaVulnerabilities som identifieras av Azure Active Directory Identity Protection | Microsoft Docs
description: "Översikt över hello sårbarheter som identifieras av Azure Active Directory Identity Protection."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5e1cb401f8b566a180eb46e3420a090bcfc66767
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Sårbarheter som identifieras av Azure Active Directory Identity Protection
Säkerhetsrisker är svagheter i din miljö som kan utnyttjas av en angripare. Vi rekommenderar att åtgärda dessa säkerhetsrisker tooimprove hello säkerhetsläget i din organisation och förhindra att angripare utnyttjar dem.


![säkerhetsproblem](./media/active-directory-identityprotection-vulnerabilities/101.png "säkerhetsrisker")



hello ger följande avsnitt dig en översikt över hello säkerhetsrisker som rapporterats av Identity Protection.

## <a name="multi-factor-authentication-registration-not-configured"></a>Multifaktorautentisering registrering har inte konfigurerats
Det här problemet kan du styra hello distribution av Azure Multi-Factor Authentication i din organisation. 

Azure Multi-Factor authentication ger ett andra säkerhetslager toouser autentiseringsmetod för nätverkssäkerhet. Skydda åtkomst toodata och program hjälper samtidigt som du uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering via en mängd alternativ för enkel verifiering – telefonsamtal, textmeddelande eller mobilapp meddelande eller verifiering kod och 3 part OATH-token.

Vi rekommenderar att du behöver Azure Multi-Factor Authentication för användarinloggningar. Multifaktorautentisering spelar en viktig roll i risk-baserade villkorliga åtkomstprinciper tillgängliga via Identity Protection.

Mer information finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Ohanterad molnappar
Det här problemet kan du identifiera ohanterade molnappar i din organisation.

I moderna företag inte IT-avdelningar ofta känner till alla hello molnprogram att användare i organisationen använder toodo sitt arbete. Det är enkelt toosee varför administratörer skulle oroar obehörig åtkomst toocorporate data, möjliga dataläckage och andra säkerhetsrisker. 

Vi rekommenderar att din organisation distribuerar Cloud App Discovery toodiscover ohanterad molnprogram och toomanage dessa program med Azure Active Directory.

Mer information finns i [hitta ohanterade molnprogram med Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

## <a name="security-alerts-from-privileged-identity-management"></a>Säkerhetsaviseringar från Privileged Identity Management
Den här säkerhetsrisken hjälper dig att identifiera och lösa aviseringar om Privilegierade identiteter i din organisation.  

tooenable användare toocarry Privilegierade åtgärder, organisationer behöver toogrant användare tillfälligt eller permanent privilegierad åtkomst i Azure AD Azure eller Office 365 resurser eller andra SaaS-appar. Var och en av dessa Privilegierade användare ökar hello risken för angrepp på din organisation. Den här säkerhetsrisken hjälper dig att identifiera användare med onödiga privilegierad åtkomst och vidta lämplig åtgärd tooreduce eller eliminera hello risker de utgör. 

Vi rekommenderar att din organisation använder Azure AD Privileged Identity Management toomanage, styra och övervaka Privilegierade identiteter och deras åtkomst tooresources i Azure AD samt andra Microsoft online services som Office 365 eller Microsoft Intune.

Mer information finns i [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="see-also"></a>Se även
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

