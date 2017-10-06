---
title: "Azure AD Connect: Välj installationstypen | Microsoft Docs"
description: "Det här avsnittet vägleder dig igenom hur tooselect hello installationen skriver toouse för Azure AD Connect"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a700e59eb05947ee1dbd9993141200c9340b2f1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="select-which-installation-type-toouse-for-azure-ad-connect"></a><span data-ttu-id="55ad7-103">Välj vilka installationen typen toouse för Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="55ad7-103">Select which installation type toouse for Azure AD Connect</span></span>
<span data-ttu-id="55ad7-104">Azure AD Connect har två typer av appinstallationer för nyinstallation: Express och anpassas.</span><span class="sxs-lookup"><span data-stu-id="55ad7-104">Azure AD Connect has two installation types for new installation: Express and customized.</span></span> <span data-ttu-id="55ad7-105">Det här avsnittet hjälper dig att toodecide som alternativet toouse under installationen.</span><span class="sxs-lookup"><span data-stu-id="55ad7-105">This topic helps you toodecide which option toouse during installation.</span></span>

## <a name="express"></a><span data-ttu-id="55ad7-106">Express</span><span class="sxs-lookup"><span data-stu-id="55ad7-106">Express</span></span>
<span data-ttu-id="55ad7-107">Snabb är de vanligaste hello-alternativ och används av ungefär 90% av alla nya installationer.</span><span class="sxs-lookup"><span data-stu-id="55ad7-107">Express is hello most common option and is used by about 90% of all new installations.</span></span> <span data-ttu-id="55ad7-108">Det var utformad tooprovide en konfiguration som passar för hello vanligaste scenarierna för kunden.</span><span class="sxs-lookup"><span data-stu-id="55ad7-108">It was designed tooprovide a configuration that works for hello most common customer scenarios.</span></span>

<span data-ttu-id="55ad7-109">Vi utgår från:</span><span class="sxs-lookup"><span data-stu-id="55ad7-109">It assumes:</span></span>

- <span data-ttu-id="55ad7-110">Du har en enda Active Directory-skogen lokalt.</span><span class="sxs-lookup"><span data-stu-id="55ad7-110">You have a single Active Directory forest on-premises.</span></span>
- <span data-ttu-id="55ad7-111">Du har ett enterprise-administratörskonto som du kan använda för hello installation.</span><span class="sxs-lookup"><span data-stu-id="55ad7-111">You have an enterprise administrator account you can use for hello installation.</span></span>
- <span data-ttu-id="55ad7-112">Du har mindre än 100 000 objekt i din lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="55ad7-112">You have less than 100,000 objects in your on-premises Active Directory.</span></span>

<span data-ttu-id="55ad7-113">Du får:</span><span class="sxs-lookup"><span data-stu-id="55ad7-113">You get:</span></span>

- <span data-ttu-id="55ad7-114">[Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) från lokala tooAzure AD för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="55ad7-114">[Password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) from on-premises tooAzure AD for single sign-on.</span></span>
- <span data-ttu-id="55ad7-115">En konfiguration som synkroniserar [användare, grupper, kontakter och Windows 10-datorer](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="55ad7-115">A configuration that synchronizes [users, groups, contacts, and Windows 10 computers](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
- <span data-ttu-id="55ad7-116">Synkroniseringen av alla tillgängliga objekt i alla domäner och alla organisationsenheter.</span><span class="sxs-lookup"><span data-stu-id="55ad7-116">Synchronization of all eligible objects in all domains and all OUs.</span></span>
- <span data-ttu-id="55ad7-117">[Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) är aktiverade toomake att du alltid använder hello senaste tillgängliga versionen.</span><span class="sxs-lookup"><span data-stu-id="55ad7-117">[Automatic upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) is enabled toomake sure you always use hello latest available version.</span></span>

<span data-ttu-id="55ad7-118">Alternativ där du kan fortfarande använda Express:</span><span class="sxs-lookup"><span data-stu-id="55ad7-118">Options where you can still use Express:</span></span>

- <span data-ttu-id="55ad7-119">Om du inte vill toosynchronize alla organisationsenheter, du kan fortfarande använda snabb och på hello sista sidan, avmarkera **starta synkroniseringsprocessen hello...** *.</span><span class="sxs-lookup"><span data-stu-id="55ad7-119">If you do not want toosynchronize all OUs, you can still use Express and on hello last page, unselect **Start hello synchronization process...***.</span></span> <span data-ttu-id="55ad7-120">Kör hello installationsguiden igen och ändra hello organisationsenheter i [konfigurationsalternativ](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) och aktivera schemalagd synkronisering.</span><span class="sxs-lookup"><span data-stu-id="55ad7-120">Then run hello installation wizard again and change hello OUs in [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) and enable scheduled sync.</span></span>
- <span data-ttu-id="55ad7-121">Vill du tooenable en av hello funktionerna i Azure AD Premium, till exempel tillbakaskrivning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="55ad7-121">You want tooenable one of hello features in Azure AD Premium, such as Password writeback.</span></span> <span data-ttu-id="55ad7-122">Först gå igenom express tooget hello inledande installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="55ad7-122">First go through express tooget hello initial installation completed.</span></span> <span data-ttu-id="55ad7-123">Kör hello installationsguiden igen och ändra hello [konfigurationsalternativ](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span><span class="sxs-lookup"><span data-stu-id="55ad7-123">Then run hello installation wizard again and change hello [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span></span>

## <a name="custom"></a><span data-ttu-id="55ad7-124">Anpassat</span><span class="sxs-lookup"><span data-stu-id="55ad7-124">Custom</span></span>
<span data-ttu-id="55ad7-125">hello anpassade sökvägen tillåter många fler alternativ än express.</span><span class="sxs-lookup"><span data-stu-id="55ad7-125">hello customized path allows many more options than express.</span></span> <span data-ttu-id="55ad7-126">Det ska användas i samtliga fall där hello konfigurationen som beskrivs i föregående avsnitt för snabba inte är representativa för din organisation.</span><span class="sxs-lookup"><span data-stu-id="55ad7-126">It should be used in all cases where hello configuration described in previous section for express is not representative for your organization.</span></span>

<span data-ttu-id="55ad7-127">Använd när:</span><span class="sxs-lookup"><span data-stu-id="55ad7-127">Use when:</span></span>

- <span data-ttu-id="55ad7-128">Du har inte åtkomst tooan företagsadministratörskonto i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="55ad7-128">You do not have access tooan enterprise admin account in Active Directory.</span></span>
- <span data-ttu-id="55ad7-129">Du har mer än en skog eller om du planerar toosynchronize mer än en skog i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="55ad7-129">You have more than one forest or you plan toosynchronize more than one forest in hello future.</span></span>
- <span data-ttu-id="55ad7-130">Du har domäner i skogen kan inte nås från hello Connect-servern.</span><span class="sxs-lookup"><span data-stu-id="55ad7-130">You have domains in your forest not reachable from hello Connect server.</span></span>
- <span data-ttu-id="55ad7-131">Du planerar toouse federation eller direktautentisering för användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="55ad7-131">You plan toouse federation or pass-through authentication for user sign-in.</span></span>
- <span data-ttu-id="55ad7-132">Du har mer än 100 000 objekt och behöver toouse en fullständig SQL Server.</span><span class="sxs-lookup"><span data-stu-id="55ad7-132">You have more than 100,000 objects and need toouse a full SQL Server.</span></span>
- <span data-ttu-id="55ad7-133">Du planerar toouse gruppbaserade filtrering och inte bara domän eller OU-baserade filtrering.</span><span class="sxs-lookup"><span data-stu-id="55ad7-133">You plan toouse group-based filtering and not only domain or OU-based filtering.</span></span>

## <a name="upgrade-from-dirsync"></a><span data-ttu-id="55ad7-134">Uppgradera från DirSync</span><span class="sxs-lookup"><span data-stu-id="55ad7-134">Upgrade from DirSync</span></span>
<span data-ttu-id="55ad7-135">Om du använder DirSync, följer du hello steg i [uppgradera från DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade den befintliga konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="55ad7-135">If you are currently using DirSync, then follow hello steps in [Upgrade from DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade your existing configuration.</span></span> <span data-ttu-id="55ad7-136">Det finns två olika uppgraderingsalternativ:</span><span class="sxs-lookup"><span data-stu-id="55ad7-136">There are two different upgrade options:</span></span>

- <span data-ttu-id="55ad7-137">Uppgradering på plats-tooinstall ansluta på hello samma server.</span><span class="sxs-lookup"><span data-stu-id="55ad7-137">In-place upgrade tooinstall Connect on hello same server.</span></span>
- <span data-ttu-id="55ad7-138">Parallell distribution tooinstall Connect på en ny server medan hello befintlig DirSync-server är fortfarande fungerar.</span><span class="sxs-lookup"><span data-stu-id="55ad7-138">Parallel deployment tooinstall Connect on a new server while hello existing DirSync server is still operational.</span></span>

## <a name="upgrade-from-azure-ad-sync"></a><span data-ttu-id="55ad7-139">Uppgradera från Azure AD Sync</span><span class="sxs-lookup"><span data-stu-id="55ad7-139">Upgrade from Azure AD Sync</span></span>
<span data-ttu-id="55ad7-140">Om du använder Azure AD Sync, så du kan följa hello [likadant](active-directory-aadconnect-upgrade-previous-version.md) som när du uppgraderar från en Anslut version tooa senare.</span><span class="sxs-lookup"><span data-stu-id="55ad7-140">If you are currently using Azure AD Sync, then you can follow hello [same steps](active-directory-aadconnect-upgrade-previous-version.md) as when you upgrade from one Connect version tooa newer.</span></span> <span data-ttu-id="55ad7-141">Det finns två olika uppgraderingsalternativ:</span><span class="sxs-lookup"><span data-stu-id="55ad7-141">There are two different upgrade options:</span></span>

- <span data-ttu-id="55ad7-142">Uppgradering på plats-tooinstall ansluta på hello samma server.</span><span class="sxs-lookup"><span data-stu-id="55ad7-142">In-place upgrade tooinstall Connect on hello same server.</span></span>
- <span data-ttu-id="55ad7-143">Öppning migrering tooinstall Connect på en ny server när hello befintliga Azure AD Sync-server är fortfarande fungerar.</span><span class="sxs-lookup"><span data-stu-id="55ad7-143">Swing-migration tooinstall Connect on a new server while hello existing Azure AD Sync server is still operational.</span></span>

## <a name="migrate-from-fim2010-or-mim2016"></a><span data-ttu-id="55ad7-144">Migrera från FIM2010 eller MIM2016</span><span class="sxs-lookup"><span data-stu-id="55ad7-144">Migrate from FIM2010 or MIM2016</span></span>
<span data-ttu-id="55ad7-145">Om du använder Forefront Identity Manager 2010 eller Microsoft Identity Manager 2016 med hello Azure AD-koppling, är det enda alternativet en migrering.</span><span class="sxs-lookup"><span data-stu-id="55ad7-145">If you are currently using Forefront Identity Manager 2010 or Microsoft Identity Manager 2016 with hello Azure AD Connector, then your only option is a migration.</span></span> <span data-ttu-id="55ad7-146">Följ hello stegen som beskrivs i [öppning migrering](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="55ad7-146">Follow hello steps described in [swing-migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span> <span data-ttu-id="55ad7-147">Ersätt nämns Azure AD Sync med FIM2010/MIM2016 i hello steg.</span><span class="sxs-lookup"><span data-stu-id="55ad7-147">In hello steps, replace any mention of Azure AD Sync with FIM2010/MIM2016.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55ad7-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55ad7-148">Next steps</span></span>
<span data-ttu-id="55ad7-149">Beroende på hello alternativ du har valt toouse använder hello tabell av innehåll toohello vänstra toofind din artikel med hello detaljerade steg.</span><span class="sxs-lookup"><span data-stu-id="55ad7-149">Depending on hello option you have selected toouse, use hello table of content toohello left toofind your article with hello detailed steps.</span></span>
