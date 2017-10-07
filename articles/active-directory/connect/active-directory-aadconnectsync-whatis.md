---
title: "Azure AD Connect-synkronisering: Förstå och anpassa synkronisering | Microsoft Docs"
description: "Förklarar hur fungerar Azure AD Connect-synkronisering och toocustomize."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: markvi
ms.openlocfilehash: 97e4bd9904b077f2628e5f8dcbaa6f1a76168a12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect-synkronisering: Förstå och anpassa synkronisering
hello Azure Active Directory Connect-synkroniseringstjänster (Azure AD Connect sync) är en komponenten i Azure AD Connect. Det hand tar om alla hello-åtgärder som är relaterade toosynchronize identitetsdata mellan din lokala miljö och Azure AD. Azure AD Connect-synkronisering är hello efterföljande DirSync och Azure AD Sync Forefront Identity Manager med hello Azure Active Directory Connector har konfigurerats.

Det här avsnittet är hello hem för **Azure AD Connect-synkronisering** (kallas även **Synkroniseringsmotorn**) och visar länkar tooall andra avsnitt relaterade tooit. För länkar tooAzure AD Connect finns [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

hello-synkroniseringstjänsten består av två komponenter, hello lokala **Azure AD Connect-synkronisering** komponenten och hello tjänstsidan i Azure AD kallas **Azure AD Connect-synkroniseringstjänsten**. hello-tjänsten är gemensamma för DirSync, Azure AD Sync och Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect sync-ämnen
| Avsnitt | Det täcker och när tooread |
| --- | --- |
| **Grunderna i Azure AD Connect-synkronisering** | |
| [Förstå hello-arkitektur](active-directory-aadconnectsync-understanding-architecture.md) |För de som är nya toohello Synkroniseringsmotorn och vill toolearn om hello-arkitekturen och hello termer som används. |
| [Tekniska begrepp](active-directory-aadconnectsync-technical-concepts.md) |En kort version av hello arkitektur ämne och en kort beskriver hello termer som används. |
| [Topologier för Azure AD Connect](active-directory-aadconnect-topologies.md) |Beskriver hello olika scenarier och topologier hello sync-motorn har stöd för. |
| **Anpassad konfiguration** | |
| [Körs hello-installationsguiden igen](active-directory-aadconnectsync-installation-wizard.md) |Beskriver vilka alternativ som finns tillgängliga när du kör installationsguiden för hello Azure AD Connect igen. |
| [Förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md) |Beskriver hello Konfigurationsmodell kallas deklarativ etablering. |
| [Förstå uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |Beskriver hello syntax för hello Uttrycksspråk som används i deklarativ etablering. |
| [Förstå hello standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md) |Beskriver hello out-of-box regler och hello standardkonfigurationen. Beskriver också hur hello regler tillsammans för hello out-of-box scenarier toowork. |
| [Förstå användare och kontakter](active-directory-aadconnectsync-understanding-users-and-contacts.md) |Fortsätter på hello föregående avsnitt och beskriver hur hello konfiguration för användare och kontakter fungerar tillsammans, särskilt i en miljö med flera skogar. |
| [Hur toomake en ändring toohello standard konfiguration](active-directory-aadconnectsync-change-the-configuration.md) |Vägleder dig igenom hur toomake en gemensam configuration ändra tooattribute flödar. |
| [Metodtips för att ändra hello standardkonfigurationen](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |Stöd för begränsningar och för att göra ändringar toohello out-of-box-konfiguration. |
| [Konfigurera filtrering](active-directory-aadconnectsync-configure-filtering.md) |Beskriver hello olika alternativ för hur toolimit vilket objekt som ska synkroniseras tooAzure AD och steg för steg hur tooconfigure dessa alternativ. |
| **Scenarier och funktioner** | |
| [Förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |Beskriver hello *förhindra oavsiktliga borttagningar* funktion och hur tooconfigure den. |
| [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) |Beskriver hello inbyggda scheduler, som importerar, synkronisering och export av data. |
| [Implementera Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) |Beskriver hur Lösenordssynkronisering fungerar, hur tooimplement, och hur toooperate och felsöka. |
| [Tillbakaskrivning av enhet.](active-directory-aadconnect-feature-device-writeback.md) |Beskriver hur tillbakaskrivning av enheter fungerar i Azure AD Connect. |
| [Katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md) |Beskriver hur tooextend hello Azure AD-schemat med din egen anpassade attribut. |
| **Synkroniseringstjänsten** | |
| [Azure AD Connect sync-tjänsten-funktioner](active-directory-aadconnectsyncservice-features.md) |Beskriver hello sync-tjänsten på klientsidan och hur toochange synkronisera inställningar i Azure AD. |
| [Duplicerat attribut återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) |Beskriver hur tooenable och använda **userPrincipalName** och **proxyAddresses** dubblettattribut värden återhämtning. |
| **Åtgärder och UI** | |
| [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) |Beskriver hello Synchronization Service Manager UI, inklusive [Operations](active-directory-aadconnectsync-service-manager-ui-operations.md), [kopplingar](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md), och [metaversumsökningen](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) flikar. |
| [Åtgärder och överväganden](active-directory-aadconnectsync-operations.md) |Beskriver Driftproblem, till exempel katastrofåterställning. |
| **Hur...** | |
| [Återställa hello Azure AD-konto](active-directory-aadconnectsync-howto-azureadaccount.md) |Hur tooreset hello autentiseringsuppgifterna för tjänstkontot för hello användas tooconnect från Azure AD Connect sync tooAzure AD. |
| **Mer information och referenser** | |
| [Portar](active-directory-aadconnect-ports.md) |Visar vilka portar som måste tooopen mellan hello Synkroniseringsmotorn och dina lokala kataloger och Azure AD. |
| [Attribut synkroniserade tooAzure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) |Visar alla attribut som synkroniseras mellan lokala AD och Azure AD. |
| [Referens för funktioner](active-directory-aadconnectsync-functions-reference.md) |Visar en lista över alla funktioner som är tillgängliga i deklarativ etablering. |

## <a name="additional-resources"></a>Ytterligare resurser
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

