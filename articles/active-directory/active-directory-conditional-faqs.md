---
title: "aaaAzure villkorlig åtkomst för Active Directory och svar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om villkorlig åtkomst i Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d23acbb01217d7e9717d1a43de1b46a929404118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory villkorlig åtkomst vanliga frågor och svar

## <a name="which-applications-work-with-conditional-access-policies"></a>Vilka program fungerar med principer för villkorlig åtkomst?

Information om program som fungerar med principer för villkorlig åtkomst finns [program och webbläsare som använder regler för villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Tillämpas principer för villkorlig åtkomst för B2B-samarbete och gästanvändare?

Principerna tillämpas för business-to-business (B2B) samarbete användare. Men i vissa fall kan kanske en användare inte kan toosatisfy hello-kraven. En gästanvändare organisation kan till exempel inte stöd för multifaktorautentisering. 



## <a name="does-a-sharepoint-online-policy-also-apply-tooonedrive-for-business"></a>En SharePoint Online policy även gäller tooOneDrive för företag?

Ja. SharePoint Online-principen gäller även tooOneDrive för företag.


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Varför kan inte ange en princip på klientprogram som Word och Outlook?

En villkorlig åtkomstprincip anger krav för åtkomst till en tjänst. Den tillämpas när toothat Autentiseringstjänsten inträffar. hello principen anges inte direkt på ett klientprogram. I stället tillämpas när en klient anropar en tjänst. Till exempel gäller en princip på SharePoint tooclients anropar SharePoint. En princip på Exchange gäller tooOutlook.

## <a name="does-a-conditional-access-policy-apply-tooservice-accounts"></a>Gäller en villkorlig åtkomstprincip tooservice konton?

Principer för villkorlig åtkomst gäller tooall användarkonton. Detta innefattar användarkonton som används som tjänstkonton. Ofta kan inte ett tjänstkonto som kör obevakad uppfylla hello av en princip för villkorlig åtkomst. Multifaktorautentisering kan komma att krävas. Tjänstkonton som kan uteslutas från en princip med hjälp av villkorlig åtkomst principinställningarna för hantering. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Finns Graph API: er för att konfigurera principer för villkorlig åtkomst?

För närvarande inte. 

## <a name="what-is-hello-default-exclusion-policy-for-unsupported-device-platforms"></a>Vad är hello undantag standardprincipen för plattformar för enheter som inte stöds?

För närvarande kan tillämpas selektivt principer för villkorlig åtkomst för användare av iOS och Android-enheter. Program för andra enhetsplattformar, som standard inte påverkas av hello principen för villkorlig åtkomst för iOS och Android-enheter. En klientadministratör kan välja toooverride hello global princip toodisallow åtkomst toousers på plattformar som inte stöds.


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Hur fungerar principer för villkorlig åtkomst för Microsoft Teams?  

Microsoft-Teams bygger kraftigt på Exchange Online och SharePoint Online för huvudscenarier produktivitet som möten, kalendrar och fildelning. Principer för villkorlig åtkomst som ställs för dessa molnappar gäller tooMicrosoft team när en användare loggar in.

Microsoft-Teams även stöds separat som en molnapp i Azure Active Directory-principer för villkorlig åtkomst. Principer för certifikat som har angetts för en molnappen gäller tooMicrosoft team när en användare loggar in.

Microsoft-Teams skrivbordsklienter för Windows och Mac stöder modern autentisering. Modern autentisering kan logga in baserat på hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office-program för olika plattformar. 
