---
title: aaaWindows autentisering och Azure MFA-Server | Microsoft Docs
description: "Detta är hello Azure Multi-Factor authentication sida som är till hjälp vid distribution av Windows-autentisering och Azure Multi-Factor Authentication-servern."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 19a4043f-c4ce-43c0-80e7-2548ee92cb74
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 0fc38fd751966bf883d4eae7c48055988922af80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-autentisering och Azure Multi-Factor Authentication Server
Använd hello avsnittet för Windows-autentisering av hello Azure Multi-Factor Authentication-servern tooenable och konfigurera Windows-autentisering för program. Innan du konfigurerar Windows-autentisering, hålla hello följande lista i åtanke:

* Starta om hello Azure Multi-Factor Authentication för Terminal Services tootake effekt efter installationen.
* Om 'Azure Multi-Factor Authentication-användarmatchning är markerat och du inte är i hello användarlistan, kommer inte att kunna toolog till hello dator efter omstart.
* Betrodda IP-adresser är beroende av att hello programmet kan uppge hello klientens IP-Adressen med hello-autentisering. För närvarande stöds endast Terminal Services.  

> [!NOTE]
> Den här funktionen är inte stöds toosecure Terminal Services i Windows Server 2012 R2.

## <a name="toosecure-an-application-with-windows-authentication-use-hello-following-procedure"></a>toosecure ett program med Windows-autentisering, Använd hello nedan.
1. Klicka på ikonen för Windows-autentisering av hello i hello Azure Multi-Factor Authentication-servern.
   ![Windows-autentisering](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Kontrollera hello **aktivera Windows-autentisering** kryssrutan. Den här rutan är avmarkerad som standard.
3. hello på fliken program kan Hej administratör tooconfigure ett eller flera program för Windows-autentisering.
4. Välj en server eller ett program – ange om hello serverprogrammet är aktiverat. Klicka på **OK**.
5. Klicka på **Lägg till...**
6. hello tillförlitliga IP-adresser fliken kan du tooskip Azure Multi-Factor Authentication för Windows-sessioner som kommer från särskilda IP-adresser. Till exempel anställda använder programmet hello från hello office och hemmet, du kan då välja du inte vill att deras telefoner ska ringa för Azure Multi-Factor Authentication på hello office. För att göra detta skulle du ange hello office undernät som betrodda IP-adresser.
7. Klicka på **Lägg till...**
8. Välj **enda IP** om du vill att tooskip en IP-adress.
9. Välj **IP-intervall** om du vill att tooskip ett hela IP-adressintervall. Exempel: 10.63.193.1-10.63.193.100.
10. Välj **undernät** om du vill att toospecify ett intervall av IP-adresser med hjälp av undernät. Ange hello undernät Start IP och välj hello lämplig nätmask hello listrutan.
11. Klicka på **OK**.

## <a name="next-steps"></a>Nästa steg

- [Konfigurera VPN-installationer från tredje part för Azure MFA Server](multi-factor-authentication-advanced-vpn-configurations.md)

- [Utöka din befintliga infrastruktur för autentisering med hello NPS-tillägg för Azure MFA](multi-factor-authentication-nps-extension.md)
