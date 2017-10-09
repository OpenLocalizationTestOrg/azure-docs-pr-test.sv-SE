---
title: "Hybrididentitet: Jämförelse av katalogintegreringsverktyg | Microsoft Docs"
description: "Detta är innehåller en omfattande tabell som jämför hello olika katalogintegreringsverktyg som kan användas för katalogintegrering."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 18ac0a0f58726eceb85510df516e8db71429b313
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hybrididentitet: Jämförelse av katalogintegreringsverktyg
Hello års hello katalogintegreringsverktygen har vuxit och utvecklats.  Det här dokumentet är toohelp ger en samlad vy över dessa verktyg och en jämförelse av hello-funktioner som är tillgängliga i varje.

<!-- hello hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> Azure AD Connect innehåller hello komponenter och funktioner som tidigare lanserats som Dirsync och AAD Sync. Dessa verktyg är inte längre ut individuellt och alla framtida förbättringar tas med i uppdateringar tooAzure AD ansluta, så att du alltid vet var tooget hello senaste funktionerna.
> 
> DirSync och Azure AD Sync är föråldrade. Mer information finns [här](active-directory-aadconnect-dirsync-deprecated.md).
> 
> 

Använd hello följande nyckel för varje hello register.

● = Tillgänglig nu  
FR = Framtida version  
PP = Offentlig granskning  

## <a name="on-premises-toocloud-synchronization"></a>Lokala tooCloud synkronisering
| Funktion | Azure Active Directory Connect | Azure Active Directory Sync Services (AAD Sync) | Verktyget Azure Active Directory Synchronization (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Ansluta toosingle lokala AD-skog |● |● |● |● |● |
| Ansluta toomultiple lokala AD-skogar |● |● | |● |● |
| Ansluta toomultiple lokala Exchange-organisationer |● | | | | |
| Ansluta toosingle lokala LDAP-katalogen |FR | | |● |● |
| Ansluta toomultiple lokala LDAP-kataloger |FR | | |● |● |
| Ansluta tooon lokala AD och lokala LDAP-kataloger |FR | | |● |● |
| Ansluta toocustom system (d.v.s. SQL, Oracle, MySQL osv.) |FR | | |● |● |
| Synkronisera kunddefinierade attribut (katalogtillägg) |● | | | | |
| Ansluta tooon lokalt HR (SAP, Oracle eBusiness, PeopleSoft osv.) |FR | | |● |● |
| Stöder FIM-Synkroniseringsregler och kopplingar för etablering tooon lokala system. | | | |● |● |

## <a name="cloud-tooon-premises-synchronization"></a>Molnet tooOn lokala synkronisering
| Funktion | Azure Active Directory Connect | Azure Active Directory Sync Services | Verktyget Azure Active Directory Synchronization (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Tillbakaskrivning av enheter |● | |● | | |
| Tillbakaskrivning av attribut (för Exchange-hybridinstallation) |● |● |● |● |● |
| Tillbakaskrivning av gruppobjekt |● | | | | |
| Tillbakaskrivning av lösenord (från lösenordsåterställning via självbetjäning (SSPR) och lösenordsändring) |● |● | | | |

## <a name="authentication-feature-support"></a>Autentiseringsfunktioner som stöds
| Funktion | Azure Active Directory Connect | Azure Active Directory Sync Services | Verktyget Azure Active Directory Synchronization (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Lösenordssynkronisering för en enda lokal AD-skog |● |● |● | | |
| Lösenordssynkronisering för flera lokala AD-skogar |● |● | | | |
| Enkel inloggning med federation |● |● |● |● |● |
| Tillbakaskrivning av lösenord (från SSPR och lösenordsändring) |● |● | | | |

## <a name="set-up-and-installation"></a>Konfiguration och installation
| Funktion | Azure Active Directory Connect | Azure Active Directory Sync Services | Verktyget Azure Active Directory Synchronization (DirSync) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|
| Stöder installation på en domänkontrollant |● |● |● | |
| Stöder installation med hjälp av SQL Express |● |● |● | |
| Enkel uppgradering från DirSync |● | | | |
| Lokalisering av administratörs-UX tooWindows serverspråk |● |● |● | |
| Lokalisering av slutanvändaren UX tooWindows serverspråk | | | |● |
| Stöd för Windows Server 2008 och Windows Server 2008 R2 |● för synkronisering, inte för federation |● |● |● |
| Stöd för Windows Server 2012 och Windows Server 2012 R2 |● |● |● |● |

## <a name="filtering-and-configuration"></a>Filtrering och konfiguration
| Funktion | Azure Active Directory Connect | Azure Active Directory Sync Services | Verktyget Azure Active Directory Synchronization (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Filtrera baserat på domäner och organisationsenheter |● |● |● |● |● |
| Filtrera baserat på objektens attributvärden |● |● |● |● |● |
| Tillåt minimal uppsättning attribut toobe synkroniseras (MinSync) |● |● | | | |
| Tillåt olika mallar toobe används för attributflöden |● |● | | | |
| Tillåt borttagning av attribut som flödar från AD tooAzure AD |● |● | | | |
| Tillåt avancerad anpassning av attributflöden |● |● | |● |● |

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

