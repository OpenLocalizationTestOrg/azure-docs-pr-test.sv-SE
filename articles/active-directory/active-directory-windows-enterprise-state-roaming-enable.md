---
title: "Aktivera Företagsroaming i Azure Active Directory | Microsoft Docs"
description: "Vanliga frågor om Enterprise tillstånd centrala inställningar i Windows-enheter. Enterprise tillstånd centrala ger användarna en enhetlig miljö på sina Windows-enheter och minskar den tid som krävs för att konfigurera en ny enhet."
services: active-directory
keywords: "företagsroaming, windows molnet, hur du aktiverar enterprise tillstånd nätverksväxling"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 77d75f4a647e44f27cd9ba8db5286f1456c3ac66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Aktivera företagsroaming i Azure Active Directory
Enterprise tillstånd växling är tillgängliga för en organisation med en prenumeration för Premium Azure Active Directory (AD Azure). Mer information om hur du hämtar en Azure AD-prenumeration, finns det [produktsida för Azure AD](https://azure.microsoft.com/services/active-directory).

När du aktiverar Enterprise tillstånd centrala beviljas organisationen automatiskt licenser för en kostnadsfri prenumeration som är begränsad användning till Azure Rights Management. Den här kostnadsfria prenumerationen är begränsad till kryptera och dekryptera Företagsinställningar och programdata synkroniseras av Enterprise tillstånd Roaming-tjänsten. Du måste ha en betald prenumeration att använda alla funktioner i Azure Rights Management.

Följ dessa steg om du vill aktivera centrala Enterprise-läge när du har fått en Premium Azure AD-prenumeration:

1. Logga in på den klassiska Azure-portalen.
2. Till vänster, Välj **ACTIVE DIRECTORY**, och välj sedan den katalog som du vill aktivera Enterprise tillstånd växling.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Gå till den **konfigurera** högst upp.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4. Rulla ned på sidan och välj **användarna kan synkronisera inställningar och företagets APPDATA**, och klicka sedan på **spara**.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Enheten måste autentiseras med hjälp av en Azure AD-identitet för för en Windows 10-enhet att flyttas med Enterprise tillstånd Roaming-tjänsten. För enheter som är anslutna till Azure AD är användarens primära inloggning Azure AD-identitet, så ingen ytterligare konfiguration krävs. För enheter som använder en traditionella lokala Active Directory IT-administratören måste [ansluta domänanslutna enheter till Azure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Synkronisera datalagring
Enterprise tillstånd centrala data finns i en eller flera [Azure-regioner](https://azure.microsoft.com/regions/) som bäst passar med värdet för land/region i Azure Active Directory-instans. Enterprise tillstånd centrala data är partitionerad baserat på tre huvudsakliga geografiska områden: Nordamerika, EMEA och APAC. Enterprise tillstånd centrala data för klienten finns lokalt med den geografiska regionen och replikeras inte över regioner.  Till exempel kunder som har sina land/region-värdet anges till ett av EMEA länder som ”Frankrike” eller ”Zambia” har sina data som finns i en eller i Azure-regioner inom Europa.  Kunder som värdet sina land/region i Azure AD till en Nordamerika länder som ”USA” eller ”Kanada” har sina data som finns i en eller flera Azure-regioner inom USA.  Kunder som värdet sina land/region i Azure AD till en APAC länder som ”Australien” eller ”Nya Zeeland” måste ha sina data som finns i en eller flera Azure-regioner i Asien.  Syd American länder och Antarktis data ska finnas i en eller flera Azure-regioner inom USA.  Land/region-värdet har angetts som en del av processen för Azure AD-katalogen och kan inte ändras senare. 

Om du behöver mer information om plats för datalagring, kontrollera filen en biljett hos [Azure-supporten](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Hantera Enterprise tillstånd nätverksväxling
Azure AD globala administratörer kan aktivera och inaktivera Enterprise tillstånd centrala i den klassiska Azure-portalen.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globala administratörer kan begränsa synkronisering av inställningar till specifika säkerhetsgrupper.

Globala administratörer kan också visa en statusrapport för per användare enheten synkronisering genom att välja en viss användare i Active Directory-instans **användare** listan och klicka på **enheter** fliken och väljer Visa **Enheter som synkroniserar inställningar och företagets AppData**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

## <a name="data-retention"></a>Datakvarhållning
Data som synkroniseras till Azure via Enterprise tillstånd centrala kommer att sparas på obestämd tid om inte en manuell borttagningsåtgärd utförs eller aktuella data bedöms vara inaktuell. 

**Explicit borttagning:** data tas bort när en Azure-administratör tar bort en användare eller en katalog eller administratör uttryckligen begär att data som ska tas bort.

* **Ta bort användaren**: när en användare tas bort i Azure AD användarkontot centrala data kommer att markeras för borttagning och kommer att tas bort 90 till 180 dagar. 
* **Ta bort katalogen**: ta bort en hel katalog i Azure AD är en omedelbar åtgärd. Alla inställningsdata associerade med directory markeras för borttagning och kommer att tas bort 90 till 180 dagar. 
* **På begäran om borttagning**: om Azure AD-administratör vill ta bort data eller inställningsdata för en viss användare manuellt, kan administratören filen en biljett hos [Azure-supporten](https://azure.microsoft.com/support/). 

**Inaktuella data tas bort**: Data som inte har öppnats för ett år (”kvarhållningsperioden”) kommer att behandlas som föråldrade och kan tas bort från Azure. Kvarhållningsperioden kan ändras, men inte mindre än 90 dagar. Inaktuella data kan vara en specifik uppsättning inställningar för Windows-program eller alla inställningar för en användare. Exempel:

* Om inga enheter åtkomst till en samling med särskilda inställningar (t.ex. Om ett program tas bort från enheten eller en grupp inställningar, till exempel ”tema” är inaktiverad för alla enheter som en användare), och sedan samlingen kommer att bli inaktuell efter kvarhållningsperioden och kan tas bort. 
* Om en användare har inaktiverat synkronisera inställningar på sina enheter, inga inställningsdata kommer att komma åt och alla inställningsdata för den användaren blir inaktuella och kan tas bort efter kvarhållningsperioden. 
* Om Azure AD directory admin inaktiverar Enterprise tillstånd centrala för hela katalogen, sedan alla användare i den directory stoppas synkroniserar inställningar och inställningsdata för alla för alla användare blir inaktuella och kan tas bort efter kvarhållningsperioden. 

**Ta bort dataåterställning**: databevarandeprincip kan inte konfigureras. När data togs bort permanent, kommer det inte att återställa. Det är dock viktigt att notera att inställningsdata endast tas bort från Azure, inte slutanvändarens enhet. Om alla enheter senare återansluts till tjänsten Enterprise tillstånd Roaming, kommer igen inställningarna synkroniseras och lagras i Azure.

## <a name="related-topics"></a>Relaterade ämnen
* [Företagsroaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Inställningar och dataroaming vanliga frågor och svar](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Inställningar för princip och MDM för inställningar för synkronisering](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Centrala referens för Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Felsökning](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
