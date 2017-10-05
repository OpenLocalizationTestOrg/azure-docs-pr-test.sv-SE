---
title: "Konfigurera inställningarna med hjälp av Azure Active Directory-cmdletarna | Microsoft Docs"
description: "Hur hanterar inställningar för grupper med hjälp av Azure Active Directory-cmdlets"
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
ms.openlocfilehash: 0d89f12955b90c7e1a8301b7c3a1a92e7f62d085
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a><span data-ttu-id="16159-103">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="16159-103">Azure Active Directory cmdlets for configuring group settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16159-104">Det här innehållet gäller endast för Office 365-grupper.</span><span class="sxs-lookup"><span data-stu-id="16159-104">This content applies only to Office 365 groups.</span></span> <span data-ttu-id="16159-105">Mer information om hur du tillåter användare att skapa säkerhetsgrupper `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` enligt beskrivningen i [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span><span class="sxs-lookup"><span data-stu-id="16159-105">For more information on how to allow users to create Security groups, set `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` as described in [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span></span> 

<span data-ttu-id="16159-106">Inställningar för Office 365-grupper har konfigurerats med ett inställningsobjekt och ett SettingsTemplate-objekt.</span><span class="sxs-lookup"><span data-stu-id="16159-106">Office 365 Groups settings are configured using a Settings object and a SettingsTemplate object.</span></span> <span data-ttu-id="16159-107">Inledningsvis visas alla för inställningsobjekt i katalogen, eftersom katalogen är konfigurerad med standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="16159-107">Initially, you don't see any Settings objects in your directory, because your directory is configured with the default settings.</span></span> <span data-ttu-id="16159-108">Om du vill ändra standardinställningarna, måste du skapa ett nytt inställningsobjekt med en mall för inställningar.</span><span class="sxs-lookup"><span data-stu-id="16159-108">To change the default settings, you must create a new settings object using a settings template.</span></span> <span data-ttu-id="16159-109">Inställningar för mallar har definierats av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="16159-109">Settings templates are defined by Microsoft.</span></span> <span data-ttu-id="16159-110">Det finns flera olika inställningar mallar.</span><span class="sxs-lookup"><span data-stu-id="16159-110">There are several different settings templates.</span></span> <span data-ttu-id="16159-111">Om du vill konfigurera inställningar för din katalog i Office 365-grupper kan du använda mallen med namnet ”Group.Unified”.</span><span class="sxs-lookup"><span data-stu-id="16159-111">To configure Office 365 group settings for your directory, you use the template named "Group.Unified".</span></span> <span data-ttu-id="16159-112">Använd mallen med namnet ”Group.Unified.Guest” om du vill konfigurera inställningar för Office 365-grupper på en enda grupp.</span><span class="sxs-lookup"><span data-stu-id="16159-112">To configure Office 365 group settings on a single group, use the template named "Group.Unified.Guest".</span></span> <span data-ttu-id="16159-113">Denna mall används för att hantera gäståtkomst till en grupp för Office 365.</span><span class="sxs-lookup"><span data-stu-id="16159-113">This template is used to manage guest access to an Office 365 group.</span></span> 

<span data-ttu-id="16159-114">Cmdletarna som ingår i Azure Active Directory PowerShell V2-modulen.</span><span class="sxs-lookup"><span data-stu-id="16159-114">The cmdlets are part of the Azure Active Directory PowerShell V2 module.</span></span> <span data-ttu-id="16159-115">Mer information hur du kan hämta och installera modulen på datorn finns i artikeln [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).</span><span class="sxs-lookup"><span data-stu-id="16159-115">For instructions how to download and install the module on your computer, see the article [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).</span></span> <span data-ttu-id="16159-116">Du kan installera med version 2 av modul från [PowerShell-galleriet](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="16159-116">You can install the version 2 release of the module from [the PowerShell gallery](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="retrieve-a-specific-settings-value"></a><span data-ttu-id="16159-117">Hämta ett värde för specifika inställningar</span><span class="sxs-lookup"><span data-stu-id="16159-117">Retrieve a specific settings value</span></span>
<span data-ttu-id="16159-118">Om du känner till namnet på den inställning som du vill hämta kan du använda den nedan för att hämta det aktuella Inställningsvärdet.</span><span class="sxs-lookup"><span data-stu-id="16159-118">If you know the name of the setting you want to retrieve, you can use the below cmdlet to retrieve the current settings value.</span></span> <span data-ttu-id="16159-119">I det här exemplet hämtar vi värdet för en inställning med namnet ”UsageGuidelinesUrl”.</span><span class="sxs-lookup"><span data-stu-id="16159-119">In this example, we're retrieving the value for a setting named "UsageGuidelinesUrl."</span></span> <span data-ttu-id="16159-120">Du kan läsa mer om inställningar och deras namn ytterligare ned i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="16159-120">You can read more about directory settings and their names further down in this article.</span></span>

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-the-directory-level"></a><span data-ttu-id="16159-121">Skapa inställningar på katalognivå</span><span class="sxs-lookup"><span data-stu-id="16159-121">Create settings at the directory level</span></span>
<span data-ttu-id="16159-122">De här stegen skapa inställningar på katalognivå, som gäller för alla Office 365-grupper (Unified grupper) i katalogen.</span><span class="sxs-lookup"><span data-stu-id="16159-122">These steps create settings at directory level, which apply to all Office 365 groups (Unified groups) in the directory.</span></span>

1. <span data-ttu-id="16159-123">Du måste ange ID för SettingsTemplate som du vill använda i DirectorySettings-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="16159-123">In the DirectorySettings cmdlets, you must specify the ID of the SettingsTemplate you want to use.</span></span> <span data-ttu-id="16159-124">Om du inte vet detta ID returnerar denna cmdlet listan över alla mallar för inställningarna:</span><span class="sxs-lookup"><span data-stu-id="16159-124">If you do not know this ID, this cmdlet returns the list of all settings templates:</span></span>
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  <span data-ttu-id="16159-125">Det här anropet cmdlet returnerar alla mallar som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="16159-125">This cmdlet call returns all templates that are available:</span></span>
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. <span data-ttu-id="16159-126">Om du vill lägga till en riktlinje URL för användning, måste du först hämta SettingsTemplate-objektet som definierar användning riktlinje URL värde. det vill säga Group.Unified mallen:</span><span class="sxs-lookup"><span data-stu-id="16159-126">To add a usage guideline URL, first you need to get the SettingsTemplate object that defines the usage guideline URL value; that is, the Group.Unified template:</span></span>
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. <span data-ttu-id="16159-127">Därefter skapa ett nytt för inställningsobjekt som baseras på mallen:</span><span class="sxs-lookup"><span data-stu-id="16159-127">Next, create a new settings object based on that template:</span></span>
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. <span data-ttu-id="16159-128">Uppdatera sedan riktlinje användarvärde:</span><span class="sxs-lookup"><span data-stu-id="16159-128">Then update the usage guideline value:</span></span>
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. <span data-ttu-id="16159-129">Slutligen tillämpas inställningarna:</span><span class="sxs-lookup"><span data-stu-id="16159-129">Finally, apply the settings:</span></span>
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

<span data-ttu-id="16159-130">Vid slutförande returnerar cmdleten ID för det nya inställningsobjektet:</span><span class="sxs-lookup"><span data-stu-id="16159-130">Upon successful completion, the cmdlet returns the ID of the new settings object:</span></span>
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
<span data-ttu-id="16159-131">Här är de inställningar som definierats i Group.Unified SettingsTemplate.</span><span class="sxs-lookup"><span data-stu-id="16159-131">Here are the settings defined in the Group.Unified SettingsTemplate.</span></span>

| <span data-ttu-id="16159-132">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="16159-132">**Setting**</span></span> | <span data-ttu-id="16159-133">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="16159-133">**Description**</span></span> |
| --- | --- |
|  <ul><li><span data-ttu-id="16159-134">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="16159-134">EnableGroupCreation</span></span><li><span data-ttu-id="16159-135">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="16159-135">Type: Boolean</span></span><li><span data-ttu-id="16159-136">Standard: True</span><span class="sxs-lookup"><span data-stu-id="16159-136">Default: True</span></span> |<span data-ttu-id="16159-137">Flagga som anger om skapande av en enhetlig grupp tillåts i katalogen.</span><span class="sxs-lookup"><span data-stu-id="16159-137">The flag indicating whether Unified Group creation is allowed in the directory.</span></span> |
|  <ul><li><span data-ttu-id="16159-138">GroupCreationAllowedGroupId</span><span class="sxs-lookup"><span data-stu-id="16159-138">GroupCreationAllowedGroupId</span></span><li><span data-ttu-id="16159-139">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="16159-139">Type: String</span></span><li><span data-ttu-id="16159-140">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="16159-140">Default: “”</span></span> |<span data-ttu-id="16159-141">GUID för säkerhetsgruppen för vilka medlemmar som tillåts skapa enhetlig grupper även om EnableGroupCreation == false.</span><span class="sxs-lookup"><span data-stu-id="16159-141">GUID of the security group for which the members are allowed to create Unified Groups even when EnableGroupCreation == false.</span></span> |
|  <ul><li><span data-ttu-id="16159-142">UsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="16159-142">UsageGuidelinesUrl</span></span><li><span data-ttu-id="16159-143">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="16159-143">Type: String</span></span><li><span data-ttu-id="16159-144">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="16159-144">Default: “”</span></span> |<span data-ttu-id="16159-145">En länk till riktlinjer för grupp-användning.</span><span class="sxs-lookup"><span data-stu-id="16159-145">A link to the Group Usage Guidelines.</span></span> |
|  <ul><li><span data-ttu-id="16159-146">ClassificationDescriptions</span><span class="sxs-lookup"><span data-stu-id="16159-146">ClassificationDescriptions</span></span><li><span data-ttu-id="16159-147">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="16159-147">Type: String</span></span><li><span data-ttu-id="16159-148">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="16159-148">Default: “”</span></span> | <span data-ttu-id="16159-149">En kommaavgränsad lista över klassificering beskrivningar.</span><span class="sxs-lookup"><span data-stu-id="16159-149">A comma-delimited list of classification descriptions.</span></span> |
|  <ul><li><span data-ttu-id="16159-150">DefaultClassification</span><span class="sxs-lookup"><span data-stu-id="16159-150">DefaultClassification</span></span><li><span data-ttu-id="16159-151">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="16159-151">Type: String</span></span><li><span data-ttu-id="16159-152">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="16159-152">Default: “”</span></span> | <span data-ttu-id="16159-153">Den klassificering som ska användas som standard klassificeringen för en grupp om inget har angetts.</span><span class="sxs-lookup"><span data-stu-id="16159-153">The classification that is to be used as the default classification for a group if none was specified.</span></span>|
|  <ul><li><span data-ttu-id="16159-154">PrefixSuffixNamingRequirement</span><span class="sxs-lookup"><span data-stu-id="16159-154">PrefixSuffixNamingRequirement</span></span><li><span data-ttu-id="16159-155">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="16159-155">Type: String</span></span><li><span data-ttu-id="16159-156">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="16159-156">Default: “”</span></span> |<span data-ttu-id="16159-157">Inte implementerat ännu</span><span class="sxs-lookup"><span data-stu-id="16159-157">Not implemented yet</span></span>
|  <ul><li><span data-ttu-id="16159-158">AllowGuestsToBeGroupOwner</span><span class="sxs-lookup"><span data-stu-id="16159-158">AllowGuestsToBeGroupOwner</span></span><li><span data-ttu-id="16159-159">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="16159-159">Type: Boolean</span></span><li><span data-ttu-id="16159-160">Standard: False</span><span class="sxs-lookup"><span data-stu-id="16159-160">Default: False</span></span> | <span data-ttu-id="16159-161">Booleskt värde som anger huruvida en gästanvändare kan vara en ägare av grupper.</span><span class="sxs-lookup"><span data-stu-id="16159-161">Boolean indicating whether or not a guest user can be an owner of groups.</span></span> |
|  <ul><li><span data-ttu-id="16159-162">AllowGuestsToAccessGroups</span><span class="sxs-lookup"><span data-stu-id="16159-162">AllowGuestsToAccessGroups</span></span><li><span data-ttu-id="16159-163">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="16159-163">Type: Boolean</span></span><li><span data-ttu-id="16159-164">Standard: True</span><span class="sxs-lookup"><span data-stu-id="16159-164">Default: True</span></span> | <span data-ttu-id="16159-165">Booleskt värde som anger huruvida en gästanvändare kan ha åtkomst till innehåll för Unified grupper.</span><span class="sxs-lookup"><span data-stu-id="16159-165">Boolean indicating whether or not a guest user can have access to Unified groups' content.</span></span> |
|  <ul><li><span data-ttu-id="16159-166">GuestUsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="16159-166">GuestUsageGuidelinesUrl</span></span><li><span data-ttu-id="16159-167">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="16159-167">Type: String</span></span><li><span data-ttu-id="16159-168">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="16159-168">Default: “”</span></span> | <span data-ttu-id="16159-169">Url till en länk till riktlinjer för gäst-användning.</span><span class="sxs-lookup"><span data-stu-id="16159-169">The url of a link to the guest usage guidelines.</span></span> |
|  <ul><li><span data-ttu-id="16159-170">AllowToAddGuests</span><span class="sxs-lookup"><span data-stu-id="16159-170">AllowToAddGuests</span></span><li><span data-ttu-id="16159-171">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="16159-171">Type: Boolean</span></span><li><span data-ttu-id="16159-172">Standard: True</span><span class="sxs-lookup"><span data-stu-id="16159-172">Default: True</span></span> | <span data-ttu-id="16159-173">Ett booleskt värde som anger om eller inte är tillåtet att lägga till gäster i den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="16159-173">A boolean indicating whether or not is allowed to add guests to this directory.</span></span>|
|  <ul><li><span data-ttu-id="16159-174">ClassificationList</span><span class="sxs-lookup"><span data-stu-id="16159-174">ClassificationList</span></span><li><span data-ttu-id="16159-175">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="16159-175">Type: String</span></span><li><span data-ttu-id="16159-176">Standard ”:”</span><span class="sxs-lookup"><span data-stu-id="16159-176">Default: “”</span></span> |<span data-ttu-id="16159-177">En kommaavgränsad lista över giltiga klassificeringsvärden som kan tillämpas på Unified grupper.</span><span class="sxs-lookup"><span data-stu-id="16159-177">A comma-delimited list of valid classification values that can be applied to Unified Groups.</span></span> |
|  <ul><li><span data-ttu-id="16159-178">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="16159-178">EnableGroupCreation</span></span><li><span data-ttu-id="16159-179">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="16159-179">Type: Boolean</span></span><li><span data-ttu-id="16159-180">Standard: True</span><span class="sxs-lookup"><span data-stu-id="16159-180">Default: True</span></span> | <span data-ttu-id="16159-181">Ett booleskt värde som anger om icke-administratörer kan skapa en ny enhetlig grupper.</span><span class="sxs-lookup"><span data-stu-id="16159-181">A boolean indicating whether or not non-admin users can create new Unified groups.</span></span> |


## <a name="read-settings-at-the-directory-level"></a><span data-ttu-id="16159-182">Läsinställningar på katalognivå</span><span class="sxs-lookup"><span data-stu-id="16159-182">Read settings at the directory level</span></span>
<span data-ttu-id="16159-183">De här stegen läsa inställningar på katalognivå, som gäller för alla Office-grupper i katalogen.</span><span class="sxs-lookup"><span data-stu-id="16159-183">These steps read settings at directory level, which apply to all Office groups in the directory.</span></span>

1. <span data-ttu-id="16159-184">Läsa alla befintliga directory-inställningar:</span><span class="sxs-lookup"><span data-stu-id="16159-184">Read all existing directory settings:</span></span>
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  <span data-ttu-id="16159-185">Denna cmdlet returnerar en lista över alla inställningar:</span><span class="sxs-lookup"><span data-stu-id="16159-185">This cmdlet returns a list of all directory settings:</span></span>
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. <span data-ttu-id="16159-186">Läsa alla inställningar för en viss grupp:</span><span class="sxs-lookup"><span data-stu-id="16159-186">Read all settings for a specific group:</span></span>
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. <span data-ttu-id="16159-187">Läsa alla värden som directory inställningar i en specifik katalog settings-objekt med hjälp av inställningarna för Id-GUID:</span><span class="sxs-lookup"><span data-stu-id="16159-187">Read all directory settings values of a specific directory settings object, using Settings Id GUID:</span></span>
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  <span data-ttu-id="16159-188">Denna cmdlet returnerar namn och värden i det här inställningsobjektet för den här specifika gruppen:</span><span class="sxs-lookup"><span data-stu-id="16159-188">This cmdlet returns the names and values in this settings object for this specific group:</span></span>
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

## <a name="update-settings-for-a-specific-group"></a><span data-ttu-id="16159-189">Uppdatera inställningarna för en specifik grupp</span><span class="sxs-lookup"><span data-stu-id="16159-189">Update settings for a specific group</span></span>

1. <span data-ttu-id="16159-190">Söka efter inställningar mallen med namnet ”Groups.Unified.Guest”</span><span class="sxs-lookup"><span data-stu-id="16159-190">Search for the settings template named "Groups.Unified.Guest"</span></span>
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
2. <span data-ttu-id="16159-191">Hämta mallobjektet för Groups.Unified.Guest mallen:</span><span class="sxs-lookup"><span data-stu-id="16159-191">Retrieve the template object for the Groups.Unified.Guest template:</span></span>
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. <span data-ttu-id="16159-192">Skapa ett nytt inställningsobjekt från mallen:</span><span class="sxs-lookup"><span data-stu-id="16159-192">Create a new settings object from the template:</span></span>
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. <span data-ttu-id="16159-193">Ange om du obligatoriskt värde:</span><span class="sxs-lookup"><span data-stu-id="16159-193">Set the setting to the required value:</span></span>
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. <span data-ttu-id="16159-194">Skapa den nya inställningen för den nödvändiga gruppen i katalogen:</span><span class="sxs-lookup"><span data-stu-id="16159-194">Create the new setting for the required group in the directory:</span></span>
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-the-directory-level"></a><span data-ttu-id="16159-195">Uppdatera inställningar på katalognivå</span><span class="sxs-lookup"><span data-stu-id="16159-195">Update settings at the directory level</span></span>

<span data-ttu-id="16159-196">De här stegen uppdatera inställningar på katalognivå, som gäller för alla Unified grupper i katalogen.</span><span class="sxs-lookup"><span data-stu-id="16159-196">These steps update settings at directory level, which apply to all Unified groups in the directory.</span></span> <span data-ttu-id="16159-197">De här exemplen antar att det finns redan ett inställningsobjekt i din katalog.</span><span class="sxs-lookup"><span data-stu-id="16159-197">These examples assume there is already a Settings object in your directory.</span></span>

1. <span data-ttu-id="16159-198">Hitta objektet befintliga inställningar:</span><span class="sxs-lookup"><span data-stu-id="16159-198">Find the existing Settings object:</span></span>
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. <span data-ttu-id="16159-199">Uppdatera värdet:</span><span class="sxs-lookup"><span data-stu-id="16159-199">Update the value:</span></span>
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. <span data-ttu-id="16159-200">Uppdatera inställningen:</span><span class="sxs-lookup"><span data-stu-id="16159-200">Update the setting:</span></span>
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-the-directory-level"></a><span data-ttu-id="16159-201">Ta bort inställningar på katalognivå</span><span class="sxs-lookup"><span data-stu-id="16159-201">Remove settings at the directory level</span></span>
<span data-ttu-id="16159-202">Det här steget tar bort inställningar på katalognivå, som gäller för alla Office-grupper i katalogen.</span><span class="sxs-lookup"><span data-stu-id="16159-202">This step removes settings at directory level, which apply to all Office groups in the directory.</span></span>
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a><span data-ttu-id="16159-203">Cmdlet-referens för syntax</span><span class="sxs-lookup"><span data-stu-id="16159-203">Cmdlet syntax reference</span></span>
<span data-ttu-id="16159-204">Du kan hitta mer Azure Active Directory PowerShell-dokumentationen på [Azure Active Directory-cmdletarna](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="16159-204">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="additional-reading"></a><span data-ttu-id="16159-205">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="16159-205">Additional reading</span></span>

* [<span data-ttu-id="16159-206">Hantera åtkomst till resurser med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="16159-206">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="16159-207">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16159-207">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
