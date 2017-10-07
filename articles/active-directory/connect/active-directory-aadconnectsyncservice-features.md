---
title: aaaAzure AD Connect-synkronisering service funktioner och konfiguration | Microsoft Docs
description: "Beskriver funktioner för tjänsten på klientsidan för Azure AD Connect-synkroniseringstjänsten."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 7ad05c45bb6b5fd5deaa3466e2936b19d3d9eabb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect sync-tjänsten-funktioner
hello synkroniseringsfunktionen av Azure AD Connect har två komponenter:

* hello lokalt komponenten **Azure AD Connect-synkronisering**, även kallat **Synkroniseringsmotorn**.
* hello-tjänst som finns i Azure AD så kallade **Azure AD Connect-synkroniseringstjänsten**

Det här avsnittet beskrivs hur hello följande funktioner för hello **Azure AD Connect-synkroniseringstjänsten** fungerar och hur du kan konfigurera dem med hjälp av Windows PowerShell.

De här inställningarna är konfigurerade med hello [Azure Active Directory-modulen för Windows PowerShell](http://aka.ms/aadposh). Hämta och installera det separat från Azure AD Connect. hello-cmdletarna som beskrivs i det här avsnittet har introducerats i hello [2016 mars-versionen (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Om du inte har hello-cmdletarna som beskrivs i det här avsnittet eller de inte producerar hello samma resultera och sedan kontrollera att kör du hello senaste versionen.

toosee hello konfigurationen i Azure AD-katalogen, kör `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures resultat](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Många av dessa inställningar kan bara ändras av Azure AD Connect.

hello följande inställningar kan konfigureras med `Set-MsolDirSyncFeature`:

| DirSyncFeature | Kommentar |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Tillåter objekt toojoin på userPrincipalName i tillägg tooprimary SMTP-adress. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Tillåter hello sync motorn tooupdate-hello attributet userPrincipalName hanteras/licensierade (ofedererad) användare. |

När du har aktiverat en funktion kan inaktiveras det inte igen.

> [!NOTE]
> Hej funktionen från 24 augusti 2016 *duplicera attribut återhämtning* är aktiverad som standard för nya Azure AD-kataloger. Den här funktionen kommer även distribuerat och aktiverad på kataloger som skapats före detta datum. Du får ett e-postmeddelande när din katalog kommer tooget den här funktionen är aktiverad.
> 
> 

hello följande inställningar konfigureras med Azure AD Connect och kan inte ändras av `Set-MsolDirSyncFeature`:

| DirSyncFeature | Kommentar |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: Aktivera tillbakaskrivning av enheter](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions |[Azure AD Connect-synkronisering: katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Gör en attributet toobe i karantän när det är en dubblett av ett annat objekt i stället misslyckas hello hela objektet under exporten. |
| PasswordSync |[Implementera Lösenordssynkronisering med Azure AD Connect-synkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) |
| UnifiedGroupWriteback |[Förhandsversion: Tillbakaskrivning av grupp](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback |Stöds inte för närvarande. |

## <a name="duplicate-attribute-resiliency"></a>Duplicerat attribut återhämtning
I stället för misslyckas tooprovision-objekt med dubblerade UPN-namn / proxyAddresses, hello duplicerade attributet ”i karantän” och ett tillfälligt värde är tilldelad. När hello konflikten löses hello tillfälligt UPN-namnet är rätt toohello-värdet har ändrats automatiskt. Mer information finns i [identitet synkronisering och dubblett attributet återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName mjuka matchning
När den här funktionen är aktiverad, mjuk-matcha är aktiverat för UPN i tillägg toohello [primära SMTP-adress](https://support.microsoft.com/kb/2641663), som alltid är aktiverat. Soft-matcha är används toomatch befintliga molnanvändare i Azure AD med lokala användare.

Om du behöver toomatch lokala AD-konton med befintliga konton som skapats i hello molnet och du använder inte Exchange Online och sedan den här funktionen är användbart. I detta scenario du normalt inte attributet orsak tooset hello SMTP i hello molnet.

Den här funktionen är på skapas som standard för nya Azure AD-kataloger. Du kan se om den här funktionen är aktiverad för du genom att köra:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Om den här funktionen inte har aktiverats för din Azure AD-katalog, kan du aktivera det genom att köra:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Synkronisera userPrincipalName uppdateringar
Tidigare har uppdateringar toohello attributet UserPrincipalName med hello synkroniseringstjänsten från lokala blockerats, om bägge dessa villkor är uppfyllda:

* hello användaren hanteras (ofedererad).
* hello användaren har inte tilldelats en licens.

Mer information finns i [användarens namn i Office 365, Azure eller Intune inte matchar hello lokal UPN eller alternativ inloggnings-ID](https://support.microsoft.com/kb/2523192).

Den här funktionen aktiveras kan hello sync motorn tooupdate hello userPrincipalName när den har ändrats lokalt och du använder Lösenordssynkronisering. Den här funktionen stöds inte om du använder federation.

Den här funktionen är på skapas som standard för nya Azure AD-kataloger. Du kan se om den här funktionen är aktiverad för du genom att köra:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Om den här funktionen inte har aktiverats för din Azure AD-katalog, kan du aktivera det genom att köra:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

När den här funktionen, befintliga userPrincipalName värden förblir-är. På Nästa ändring av hello userPrincipalName attributet lokal uppdaterar hello normal Deltasynkronisering på användare hello UPN.  

## <a name="see-also"></a>Se även
* [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

