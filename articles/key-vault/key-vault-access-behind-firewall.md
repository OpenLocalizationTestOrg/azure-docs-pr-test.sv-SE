---
title: "aaaAccess Key Vault bakom en brandvägg | Microsoft Docs"
description: "Lär dig hur tooaccess Azure nyckeln valvet från ett program som finns bakom en brandvägg"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 50d21774-2ee1-4212-8995-570c9de603c5
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: ab4bb0c27a41fef878a20dace6cab203df04e579
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Få åtkomst till Azure Key Vault bakom en brandvägg
### <a name="q-my-key-vault-client-application-needs-toobe-behind-a-firewall-what-ports-hosts-or-ip-addresses-should-i-open-tooenable-access-tooa-key-vault"></a>F: min nyckelvalv klientprogrammet måste toobe bakom en brandvägg. Vilka portar, värdar eller IP-adresser ska jag öppnar åtkomst tooenable tooa nyckelvalv?
tooaccess ett nyckelvalv nyckelvalv klientprogrammet har tooaccess flera slutpunkter för olika funktioner:

* Autentisering via Azure Active Directory (Azure AD).
* Hantering av Azure Key Vault. Detta omfattar att skapa, läsa, uppdatera, ta bort och ange åtkomstprinciper via Azure Resource Manager.
* Komma åt och hantera objekt (nycklar och hemligheter) som lagras i Nyckelvalvet sig själv, gå igenom hello Key Vault-specifika slutpunkt (till exempel [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Det finns vissa varianter beroende på din konfiguration och miljö.   

## <a name="ports"></a>Portar
Alla trafik tooa nyckelvalv för alla tre funktioner (autentisering, hantering och plan åtkomst) går över HTTPS: port 443. Det är dock ibland HTTP (port 80)-trafik för CRL. Klienter som har stöd för OCSP ska inte nå CRL, men kan ibland nå [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Autentisering
Nyckelvalv klientprogram behöver tooaccess slutpunkter i Azure Active Directory för autentisering. hello-slutpunkt som används beror på hello Azure AD-klientkonfiguration, hello typ av huvudnamn (UPN eller tjänstens huvudnamn) och hello typ av konto – till exempel ett Microsoft-konto eller ett arbets- eller skolkonto.  

| Typ av huvudkonto | Slutpunkt:port |
| --- | --- |
| Användare som använder Microsoft-konto<br> (till exempel user@hotmail.com) |**Globalt:**<br> login.microsoftonline.com:443<br><br> **Azure i Kina:**<br> login.chinacloudapi.cn:443<br><br>**Azure för amerikanska myndigheter:**<br> login-us.microsoftonline.com:443<br><br>**Azure i Tyskland:**<br> login.microsoftonline.de:443<br><br> och <br>login.live.com:443 |
| Användaren eller tjänstens huvudnamn med ett arbets- eller skolkonto med Azure AD (till exempel user@contoso.com) |**Globalt:**<br> login.microsoftonline.com:443<br><br> **Azure i Kina:**<br> login.chinacloudapi.cn:443<br><br>**Azure för amerikanska myndigheter:**<br> login-us.microsoftonline.com:443<br><br>**Azure i Tyskland:**<br> login.microsoftonline.de:443 |
| Användaren eller tjänstens huvudnamn med hjälp av en arbets- eller skolkonto, plus Active Directory Federation Services (AD FS) eller andra externa slutpunkten (till exempel user@contoso.com) |Alla slutpunkter för ett arbets- eller skolkonto, plus AD FS eller andra federerade slutpunkter |

Det finns andra möjliga avancerade scenarier. Se för[Azure Active Directory Authentication flöda](/documentation/articles/active-directory-authentication-scenarios/), [integrera program med Azure Active Directory](/documentation/articles/active-directory-integrating-applications/), och [Active Directory-autentiseringsprotokoll](https://msdn.microsoft.com/library/azure/dn151124.aspx) för ytterligare information.  

## <a name="key-vault-management"></a>Hantering av Nyckelvalv
För Key Vault-hantering (CRUD och principen för access), måste klientprogrammet för hello nyckelvalv tooaccess en Azure Resource Manager-slutpunkt.  

| Typ av åtgärd | Slutpunkt:port |
| --- | --- |
| Kontrollplanåtgärder för Nyckelvalv<br> via Azure Resource Manager |**Globalt:**<br> management.azure.com:443<br><br> **Azure i Kina:**<br> management.chinacloudapi.cn:443<br><br> **Azure för amerikanska myndigheter:**<br> management.usgovcloudapi.net:443<br><br> **Azure i Tyskland:**<br> management.microsoftazure.de:443 |
| Azure Active Directory Graph API |**Globalt:**<br> graph.windows.net:443<br><br> **Azure i Kina:**<br> graph.chinacloudapi.cn:443<br><br> **Azure för amerikanska myndigheter:**<br> graph.windows.net:443<br><br> **Azure i Tyskland:**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Åtgärder i Nyckelvalv
För alla nyckelvalv objekt (nycklar och hemligheter) hantering och kryptografiska åtgärder behöver hello nyckelvalv klienten tooaccess hello nyckelvalv slutpunkt. hello endpoint DNS-suffix varierar beroende på hello platsen för nyckelvalvet. Hej nyckelvalv slutpunkt har hello format *valvnamnet*. *region-specifika-dns-suffixet*, enligt beskrivningen i följande tabell hello.  

| Typ av åtgärd | Slutpunkt:port |
| --- | --- |
| Åtgärder inklusive kryptografiska åtgärder på nycklar, skapa, läsa, uppdatera och ta bort nycklar och hemligheter, ställa in eller få taggar och andra attribut för nyckelvalvobjekt (nycklar eller hemligheter) |**Globalt:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure i Kina:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure för amerikanska myndigheter:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure i Tyskland:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-adressintervall
hello Key Vault-tjänsten använder andra Azure-resurser som PaaS-infrastruktur. Så det inte är möjligt tooprovide en specifik IP-adressintervall som Key Vault slutpunkter har vid en viss tidpunkt. Om brandväggen stöder endast IP-adressintervall, se toohello [IP-intervall för Microsoft Azure-Datacenter](https://www.microsoft.com/download/details.aspx?id=41653) dokumentet. För autentisering och identitet (Azure Active Directory), måste programmet vara kan tooconnect toohello slutpunkter som beskrivs i [autentisering och identitet adresser](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Nästa steg
Om du har frågor om Key Vault finns hello [Azure Key Vault-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

