---
title: "aaaConfigure gruppinställningar med hjälp av Azure Active Directory-cmdletarna | Microsoft Docs"
description: "Hur hanterar hello inställningar för grupper med hjälp av Azure Active Directory-cmdlets"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 9f2090e6-3af4-4f07-bbb2-1d18dae89b73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;
ms.openlocfilehash: 46db49d9dec3eaa41c97ca74c57854189eddc16d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a><span data-ttu-id="69cde-103">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="69cde-103">Azure Active Directory cmdlets for configuring group settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="69cde-104">Det här innehållet gäller endast tooOffice 365 grupper.</span><span class="sxs-lookup"><span data-stu-id="69cde-104">This content applies only tooOffice 365 groups.</span></span> <span data-ttu-id="69cde-105">Mer information om hur tooallow användare toocreate säkerhetsgrupper, ställa in `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` enligt beskrivningen i [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span><span class="sxs-lookup"><span data-stu-id="69cde-105">For more information on how tooallow users toocreate Security groups, set `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` as described in [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span></span> 

<span data-ttu-id="69cde-106">Inställningar för Office 365-grupper har konfigurerats med ett inställningsobjekt och ett SettingsTemplate-objekt.</span><span class="sxs-lookup"><span data-stu-id="69cde-106">Office 365 Groups settings are configured using a Settings object and a SettingsTemplate object.</span></span> <span data-ttu-id="69cde-107">Inledningsvis visas alla för inställningsobjekt i katalogen, eftersom katalogen är konfigurerad med hello standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="69cde-107">Initially, you don't see any Settings objects in your directory, because your directory is configured with hello default settings.</span></span> <span data-ttu-id="69cde-108">standardinställningarna för toochange hello, måste du skapa ett nytt inställningsobjekt med en mall för inställningar.</span><span class="sxs-lookup"><span data-stu-id="69cde-108">toochange hello default settings, you must create a new settings object using a settings template.</span></span> <span data-ttu-id="69cde-109">Inställningar för mallar har definierats av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="69cde-109">Settings templates are defined by Microsoft.</span></span> <span data-ttu-id="69cde-110">Det finns flera olika inställningar mallar.</span><span class="sxs-lookup"><span data-stu-id="69cde-110">There are several different settings templates.</span></span> <span data-ttu-id="69cde-111">tooconfigure Office 365 gruppinställningar för din katalog du använder hello mall med namnet ”Group.Unified”.</span><span class="sxs-lookup"><span data-stu-id="69cde-111">tooconfigure Office 365 group settings for your directory, you use hello template named "Group.Unified".</span></span> <span data-ttu-id="69cde-112">inställningar för tooconfigure Office 365-grupper på en enda grupp Använd hello mall med namnet ”Group.Unified.Guest”.</span><span class="sxs-lookup"><span data-stu-id="69cde-112">tooconfigure Office 365 group settings on a single group, use hello template named "Group.Unified.Guest".</span></span> <span data-ttu-id="69cde-113">Den här mallen är används toomanage gäst åtkomstgruppen tooan Office 365.</span><span class="sxs-lookup"><span data-stu-id="69cde-113">This template is used toomanage guest access tooan Office 365 group.</span></span> 

<span data-ttu-id="69cde-114">hello cmdlets är en del av hello Azure Active Directory PowerShell V2-modulen.</span><span class="sxs-lookup"><span data-stu-id="69cde-114">hello cmdlets are part of hello Azure Active Directory PowerShell V2 module.</span></span> <span data-ttu-id="69cde-115">Anvisningar hur toodownload och installera hello-modulen på din dator, se hello artikel [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).</span><span class="sxs-lookup"><span data-stu-id="69cde-115">For instructions how toodownload and install hello module on your computer, see hello article [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).</span></span> <span data-ttu-id="69cde-116">Du kan installera hello version 2-versionen av hello modul från [hello PowerShell-galleriet](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="69cde-116">You can install hello version 2 release of hello module from [hello PowerShell gallery](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="retrieve-a-specific-settings-value"></a><span data-ttu-id="69cde-117">Hämta ett värde för specifika inställningar</span><span class="sxs-lookup"><span data-stu-id="69cde-117">Retrieve a specific settings value</span></span>
<span data-ttu-id="69cde-118">Om du känner till hello namn hello inställning du vill tooretrieve kan du använda hello nedan cmdlet tooretrieve hello aktuella inställningar för värdet.</span><span class="sxs-lookup"><span data-stu-id="69cde-118">If you know hello name of hello setting you want tooretrieve, you can use hello below cmdlet tooretrieve hello current settings value.</span></span> <span data-ttu-id="69cde-119">I det här exemplet hämtar vi hello-värdet för en inställning med namnet ”UsageGuidelinesUrl”.</span><span class="sxs-lookup"><span data-stu-id="69cde-119">In this example, we're retrieving hello value for a setting named "UsageGuidelinesUrl."</span></span> <span data-ttu-id="69cde-120">Du kan läsa mer om inställningar och deras namn ytterligare ned i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="69cde-120">You can read more about directory settings and their names further down in this article.</span></span>

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-hello-directory-level"></a><span data-ttu-id="69cde-121">Skapa inställningar på hello katalog-nivå</span><span class="sxs-lookup"><span data-stu-id="69cde-121">Create settings at hello directory level</span></span>
<span data-ttu-id="69cde-122">De här stegen skapa inställningar på katalognivå, som gäller tooall Office 365-grupper (Unified grupper) i hello directory.</span><span class="sxs-lookup"><span data-stu-id="69cde-122">These steps create settings at directory level, which apply tooall Office 365 groups (Unified groups) in hello directory.</span></span>

1. <span data-ttu-id="69cde-123">I hello DirectorySettings cmdlets, måste du ange hello-ID för hello SettingsTemplate som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="69cde-123">In hello DirectorySettings cmdlets, you must specify hello ID of hello SettingsTemplate you want toouse.</span></span> <span data-ttu-id="69cde-124">Om du inte vet detta ID returnerar denna cmdlet hello lista med alla inställningar-mallar:</span><span class="sxs-lookup"><span data-stu-id="69cde-124">If you do not know this ID, this cmdlet returns hello list of all settings templates:</span></span>
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  <span data-ttu-id="69cde-125">Det här anropet cmdlet returnerar alla mallar som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="69cde-125">This cmdlet call returns all templates that are available:</span></span>
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define hello different settings that can be used for hello associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. <span data-ttu-id="69cde-126">tooadd riktlinje URL-Adressen för användning, måste du först tooget hello SettingsTemplate objekt som definierar hello användning riktlinje URL-värdet; det vill säga hello Group.Unified mallen:</span><span class="sxs-lookup"><span data-stu-id="69cde-126">tooadd a usage guideline URL, first you need tooget hello SettingsTemplate object that defines hello usage guideline URL value; that is, hello Group.Unified template:</span></span>
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. <span data-ttu-id="69cde-127">Därefter skapa ett nytt för inställningsobjekt som baseras på mallen:</span><span class="sxs-lookup"><span data-stu-id="69cde-127">Next, create a new settings object based on that template:</span></span>
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. <span data-ttu-id="69cde-128">Uppdatera sedan hello användarvärde riktlinjer:</span><span class="sxs-lookup"><span data-stu-id="69cde-128">Then update hello usage guideline value:</span></span>
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. <span data-ttu-id="69cde-129">Slutligen gäller hello inställningar:</span><span class="sxs-lookup"><span data-stu-id="69cde-129">Finally, apply hello settings:</span></span>
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

<span data-ttu-id="69cde-130">Vid slutförande returnerar cmdlet hello hello-ID för hello nya inställningsobjektet:</span><span class="sxs-lookup"><span data-stu-id="69cde-130">Upon successful completion, hello cmdlet returns hello ID of hello new settings object:</span></span>
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
<span data-ttu-id="69cde-131">Här följer hello inställningarna i hello Group.Unified SettingsTemplate.</span><span class="sxs-lookup"><span data-stu-id="69cde-131">Here are hello settings defined in hello Group.Unified SettingsTemplate.</span></span>

| <span data-ttu-id="69cde-132">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="69cde-132">**Setting**</span></span> | <span data-ttu-id="69cde-133">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="69cde-133">**Description**</span></span> |
| --- | --- |
|  <ul><li><span data-ttu-id="69cde-134">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="69cde-134">EnableGroupCreation</span></span><li><span data-ttu-id="69cde-135">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="69cde-135">Type: Boolean</span></span><li><span data-ttu-id="69cde-136">Standard: True</span><span class="sxs-lookup"><span data-stu-id="69cde-136">Default: True</span></span> |<span data-ttu-id="69cde-137">hello flagga som indikerar om skapande av en enhetlig grupp tillåts i hello directory.</span><span class="sxs-lookup"><span data-stu-id="69cde-137">hello flag indicating whether Unified Group creation is allowed in hello directory.</span></span> |
|  <ul><li><span data-ttu-id="69cde-138">GroupCreationAllowedGroupId</span><span class="sxs-lookup"><span data-stu-id="69cde-138">GroupCreationAllowedGroupId</span></span><li><span data-ttu-id="69cde-139">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="69cde-139">Type: String</span></span><li><span data-ttu-id="69cde-140">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="69cde-140">Default: “”</span></span> |<span data-ttu-id="69cde-141">GUID för hello säkerhetsgrupp för vilka hello medlemmar får toocreate Unified grupper även om EnableGroupCreation == false.</span><span class="sxs-lookup"><span data-stu-id="69cde-141">GUID of hello security group for which hello members are allowed toocreate Unified Groups even when EnableGroupCreation == false.</span></span> |
|  <ul><li><span data-ttu-id="69cde-142">UsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="69cde-142">UsageGuidelinesUrl</span></span><li><span data-ttu-id="69cde-143">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="69cde-143">Type: String</span></span><li><span data-ttu-id="69cde-144">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="69cde-144">Default: “”</span></span> |<span data-ttu-id="69cde-145">Länk-toohello riktlinjer för användning av grupp.</span><span class="sxs-lookup"><span data-stu-id="69cde-145">A link toohello Group Usage Guidelines.</span></span> |
|  <ul><li><span data-ttu-id="69cde-146">ClassificationDescriptions</span><span class="sxs-lookup"><span data-stu-id="69cde-146">ClassificationDescriptions</span></span><li><span data-ttu-id="69cde-147">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="69cde-147">Type: String</span></span><li><span data-ttu-id="69cde-148">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="69cde-148">Default: “”</span></span> | <span data-ttu-id="69cde-149">En kommaavgränsad lista över klassificering beskrivningar.</span><span class="sxs-lookup"><span data-stu-id="69cde-149">A comma-delimited list of classification descriptions.</span></span> |
|  <ul><li><span data-ttu-id="69cde-150">DefaultClassification</span><span class="sxs-lookup"><span data-stu-id="69cde-150">DefaultClassification</span></span><li><span data-ttu-id="69cde-151">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="69cde-151">Type: String</span></span><li><span data-ttu-id="69cde-152">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="69cde-152">Default: “”</span></span> | <span data-ttu-id="69cde-153">hello klassificering som är toobe som används som hello standardklassificering för en grupp om inget har angetts.</span><span class="sxs-lookup"><span data-stu-id="69cde-153">hello classification that is toobe used as hello default classification for a group if none was specified.</span></span>|
|  <ul><li><span data-ttu-id="69cde-154">PrefixSuffixNamingRequirement</span><span class="sxs-lookup"><span data-stu-id="69cde-154">PrefixSuffixNamingRequirement</span></span><li><span data-ttu-id="69cde-155">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="69cde-155">Type: String</span></span><li><span data-ttu-id="69cde-156">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="69cde-156">Default: “”</span></span> |<span data-ttu-id="69cde-157">Inte implementerat ännu</span><span class="sxs-lookup"><span data-stu-id="69cde-157">Not implemented yet</span></span>
|  <ul><li><span data-ttu-id="69cde-158">AllowGuestsToBeGroupOwner</span><span class="sxs-lookup"><span data-stu-id="69cde-158">AllowGuestsToBeGroupOwner</span></span><li><span data-ttu-id="69cde-159">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="69cde-159">Type: Boolean</span></span><li><span data-ttu-id="69cde-160">Standard: False</span><span class="sxs-lookup"><span data-stu-id="69cde-160">Default: False</span></span> | <span data-ttu-id="69cde-161">Booleskt värde som anger huruvida en gästanvändare kan vara en ägare av grupper.</span><span class="sxs-lookup"><span data-stu-id="69cde-161">Boolean indicating whether or not a guest user can be an owner of groups.</span></span> |
|  <ul><li><span data-ttu-id="69cde-162">AllowGuestsToAccessGroups</span><span class="sxs-lookup"><span data-stu-id="69cde-162">AllowGuestsToAccessGroups</span></span><li><span data-ttu-id="69cde-163">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="69cde-163">Type: Boolean</span></span><li><span data-ttu-id="69cde-164">Standard: True</span><span class="sxs-lookup"><span data-stu-id="69cde-164">Default: True</span></span> | <span data-ttu-id="69cde-165">Booleskt värde som anger huruvida en gästanvändare kan ha åtkomst tooUnified grupper innehåll.</span><span class="sxs-lookup"><span data-stu-id="69cde-165">Boolean indicating whether or not a guest user can have access tooUnified groups' content.</span></span> |
|  <ul><li><span data-ttu-id="69cde-166">GuestUsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="69cde-166">GuestUsageGuidelinesUrl</span></span><li><span data-ttu-id="69cde-167">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="69cde-167">Type: String</span></span><li><span data-ttu-id="69cde-168">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="69cde-168">Default: “”</span></span> | <span data-ttu-id="69cde-169">hello-url för riktlinjer för en länk toohello gäst-användning.</span><span class="sxs-lookup"><span data-stu-id="69cde-169">hello url of a link toohello guest usage guidelines.</span></span> |
|  <ul><li><span data-ttu-id="69cde-170">AllowToAddGuests</span><span class="sxs-lookup"><span data-stu-id="69cde-170">AllowToAddGuests</span></span><li><span data-ttu-id="69cde-171">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="69cde-171">Type: Boolean</span></span><li><span data-ttu-id="69cde-172">Standard: True</span><span class="sxs-lookup"><span data-stu-id="69cde-172">Default: True</span></span> | <span data-ttu-id="69cde-173">Ett booleskt värde som anger om eller inte är tillåtna tooadd gäster toothis directory.</span><span class="sxs-lookup"><span data-stu-id="69cde-173">A boolean indicating whether or not is allowed tooadd guests toothis directory.</span></span>|
|  <ul><li><span data-ttu-id="69cde-174">ClassificationList</span><span class="sxs-lookup"><span data-stu-id="69cde-174">ClassificationList</span></span><li><span data-ttu-id="69cde-175">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="69cde-175">Type: String</span></span><li><span data-ttu-id="69cde-176">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="69cde-176">Default: “”</span></span> |<span data-ttu-id="69cde-177">En kommaavgränsad lista över giltiga klassificeringsvärden som kan vara tillämpade tooUnified grupper.</span><span class="sxs-lookup"><span data-stu-id="69cde-177">A comma-delimited list of valid classification values that can be applied tooUnified Groups.</span></span> |
|  <ul><li><span data-ttu-id="69cde-178">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="69cde-178">EnableGroupCreation</span></span><li><span data-ttu-id="69cde-179">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="69cde-179">Type: Boolean</span></span><li><span data-ttu-id="69cde-180">Standard: True</span><span class="sxs-lookup"><span data-stu-id="69cde-180">Default: True</span></span> | <span data-ttu-id="69cde-181">Ett booleskt värde som anger om icke-administratörer kan skapa en ny enhetlig grupper.</span><span class="sxs-lookup"><span data-stu-id="69cde-181">A boolean indicating whether or not non-admin users can create new Unified groups.</span></span> |


## <a name="read-settings-at-hello-directory-level"></a><span data-ttu-id="69cde-182">Läsinställningar på hello katalog-nivå</span><span class="sxs-lookup"><span data-stu-id="69cde-182">Read settings at hello directory level</span></span>
<span data-ttu-id="69cde-183">De här stegen läsa inställningar på katalognivå, som gäller tooall Office-grupper i hello directory.</span><span class="sxs-lookup"><span data-stu-id="69cde-183">These steps read settings at directory level, which apply tooall Office groups in hello directory.</span></span>

1. <span data-ttu-id="69cde-184">Läsa alla befintliga directory-inställningar:</span><span class="sxs-lookup"><span data-stu-id="69cde-184">Read all existing directory settings:</span></span>
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  <span data-ttu-id="69cde-185">Denna cmdlet returnerar en lista över alla inställningar:</span><span class="sxs-lookup"><span data-stu-id="69cde-185">This cmdlet returns a list of all directory settings:</span></span>
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. <span data-ttu-id="69cde-186">Läsa alla inställningar för en viss grupp:</span><span class="sxs-lookup"><span data-stu-id="69cde-186">Read all settings for a specific group:</span></span>
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. <span data-ttu-id="69cde-187">Läsa alla värden som directory inställningar i en specifik katalog settings-objekt med hjälp av inställningarna för Id-GUID:</span><span class="sxs-lookup"><span data-stu-id="69cde-187">Read all directory settings values of a specific directory settings object, using Settings Id GUID:</span></span>
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  <span data-ttu-id="69cde-188">Denna cmdlet returnerar hello namn och värden i det här inställningsobjektet för den här specifika gruppen:</span><span class="sxs-lookup"><span data-stu-id="69cde-188">This cmdlet returns hello names and values in this settings object for this specific group:</span></span>
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            <https://guideline.com>
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a><span data-ttu-id="69cde-189">Uppdatera inställningarna för en specifik grupp</span><span class="sxs-lookup"><span data-stu-id="69cde-189">Update settings for a specific group</span></span>

1. <span data-ttu-id="69cde-190">Sök efter hello inställningar mall med namnet ”Groups.Unified.Guest”</span><span class="sxs-lookup"><span data-stu-id="69cde-190">Search for hello settings template named "Groups.Unified.Guest"</span></span>
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Unified Group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. <span data-ttu-id="69cde-191">Hämta hello mallobjekt hello Groups.Unified.Guest mallen:</span><span class="sxs-lookup"><span data-stu-id="69cde-191">Retrieve hello template object for hello Groups.Unified.Guest template:</span></span>
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. <span data-ttu-id="69cde-192">Skapa ett nytt inställningsobjekt från hello mall:</span><span class="sxs-lookup"><span data-stu-id="69cde-192">Create a new settings object from hello template:</span></span>
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. <span data-ttu-id="69cde-193">Ange hello Inställningsvärdet toohello som krävs:</span><span class="sxs-lookup"><span data-stu-id="69cde-193">Set hello setting toohello required value:</span></span>
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. <span data-ttu-id="69cde-194">Skapa ny hello-inställning för hello nödvändiga gruppen i hello directory:</span><span class="sxs-lookup"><span data-stu-id="69cde-194">Create hello new setting for hello required group in hello directory:</span></span>
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-hello-directory-level"></a><span data-ttu-id="69cde-195">Uppdatera inställningar på hello katalog-nivå</span><span class="sxs-lookup"><span data-stu-id="69cde-195">Update settings at hello directory level</span></span>

<span data-ttu-id="69cde-196">De här stegen uppdatera inställningar på katalognivå, som gäller tooall Unified grupper i hello directory.</span><span class="sxs-lookup"><span data-stu-id="69cde-196">These steps update settings at directory level, which apply tooall Unified groups in hello directory.</span></span> <span data-ttu-id="69cde-197">De här exemplen antar att det finns redan ett inställningsobjekt i din katalog.</span><span class="sxs-lookup"><span data-stu-id="69cde-197">These examples assume there is already a Settings object in your directory.</span></span>

1. <span data-ttu-id="69cde-198">Hitta hello befintligt inställningsobjekt:</span><span class="sxs-lookup"><span data-stu-id="69cde-198">Find hello existing Settings object:</span></span>
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. <span data-ttu-id="69cde-199">Uppdatera hello värdet:</span><span class="sxs-lookup"><span data-stu-id="69cde-199">Update hello value:</span></span>
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. <span data-ttu-id="69cde-200">Uppdatera hello inställning:</span><span class="sxs-lookup"><span data-stu-id="69cde-200">Update hello setting:</span></span>
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-hello-directory-level"></a><span data-ttu-id="69cde-201">Ta bort inställningar på hello katalog-nivå</span><span class="sxs-lookup"><span data-stu-id="69cde-201">Remove settings at hello directory level</span></span>
<span data-ttu-id="69cde-202">Det här steget tar bort inställningar på katalognivå, som gäller tooall Office-grupper i hello directory.</span><span class="sxs-lookup"><span data-stu-id="69cde-202">This step removes settings at directory level, which apply tooall Office groups in hello directory.</span></span>
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a><span data-ttu-id="69cde-203">Cmdlet-referens för syntax</span><span class="sxs-lookup"><span data-stu-id="69cde-203">Cmdlet syntax reference</span></span>
<span data-ttu-id="69cde-204">Du kan hitta mer Azure Active Directory PowerShell-dokumentationen på [Azure Active Directory-cmdletarna](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="69cde-204">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="additional-reading"></a><span data-ttu-id="69cde-205">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="69cde-205">Additional reading</span></span>

* [<span data-ttu-id="69cde-206">Hantera åtkomst tooresources med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="69cde-206">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="69cde-207">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="69cde-207">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
