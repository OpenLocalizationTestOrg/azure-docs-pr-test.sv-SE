---
title: "aaaEnable Enterprise tillstånd centrala i Azure Active Directory | Microsoft Docs"
description: "Vanliga frågor om Enterprise tillstånd centrala inställningar i Windows-enheter. Enterprise tillstånd centrala ger användarna en enhetlig miljö på sina Windows-enheter och minskar hello tid som behövs för att konfigurera en ny enhet."
services: active-directory
keywords: "företagsroaming, windows molnet, hur tooenable enterprise tillstånd nätverksväxling"
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
ms.openlocfilehash: c69a9cfa8055f418a3b81c62939885d5bec8a6f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Aktivera enterprise tillståndsväxling i Azure Active Directory
Enterprise tillstånd växling är tillgängliga tooany organisation med en prenumeration för Premium Azure Active Directory (AD Azure). Mer information om hur tooget en Azure AD-prenumeration finns hello [produktsida för Azure AD](https://azure.microsoft.com/services/active-directory).

När du aktiverar Enterprise tillstånd centrala beviljas organisationen automatiskt licenser för en kostnadsfri prenumeration som är begränsad användning tooAzure Rights Management. Den här kostnadsfria prenumerationen är begränsad tooencrypting och dekryptera Företagsinställningar och programdata synkroniseras av hello Enterprise tillstånd Roaming-tjänst. Du måste ha en betald prenumeration toouse hello alla funktioner i Azure Rights Management.

När du har fått en Premium Azure AD-prenumeration, följer du dessa steg tooenable Enterprise tillstånd nätverksväxling:

1. Inloggningen toohello klassiska Azure-portalen.
2. Hello vänster markerar **ACTIVE DIRECTORY**, och välj sedan hello katalog som du vill tooenable Enterprise tillstånd nätverksväxling.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Gå toohello **konfigurera** fliken hello längst upp.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4. Bläddra nedåt hello sida och välj **användarna kan synkronisera inställningar och företagets APPDATA**, och klicka sedan på **spara**.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

För en Windows 10 tooroam Enhetsinställningar med hello Enterprise tillstånd centrala service, måste hello enheten autentisera med hjälp av en Azure AD-identitet. För enheter som är kopplade tooAzure AD är hello användarens primära inloggningen hello Azure AD identity så krävs ingen ytterligare konfiguration. För enheter som använder en traditionella lokala Active Directory hello IT-administratören måste [ansluta hello domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Synkronisera datalagring
Enterprise tillstånd centrala data finns i en eller flera [Azure-regioner](https://azure.microsoft.com/regions/) som bäst överensstämmer med hello land/region värdet i hello Azure Active Directory-instans. Enterprise tillstånd centrala data är partitionerad baserat på tre huvudsakliga geografiska områden: Nordamerika, EMEA och APAC. Enterprise tillstånd centrala data för hello klient finns lokalt med hello geografisk region och replikeras inte över regioner.  Till exempel finns kunder som har sina land/region värdet set tooone EMEA-länder som ”Frankrike” eller ”Zambia” måste ha sina data i ett eller av hello Azure-regioner inom Europa.  Kunder som värdet sina land/region i Azure AD tooone för Nordamerika länder som ”USA” eller ”Kanada” har sina data som finns i en eller flera av hello Azure-regioner inom hello USA.  Kunder som värdet sina land/region i Azure AD tooone APAC länder som ”Australien” eller ”Nya Zeeland” måste ha sina data som finns i en eller flera av hello Azure-regioner i Asien.  Syd American länder och Antarktis data ska finnas i en eller flera Azure-regioner inom hello USA.  hello land/region värdet har angetts som en del av hello processen för Azure AD-katalogen och kan inte ändras senare. 

Om du behöver mer information om plats för datalagring, kontrollera filen en biljett hos [Azure-supporten](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Hantera Enterprise tillstånd nätverksväxling
Azure AD globala administratörer kan aktivera och inaktivera Enterprise tillstånd centrala i hello klassiska Azure-portalen.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globala administratörer kan begränsa inställningar sync toospecific säkerhetsgrupper.

Globala administratörer kan också visa en statusrapport för per användare enheten synkronisering genom att välja en viss användare i Active Directory-instans för hello **användare** listan och klicka på **enheter** fliken och väljer Visa **Enheter som synkroniserar inställningar och företagets AppData**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

## <a name="data-retention"></a>Datakvarhållning
Data synkroniseras tooAzure via Enterprise tillstånd centrala kommer att sparas på obestämd tid om inte en manuell borttagningsåtgärd utförs eller hello data i fråga är definieras toobe inaktuella. 

**Explicit borttagning:** hello data tas bort när en Azure-administratör tar bort en användare eller en katalog eller administratör uttryckligen begär att data är toobe tas bort.

* **Ta bort användaren**: när en användare tas bort i Azure AD hello användarkonto centrala data kommer att markeras för borttagning och tas bort 90 too180 dagar. 
* **Ta bort katalogen**: ta bort en hel katalog i Azure AD är en omedelbar åtgärd. Alla hello inställningsdata associerade med directory markeras för borttagning och kommer att tas bort 90 dagar i too180. 
* **På begäran om borttagning**: om hello Azure AD-administratör vill toomanually tar bort data eller inställningsdata för en viss användare, Hej administratör kan filen en biljett hos [Azure-supporten](https://azure.microsoft.com/support/). 

**Inaktuella data tas bort**: Data som inte har öppnats för ett år (”Hej kvarhållningsperiod”) kommer att behandlas som föråldrade och kan tas bort från Azure. hello kvarhållningsperiod är ämne toochange men inte mindre än 90 dagar. hello inaktuella data kan vara en specifik uppsättning inställningar för Windows-program eller alla inställningar för en användare. Exempel:

* Om inga enheter åtkomst till en samling med särskilda inställningar (t.ex. Om ett program tas bort från hello enhet eller en grupp inställningar, till exempel ”tema” är inaktiverad för alla enheter som en användare), och sedan samlingen blir inaktuella efter hello Bevarandeperiod och kan vara ta bort. 
* Om en användare har inaktiverat synkronisera inställningar på sina enheter, ingen hälsningspaket inställningsdata kommer att komma åt och alla hello inställningsdata för den användaren blir inaktuella och kan tas bort efter kvarhållningsperioden hello. 
* Om hello Azure AD directory admin inaktiverar Enterprise tillstånd centrala för hello hela katalogen sedan alla användare i den directory stoppas synkroniserar inställningar och inställningsdata för alla för alla användare blir inaktuella och kan tas bort efter kvarhållningsperioden hello. 

**Ta bort dataåterställning**: hello databevarandeprincip kan inte konfigureras. När hello data har tagits bort permanent kan den inte återställas. Det är dock viktigt toonote som hello inställningsdata endast tas bort från Azure, inte hello slutanvändarens enhet. Om alla enheter återansluts senare toohello Enterprise tillstånd centrala service, kommer igen hello inställningar synkroniseras och lagras i Azure.

## <a name="related-topics"></a>Relaterade ämnen
* [Företagsroaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Inställningar och dataroaming vanliga frågor och svar](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Inställningar för princip och MDM för inställningar för synkronisering](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Centrala referens för Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Felsökning](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
