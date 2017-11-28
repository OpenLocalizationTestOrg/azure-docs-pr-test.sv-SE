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
# <a name="azure-ad-connect-sync-service-features"></a><span data-ttu-id="791e3-103">Azure AD Connect sync-tjänsten-funktioner</span><span class="sxs-lookup"><span data-stu-id="791e3-103">Azure AD Connect sync service features</span></span>
<span data-ttu-id="791e3-104">hello synkroniseringsfunktionen av Azure AD Connect har två komponenter:</span><span class="sxs-lookup"><span data-stu-id="791e3-104">hello synchronization feature of Azure AD Connect has two components:</span></span>

* <span data-ttu-id="791e3-105">hello lokalt komponenten **Azure AD Connect-synkronisering**, även kallat **Synkroniseringsmotorn**.</span><span class="sxs-lookup"><span data-stu-id="791e3-105">hello on-premises component named **Azure AD Connect sync**, also called **sync engine**.</span></span>
* <span data-ttu-id="791e3-106">hello-tjänst som finns i Azure AD så kallade **Azure AD Connect-synkroniseringstjänsten**</span><span class="sxs-lookup"><span data-stu-id="791e3-106">hello service residing in Azure AD also known as **Azure AD Connect sync service**</span></span>

<span data-ttu-id="791e3-107">Det här avsnittet beskrivs hur hello följande funktioner för hello **Azure AD Connect-synkroniseringstjänsten** fungerar och hur du kan konfigurera dem med hjälp av Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="791e3-107">This topic explains how hello following features of hello **Azure AD Connect sync service** work and how you can configure them using Windows PowerShell.</span></span>

<span data-ttu-id="791e3-108">De här inställningarna är konfigurerade med hello [Azure Active Directory-modulen för Windows PowerShell](http://aka.ms/aadposh).</span><span class="sxs-lookup"><span data-stu-id="791e3-108">These settings are configured by hello [Azure Active Directory Module for Windows PowerShell](http://aka.ms/aadposh).</span></span> <span data-ttu-id="791e3-109">Hämta och installera det separat från Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="791e3-109">Download and install it separately from Azure AD Connect.</span></span> <span data-ttu-id="791e3-110">hello-cmdletarna som beskrivs i det här avsnittet har introducerats i hello [2016 mars-versionen (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span><span class="sxs-lookup"><span data-stu-id="791e3-110">hello cmdlets documented in this topic were introduced in hello [2016 March release (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span></span> <span data-ttu-id="791e3-111">Om du inte har hello-cmdletarna som beskrivs i det här avsnittet eller de inte producerar hello samma resultera och sedan kontrollera att kör du hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="791e3-111">If you do not have hello cmdlets documented in this topic or they do not produce hello same result, then make sure you run hello latest version.</span></span>

<span data-ttu-id="791e3-112">toosee hello konfigurationen i Azure AD-katalogen, kör `Get-MsolDirSyncFeatures`.</span><span class="sxs-lookup"><span data-stu-id="791e3-112">toosee hello configuration in your Azure AD directory, run `Get-MsolDirSyncFeatures`.</span></span>  
<span data-ttu-id="791e3-113">![Get-MsolDirSyncFeatures resultat](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span><span class="sxs-lookup"><span data-stu-id="791e3-113">![Get-MsolDirSyncFeatures result](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span></span>

<span data-ttu-id="791e3-114">Många av dessa inställningar kan bara ändras av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="791e3-114">Many of these settings can only be changed by Azure AD Connect.</span></span>

<span data-ttu-id="791e3-115">hello följande inställningar kan konfigureras med `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="791e3-115">hello following settings can be configured by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="791e3-116">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="791e3-116">DirSyncFeature</span></span> | <span data-ttu-id="791e3-117">Kommentar</span><span class="sxs-lookup"><span data-stu-id="791e3-117">Comment</span></span> |
| --- | --- |
| [<span data-ttu-id="791e3-118">EnableSoftMatchOnUpn</span><span class="sxs-lookup"><span data-stu-id="791e3-118">EnableSoftMatchOnUpn</span></span>](#userprincipalname-soft-match) |<span data-ttu-id="791e3-119">Tillåter objekt toojoin på userPrincipalName i tillägg tooprimary SMTP-adress.</span><span class="sxs-lookup"><span data-stu-id="791e3-119">Allows objects toojoin on userPrincipalName in addition tooprimary SMTP address.</span></span> |
| [<span data-ttu-id="791e3-120">SynchronizeUpnForManagedUsers</span><span class="sxs-lookup"><span data-stu-id="791e3-120">SynchronizeUpnForManagedUsers</span></span>](#synchronize-userprincipalname-updates) |<span data-ttu-id="791e3-121">Tillåter hello sync motorn tooupdate-hello attributet userPrincipalName hanteras/licensierade (ofedererad) användare.</span><span class="sxs-lookup"><span data-stu-id="791e3-121">Allows hello sync engine tooupdate hello userPrincipalName attribute for managed/licensed (non-federated) users.</span></span> |

<span data-ttu-id="791e3-122">När du har aktiverat en funktion kan inaktiveras det inte igen.</span><span class="sxs-lookup"><span data-stu-id="791e3-122">After you have enabled a feature, it cannot be disabled again.</span></span>

> [!NOTE]
> <span data-ttu-id="791e3-123">Hej funktionen från 24 augusti 2016 *duplicera attribut återhämtning* är aktiverad som standard för nya Azure AD-kataloger.</span><span class="sxs-lookup"><span data-stu-id="791e3-123">From August 24, 2016 hello feature *Duplicate attribute resiliency* is enabled by default for new Azure AD directories.</span></span> <span data-ttu-id="791e3-124">Den här funktionen kommer även distribuerat och aktiverad på kataloger som skapats före detta datum.</span><span class="sxs-lookup"><span data-stu-id="791e3-124">This feature will also be rolled out and enabled on directories created before this date.</span></span> <span data-ttu-id="791e3-125">Du får ett e-postmeddelande när din katalog kommer tooget den här funktionen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="791e3-125">You will receive an email notification when your directory is about tooget this feature enabled.</span></span>
> 
> 

<span data-ttu-id="791e3-126">hello följande inställningar konfigureras med Azure AD Connect och kan inte ändras av `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="791e3-126">hello following settings are configured by Azure AD Connect and cannot be modified by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="791e3-127">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="791e3-127">DirSyncFeature</span></span> | <span data-ttu-id="791e3-128">Kommentar</span><span class="sxs-lookup"><span data-stu-id="791e3-128">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="791e3-129">DeviceWriteback</span><span class="sxs-lookup"><span data-stu-id="791e3-129">DeviceWriteback</span></span> |[<span data-ttu-id="791e3-130">Azure AD Connect: Aktivera tillbakaskrivning av enheter</span><span class="sxs-lookup"><span data-stu-id="791e3-130">Azure AD Connect: Enabling device writeback</span></span>](active-directory-aadconnect-feature-device-writeback.md) |
| <span data-ttu-id="791e3-131">DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="791e3-131">DirectoryExtensions</span></span> |[<span data-ttu-id="791e3-132">Azure AD Connect-synkronisering: katalogtillägg</span><span class="sxs-lookup"><span data-stu-id="791e3-132">Azure AD Connect sync: Directory extensions</span></span>](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [<span data-ttu-id="791e3-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span><span class="sxs-lookup"><span data-stu-id="791e3-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span></span>](#duplicate-attribute-resiliency) |<span data-ttu-id="791e3-134">Gör en attributet toobe i karantän när det är en dubblett av ett annat objekt i stället misslyckas hello hela objektet under exporten.</span><span class="sxs-lookup"><span data-stu-id="791e3-134">Allows an attribute toobe quarantined when it is a duplicate of another object rather than failing hello entire object during export.</span></span> |
| <span data-ttu-id="791e3-135">PasswordSync</span><span class="sxs-lookup"><span data-stu-id="791e3-135">PasswordSync</span></span> |[<span data-ttu-id="791e3-136">Implementera Lösenordssynkronisering med Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="791e3-136">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md) |
| <span data-ttu-id="791e3-137">UnifiedGroupWriteback</span><span class="sxs-lookup"><span data-stu-id="791e3-137">UnifiedGroupWriteback</span></span> |[<span data-ttu-id="791e3-138">Förhandsversion: Tillbakaskrivning av grupp</span><span class="sxs-lookup"><span data-stu-id="791e3-138">Preview: Group writeback</span></span>](active-directory-aadconnect-feature-preview.md#group-writeback) |
| <span data-ttu-id="791e3-139">UserWriteback</span><span class="sxs-lookup"><span data-stu-id="791e3-139">UserWriteback</span></span> |<span data-ttu-id="791e3-140">Stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="791e3-140">Not currently supported.</span></span> |

## <a name="duplicate-attribute-resiliency"></a><span data-ttu-id="791e3-141">Duplicerat attribut återhämtning</span><span class="sxs-lookup"><span data-stu-id="791e3-141">Duplicate attribute resiliency</span></span>
<span data-ttu-id="791e3-142">I stället för misslyckas tooprovision-objekt med dubblerade UPN-namn / proxyAddresses, hello duplicerade attributet ”i karantän” och ett tillfälligt värde är tilldelad.</span><span class="sxs-lookup"><span data-stu-id="791e3-142">Instead of failing tooprovision objects with duplicate UPNs / proxyAddresses, hello duplicated attribute is “quarantined” and a temporary value is assigned.</span></span> <span data-ttu-id="791e3-143">När hello konflikten löses hello tillfälligt UPN-namnet är rätt toohello-värdet har ändrats automatiskt.</span><span class="sxs-lookup"><span data-stu-id="791e3-143">When hello conflict is resolved, hello temporary UPN is changed toohello proper value automatically.</span></span> <span data-ttu-id="791e3-144">Mer information finns i [identitet synkronisering och dubblett attributet återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="791e3-144">For more details, see [Identity synchronization and duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="userprincipalname-soft-match"></a><span data-ttu-id="791e3-145">UserPrincipalName mjuka matchning</span><span class="sxs-lookup"><span data-stu-id="791e3-145">UserPrincipalName soft match</span></span>
<span data-ttu-id="791e3-146">När den här funktionen är aktiverad, mjuk-matcha är aktiverat för UPN i tillägg toohello [primära SMTP-adress](https://support.microsoft.com/kb/2641663), som alltid är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="791e3-146">When this feature is enabled, soft-match is enabled for UPN in addition toohello [primary SMTP address](https://support.microsoft.com/kb/2641663), which is always enabled.</span></span> <span data-ttu-id="791e3-147">Soft-matcha är används toomatch befintliga molnanvändare i Azure AD med lokala användare.</span><span class="sxs-lookup"><span data-stu-id="791e3-147">Soft-match is used toomatch existing cloud users in Azure AD with on-premises users.</span></span>

<span data-ttu-id="791e3-148">Om du behöver toomatch lokala AD-konton med befintliga konton som skapats i hello molnet och du använder inte Exchange Online och sedan den här funktionen är användbart.</span><span class="sxs-lookup"><span data-stu-id="791e3-148">If you need toomatch on-premises AD accounts with existing accounts created in hello cloud and you are not using Exchange Online, then this feature is useful.</span></span> <span data-ttu-id="791e3-149">I detta scenario du normalt inte attributet orsak tooset hello SMTP i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="791e3-149">In this scenario, you generally don’t have a reason tooset hello SMTP attribute in hello cloud.</span></span>

<span data-ttu-id="791e3-150">Den här funktionen är på skapas som standard för nya Azure AD-kataloger.</span><span class="sxs-lookup"><span data-stu-id="791e3-150">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="791e3-151">Du kan se om den här funktionen är aktiverad för du genom att köra:</span><span class="sxs-lookup"><span data-stu-id="791e3-151">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

<span data-ttu-id="791e3-152">Om den här funktionen inte har aktiverats för din Azure AD-katalog, kan du aktivera det genom att köra:</span><span class="sxs-lookup"><span data-stu-id="791e3-152">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a><span data-ttu-id="791e3-153">Synkronisera userPrincipalName uppdateringar</span><span class="sxs-lookup"><span data-stu-id="791e3-153">Synchronize userPrincipalName updates</span></span>
<span data-ttu-id="791e3-154">Tidigare har uppdateringar toohello attributet UserPrincipalName med hello synkroniseringstjänsten från lokala blockerats, om bägge dessa villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="791e3-154">Historically, updates toohello UserPrincipalName attribute using hello sync service from on-premises has been blocked, unless both of these conditions are true:</span></span>

* <span data-ttu-id="791e3-155">hello användaren hanteras (ofedererad).</span><span class="sxs-lookup"><span data-stu-id="791e3-155">hello user is managed (non-federated).</span></span>
* <span data-ttu-id="791e3-156">hello användaren har inte tilldelats en licens.</span><span class="sxs-lookup"><span data-stu-id="791e3-156">hello user has not been assigned a license.</span></span>

<span data-ttu-id="791e3-157">Mer information finns i [användarens namn i Office 365, Azure eller Intune inte matchar hello lokal UPN eller alternativ inloggnings-ID](https://support.microsoft.com/kb/2523192).</span><span class="sxs-lookup"><span data-stu-id="791e3-157">For more details, see [User names in Office 365, Azure, or Intune don't match hello on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192).</span></span>

<span data-ttu-id="791e3-158">Den här funktionen aktiveras kan hello sync motorn tooupdate hello userPrincipalName när den har ändrats lokalt och du använder Lösenordssynkronisering. Den här funktionen stöds inte om du använder federation.</span><span class="sxs-lookup"><span data-stu-id="791e3-158">Enabling this feature allows hello sync engine tooupdate hello userPrincipalName when it is changed on-premises and you use password sync. If you use federation, this feature is not supported.</span></span>

<span data-ttu-id="791e3-159">Den här funktionen är på skapas som standard för nya Azure AD-kataloger.</span><span class="sxs-lookup"><span data-stu-id="791e3-159">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="791e3-160">Du kan se om den här funktionen är aktiverad för du genom att köra:</span><span class="sxs-lookup"><span data-stu-id="791e3-160">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

<span data-ttu-id="791e3-161">Om den här funktionen inte har aktiverats för din Azure AD-katalog, kan du aktivera det genom att köra:</span><span class="sxs-lookup"><span data-stu-id="791e3-161">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

<span data-ttu-id="791e3-162">När den här funktionen, befintliga userPrincipalName värden förblir-är.</span><span class="sxs-lookup"><span data-stu-id="791e3-162">After enabling this feature, existing userPrincipalName values will remain as-is.</span></span> <span data-ttu-id="791e3-163">På Nästa ändring av hello userPrincipalName attributet lokal uppdaterar hello normal Deltasynkronisering på användare hello UPN.</span><span class="sxs-lookup"><span data-stu-id="791e3-163">On next change of hello userPrincipalName attribute on-premises, hello normal delta sync on users will update hello UPN.</span></span>  

## <a name="see-also"></a><span data-ttu-id="791e3-164">Se även</span><span class="sxs-lookup"><span data-stu-id="791e3-164">See also</span></span>
* [<span data-ttu-id="791e3-165">Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="791e3-165">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="791e3-166">[Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="791e3-166">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

