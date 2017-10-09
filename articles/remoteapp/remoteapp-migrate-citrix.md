---
title: "aaaMigrate från Azure RemoteApp tooCitrix XenApp Essentials | Microsoft Docs"
description: "Hur toomigrate från Azure RemoteApp tooCitrix XenApp Essentials"
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
ms.openlocfilehash: aa3ce28bc5a86d5b1dd3408196d51935395f55c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-azure-remoteapp-toocitrix-xenapp-essentials"></a>Migrera från Azure RemoteApp tooCitrix XenApp Essentials

Om du använder Azure RemoteApp och du vill toomigrate tooCitrix XenApp Essentials, finns det några krav tookeep i åtanke. Läsa Citrix's [steg för steg teknisk guide för Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) och dess [online tekniska biblioteket](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html). 

## <a name="prerequisite-steps-for-migration"></a>Förkraven för migrering

1. Skapa ett nytt virtuellt nätverk eller ta reda på vilka virtuella Azure-nätverket i Azure Resource Manager som du ska distribuera Citrix XenApp Essentials. Azure RemoteApp använder hello klassiska Azure-portalen; Citrix XenApp Essentials har endast stöd för Azure Resource Manager.  
2. Kontrollera att hello virtuella nätverk som du har valt har nätverk åtkomst tooyour domänkontrollant, eftersom Citrix stöder endast hybriddistribution. Om du använder en distribution av Azure RemoteApp kan du kontrollera att det virtuella nätverket har nätverk åtkomst tooan Active Directory-domänkontrollant. Du kan också använda Azure Active Directory Domain Services (Azure AD DS). 
3. Se till att hello DNS har konfigurerats korrekt för hello virtuellt nätverk, så att domänanslutningen är misslyckas på din första försöket. Du kan skapa en virtuell dator (VM) i hello virtuella nätverk som du har valt och utföra en manuell domän koppling tooverify som hello DNS och domänanslutning fungerar som förväntat. Gör att du lyckas hello första gången du distribuerar Citrix XenApp Essentials. 
4. Om det behövs kan du skapa ett virtuellt nätverk peering mellan Azure klassiska portal-nätverk som används med Azure RemoteApp och ditt virtuella nätverk i Azure Resource Manager. Den här peering processen fungerar om hello två nätverk finns i hello samma region. Om de inte Använd plats-till-plats VPN-tooconnect hello virtuella nätverk för nätverk. 
5. Om det behövs kan du läsa [hur toomigrate data till och från Azure RemoteApp](remoteapp-migrate.md). 
6. Uppdatera din befintliga Azure RemoteApp-avbildning tooinclude hello Citrix VDA komponent (instruktioner finns i dokumentationen för hello Citrix). 
7. Gå toohello Azure Marketplace och Citrix XenApp Essentials distributionen.

## <a name="other-considerations"></a>Andra överväganden

Tänk på följande ytterligare överväganden när du migrerar hello:
- Citrix XenApp Essentials har endast stöd för hybriddistributioner. Med andra ord kräver åtkomst tooa domänkontrollant i ordning tooperform domänanslutning. Om du använder en distribution av Azure RemoteApp kan använda Azure AD DS eller se till att det virtuella nätverket har åtkomst tooActive Directory för domänanslutning. 
- hur toomove användaren data tooCitrix XenApp Essentials, se toolearn [hur toomigrate data till och från Azure RemoteApp](remoteapp-migrate.md). 
- Citrix XenApp Essentials har endast stöd för Active Directory-konton. Det stöder inte Microsoft-konton (till exempel outlook.com, msn.com eller hotmail.com). 

## <a name="citrix-xenapp-essentials-billing"></a>Citrix XenApp Essentials fakturering

Fullständig information om priser finns i hello [vanliga frågor och svar](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) och [Citrix översiktsartikel](https://www.citrix.com/global-partners/microsoft/remote-app.html). Det finns tre fakturering komponenter tooCitrix XenApp Essentials:

- hello Citrix service kostnad, vilket är $12 per användare per månad. Precis som alla Azure Marketplace-inköp är detta fakturerade toohello betalningsmetod som associeras med din Azure-prenumeration. Azure penningkrediten för Enterprise-avtal (EA) kunder kan inte användas. 
- Remote Data Services (RDS) klientåtkomstlicenser (CAL). För närvarande kan du köpa hello Remote Access avgift som är paketerade med hello Citrix XenApp Essentials betalning för $6,25. Om du är en EA-kund kan du använda Azure penningkrediten toopay för den här. Om du vill toouse befintliga Klientåtkomstlicenserna för Fjärrskrivbordstjänster, kontaktar du oss på [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com), så att vi kan använda tooyour räkningen. 
- Azure-beräkning och lagring. Det kostar hello Azure-lagring och beräkning förbrukningen för virtuella datorer hello används. Tänk på att priser när du väljer din VM-storlek och användaren densitet. Om du är en EA-kund kan du använda Azure penningkrediten toopay för den här.

Om du fortfarande har frågor kan du:
- Skicka e-post på [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
- [Kontakta Azure-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Börja med [öppna ett Azure supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toohelp med nödvändiga steg 1-5. Steg 6 – 7 Kontakta Citrix genom att öppna ett supportärende inom hello Citrix-hanteringsportalen. 
