---
title: "aaaPublishing program på separata nätverk och platser med hjälp av kopplingen grupper i Azure AD App Proxy | Microsoft Docs"
description: Omfattar hur toocreate och hantera grupper av kopplingar i Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 8c9a84b365eab28eaaeb343d4d1e2e6990537fec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publicera program på separata nätverk och platser med hjälp av connector-grupper
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-application-proxy-connectors.md)
>

Kunder kan använda Azure AD Application Proxy för fler och fler scenarier och program. Så att vi har gjort App Proxy ännu mer flexibla genom att aktivera flera topologier. Du kan skapa Application Proxy connector grupper så att du kan tilldela specifika kopplingar tooserve specifika program. Den här funktionen ger mer kontroll och sätt toooptimize Application Proxy-distribution. 

Varje Application Proxy connector tilldelas tooa connector grupp. Alla hello kopplingar som tillhör samma grupp av kopplingen fungerar som en separat enhet för hög tillgänglighet och belastningsutjämning toohello. Alla kopplingar hör tooa connector grupp. Om du inte skapa grupper, är alla kopplingar i en standardgrupp. Administratören kan skapa nya grupper och tilldela kopplingar toothem i hello Azure-portalen. 

Alla program som har tilldelats tooa connector grupp. Om du inte skapa grupper, har alla program tilldelats tooa standardgruppen. Men du kan ange varje program toowork med en viss koppling grupp om du organisera dina kontakter i grupper. I det här fallet fungerar endast hello kopplingar i gruppen hello-programmet på begäran. Den här funktionen är användbart om dina program som finns på olika platser. Du kan skapa kopplingen grupper baserat på plats, så att program hanteras alltid av kopplingar som är fysiskt Stäng toothem.

>[!TIP] 
>Om du har en stor Application Proxy-distribution kan inte tilldela alla program toohello connector standardgruppen. På så sätt kan nya anslutningar får inte någon live trafik förrän du tilldelar tooan active kopplingen grupp. Den här konfigurationen kan du också tooput kopplingar i ett inaktivt tillstånd genom att flytta dem tillbaka toohello standardgruppen, så att du kan utföra underhåll utan att påverka dina användare.

## <a name="prerequisites"></a>Krav
toogroup kopplingar, du har toomake att du [installerat flera kopplingar](active-directory-application-proxy-enable.md). När du installerar en ny koppling den automatiskt ansluter hello **standard** connector grupp.

## <a name="create-connector-groups"></a>Skapa grupper för anslutningstjänsten
Använd dessa steg toocreate så många connector-grupper som du vill använda. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
1. Välj **Azure Active Directory** > **företagsprogram** > **programproxy**.
2. Välj **ny koppling grupp**. hello ny koppling grupp bladet visas.

   ![Välj den nya kopplingen gruppen](./media/active-directory-application-proxy-connectors-azure-portal/new-group.png)

3. Namnge din nya grupp koppling och sedan använda hello nedrullningsbara menyn tooselect där kopplingar som ingår i den här gruppen.
4. Välj **Spara**.

## <a name="assign-applications-tooyour-connector-groups"></a>Tilldela program tooyour connector grupper
Följ dessa steg för varje program som du har publicerat med Application Proxy. Du kan tilldela en programgrupp tooa connector när du publicerar den eller när du vill kan du använda dessa steg toochange hello tilldelning.   

1. Hej hanteringspanel för din katalog, Välj **företagsprogram** > **alla program** > Hej program som du vill tooassign tooa connector grupp >  **Programproxy**.
2. Använd hello **Connector grupp** nedrullningsbara menyn tooselect hello grupp som du vill hello programmet toouse.
3. Välj **spara** tooapply hello ändringen.

## <a name="use-cases-for-connector-groups"></a>Användningsfall för koppling grupper 

Kopplingen grupper är användbara för olika scenarier, inklusive:

### <a name="sites-with-multiple-interconnected-datacenters"></a>Webbplatser med flera sammankopplade Datacenter

Många organisationer har ett antal sammankopplade datacenter. I detta fall vill du tookeep så mycket trafik i hello datacenter som möjligt eftersom länkar mellan datacenter är dyrt och långsamt. Du kan distribuera kopplingar i varje datacenter tooserve endast hello program som finns inom hello datacenter. Den här metoden minimerar länkar mellan datacenter och ger en helt transparent upplevelse tooyour användare.

### <a name="applications-installed-on-isolated-networks"></a>Installerade program på isolerade nätverk

Program kan finnas i nätverk som inte är en del av hello huvudsakliga företagsnätverket. Du kan använda anslutningstjänsten grupper tooinstall dedikerad kopplingar på isolerade nätverk tooalso isolera program toohello nätverk. Detta inträffar vanligtvis när en utomstående leverantör upprätthåller ett visst program för din organisation. 

Kopplingen grupper kan du tooinstall dedikerad kopplingar för nätverken som publicerar endast specifika program, vilket gör det enklare och mer säker toooutsource programhantering toothird leverantörer.

### <a name="applications-installed-on-iaas"></a>Installerade program på IaaS 

För program som installerats på IaaS för molnåtkomst anger connector grupper du en gemensam toosecure hello åtkomst tooall hello-appar. Kopplingen grupper inte skapa ytterligare beroende på företagets nätverk eller Fragmentera hello app upplevelse. Kopplingar kan installeras på varje molndatacenter och hantera program som finns i nätverket. Du kan installera flera kopplingar tooachieve hög tillgänglighet.

Ta ett exempel som en organisation som har flera virtuella datorer är anslutna till tootheir egna IaaS värdbaserade virtuella. tooallow anställda toouse dessa program dessa privata nätverk är anslutna toohello företagsnätverket med hjälp av plats-till-plats-VPN. Detta ger en bra upplevelse för medarbetare som är belägna i företagets lokaler. Men det kanske inte är optimal för fjärranslutna anställda, eftersom den kräver ytterligare lokala infrastruktur tooroute åtkomst som du ser i hello diagrammet nedan:

![AzureAD Iaas-nätverk](./media/application-proxy-publish-apps-separate-networks/application-proxy-iaas-network.png)
  
Du kan aktivera en gemensam toosecure hello åtkomst tooall tjänstprogram med Azure AD Application Proxy connector grupper utan att skapa ytterligare beroende på företagets nätverk:

![AzureAD Iaas flera molnet leverantörer](./media/application-proxy-publish-apps-separate-networks/application-proxy-multiple-cloud-vendors.png)

### <a name="multi-forest--different-connector-groups-for-each-forest"></a>Flera skogar – olika connector grupper för varje skog

De flesta kunder som har distribuerat Application Proxy använder dess single-sign-on (SSO) funktioner genom att utföra Kerberos-begränsad delegering (KCD). tooachieve detta hello connector datorer behöver toobe kopplade tooa domän som kan delegera hello användare mot hello program. KCD stöder funktioner för mellan skogar. Men för företag som har olika miljöer med flera skogar utan förtroende mellan dem, en enskild koppling kan inte användas för alla skogar. 

I det här fallet specifika kopplingar kan distribueras per skog och ange tooserve program som var publicerade tooserve hello endast användare av den specifika skogen. Varje koppling grupp representerar en annan skog. Medan hello klient och de flesta av hello finns unified för alla skogar, kan användare tilldelas tootheir skog program med Azure AD-grupper.
 
### <a name="disaster-recovery-sites"></a>Katastrofåterställningsplatser

Det finns två olika metoder som du kan använda med en plats för disaster recovery (DR), beroende på hur dina platser implementeras:

* Om DR-plats som är inbyggd i aktivt-aktivt läge där den precis som hello huvudwebbplatsen och har hello samma nätverk och AD-inställningar, du kan skapa hello kopplingar på hello DR-plats i hello samma connector-grupp som hello huvudwebbplatsen. Detta gör att Azure AD toodetect redundans för dig.
* Om webbplatsen DR skiljer sig från hello huvudwebbplatsen, du kan skapa en annan koppling grupp i hello DR-plats, och antingen 1) har säkerhetskopieringsprogram eller omdirigera hello befintliga toohello DR connector programgruppen 2) manuellt vid behov.
 
### <a name="serve-multiple-companies-from-a-single-tenant"></a>Hantera flera företag från en enda klient

Det finns många olika sätt tooimplement en modell där en enda leverantör distribuerar och underhåller Azure AD-relaterade tjänster för flera företag. Kopplingen grupper kan Hej administratör särskilja hello kopplingar och program till olika grupper. Ett sätt som är lämplig för små företag, är toohave ett enda Azure AD-klient medan hello olika företag har sina egna domännamn och nätverk. Detta gäller även för M & A scenarier och situationer där en enskild IT-avdelning hanterar flera företag reglerande eller affärsmässiga skäl. 

## <a name="sample-configurations"></a>Exempel konfigurationer

Vissa exempel som du kan implementera är hello efter anslutningen grupper.
 
### <a name="default-configuration--no-use-for-connector-groups"></a>Standardkonfigurationen – ingen användning för koppling grupper

Om du inte använder connector grupper, konfigurationen skulle se ut så här:

![AzureAD inga Connector-grupper](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-1.png)
 
Den här konfigurationen är tillräcklig för mindre distributioner och tester. Den fungerar också bra om din organisation har en platt nätverkstopologi.
 
### <a name="default-configuration-and-an-isolated-network"></a>Standardkonfigurationen och ett isolerat nätverk

Den här konfigurationen är en utveckling av hello standardversionen, där det finns en viss app som körs i ett isolerat nätverk, till exempel IaaS virtuellt nätverk: 

![AzureAD inga Connector-grupper](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-2.png)
 
### <a name="recommended-configuration--several-specific-groups-and-a-default-group-for-idle"></a>Rekommenderad konfiguration – flera specifika grupper och en standardgrupp för inaktiv

hello Rekommenderad konfiguration för stora och komplexa organisationer är toohave hello standardgruppen koppling som en grupp som inte fungerar för alla program och används för inaktiva eller nyligen installerade kopplingar. Alla program som hanteras med hjälp av anpassade connector grupper. Detta gör att alla hello komplexitet hello-scenarier som beskrivs ovan.

I hello exemplet nedan har hello företaget två datacenter, A och B, med två kopplingar som fungerar varje plats. Varje plats har olika program som körs på den. 

![AzureAD inga Connector-grupper](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-3.png)
 
## <a name="next-steps"></a>Nästa steg

* [Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
* [Aktivera enkel inloggning](application-proxy-sso-overview.md)


