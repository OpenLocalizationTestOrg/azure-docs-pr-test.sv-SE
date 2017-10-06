---
title: "aaaAzure API Management vanliga frågor och svar | Microsoft Docs"
description: "Lär dig hello svar toocommon frågor, mönster och bästa praxis i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 9e7cdf1b881a4dfed4bd2cfd7fbb4994f48b5f79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-faqs"></a>Azure API Management vanliga frågor och svar
Hämta hello svar toocommon frågor, mönster och bästa praxis för Azure API Management.

## <a name="contact-us"></a>Kontakta oss
* [Hur kan jag fråga hello Microsoft Azure API Management-teamet?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)


## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
* [Vad innebär det när en funktion i förhandsversionen?](#what-does-it-mean-when-a-feature-is-in-preview)
* [Hur kan jag skydda hello anslutningen mellan hello API Management gateway och mitt backend-tjänster?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [Hur kopierar min API Management service-instans tooa ny instans?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [Kan jag hantera min API Management-instans genom programmering?](#can-i-manage-my-api-management-instance-programmatically)
* [Hur lägger jag till en användargrupp toohello administratörer?](#how-do-i-add-a-user-to-the-administrators-group)
* [Varför är hello princip som jag vill tooadd inte tillgänglig i Redigeraren för hello?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [Hur använder API versionshantering i API Management?](#how-do-i-use-api-versioning-in-api-management)
* [Hur ställer jag in flera miljöer i ett enda API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [Kan jag använda SOAP med API Management?](#can-i-use-soap-with-api-management)
* [Är hello API Management gateway IP-adress konstant? Kan jag använda den i brandväggsregler?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
* [Kan jag konfigurera en server för OAuth 2.0-auktorisering med AD FS-säkerhet?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
* [Vilka routningsmetoden använder API-hantering i distributioner toomultiple geografiska platser?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [Kan jag använda en Azure Resource Manager mallen toocreate en instans för API Management-tjänsten?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Kan jag använda ett självsignerat certifikat för SSL för en serverdel?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
* [Varför visas ett autentiseringsfel när jag försöker tooclone en GIT-lagringsplats?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [Fungerar API-hantering med Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
* [Varför behöver vi en dedikerad undernät i hanteraren för filserverresurser style Vnet när API Management har distribuerats till dem?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [Vad är hello undernät minsta storlek som krävs vid distribution av API-hantering i ett virtuellt nätverk?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [Kan jag flytta en API Management-tjänst från en prenumeration tooanother?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [Finns det begränsningar eller kända problem med att importera min API?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-hello-microsoft-azure-api-management-team-a-question"></a>Hur kan jag fråga hello Microsoft Azure API Management-teamet?
Du kan kontakta oss genom att använda något av följande alternativ:

* Dina frågor i våra [API Management MSDN-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
* Skicka ett e-postmeddelande för<mailto:apimgmt@microsoft.com>.
* Skicka en funktionsbegäran i hello [Azure Feedbackforum](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Vad innebär det när en funktion i förhandsversionen?
När det är en funktion i förhandsversionen, innebär det att vi aktivt som begär feedback på hur hello-funktionen fungerar för dig. En funktion i förhandsversionen är funktionellt klar, men det är möjligt att vi ska skapa en ny ändring i svaret toocustomer feedback. Vi rekommenderar att du inte beroende av en funktion i förhandsversionen i produktionsmiljön. Om du har feedback på förhandsgranskningsfunktioner berätta genom en hello Kontaktalternativ i [hur kan jag fråga hello Microsoft Azure API Management-teamet?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-hello-connection-between-hello-api-management-gateway-and-my-back-end-services"></a>Hur kan jag skydda hello anslutningen mellan hello API Management gateway och mitt backend-tjänster?
Du har flera alternativ toosecure hello anslutning mellan hello API Management gateway och backend-tjänster. Du kan:

* Använd grundläggande HTTP-autentisering. Mer information finns i [konfigurera API-inställningarna](api-management-howto-create-apis.md#configure-api-settings).
* Använda SSL ömsesidig autentisering som beskrivs i [hur toosecure backend-tjänster med hjälp av förautentisering av klientcertifikat i Azure API Management](api-management-howto-mutual-certificates.md).
* Använd IP-vitlistning på backend-tjänst. Om du har en Standard eller Premium-nivån API Management instans konstant hello IP-adressen för hello gateway. Du kan ange godkända-tooallow denna IP-adress. Du kan hämta hello IP-adressen för din API Management-instans på hello instrumentpanelen i hello Azure-portalen.
* Anslut din API Management instans tooan Azure Virtual Network.

### <a name="how-do-i-copy-my-api-management-service-instance-tooa-new-instance"></a>Hur kopierar min API Management service-instans tooa ny instans?
Om du vill toocopy en ny instans för API Management instans tooa har du flera alternativ. Du kan:

* Använd hello säkerhetskopiering och återställning funktion i API-hantering. Mer information finns i [hur tooimplement katastrofåterställning genom att använda tjänsten säkerhetskopiering och återställning på Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
* Skapa din egen säkerhetskopia och återställa funktionen med hjälp av hello [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Använd hello REST API toosave och återställa hello entiteter från hello service-instans som du vill.
* Hämta hello tjänstkonfiguration med hjälp av Git och sedan ladda upp den nya tooa-instansen. Mer information finns i [hur toosave och konfigurera din konfiguration för API Management-tjänsten med hjälp av Git](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Kan jag hantera min API Management-instans genom programmering?
Ja, kan du hantera API-hantering via programmering med hjälp av:

* Hej [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
* Hej [Microsoft Azure ApiManagement Service Management biblioteket SDK](http://aka.ms/apimsdk).
* Hej [Tjänstedistributionen](https://msdn.microsoft.com/library/mt619282.aspx) och [Service management](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell-cmdlets.

### <a name="how-do-i-add-a-user-toohello-administrators-group"></a>Hur lägger jag till en användargrupp toohello administratörer?
Här är hur du kan lägga till en användargrupp toohello administratörer:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Gå toohello resursgrupp som har hello API Management-instans som du vill tooupdate.
3. I API-hantering, tilldela hello **Api Management deltagare** rollen toohello användare.

Nu hello nyligen tillagda deltagare kan använda Azure PowerShell [cmdlets](https://msdn.microsoft.com/library/mt613507.aspx). Här är hur toosign i som administratör:

1. Använd hello `Login-AzureRmAccount` cmdlet toosign i.
2. Ange hello kontexten toohello prenumeration som har hello service med hjälp av `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Få en URL för enkel inloggning med hjälp av `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Använda hello URL tooaccess hello administrationsportalen.

### <a name="why-is-hello-policy-that-i-want-tooadd-unavailable-in-hello-policy-editor"></a>Varför är hello princip som jag vill tooadd inte tillgänglig i Redigeraren för hello?
Om hello-principen som du vill tooadd visas nedtonat eller skuggade i Redigeraren för hello till att du är i hello rätt omfattning för hello principen. Varje Principframställning är utformad för toouse specifika omfattningar och principen avsnitt. tooreview hello princip avsnitt och scope för en princip finns hello princip användning i avsnittet [API Management-principer](https://msdn.microsoft.com/library/azure/dn894080.aspx).

### <a name="how-do-i-use-api-versioning-in-api-management"></a>Hur använder API versionshantering i API Management?
Du har några alternativ toouse API versionshantering i API-hantering:

* Du kan konfigurera olika versioner av API: er toorepresent i API-hantering. Du kan till exempel ha två olika API: er, MyAPIv1 och MyAPIv2. En utvecklare kan välja hello-version som hello utvecklare vill toouse.
* Du kan också konfigurera din API med en URL som inte innehåller en versionssegment, till exempel https://my.api. Konfigurera sedan ett versionssegment på varje åtgärd [omarbetning URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) mall. Du kan till exempel ha en åtgärd med en [URL mallen](api-management-howto-add-operations.md#url-template) kallas /resource och en [omarbetning URL](api-management-howto-add-operations.md#rewrite-url-template) mallen kallas v1-resurs. Du kan ändra värdet för hello segment separat för varje åtgärd.
* Om du vill att tookeep ett ”default” versionssegment i hello API: er tjänst-URL i valda åtgärder, ange en princip som använder hello [ange serverdelstjänst](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) toochange hello sökväg till backend-begäran.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Hur ställer jag in flera miljöer i ett enda API?
tooset in flera miljöer, till exempel en testmiljö och en produktionsmiljö, i ett enda API har du två alternativ. Du kan:

* Värden olika API: er i hello samma klientorganisation.
* Värden hello samma API: er på olika klienter.

### <a name="can-i-use-soap-with-api-management"></a>Kan jag använda SOAP med API Management?
[SOAP-direkt](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) stöd är nu tillgänglig. Administratörer kan importera hello WSDL av deras SOAP-tjänst och Azure API Management skapar en SOAP-klientdel. Portalen utvecklardokumentationen, test-konsolen, principer och analytics är tillgängliga för SOAP-tjänster.

### <a name="is-hello-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Är hello API Management gateway IP-adress konstant? Kan jag använda den i brandväggsregler?
Vid hello Standard och Premium nivåer är hello offentliga IP-adress (VIP) hello API Management-klienten statisk under hello livstid hello-klient med vissa undantag. hello IP-adress ändras under dessa omständigheter:

* hello tjänsten bort och sedan återskapas.
* Hej tjänstprenumeration är [avbruten](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) eller [varnad](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (t.ex, för nonpayment) och sedan återupprättas.
* Du lägger till eller ta bort Azure Virtual Network (du kan använda virtuella nätverk endast på hello utvecklare och Premium-nivån).

Hej regionala adress ändras för distribution av flera regioner om hello region vacated och sedan återupprättas (du kan använda flera regioner distribution endast på hello Premium-nivån).

Premium-nivån klienter som är konfigurerade för distribution av flera regioner har tilldelats en offentlig IP-adress per region.

Du kan hämta IP-adressen (eller adresserna i en distribution med flera regioner) på hello klient sida i hello Azure-portalen.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Kan jag konfigurera en server för OAuth 2.0-auktorisering med AD FS-säkerhet?
hur tooconfigure en OAuth 2.0-auktorisering server med Active Directory Federation Services (AD FS) säkerhet, se toolearn [med hjälp av AD FS i API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-toomultiple-geographic-locations"></a>Vilka routningsmetoden använder API-hantering i distributioner toomultiple geografiska platser?
API Management används hello [prestanda trafikroutningsmetoden](../traffic-manager/traffic-manager-routing-methods.md#priority) i distributioner toomultiple geografiska platser. Inkommande trafik är routade toohello närmaste API-gateway. Om en region frånkopplas är inkommande trafik dirigeras automatiskt toohello nästa närmaste gateway. Mer information om metoder i [routningsmetoder för Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-toocreate-an-api-management-service-instance"></a>Kan jag använda en Azure Resource Manager mallen toocreate en instans för API Management-tjänsten?
Ja. Se hello [Azure API Management-tjänsten](http://aka.ms/apimtemplate) snabbstartsmallar.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Kan jag använda ett självsignerat certifikat för SSL för en serverdel?
Ja. Här är hur toouse en självsignerat Secure Sockets Layer (SSL)-certifikatet för en serverdel:

1. Skapa en [Backend](https://msdn.microsoft.com/library/azure/dn935030.aspx) entiteten med hjälp av API-hantering.
2. Ange hello **skipCertificateChainValidation** egenskapen för**SANT**.
3. Om du inte längre vill tooallow självsignerade certifikat, ta bort hello Backend entitet eller ange hello **skipCertificateChainValidation** egenskapen för**FALSKT**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-tooclone-a-git-repository"></a>Varför visas ett autentiseringsfel när jag försöker tooclone en Git-lagringsplats?
Om du använder Git Autentiseringshanteraren, eller om du försöker tooclone en Git-lagringsplats med hjälp av Visual Studio, kan som uppstå ett känt problem med hello dialogrutan för Windows-autentiseringsuppgifter. hello dialogrutan begränsar längd för lösenord too127 tecken och trunkeras hello Microsoft genererat lösenord. Vi arbetar på förkorta hello lösenord. För tillfället, Använd Git Bash tooclone Git-lagringsplatsen.

### <a name="does-api-management-work-with-azure-expressroute"></a>Fungerar API-hantering med Azure ExpressRoute?
Ja. API Management fungerar med Azure ExpressRoute.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>Varför behöver vi en dedikerad undernät i hanteraren för filserverresurser style Vnet när API Management har distribuerats till dem?
hello dedikerade undernät krav för API-hantering kommer från hello fakta som bygger på modell-distribution (PAAS V1 layer). Vi kan distribuera till Resource Manager-VNET (V2 layer), finns men det konsekvenser toothat. hello klassiska distributionsmodellen i Azure inte är direkt kopplade till hello Resource Manager-modellen, så om du skapar en resurs i V2 layer, hello V1 layer inte känner till den och problem kan inträffa, till exempel API-hantering och försöker toouse en IP-adress som redan har allokerats tooa NIC (bygger på V2).
Läs mer om skillnaden mellan klassisk och Resource Manager modeller i Azure för toolearn[skillnad i distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md).

### <a name="what-is-hello-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>Vad är hello undernät minsta storlek som krävs vid distribution av API-hantering i ett virtuellt nätverk?
hello minsta undernätets storlek behövs toodeploy API-hantering är [/29](../virtual-network/virtual-networks-faq.md#configuration), vilket är hello minsta undernätets storlek som har stöd för Azure.

### <a name="can-i-move-an-api-management-service-from-one-subscription-tooanother"></a>Kan jag flytta en API Management-tjänst från en prenumeration tooanother?
Ja. toolearn finns i avsnittet [flytta resurser tooa ny resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md).

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>Finns det begränsningar eller kända problem med att importera min API?
[Kända problem och begränsningar](api-management-api-import-restrictions.md) öppna API(Swagger) WSDL och WADL format.
