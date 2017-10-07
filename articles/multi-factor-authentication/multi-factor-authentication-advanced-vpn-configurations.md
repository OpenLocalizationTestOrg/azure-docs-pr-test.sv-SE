---
title: aaaAdvanced scenarier med Azure MFA och tredje parts VPN-anslutningar
description: "Steg för steg-konfiguration guider för Azure MFA toointegrate med Cisco, Citrix och Juniper."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1f94a214-d6f6-48a8-8a12-006b5896ae45
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2017
ms.author: kgremban
ms.openlocfilehash: e23960ca4977cc01271f99fa2bec70449e9acfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Avancerade scenarier med Azure Multi-Factor Authentication och VPN-lösningar
Azure Multi-Factor Authentication kan användas för tooseamlessly ansluta med olika VPN-lösningar från tredje part. Den här artikeln fokuserar på Cisco® ASA VPN-enhet, Citrix NetScaler SSL VPN-enhet och hello Juniper nätverk säker åtkomst/Pulse Secure ansluta säkra SSL VPN-enhet. Vi har skapat configuration guider tooaddress tre vanliga hushållsapparater, men Multi-Factor Authentication-servern kan integreras med de flesta system som använder RADIUS, LDAP, IIS eller Anspråksbaserad autentisering tooAD FS. Du hittar mer information finns i [MFA serverkonfigurationer](multi-factor-authentication-get-started-server.md#next-steps).

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN-enhet och Azure Multi-Factor Authentication
Azure Multi-Factor Authentication integreras med Cisco® ASA VPN-enhet tooprovide ytterligare säkerheten för Cisco AnyConnect® VPN-inloggningar och portalåtkomst.  Detta kan göras med hjälp av antingen hello LDAP eller RADIUS-protokollet.  Välj något av hello följande toodownload hello detaljerade stegvisa konfigurationen hjälper.

| Konfigurationsguiden | Beskrivning |
| --- | --- |
| [Cisco ASA med Anyconnect VPN och Azure MFA-konfiguration för LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Integrera din Cisco ASA VPN-enhet med Azure MFA med LDAP |
| [Cisco ASA med Anyconnect VPN och Azure MFA-konfiguration för RADIUS](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Integrera din Cisco ASA VPN-enhet med Azure MFA med RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN- och Azure Multi-Factor Authentication
Azure Multi-Factor Authentication integreras med din Citrix NetScaler SSL VPN-enhet tooprovide ytterligare säkerhet för Citrix NetScaler SSL VPN-inloggningar och portalåtkomst.  Detta kan göras med hjälp av antingen hello LDAP eller RADIUS-protokollet.  Välj något av hello följande toodownload hello detaljerade stegvisa konfigurationen hjälper.

| Konfigurationsguiden | Beskrivning |
| --- | --- |
| [Citrix NetScaler SSL VPN- och Azure MFA-konfiguration för LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Integrera din Citrix NetScaler SSL VPN med Azure MFA-enhet med LDAP |
| [Citrix NetScaler SSL VPN- och Azure MFA-konfigurationen för RADIUS-](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Integrera din Citrix NetScaler SSL VPN-enhet med Azure MFA med RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse Secure SSL VPN-enhet och Azure Multi-Factor Authentication
Azure Multi-Factor Authentication integreras med din Juniper/Pulse Secure SSL VPN-enhet tooprovide ytterligare säkerhet för Juniper/Pulse Secure SSL VPN-inloggningar och portalåtkomst.  Detta kan göras med hjälp av antingen hello LDAP eller RADIUS-protokollet.  Välj något av hello följande toodownload hello detaljerade stegvisa konfigurationen hjälper.

| Konfigurationsguiden | Beskrivning |
| --- | --- |
| [Juniper/Pulse Secure SSL VPN- och Azure MFA-konfiguration för LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Integrera din Juniper/Pulse Secure SSL VPN med Azure MFA-enhet med LDAP |
| [Juniper/Pulse Secure SSL VPN- och Azure MFA-konfigurationen för RADIUS-](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Integrera din Juniper/Pulse Secure SSL VPN-enhet med Azure MFA med RADIUS |

## <a name="next-steps"></a>Nästa steg

- [Utöka din befintliga infrastruktur för autentisering med hello NPS-tillägg för Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md)

- [Konfigurera inställningar för Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md)