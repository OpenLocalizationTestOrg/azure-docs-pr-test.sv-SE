---
title: "Migrera från Azure RemoteApp till Citrix XenApp Essentials | Microsoft Docs"
description: "Hur du migrerar från Azure RemoteApp till Citrix XenApp Essentials"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 695a8165-3454-4855-8e21-f2eb2c61201b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: fcd96a466d1c0dad17d7012308281ef868463b19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-from-azure-remoteapp-to-citrix-xenapp-essentials"></a>Migrera från Azure RemoteApp till Citrix XenApp Essentials

Om du använder Azure RemoteApp och du vill migrera till Citrix XenApp Essentials, finns det några krav att tänka på. Läsa Citrix's [steg för steg teknisk guide för Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) och dess [online tekniska biblioteket](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html). 

## <a name="prerequisite-steps-for-migration"></a>Förkraven för migrering

1. Skapa ett nytt virtuellt nätverk eller ta reda på vilka virtuella Azure-nätverket i Azure Resource Manager som du ska distribuera Citrix XenApp Essentials. Azure RemoteApp använder den klassiska Azure-portalen; Citrix XenApp Essentials har endast stöd för Azure Resource Manager.  
2. Kontrollera att det virtuella nätverket som du har valt har åtkomst till domänkontrollanten eftersom Citrix stöder endast hybriddistribution. Om du använder en distribution av Azure RemoteApp kan du kontrollera att ditt virtuella nätverk har åtkomst till en Active Directory-domänkontrollant. Du kan också använda Azure Active Directory Domain Services (Azure AD DS). 
3. Kontrollera att DNS är korrekt konfigurerad för det virtuella nätverket så att domänanslutningen är misslyckas på din första försöket. Du kan skapa en virtuell dator (VM) i det virtuella nätverket som du har valt och utföra en manuell Domänanslutning för att verifiera att den DNS- och ansluta till fungerar som förväntat. Gör att du lyckas första gången du distribuerar Citrix XenApp Essentials. 
4. Om det behövs kan du skapa ett virtuellt nätverk peering mellan Azure klassiska portal-nätverk som används med Azure RemoteApp och ditt virtuella nätverk i Azure Resource Manager. Den här peering processen fungerar om två nätverk finns i samma region. Om de inte gör det, kan du använda plats-till-plats VPN för att ansluta virtuella nätverk för nätverk. 
5. Om det behövs kan du läsa [hur du migrerar data till och från Azure RemoteApp](remoteapp-migrate.md). 
6. Uppdatera din befintliga Azure RemoteApp-avbildning för att inkludera komponenten Citrix VDA (Mer information finns i Citrix-dokumentationen). 
7. Gå till Azure Marketplace och Citrix XenApp Essentials distributionen.

## <a name="other-considerations"></a>Andra överväganden

Tänk på följande ytterligare överväganden när du migrerar:
- Citrix XenApp Essentials har endast stöd för hybriddistributioner. Med andra ord kräver den nätverksåtkomst till en domänkontrollant för att kunna utföra domänanslutning. Om du använder en distribution av Azure RemoteApp kan använda Azure AD DS eller se till att det virtuella nätverket har åtkomst till Active Directory för domänanslutning. 
- Information om hur du flyttar användardata till Citrix XenApp Essentials finns [hur du migrerar data till och från Azure RemoteApp](remoteapp-migrate.md). 
- Citrix XenApp Essentials har endast stöd för Active Directory-konton. Det stöder inte Microsoft-konton (till exempel outlook.com, msn.com eller hotmail.com). 

## <a name="citrix-xenapp-essentials-billing"></a>Citrix XenApp Essentials fakturering

Fullständig information om priser finns i [vanliga frågor och svar](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) och [Citrix översiktsartikel](https://www.citrix.com/global-partners/microsoft/remote-app.html). Det finns tre fakturering komponenter till Citrix XenApp Essentials:

- Kostnad Citrix-tjänsten, som är $12 per användare per månad. Precis som alla Azure Marketplace-inköp, är detta debiteras betalningsmetod som är associerade med din Azure-prenumeration. Azure penningkrediten för Enterprise-avtal (EA) kunder kan inte användas. 
- Remote Data Services (RDS) klientåtkomstlicenser (CAL). För närvarande kan du köpa Remote Access avgiften som medföljer Citrix XenApp Essentials betalningen för $6,25. Om du är en EA-kund kan använda du Azure penningkrediten betala för detta. Om du vill använda din befintliga klientåtkomstlicenser för Fjärrskrivbordstjänster kontaktar du oss på [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com), så vi kan använda detta för din räkning. 
- Azure-beräkning och lagring. Det är kostnaden för Azure-lagring och beräkning förbrukningen för de virtuella datorerna används. Tänk på att priser när du väljer din VM-storlek och användaren densitet. Om du är en EA-kund kan använda du Azure penningkrediten betala för detta.

Om du fortfarande har frågor kan du:
- Skicka e-post på [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
- [Kontakta Azure-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Börja med [öppna ett Azure supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) för att hjälpa med nödvändiga steg 1-5. Steg 6 – 7 Kontakta Citrix genom att öppna ett supportärende i hanteringsportalen för Citrix. 
