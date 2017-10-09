---
title: "aaaNo användare som etablerade tooan Azure AD-galleriet program | Microsoft Docs"
description: "Hur vanliga problem med tootroubleshoot inför när du inte ser användarna som visas i en en Azure AD-galleriet program som du har konfigurerat för användaretablering med Azure AD"
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
ms.date: 05/04/2017
ms.author: asteen
ms.openlocfilehash: 4d9693a202ed657e1de5571b50e5d499bef1bb3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="no-users-are-being-provisioned-tooan-azure-ad-gallery-application"></a>Inga användare som etablerade tooan Azure AD-galleriet program

När automatisk etablering har konfigurerats för ett program (inklusive verifierar att hello app autentiseringsuppgifterna för tooAzure AD tooconnect toohello app är giltiga). Användare och/eller grupper är etablerad toohello app och bestäms av hello följande saker:

-   Som användare och grupper har **tilldelade** toohello program. Mer information om tilldelning, se [tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Huruvida **attributet mappningar** är aktiverade och konfigurerade toosync giltiga attribut från Azure AD toohello app. Mer information om attributmappning finns [anpassa användaren etablering attributmappning för SaaS-program i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Oavsett om det finns en **målgrupp filter** finns som filtrerar användare baserat på specifika attributvärden. Mer information om Omfångsfilter finns [attributbaserad programmet etablering med Omfångsfilter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

När sett att användarna inte tillhandahålls, kontakta hello granskningsloggar i Azure AD och Sök efter loggposter för en viss användare.

hello etablering granskningsloggar kan nås i hello Azure-portalen i hello **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt; granskningsloggarna**fliken. Filter hello loggar in hello **Kontoetablering** kategori tooonly finns hello etablering händelser för appen. Du kan söka efter användare baserat på hello ”matchande ID” som konfigurerats för dem i hello attributmappning. Om du har konfigurerat hello ”huvudnamn” eller ”e-postadress” som hello matchar attributet på hello Azure AD-sidan och hello användaren inte etablering har värdet till exempel ”audrey@contoso.com”. Sök sedan hello granskningsloggar för ”audrey@contoso.com” och granska sedan poster returnerades.

hello etablering audit loggar alla hello åtgärder som utförs av hello etablering tjänsten, inklusive frågor till Azure AD för tilldelade användare som ingår i omfånget för etablering, frågar hello mål app hello befintliga användare, genom att jämföra hello post användarobjekt mellan hello system. Lägg sedan till, uppdatera eller inaktivera hello användarkonto i hello målsystemet baserat på hello jämförelse.

## <a name="general-problem-areas-with-provisioning-tooconsider"></a>Allmänna problemområden etablering tooconsider

Nedan visas en lista över hello allmänna problemområden detaljer om om du har en uppfattning om var toostart.

* [Etablerar tjänsten visas inte toostart](#provisioning-service-does-not-appear-to-start)
* [Granskningsloggar anta att även om de är tilldelade användare hoppas över och inte etablerats](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-toostart"></a>Etablerar tjänsten visas inte toostart

Om du ställer in hello **Status för etablering** toobe **på** i hello **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt;Etablering** avsnitt i hello Azure-portalen. Men ingen statusinformation visas på sidan när efterföljande laddar är det troligt att hello-tjänsten körs, men har inte slutförts ännu en inledande synkronisering. Kontrollera hello **granskningsloggar** ovan toodetermine vilka operations hello-tjänsten fungerar och om det finns några fel.

>[!NOTE]
>En inledande synkronisering kan ta allt från 20 minuter tooseveral timmar, beroende på hello storleken på hello Azure AD-katalog och hello antalet användare i omfånget för etablering. Efterföljande synkroniseringar efter hello inledande synkronisering är snabbare, som hello Etablerar tjänsten lagrar vattenstämplar som representerar hello tillståndet för båda systemen efter hello inledande synkronisering. Detta förbättrar prestanda för efterföljande synkronisering.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Granskningsloggar säger användare hoppas över och inte har etablerats även om de är tilldelade

När en användare visas som ”hoppas över” i granskningsloggarna för hello, är det mycket viktigt tooread hello utökad information i hello logga meddelandet toodetermine hello orsak. Nedan visas vanliga orsaker och lösningar:

-   **En målgrupp filter har konfigurerats** **som filtrera hello användare baserat på ett attributvärde**. Mer information om Omfångsfilter finns <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **hello användaren har ”inte effektivt rätt”.** Om du ser det aktuella felmeddelandet beror det på att det är problem med hello tilldelning användarpost lagras i Azure AD. toofix problemet kan tilldela hello användare (eller grupp) från hello app och tilldela den igen. Mer information om tilldelning, se <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Ett obligatoriskt attribut är inte ifyllda för en användare eller saknas.** En viktig sak tooconsider när du konfigurerar etablering vara tooreview och konfigurera hello attributmappning och arbetsflöden som definierar vilka egenskaper flödet för användare (eller grupp) från Azure AD toohello program. Detta innefattar att ställa in hello ”matchning av egenskap” som ska använda toouniquely identifiera och matchar användare eller grupper mellan hello två system. Mer information om viktiga processen finns [anpassa användaren etablering attributmappning för SaaS-program i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Attributet mappningar för grupper:** etablering av hello gruppera namn och information i tillägg toohello medlemmar om stöd för vissa program. Du kan aktivera eller inaktivera den här funktionen genom att aktivera eller inaktivera hello **mappning** för gruppobjekt visas i hello **etablering** fliken. Om etablering grupper är aktiverat, måste tooreview hello attributet mappningar tooensure ett lämpligt fält som används för hello ”matchande ID”. Det kan vara hello Visa namn eller e-postalias), som hello grupp och dess medlemmar inte etableras om hello matchar egenskapen är tom eller inte är ifylld för en grupp i Azure AD.

## <a name="next-steps"></a>Nästa steg
[Azure AD Connect-synkronisering: Förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md)

