---
title: "aaaHow tooconfigure användaretablering tooan Azure AD-galleriet program | Microsoft Docs"
description: "Hur du snabbt kan konfigurera omfattande användarkonto etablering och borttagning tooapplications redan visas i hello Azure AD Application Gallery"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2c28e59a3ac8f221ed93b2f6b0b1221f7604af23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-user-provisioning-tooan-azure-ad-gallery-application"></a>Hur tooconfigure användaretablering tooan Azure AD-galleriet program

*Etablering av konto* är hello handling för att skapa, uppdatera eller inaktiverar kontot användarposter i ett program lokal profil användararkivet. De flesta moln och SaaS-program lagrar hello användarrollen och behörigheterna i sina egna lokala användararkivet profil och förekomsten av sådana en användarpost i sina lokala arkivet är *krävs* för enkel inloggning och åtkomst toowork.

I hello Azure-portalen, hello **etablering** fliken i hello vänstra navigeringsfönstret för en Enterprise-App visar vilka etablering lägen stöds för appen. Detta kan vara ett av två värden:

## <a name="configuring-an-application-for-manual-provisioning"></a>Konfigurera ett program för manuell etablering

*Manuell* etablering innebär att användarkonton måste skapas manuellt med hjälp av hello-metoder som tillhandahålls av appen. Detta kan betyda att logga in på en administrativ portal för appen och lägga till användare som använder ett webbaserat användargränssnitt. Eller överför ett kalkylblad med kontoinformation för användare med hjälp av en mekanism som tillhandahålls av programmet. Dokumentationen hello angivna hello app eller kontakta hello app developer toodetermine wat metoder är tillgängliga.

Om manuell visas hello läge för ett visst program innebär det att ingen automatisk Azure AD connector-etablering har skapats för hello app ännu. Eller innebär hello appen har inte stöd för hello förutvärdering användaren management API på vilka toobuild en automatisk etablering koppling.

Om du vill toorequest stöd för automatisk etablering för en viss app kan du fylla i en begäran i <http://aka.ms/aadapprequest>.

## <a name="configuring-an-application-for-automatic-provisioning"></a>Konfigurera ett program för automatisk etablering

*Automatisk* innebär att en Azure AD connector-etablering har utvecklats för det här programmet. Mer information om hello Azure AD etablering tjänsten och hur det fungerar, se [automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).

Mer information om hur tooprovision specifika användare och grupper tooan program, se [hantera användaretablering konto för företagsappar](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).

Hej faktiska steg krävs tooenable och konfigurera automatisk etablering varierar beroende på programmet hello.

>[!NOTE]
>Starta genom att söka efter hello installationsprogrammet självstudiekursen specifika toosetting in etablering för ditt program och följa dessa steg tooconfigure både hello-appen och Azure AD toocreate hello etablering anslutning. 
>
>

Appen självstudier finns på [lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

En viktig sak tooconsider när du konfigurerar etablering vara tooreview och konfigurera hello attributmappning och arbetsflöden som definierar vilka egenskaper flödet för användare (eller grupp) från Azure AD toohello program. Detta innefattar att ställa in hello ”matchning av egenskap” som ska använda toouniquely identifiera och matchar användare eller grupper mellan hello två system. Mer information om viktiga processen.

## <a name="next-steps"></a>Nästa steg
[Anpassa attributmappning för Användaretablering för SaaS-program i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

