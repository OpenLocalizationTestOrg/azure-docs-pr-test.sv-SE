---
title: "Azure AD Connect sync tjänstens funktioner och konfiguration | Microsoft Docs"
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
ms.openlocfilehash: c2873510c280a2683c235cfdce3d2617c3b665cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-service-features"></a><span data-ttu-id="854d7-103">Azure AD Connect sync-tjänsten-funktioner</span><span class="sxs-lookup"><span data-stu-id="854d7-103">Azure AD Connect sync service features</span></span>
<span data-ttu-id="854d7-104">Synkroniseringsfunktionen av Azure AD Connect har två komponenter:</span><span class="sxs-lookup"><span data-stu-id="854d7-104">The synchronization feature of Azure AD Connect has two components:</span></span>

* <span data-ttu-id="854d7-105">Lokala komponenten **Azure AD Connect-synkronisering**, även kallat **Synkroniseringsmotorn**.</span><span class="sxs-lookup"><span data-stu-id="854d7-105">The on-premises component named **Azure AD Connect sync**, also called **sync engine**.</span></span>
* <span data-ttu-id="854d7-106">Tjänsten som finns i Azure AD så kallade **Azure AD Connect-synkroniseringstjänsten**</span><span class="sxs-lookup"><span data-stu-id="854d7-106">The service residing in Azure AD also known as **Azure AD Connect sync service**</span></span>

<span data-ttu-id="854d7-107">Det här avsnittet beskrivs hur följande funktioner i den **Azure AD Connect-synkroniseringstjänsten** fungerar och hur du kan konfigurera dem med hjälp av Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="854d7-107">This topic explains how the following features of the **Azure AD Connect sync service** work and how you can configure them using Windows PowerShell.</span></span>

<span data-ttu-id="854d7-108">De här inställningarna är konfigurerade med den [Azure Active Directory-modulen för Windows PowerShell](http://aka.ms/aadposh).</span><span class="sxs-lookup"><span data-stu-id="854d7-108">These settings are configured by the [Azure Active Directory Module for Windows PowerShell](http://aka.ms/aadposh).</span></span> <span data-ttu-id="854d7-109">Hämta och installera det separat från Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="854d7-109">Download and install it separately from Azure AD Connect.</span></span> <span data-ttu-id="854d7-110">De cmdlets som beskrivs i det här avsnittet har introducerats i den [2016 mars-versionen (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span><span class="sxs-lookup"><span data-stu-id="854d7-110">The cmdlets documented in this topic were introduced in the [2016 March release (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span></span> <span data-ttu-id="854d7-111">Om du inte har de cmdlets som beskrivs i det här avsnittet eller om de inte ger samma resultat, se till att du kör den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="854d7-111">If you do not have the cmdlets documented in this topic or they do not produce the same result, then make sure you run the latest version.</span></span>

<span data-ttu-id="854d7-112">Om du vill se konfigurationen i Azure AD-katalogen kör `Get-MsolDirSyncFeatures`.</span><span class="sxs-lookup"><span data-stu-id="854d7-112">To see the configuration in your Azure AD directory, run `Get-MsolDirSyncFeatures`.</span></span>  
<span data-ttu-id="854d7-113">![Get-MsolDirSyncFeatures resultat](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span><span class="sxs-lookup"><span data-stu-id="854d7-113">![Get-MsolDirSyncFeatures result](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span></span>

<span data-ttu-id="854d7-114">Många av dessa inställningar kan bara ändras av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="854d7-114">Many of these settings can only be changed by Azure AD Connect.</span></span>

<span data-ttu-id="854d7-115">Följande inställningar kan konfigureras med `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="854d7-115">The following settings can be configured by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="854d7-116">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="854d7-116">DirSyncFeature</span></span> | <span data-ttu-id="854d7-117">Kommentar</span><span class="sxs-lookup"><span data-stu-id="854d7-117">Comment</span></span> |
| --- | --- |
| [<span data-ttu-id="854d7-118">EnableSoftMatchOnUpn</span><span class="sxs-lookup"><span data-stu-id="854d7-118">EnableSoftMatchOnUpn</span></span>](#userprincipalname-soft-match) |<span data-ttu-id="854d7-119">Gör att objekt som ska delta i userPrincipalName utöver primära SMTP-adress.</span><span class="sxs-lookup"><span data-stu-id="854d7-119">Allows objects to join on userPrincipalName in addition to primary SMTP address.</span></span> |
| [<span data-ttu-id="854d7-120">SynchronizeUpnForManagedUsers</span><span class="sxs-lookup"><span data-stu-id="854d7-120">SynchronizeUpnForManagedUsers</span></span>](#synchronize-userprincipalname-updates) |<span data-ttu-id="854d7-121">Gör att Synkroniseringsmotorn att uppdatera attributet userPrincipalName för hanterade/licensierade (ofedererad) användare.</span><span class="sxs-lookup"><span data-stu-id="854d7-121">Allows the sync engine to update the userPrincipalName attribute for managed/licensed (non-federated) users.</span></span> |

<span data-ttu-id="854d7-122">När du har aktiverat en funktion kan inaktiveras det inte igen.</span><span class="sxs-lookup"><span data-stu-id="854d7-122">After you have enabled a feature, it cannot be disabled again.</span></span>

> [!NOTE]
> <span data-ttu-id="854d7-123">Från 24 augusti 2016 funktionen *duplicera attribut återhämtning* är aktiverad som standard för nya Azure AD-kataloger.</span><span class="sxs-lookup"><span data-stu-id="854d7-123">From August 24, 2016 the feature *Duplicate attribute resiliency* is enabled by default for new Azure AD directories.</span></span> <span data-ttu-id="854d7-124">Den här funktionen kommer även distribuerat och aktiverad på kataloger som skapats före detta datum.</span><span class="sxs-lookup"><span data-stu-id="854d7-124">This feature will also be rolled out and enabled on directories created before this date.</span></span> <span data-ttu-id="854d7-125">Du får ett e-postmeddelande när din katalog kommer att få den här funktionen aktiverad.</span><span class="sxs-lookup"><span data-stu-id="854d7-125">You will receive an email notification when your directory is about to get this feature enabled.</span></span>
> 
> 

<span data-ttu-id="854d7-126">Följande inställningar konfigureras med Azure AD Connect och kan inte ändras av `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="854d7-126">The following settings are configured by Azure AD Connect and cannot be modified by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="854d7-127">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="854d7-127">DirSyncFeature</span></span> | <span data-ttu-id="854d7-128">Kommentar</span><span class="sxs-lookup"><span data-stu-id="854d7-128">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="854d7-129">DeviceWriteback</span><span class="sxs-lookup"><span data-stu-id="854d7-129">DeviceWriteback</span></span> |[<span data-ttu-id="854d7-130">Azure AD Connect: Aktivera tillbakaskrivning av enheter</span><span class="sxs-lookup"><span data-stu-id="854d7-130">Azure AD Connect: Enabling device writeback</span></span>](active-directory-aadconnect-feature-device-writeback.md) |
| <span data-ttu-id="854d7-131">DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="854d7-131">DirectoryExtensions</span></span> |[<span data-ttu-id="854d7-132">Azure AD Connect-synkronisering: katalogtillägg</span><span class="sxs-lookup"><span data-stu-id="854d7-132">Azure AD Connect sync: Directory extensions</span></span>](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [<span data-ttu-id="854d7-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span><span class="sxs-lookup"><span data-stu-id="854d7-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span></span>](#duplicate-attribute-resiliency) |<span data-ttu-id="854d7-134">Tillåter ett attribut som ska placeras i karantän när det är en dubblett av ett annat objekt i stället misslyckas hela objektet under exporten.</span><span class="sxs-lookup"><span data-stu-id="854d7-134">Allows an attribute to be quarantined when it is a duplicate of another object rather than failing the entire object during export.</span></span> |
| <span data-ttu-id="854d7-135">PasswordSync</span><span class="sxs-lookup"><span data-stu-id="854d7-135">PasswordSync</span></span> |[<span data-ttu-id="854d7-136">Implementera Lösenordssynkronisering med Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="854d7-136">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md) |
| <span data-ttu-id="854d7-137">UnifiedGroupWriteback</span><span class="sxs-lookup"><span data-stu-id="854d7-137">UnifiedGroupWriteback</span></span> |[<span data-ttu-id="854d7-138">Förhandsversion: Tillbakaskrivning av grupp</span><span class="sxs-lookup"><span data-stu-id="854d7-138">Preview: Group writeback</span></span>](active-directory-aadconnect-feature-preview.md#group-writeback) |
| <span data-ttu-id="854d7-139">UserWriteback</span><span class="sxs-lookup"><span data-stu-id="854d7-139">UserWriteback</span></span> |<span data-ttu-id="854d7-140">Stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="854d7-140">Not currently supported.</span></span> |

## <a name="duplicate-attribute-resiliency"></a><span data-ttu-id="854d7-141">Duplicerat attribut återhämtning</span><span class="sxs-lookup"><span data-stu-id="854d7-141">Duplicate attribute resiliency</span></span>
<span data-ttu-id="854d7-142">I stället för misslyckas att etablera objekt med dubblerade UPN-namn / proxyAddresses, duplicerade attributet ”i karantän” och ett tillfälligt värde är tilldelad.</span><span class="sxs-lookup"><span data-stu-id="854d7-142">Instead of failing to provision objects with duplicate UPNs / proxyAddresses, the duplicated attribute is “quarantined” and a temporary value is assigned.</span></span> <span data-ttu-id="854d7-143">När konflikten löses ändras tillfälliga UPN automatiskt till rätt värde.</span><span class="sxs-lookup"><span data-stu-id="854d7-143">When the conflict is resolved, the temporary UPN is changed to the proper value automatically.</span></span> <span data-ttu-id="854d7-144">Mer information finns i [identitet synkronisering och dubblett attributet återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="854d7-144">For more details, see [Identity synchronization and duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="userprincipalname-soft-match"></a><span data-ttu-id="854d7-145">UserPrincipalName mjuka matchning</span><span class="sxs-lookup"><span data-stu-id="854d7-145">UserPrincipalName soft match</span></span>
<span data-ttu-id="854d7-146">När den här funktionen är aktiverad, mjuk-matcha är aktiverad för UPN förutom den [primära SMTP-adress](https://support.microsoft.com/kb/2641663), som alltid är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="854d7-146">When this feature is enabled, soft-match is enabled for UPN in addition to the [primary SMTP address](https://support.microsoft.com/kb/2641663), which is always enabled.</span></span> <span data-ttu-id="854d7-147">Soft-matcha används för att matcha befintliga molnanvändare i Azure AD med lokala användare.</span><span class="sxs-lookup"><span data-stu-id="854d7-147">Soft-match is used to match existing cloud users in Azure AD with on-premises users.</span></span>

<span data-ttu-id="854d7-148">Om du behöver matcha lokala AD-konton med befintliga konton som skapats i molnet och du använder inte Exchange Online och sedan den här funktionen är användbart.</span><span class="sxs-lookup"><span data-stu-id="854d7-148">If you need to match on-premises AD accounts with existing accounts created in the cloud and you are not using Exchange Online, then this feature is useful.</span></span> <span data-ttu-id="854d7-149">I det här scenariot kan du vanligtvis inte en orsak till att ange SMTP-attributet i molnet.</span><span class="sxs-lookup"><span data-stu-id="854d7-149">In this scenario, you generally don’t have a reason to set the SMTP attribute in the cloud.</span></span>

<span data-ttu-id="854d7-150">Den här funktionen är på skapas som standard för nya Azure AD-kataloger.</span><span class="sxs-lookup"><span data-stu-id="854d7-150">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="854d7-151">Du kan se om den här funktionen är aktiverad för du genom att köra:</span><span class="sxs-lookup"><span data-stu-id="854d7-151">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

<span data-ttu-id="854d7-152">Om den här funktionen inte har aktiverats för din Azure AD-katalog, kan du aktivera det genom att köra:</span><span class="sxs-lookup"><span data-stu-id="854d7-152">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a><span data-ttu-id="854d7-153">Synkronisera userPrincipalName uppdateringar</span><span class="sxs-lookup"><span data-stu-id="854d7-153">Synchronize userPrincipalName updates</span></span>
<span data-ttu-id="854d7-154">Tidigare uppdateringar för attributet UserPrincipalName med synkroniseringstjänsten från lokala har blockerats, om bägge dessa villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="854d7-154">Historically, updates to the UserPrincipalName attribute using the sync service from on-premises has been blocked, unless both of these conditions are true:</span></span>

* <span data-ttu-id="854d7-155">Användaren hanteras (ofedererad).</span><span class="sxs-lookup"><span data-stu-id="854d7-155">The user is managed (non-federated).</span></span>
* <span data-ttu-id="854d7-156">Användaren har inte tilldelats en licens.</span><span class="sxs-lookup"><span data-stu-id="854d7-156">The user has not been assigned a license.</span></span>

<span data-ttu-id="854d7-157">Mer information finns i [användarnamnen i Office 365, Azure eller Intune matchar inte lokal UPN eller alternativ inloggnings-ID](https://support.microsoft.com/kb/2523192).</span><span class="sxs-lookup"><span data-stu-id="854d7-157">For more details, see [User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192).</span></span>

<span data-ttu-id="854d7-158">Den här funktionen aktiveras kan Synkroniseringsmotorn att uppdatera userPrincipalName när den har ändrats lokalt och du använder Lösenordssynkronisering.</span><span class="sxs-lookup"><span data-stu-id="854d7-158">Enabling this feature allows the sync engine to update the userPrincipalName when it is changed on-premises and you use password sync.</span></span> <span data-ttu-id="854d7-159">Den här funktionen stöds inte om du använder federation.</span><span class="sxs-lookup"><span data-stu-id="854d7-159">If you use federation, this feature is not supported.</span></span>

<span data-ttu-id="854d7-160">Den här funktionen är på skapas som standard för nya Azure AD-kataloger.</span><span class="sxs-lookup"><span data-stu-id="854d7-160">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="854d7-161">Du kan se om den här funktionen är aktiverad för du genom att köra:</span><span class="sxs-lookup"><span data-stu-id="854d7-161">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

<span data-ttu-id="854d7-162">Om den här funktionen inte har aktiverats för din Azure AD-katalog, kan du aktivera det genom att köra:</span><span class="sxs-lookup"><span data-stu-id="854d7-162">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

<span data-ttu-id="854d7-163">När den här funktionen, befintliga userPrincipalName värden förblir-är.</span><span class="sxs-lookup"><span data-stu-id="854d7-163">After enabling this feature, existing userPrincipalName values will remain as-is.</span></span> <span data-ttu-id="854d7-164">På Nästa ändring av userPrincipalName attributet lokal uppdaterar normal Deltasynkronisering på användare UPN.</span><span class="sxs-lookup"><span data-stu-id="854d7-164">On next change of the userPrincipalName attribute on-premises, the normal delta sync on users will update the UPN.</span></span>  

## <a name="see-also"></a><span data-ttu-id="854d7-165">Se även</span><span class="sxs-lookup"><span data-stu-id="854d7-165">See also</span></span>
* [<span data-ttu-id="854d7-166">Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="854d7-166">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="854d7-167">[Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="854d7-167">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

