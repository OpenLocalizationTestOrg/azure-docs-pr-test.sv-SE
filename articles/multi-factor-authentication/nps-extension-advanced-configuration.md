---
title: "aaaConfigure hello Azure MFA NPS-tillägg | Microsoft Docs"
description: "När du installerar tillägget för hello NPS, Följ dessa steg för avancerad konfiguration som vitlistning av IP- och UPN-ersättning."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: c3aed077b23c95f874861eb00c8e6dca668329c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-options-for-hello-nps-extension-for-multi-factor-authentication"></a>Avancerade konfigurationsalternativ för hello NPS-tillägg för Multifaktorautentisering

hello Server (NPS) tillägget utökar din molnbaserade Azure Multi-Factor Authentication-funktioner till din lokala infrastruktur. Den här artikeln förutsätter att du redan har hello tillägget installeras och nu vill tooknow hur toocustomize hello-tillägget som du behöver. 

## <a name="alternate-login-id"></a>Alternativt inloggnings-ID

Eftersom hello NPS tillägget ansluter tooboth dina lokala kataloger och molnet kataloger, som kan uppstå ett problem där din lokala användarens huvudnamn (UPN) inte matchar hello namn i hello molnet. toosolve problemet, Använd alternativa inloggnings-ID: N. 

Du kan ange en toobe för Active Directory-attribut som används i stället för hello UPN för Azure Multi-Factor Authentication i hello NPS-tillägget. Detta gör att du tooprotect dina lokala resurser med tvåstegsverifiering utan att ändra din lokala UPN-namn. 

tooconfigure alternativt inloggnings-ID, gå för`HKLM\SOFTWARE\Microsoft\AzureMfa` och redigera hello följande registervärden:

| Namn | Typ | Standardvärde | Beskrivning |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | Sträng | tom | Ange hello namnet på Active Directory-attribut som du vill toouse i stället för hello UPN. Det här attributet används som hello AlternateLoginId attribut. Om värdet anges tooa [giltigt Active Directory-attribut](https://msdn.microsoft.com/library/ms675090.aspx) (till exempel e-post eller displayName), sedan hello attributvärde används i stället för hello användarens UPN för autentisering. Om värdet är tomt eller inte konfigurerad, sedan AlternateLoginId är inaktiverat och hello användares UPN används för autentisering. |
| LDAP_FORCE_GLOBAL_CATALOG | Booleskt värde | False | Använd den här flaggan tooforce hello användningen av den globala katalogen för LDAP-sökningar när du söker efter AlternateLoginId. Konfigurera en domänkontrollant som en Global katalog, Lägg till hello AlternateLoginId attributet toohello Global katalog och sedan aktivera den här flaggan. <br><br> Om LDAP_LOOKUP_FORESTS konfigureras (inte tomt) **flaggan tillämpas som SANT**, oavsett hello hello registret inställningens värde. I det här fallet kräver hello NPS tillägget hello Global katalog toobe konfigurerats med hello AlternateLoginId attribut för varje skog. |
| LDAP_LOOKUP_FORESTS | Sträng | tom | Ange en lista över skogar toosearch som gäller avgränsas med semikolon. Till exempel *contoso.com;foobar.com*. Om det här registervärdet som är konfigurerad, söker hello NPS tillägget upprepade gånger alla hello skogar i hello ordning som de visas i listan, och returnerar hello första lyckade AlternateLoginId värdet. Om det här registervärdet som inte har konfigurerats är hello AlternateLoginId sökning slutna toohello aktuella domänen.|

tootroubleshoot problem med alternativt inloggnings-ID, använder hello rekommenderade steg för [alternativa inloggnings-ID fel](multi-factor-authentication-nps-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>IP-undantag

Om du behöver toomonitor servertillgänglighet, som om belastningsutjämnare kontrollera vilka servrar som körs innan du skickar arbetsbelastningar kan vill du inte att dessa kontroller toobe blockeras av verifiering begäranden. I stället skapa en lista med IP-adresser som du vet används av tjänstkonton och inaktivera Multifaktorautentisering krav för listan. 

tooconfigure en godkänd IP-lista, gå för`HKLM\SOFTWARE\Microsoft\AzureMfa` och konfigurera hello följande registervärde: 

| Namn | Typ | Standardvärde | Beskrivning |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | Sträng | tom | Ange en lista över IP-adresser som gäller avgränsas med semikolon. Inkludera hello IP-adresserna för de datorer där tjänstbegäranden kommer som hello NAS-VPN-server. IP-adressintervall är undernät inte stöds. <br><br> Till exempel *10.0.0.1;10.0.0.2;10.0.0.3*.

När en begäran kommer från en IP-adress som finns i listan över godkända hello ignoreras tvåstegsverifiering. hello godkänd IP-lista är jämfört med toohello IP-adress som har angetts i hello *ratNASIPAddress* attribut för hello RADIUS-begäranden. Om en RADIUS-begäran kommer utan hello ratNASIPAddress attribut, loggas hello följande varning: ”P_WHITE_LIST_WARNING::IP godkända ignoreras som käll-IP saknas i RADIUS-begäranden i NasIpAddress attribut”.

## <a name="next-steps"></a>Nästa steg

[Lösa felmeddelanden från hello NPS-tillägg för Azure Multi-Factor Authentication](multi-factor-authentication-nps-errors.md)
