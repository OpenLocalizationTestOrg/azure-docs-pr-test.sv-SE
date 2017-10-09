---
title: "Konfigurera användaretablering tooan Azure AD-galleriet programmet aaaProblem | Microsoft Docs"
description: "Hur vanliga problem med tootroubleshoot står inför när du konfigurerar användaretablering tooan program som redan finns i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 19b12be019b05faecef74c6f92a9de03b52a5ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-user-provisioning-tooan-azure-ad-gallery-application"></a>Konfigurera användaretablering tooan Azure AD-galleriet programmet problem

Konfigurera [automatisk användaretablering](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) för en app (där de stöds), kräver att särskilda instruktioner följt tooprepare hello program för automatisk etablering. Du kan använda hello Azure portal tooconfigure hello etablering toosynchronize användaren konton toohello tjänstprogram.

Du bör alltid börja med att hitta hello installationsprogrammet självstudiekursen specifika toosetting in etablering för ditt program. Följ dessa steg tooconfigure båda hello-app och Azure AD toocreate hello etablering anslutning. En lista över app självstudier finns på [lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

## <a name="how-toosee-if-provisioning-is-working"></a>Hur fungerar toosee om etablering 

När hello-tjänsten har konfigurerats, kan de flesta insikter om hello driften av tjänsten hello ritas från två platser:

-   **Granskningsloggar** – hello etablering loggar granskningspost alla hello-åtgärder som utförs av hello etableras, inklusive frågor till Azure AD för tilldelade användare som ingår i omfånget för etablering. Fråga hello mål app hello befintliga användare jämföra hello användarobjekt mellan hello system. Lägg sedan till, uppdatera eller inaktivera hello användarkonto i hello målsystemet baserat på hello jämförelse. hello etablering granskningsloggar kan nås i hello Azure-portalen i hello **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt; granskningsloggarna**fliken. Filter hello loggar in hello **Kontoetablering** kategori tooonly finns hello etablering händelser för appen.

-   **Etableringsstatus –** en sammanfattning av hello senaste etablering kör för en viss app kan ses i hello **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt; Etablering** avsnittet längst ned hello hello-skärmen under hello-tjänstinställningar. Det här avsnittet sammanfattar hur många användare (och/eller grupper) håller på att synkroniseras mellan hello två system och om det finns några fel. Felinformation vara hello granskningsloggar. Observera att hello Etableringsstatus inte fyllas tills en första fullständig synkronisering har slutförts mellan Azure AD och hello app.

## <a name="general-problem-areas-with-provisioning-tooconsider"></a>Områden som allmänt problem med etablering tooconsider

Nedan visas en lista över hello allmänna problemområden detaljer om om du har en uppfattning om var toostart.

* [Etablerar tjänsten visas inte toostart](#provisioning-service-does-not-appear-to-start)
* [Det går inte att spara konfigurationen på grund av tooapp autentiseringsuppgifter fungerar inte](#can’t-save-configuration-due-to-app-credentials-not-working)
* [Granskningsloggar anta att även om de är tilldelade användare ”hoppas över” och inte etablerats](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-toostart"></a>Etablerar tjänsten visas inte toostart

Om du ställer in hello **Status för etablering** toobe **på** i hello **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt;Etablering** avsnitt i hello Azure-portalen. Men ingen annan statusinformation visas på sidan när efterföljande läses in på nytt. Det är troligt att hello-tjänsten körs men har inte slutförts ännu en inledande synkronisering. Kontrollera hello **granskningsloggar** ovan toodetermine vilka operations hello-tjänsten fungerar och om det finns några fel.

>[!NOTE]
>En inledande synkronisering kan ta allt från 20 minuter tooseveral timmar, beroende på hello storleken på hello Azure AD-katalog och hello antalet användare i omfånget för etablering. Efterföljande synkroniseringar efter hello inledande synkronisering vara snabbare som hello Etablerar tjänsten lagrar vattenstämplar som representerar hello tillståndet för båda systemen efter hello inledande synkronisering, förbättra prestanda för efterföljande synkronisering.
>
>

## <a name="cant-save-configuration-due-tooapp-credentials-not-working"></a>Det går inte att spara konfigurationen på grund av tooapp autentiseringsuppgifter fungerar inte

För etablering toowork kräver Azure AD giltiga autentiseringsuppgifter som tillåter den tooconnect tooa Användarhantering API som tillhandahålls av appen. Om dessa autentiseringsuppgifter fungerar inte eller om du inte vet wat som de är, granska hello självstudiekursen för att ställa in den här appen som beskrivs ovan.

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Granskningsloggar säger användare hoppas över och inte har etablerats även om de är tilldelade

När en användare visas som ”hoppas över” i granskningsloggarna för hello, är det mycket viktigt tooread hello utökad information i hello logga meddelandet toodetermine hello orsak. Nedan visas vanliga orsaker och lösningar:

-   **En målgrupp filter har konfigurerats** **som filtrera hello användare baserat på ett attributvärde**. Mer information om Omfångsfilter finns <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **hello användaren har ”inte effektivt rätt”.** Om du ser det aktuella felmeddelandet beror det på att det är problem med hello tilldelning användarpost lagras i Azure AD. toofix problemet kan tilldela hello användare (eller grupp) från hello app och tilldela den igen. Mer information om tilldelning, se <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Ett obligatoriskt attribut är inte ifyllda för en användare eller saknas.** En viktig sak tooconsider när du konfigurerar etablering vara tooreview och konfigurera hello attributmappning och arbetsflöden som definierar vilka egenskaper flödet för användare (eller grupp) från Azure AD toohello program. Detta innefattar att ställa in hello ”matchning av egenskap” som ska använda toouniquely identifiera och matchar användare eller grupper mellan hello två system. Mer information om viktiga processen finns <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

   * **Attributet mappningar för grupper:** etablering av hello gruppera namn och information i tillägg toohello medlemmar om stöd för vissa program. Du kan aktivera eller inaktivera den här funktionen genom att aktivera eller inaktivera hello **mappning** för gruppobjekt visas i hello **etablering** fliken. Om etablering grupper är aktiverat, måste tooreview hello attributet mappningar tooensure ett lämpligt fält som används för hello ”matchande ID”. Det kan vara hello Visa namn eller e-postalias), som hello grupp och dess medlemmar inte etableras om hello matchar egenskapen är tom eller inte är ifylld för en grupp i Azure AD.

#<a name="next-steps"></a>Nästa steg
[Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md)
