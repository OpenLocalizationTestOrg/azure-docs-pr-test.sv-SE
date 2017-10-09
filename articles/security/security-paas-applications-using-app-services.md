---
title: aaaSecuring PaaS webb- och mobila program med Azure App Service | Microsoft Docs
description: " Lär dig mer om Azure App Service säkerhetsmetoder för att skydda din PaaS webb- och mobilprogram. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: terrylan
ms.openlocfilehash: 68a741c668efe2157cff114b649e0e287ef196f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-services"></a>Att säkra PaaS-webb- och mobila program med Azure App Service

I den här artikeln tar vi upp en samling [Azure App Service](https://azure.microsoft.com/services/app-service/) säkerhetsmetoder för att skydda din PaaS webb- och mobilprogram. Följande rekommendationer härleds från våra erfarenhet av Azure och hello erfarenheter från kunder som dig själv.

## <a name="azure-app-services"></a>Azure Apptjänster
[Azure Apptjänst](../app-service/app-service-value-prop-what-is.md) är en PaaS erbjudande som låter dig skapa webb- och mobilappar för alla plattformar och enheter och Anslut toodata var som helst, i hello molnet eller lokalt. Apptjänster innehåller hello webb- och mobilfunktioner som har tidigare levererat separat som Azure Websites och Azure Mobile Services. Det finns nya funktioner för att automatisera affärsprocesser och hantera moln-API:er. Som en enda integrerad tjänst ger Apptjänster en omfattande uppsättning tooweb-, mobil- och integrering scenarier.

toolearn fler finns översikter på [Mobilappar](../app-service-mobile/app-service-mobile-value-prop.md) och [Web Apps](../app-service-web/app-service-web-overview.md).

## <a name="best-practices"></a>Bästa praxis

När du använder App-tjänster måste du följa dessa rekommendationer:

- [Autentisera via Azure Active Directory (AD)](../app-service-web/web-sites-authentication-authorization.md#authenticate-through-azure-active-directory). Apptjänster innehåller en OAuth 2.0 för din identitetsleverantör. OAuth 2.0 fokuserar på klienten developer enkelhet samtidigt som man tillstånd flöden för webbprogram, program och mobiltelefoner. Azure AD använder OAuth 2.0 tooenable du tooauthorize åtkomst toomobile webbprogrammen och.
- Begränsa åtkomst baserat på hello måste tooknow och minsta privilegium säkerhetsprinciper. Begränsa åtkomst är viktigt för organisationer som vill tooenforce säkerhetsprinciper för dataåtkomst. Rollbaserad åtkomstkontroll (RBAC) kan vara används tooassign behörigheter toousers, grupper och program för ett visst område. toolearn mer information om hur du ger användare åtkomst till tooapplications, se [Kom igång med åtkomsthantering](../active-directory/role-based-access-control-what-is.md).
- Skydda dina nycklar. Det spelar ingen roll hur bra säkerheten är om du tappar bort din prenumeration nycklar. Azure Key Vault hjälper dig att skydda krypteringsnycklar och hemligheter som används av molnprogram och molntjänster. Med Key Vault kan du kryptera nycklar och hemligheter (till exempel autentiseringsnycklar, lagringskontonycklar, datakrypteringsnycklar, PFX-filer och lösenord) med hjälp av nycklar som skyddas av maskinvarusäkerhetsmoduler (HSM). För ytterligare säkerhet kan du importera eller generera nycklar i HSM-moduler. Se [Azure Key Vault](../key-vault/key-vault-whatis.md) toolearn mer. Du kan också använda Key Vault toomanage TLS-certifikat med automatisk förnyelse.
- Begränsa inkommande käll-IP-adresser. [Tjänster-Appmiljön](../app-service-web/app-service-app-service-environment-intro.md) har ett virtuellt nätverk integrationsfunktionen som hjälper dig att begränsa inkommande käll-IP-adresser via nätverkssäkerhetsgrupper (NSG: er). Om du inte känner till virtuella Azure-nätverk (Vnet) är en funktion som gör tooplace många av dina Azure-resurser i ett icke-internet, dirigerbara nätverk som du styr åtkomst till. Det finns fler toolearn [integrera din app med ett Azure Virtual Network](../app-service-web/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Nästa steg
Den här artikeln introduceras tooa samling Apptjänster säkerhetsmetoder för att skydda din PaaS webb- och mobilprogram. toolearn mer om hur du skyddar dina PaaS-distributioner finns:

- [Skydda PaaS-distributioner](security-paas-deployments.md)
- [Att säkra PaaS-webb- och mobila program med Azure SQL Database och SQL Data Warehouse](security-paas-applications-using-sql.md)
