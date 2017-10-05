---
title: "Azure Active Directory Domain Services: Felsökningsguide | Microsoft Docs"
description: "Felsökningsguide för Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: d6695b0c40f56093e8701dfe6394143268114453
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a><span data-ttu-id="554ce-103">Azure AD Domain Services - guide för felsökning</span><span class="sxs-lookup"><span data-stu-id="554ce-103">Azure AD Domain Services - Troubleshooting guide</span></span>
<span data-ttu-id="554ce-104">Den här artikeln innehåller tips för felsökning för problem som kan uppstå när du konfigurerar eller administrera Azure Active Directory (AD) Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-104">This article provides troubleshooting hints for issues you may encounter when setting up or administering Azure Active Directory (AD) Domain Services.</span></span>

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a><span data-ttu-id="554ce-105">Du kan inte aktivera Azure AD Domain Services för din Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="554ce-105">You cannot enable Azure AD Domain Services for your Azure AD directory</span></span>
<span data-ttu-id="554ce-106">Det här avsnittet hjälper dig att felsöka när du försöker aktivera Azure AD Domain Services för din katalog och misslyckas eller växlas tillbaka till ”inaktiverad”.</span><span class="sxs-lookup"><span data-stu-id="554ce-106">This section helps you troubleshoot errors when you try to enable Azure AD Domain Services for your directory and it fails or gets toggled back to 'Disabled'.</span></span>

<span data-ttu-id="554ce-107">Välj felsökningsstegen som motsvarar att du får felmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="554ce-107">Pick the troubleshooting steps that correspond to the error message you encounter.</span></span>

| <span data-ttu-id="554ce-108">**Felmeddelande**</span><span class="sxs-lookup"><span data-stu-id="554ce-108">**Error Message**</span></span> | <span data-ttu-id="554ce-109">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="554ce-109">**Resolution**</span></span> |
| --- |:--- |
| <span data-ttu-id="554ce-110">*Namnet contoso100.com används redan i nätverket. Ange ett namn som inte används.*</span><span class="sxs-lookup"><span data-stu-id="554ce-110">*The name contoso100.com is already in use on this network. Specify a name that is not in use.*</span></span> |[<span data-ttu-id="554ce-111">Domän namnkonflikt i virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="554ce-111">Domain name conflict in the virtual network</span></span>](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| <span data-ttu-id="554ce-112">*Det gick inte att aktivera Domain Services i den här Azure AD-klienten. Tjänsten har inte tillräcklig behörighet för programmet ”Azure AD Domain Services Sync”. Ta bort programmet ”Azure AD Domain Services Sync” och försök sedan aktivera Domain Services för din Azure AD-klient.*</span><span class="sxs-lookup"><span data-stu-id="554ce-112">*Domain Services could not be enabled in this Azure AD tenant. The service does not have adequate permissions to the application called 'Azure AD Domain Services Sync'. Delete the application called 'Azure AD Domain Services Sync' and then try to enable Domain Services for your Azure AD tenant.*</span></span> |[<span data-ttu-id="554ce-113">Domain Services har inte tillräcklig behörighet för att programmet Azure AD Domain Services-synkronisering</span><span class="sxs-lookup"><span data-stu-id="554ce-113">Domain Services does not have adequate permissions to the Azure AD Domain Services Sync application</span></span>](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| <span data-ttu-id="554ce-114">*Det gick inte att aktivera Domain Services i den här Azure AD-klienten. Programmet Domain Services i din Azure AD-klient har inte tillräcklig behörighet för att aktivera Domain Services. Ta bort programmet med programidentifieraren d87dcbc6-a371-462e-88e3-28ad15ec4e64 och försök sedan aktivera Domain Services för din Azure AD-klient.*</span><span class="sxs-lookup"><span data-stu-id="554ce-114">*Domain Services could not be enabled in this Azure AD tenant. The Domain Services application in your Azure AD tenant does not have the required permissions to enable Domain Services. Delete the application with the application identifier d87dcbc6-a371-462e-88e3-28ad15ec4e64 and then try to enable Domain Services for your Azure AD tenant.*</span></span> |[<span data-ttu-id="554ce-115">Domain Services-tjänstprogrammet har inte konfigurerats korrekt i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="554ce-115">The Domain Services application is not configured properly in your tenant</span></span>](active-directory-ds-troubleshooting.md#invalid-configuration) |
| <span data-ttu-id="554ce-116">*Det gick inte att aktivera Domain Services i den här Azure AD-klienten. Programmet Microsoft Azure AD är inaktiverat i din Azure AD-klient. Aktivera programmet med programidentifieraren 00000002-0000-0000-c000-000000000000 och försök aktivera Domain Services för din Azure AD-klient.*</span><span class="sxs-lookup"><span data-stu-id="554ce-116">*Domain Services could not be enabled in this Azure AD tenant. The Microsoft Azure AD application is disabled in your Azure AD tenant. Enable the application with the application identifier 00000002-0000-0000-c000-000000000000 and then try to enable Domain Services for your Azure AD tenant.*</span></span> |[<span data-ttu-id="554ce-117">Programmet Microsoft Graph har inaktiverats i Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="554ce-117">The Microsoft Graph application is disabled in your Azure AD tenant</span></span>](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a><span data-ttu-id="554ce-118">Namnkonflikt i domänen</span><span class="sxs-lookup"><span data-stu-id="554ce-118">Domain Name conflict</span></span>
<span data-ttu-id="554ce-119">**Ett felmeddelande visas:**</span><span class="sxs-lookup"><span data-stu-id="554ce-119">**Error message:**</span></span>

<span data-ttu-id="554ce-120">*Namnet contoso100.com används redan i nätverket. Ange ett namn som inte används.*</span><span class="sxs-lookup"><span data-stu-id="554ce-120">*The name contoso100.com is already in use on this network. Specify a name that is not in use.*</span></span>

<span data-ttu-id="554ce-121">**Reparation:**</span><span class="sxs-lookup"><span data-stu-id="554ce-121">**Remediation:**</span></span>

<span data-ttu-id="554ce-122">Se till att du inte har en befintlig domän med samma domännamn tillgänglig i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="554ce-122">Ensure that you do not have an existing domain with the same domain name available on that virtual network.</span></span> <span data-ttu-id="554ce-123">Anta exempelvis att det redan finns en domän som heter ”contoso.com” i det valda virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="554ce-123">For instance, assume you have a domain called 'contoso.com' already available on the selected virtual network.</span></span> <span data-ttu-id="554ce-124">Senare, försök att aktivera en Azure AD Domain Services-hanterad domän med samma domännamn (dvs, contoso.com) i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="554ce-124">Later, you try to enable an Azure AD Domain Services managed domain with the same domain name (that is, 'contoso.com') on that virtual network.</span></span> <span data-ttu-id="554ce-125">Det uppstår ett fel vid försök att aktivera Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-125">You encounter a failure when trying to enable Azure AD Domain Services.</span></span>

<span data-ttu-id="554ce-126">Det här felet beror på namnkonflikter för domännamnet i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="554ce-126">This failure is due to name conflicts for the domain name on that virtual network.</span></span> <span data-ttu-id="554ce-127">I den här situationen måste du använda ett annat namn för att ställa in din Azure AD Domain Services-hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="554ce-127">In this situation, you must use a different name to set up your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="554ce-128">Du kan också avetablera den befintliga domänen och sedan aktivera Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-128">Alternately, you can de-provision the existing domain and then proceed to enable Azure AD Domain Services.</span></span>

### <a name="inadequate-permissions"></a><span data-ttu-id="554ce-129">Otillräckliga behörigheter</span><span class="sxs-lookup"><span data-stu-id="554ce-129">Inadequate permissions</span></span>
<span data-ttu-id="554ce-130">**Ett felmeddelande visas:**</span><span class="sxs-lookup"><span data-stu-id="554ce-130">**Error message:**</span></span>

<span data-ttu-id="554ce-131">*Det gick inte att aktivera Domain Services i den här Azure AD-klienten. Tjänsten har inte tillräcklig behörighet för programmet ”Azure AD Domain Services Sync”. Ta bort programmet ”Azure AD Domain Services Sync” och försök sedan aktivera Domain Services för din Azure AD-klient.*</span><span class="sxs-lookup"><span data-stu-id="554ce-131">*Domain Services could not be enabled in this Azure AD tenant. The service does not have adequate permissions to the application called 'Azure AD Domain Services Sync'. Delete the application called 'Azure AD Domain Services Sync' and then try to enable Domain Services for your Azure AD tenant.*</span></span>

<span data-ttu-id="554ce-132">**Reparation:**</span><span class="sxs-lookup"><span data-stu-id="554ce-132">**Remediation:**</span></span>

<span data-ttu-id="554ce-133">Kontrollera om det finns ett program med namnet 'Azure AD Domain Services Sync' i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="554ce-133">Check to see if there is an application with the name 'Azure AD Domain Services Sync' in your Azure AD directory.</span></span> <span data-ttu-id="554ce-134">Om programmet finns, ta bort den och sedan återaktivera Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-134">If this application exists, delete it and then re-enable Azure AD Domain Services.</span></span>

<span data-ttu-id="554ce-135">Utför följande steg för att kontrollera förekomst av programmet och ta bort den om programmet redan finns:</span><span class="sxs-lookup"><span data-stu-id="554ce-135">Perform the following steps to check for the presence of the application and to delete it, if the application exists:</span></span>

1. <span data-ttu-id="554ce-136">Gå till **den klassiska Azure-portalen** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).</span><span class="sxs-lookup"><span data-stu-id="554ce-136">Navigate to the **Azure classic portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).</span></span>
2. <span data-ttu-id="554ce-137">Välj noden **Active Directory** i det vänstra fönstret.</span><span class="sxs-lookup"><span data-stu-id="554ce-137">Select the **Active Directory** node on the left pane.</span></span>
3. <span data-ttu-id="554ce-138">Välj Azure AD-klienten (katalogen) som du vill aktivera Azure AD Domain Services för.</span><span class="sxs-lookup"><span data-stu-id="554ce-138">Select the Azure AD tenant (directory) for which you would like to enable Azure AD Domain Services.</span></span>
4. <span data-ttu-id="554ce-139">Navigera till den **program** fliken.</span><span class="sxs-lookup"><span data-stu-id="554ce-139">Navigate to the **Applications** tab.</span></span>
5. <span data-ttu-id="554ce-140">Välj den **program som företaget äger** alternativ i listrutan.</span><span class="sxs-lookup"><span data-stu-id="554ce-140">Select the **Applications my company owns** option in the dropdown.</span></span>
6. <span data-ttu-id="554ce-141">Kontrollera om ett program som kallas **Azure AD Domain Services Sync**.</span><span class="sxs-lookup"><span data-stu-id="554ce-141">Check for an application called **Azure AD Domain Services Sync**.</span></span> <span data-ttu-id="554ce-142">Om programmet redan finns fortsätter du att ta bort den.</span><span class="sxs-lookup"><span data-stu-id="554ce-142">If the application exists, proceed to delete it.</span></span>
7. <span data-ttu-id="554ce-143">När du har tagit bort programmet, försök att aktivera Azure AD Domain Services igen.</span><span class="sxs-lookup"><span data-stu-id="554ce-143">Once you have deleted the application, try to enable Azure AD Domain Services once again.</span></span>

### <a name="invalid-configuration"></a><span data-ttu-id="554ce-144">Ogiltig konfiguration</span><span class="sxs-lookup"><span data-stu-id="554ce-144">Invalid configuration</span></span>
<span data-ttu-id="554ce-145">**Ett felmeddelande visas:**</span><span class="sxs-lookup"><span data-stu-id="554ce-145">**Error message:**</span></span>

<span data-ttu-id="554ce-146">*Det gick inte att aktivera Domain Services i den här Azure AD-klienten. Programmet Domain Services i din Azure AD-klient har inte tillräcklig behörighet för att aktivera Domain Services. Ta bort programmet med programidentifieraren d87dcbc6-a371-462e-88e3-28ad15ec4e64 och försök sedan aktivera Domain Services för din Azure AD-klient.*</span><span class="sxs-lookup"><span data-stu-id="554ce-146">*Domain Services could not be enabled in this Azure AD tenant. The Domain Services application in your Azure AD tenant does not have the required permissions to enable Domain Services. Delete the application with the application identifier d87dcbc6-a371-462e-88e3-28ad15ec4e64 and then try to enable Domain Services for your Azure AD tenant.*</span></span>

<span data-ttu-id="554ce-147">**Reparation:**</span><span class="sxs-lookup"><span data-stu-id="554ce-147">**Remediation:**</span></span>

<span data-ttu-id="554ce-148">Kontrollera om du har ett program med namnet 'AzureActiveDirectoryDomainControllerServices' (med en identifierare för d87dcbc6-a371-462e-88e3-28ad15ec4e64) i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="554ce-148">Check to see if you have an application with the name 'AzureActiveDirectoryDomainControllerServices' (with an application identifier of d87dcbc6-a371-462e-88e3-28ad15ec4e64) in your Azure AD directory.</span></span> <span data-ttu-id="554ce-149">Om det här programmet finns måste du ta bort den och sedan återaktivera Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-149">If this application exists, you need to delete it and then re-enable Azure AD Domain Services.</span></span>

<span data-ttu-id="554ce-150">Använd följande PowerShell-skript för att hitta programmet och ta bort den.</span><span class="sxs-lookup"><span data-stu-id="554ce-150">Use the following PowerShell script to find the application and delete it.</span></span>

> [!NOTE]
> <span data-ttu-id="554ce-151">Det här skriptet använder **Azure AD PowerShell version 2** cmdlets.</span><span class="sxs-lookup"><span data-stu-id="554ce-151">This script uses **Azure AD PowerShell version 2** cmdlets.</span></span> <span data-ttu-id="554ce-152">En fullständig lista över alla tillgängliga cmdletar och för att hämta modulen kan läsa den [AzureAD PowerShell referensdokumentationen](https://msdn.microsoft.com/library/azure/mt757189.aspx).</span><span class="sxs-lookup"><span data-stu-id="554ce-152">For a full list of all available cmdlets and to download the module, read the [AzureAD PowerShell reference documentation](https://msdn.microsoft.com/library/azure/mt757189.aspx).</span></span>
>
>

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a><span data-ttu-id="554ce-153">Microsoft Graph inaktiverad</span><span class="sxs-lookup"><span data-stu-id="554ce-153">Microsoft Graph disabled</span></span>
<span data-ttu-id="554ce-154">**Ett felmeddelande visas:**</span><span class="sxs-lookup"><span data-stu-id="554ce-154">**Error message:**</span></span>

<span data-ttu-id="554ce-155">Det gick inte att aktivera Domain Services i Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="554ce-155">Domain Services could not be enabled in this Azure AD tenant.</span></span> <span data-ttu-id="554ce-156">Programmet Microsoft Azure AD är inaktiverat i din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="554ce-156">The Microsoft Azure AD application is disabled in your Azure AD tenant.</span></span> <span data-ttu-id="554ce-157">Aktivera programmet med 00000002-0000-0000-c000-000000000000 för program-ID och försök sedan att aktivera Domain Services för din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="554ce-157">Enable the application with the application identifier 00000002-0000-0000-c000-000000000000 and then try to enable Domain Services for your Azure AD tenant.</span></span>

<span data-ttu-id="554ce-158">**Reparation:**</span><span class="sxs-lookup"><span data-stu-id="554ce-158">**Remediation:**</span></span>

<span data-ttu-id="554ce-159">Kontrollera om du har inaktiverat ett program med identifierare 00000002-0000-0000-c000-000000000000.</span><span class="sxs-lookup"><span data-stu-id="554ce-159">Check to see if you have disabled an application with the identifier 00000002-0000-0000-c000-000000000000.</span></span> <span data-ttu-id="554ce-160">Det här programmet är Microsoft Azure AD-program och ger Graph API-åtkomst till Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="554ce-160">This application is the Microsoft Azure AD application and provides Graph API access to your Azure AD tenant.</span></span> <span data-ttu-id="554ce-161">Azure AD Domain Services måste det här programmet måste vara aktiverat för att synkronisera din Azure AD-klient till din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="554ce-161">Azure AD Domain Services needs this application to be enabled to synchronize your Azure AD tenant to your managed domain.</span></span>

<span data-ttu-id="554ce-162">Aktivera det här programmet och försök sedan att aktivera Domain Services för din Azure AD-klient för att lösa det här felet.</span><span class="sxs-lookup"><span data-stu-id="554ce-162">To resolve this error, enable this application and then try to enable Domain Services for your Azure AD tenant.</span></span>

## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="554ce-163">Användarna kan inte logga in på den hanterade domänen för Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="554ce-163">Users are unable to sign in to the Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="554ce-164">Om en eller flera användare i Azure AD-klienten inte kan logga in på den nyligen skapade hanterade domänen, utför du följande felsökningssteg:</span><span class="sxs-lookup"><span data-stu-id="554ce-164">If one or more users in your Azure AD tenant are unable to sign in to the newly created managed domain, perform the following troubleshooting steps:</span></span>

* <span data-ttu-id="554ce-165">**Logga in med UPN-format:** försöker logga in med UPN-format (till exempel ”joeuser@contoso.com”) i stället för SAMAccountName format (CONTOSO\joeuser).</span><span class="sxs-lookup"><span data-stu-id="554ce-165">**Sign-in using UPN format:** Try to sign in using the UPN format (for example, 'joeuser@contoso.com') instead of the SAMAccountName format ('CONTOSO\joeuser').</span></span> <span data-ttu-id="554ce-166">SAMAccountName kan genereras automatiskt för användare vars UPN-prefixet är alltför lång eller är detsamma som en annan användare på den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="554ce-166">The SAMAccountName may be automatically generated for users whose UPN prefix is overly long or is the same as another user on the managed domain.</span></span> <span data-ttu-id="554ce-167">UPN-format är säkert att vara unika inom en Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="554ce-167">The UPN format is guaranteed to be unique within an Azure AD tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="554ce-168">Vi rekommenderar att du använder UPN-format för att logga in på den hanterade domänen med Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-168">We recommend using the UPN format to sign in to the Azure AD Domain Services managed domain.</span></span>
>
>

* <span data-ttu-id="554ce-169">Kontrollera att du har [aktiverat lösenordssynkronisering](active-directory-ds-getting-started-password-sync.md) i enlighet med anvisningarna i guiden Komma igång.</span><span class="sxs-lookup"><span data-stu-id="554ce-169">Ensure that you have [enabled password synchronization](active-directory-ds-getting-started-password-sync.md) in accordance with the steps outlined in the Getting Started guide.</span></span>
* <span data-ttu-id="554ce-170">**Externa konton:** Kontrollera att användarkontot påverkas inte är ett externt konto i Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="554ce-170">**External accounts:** Ensure that the affected user account is not an external account in the Azure AD tenant.</span></span> <span data-ttu-id="554ce-171">Exempel på externa konton är Microsoft-konton (till exempel 'joe@live.com') eller användarkonton från en extern Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="554ce-171">Examples of external accounts include Microsoft accounts (for example, 'joe@live.com') or user accounts from an external Azure AD directory.</span></span> <span data-ttu-id="554ce-172">Eftersom Azure AD Domain Services inte har autentiseringsuppgifter för dessa konton kan kan inte dessa användare logga in på den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="554ce-172">Since Azure AD Domain Services does not have credentials for such user accounts, these users cannot sign in to the managed domain.</span></span>
* <span data-ttu-id="554ce-173">**Synkroniserats konton:** om de berörda användarkontona synkroniseras från en lokal katalog, kontrollerar du att:</span><span class="sxs-lookup"><span data-stu-id="554ce-173">**Synced accounts:** If the affected user accounts are synchronized from an on-premises directory, verify that:</span></span>

  * <span data-ttu-id="554ce-174">Du har distribuerat eller uppdateras till den [senaste rekommenderade versionen av Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594).</span><span class="sxs-lookup"><span data-stu-id="554ce-174">You have deployed or updated to the [latest recommended release of Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594).</span></span>
  * <span data-ttu-id="554ce-175">Du har konfigurerat Azure AD Connect till [utför en fullständig synkronisering](active-directory-ds-getting-started-password-sync.md).</span><span class="sxs-lookup"><span data-stu-id="554ce-175">You have configured Azure AD Connect to [perform a full synchronization](active-directory-ds-getting-started-password-sync.md).</span></span>
  * <span data-ttu-id="554ce-176">Beroende på storleken på din katalog kan det ta en stund för användarkonton och autentiseringsuppgifter hash-värden ska vara tillgängliga i Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-176">Depending on the size of your directory, it may take a while for user accounts and credential hashes to be available in Azure AD Domain Services.</span></span> <span data-ttu-id="554ce-177">Se till att du vänta tillräckligt länge innan du försöker autentisering (beroende på storleken på din katalog - några timmar på en dag eller två för stora kataloger).</span><span class="sxs-lookup"><span data-stu-id="554ce-177">Ensure you wait long enough before retrying authentication (depending on the size of your directory - a few hours to a day or two for large directories).</span></span>
  * <span data-ttu-id="554ce-178">Om problemet kvarstår efter verifiering av ovanstående steg kan du prova att starta om tjänsten Microsoft Azure AD Sync.</span><span class="sxs-lookup"><span data-stu-id="554ce-178">If the issue persists after verifying the preceding steps, try restarting the Microsoft Azure AD Sync Service.</span></span> <span data-ttu-id="554ce-179">Starta en kommandotolk och kör följande kommandon från datorn synkronisering:</span><span class="sxs-lookup"><span data-stu-id="554ce-179">From your sync machine, launch a command prompt and execute the following commands:</span></span>

    1. <span data-ttu-id="554ce-180">net stop ”Microsoft Azure AD Sync'</span><span class="sxs-lookup"><span data-stu-id="554ce-180">net stop 'Microsoft Azure AD Sync'</span></span>
    2. <span data-ttu-id="554ce-181">net start ”Microsoft Azure AD Sync'</span><span class="sxs-lookup"><span data-stu-id="554ce-181">net start 'Microsoft Azure AD Sync'</span></span>
* <span data-ttu-id="554ce-182">**Endast molnbaserad konton**: om berörda användarkontot är en molnbaserad användarkonto, ska du kontrollera att användaren har ändrat sitt lösenord när du har aktiverat Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="554ce-182">**Cloud-only accounts**: If the affected user account is a cloud-only user account, ensure that the user has changed their password after you enabled Azure AD Domain Services.</span></span> <span data-ttu-id="554ce-183">Det här steget gör att de autentiseringshashvärden som krävs för Azure AD Domain Services genereras.</span><span class="sxs-lookup"><span data-stu-id="554ce-183">This step causes the credential hashes required for Azure AD Domain Services to be generated.</span></span>

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a><span data-ttu-id="554ce-184">Användare har tagits bort från Azure AD-klienten tas inte bort från din hanterade domän</span><span class="sxs-lookup"><span data-stu-id="554ce-184">Users removed from your Azure AD tenant are not removed from your managed domain</span></span>
<span data-ttu-id="554ce-185">Azure AD skyddar dig mot oavsiktlig borttagning av användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="554ce-185">Azure AD protects you from accidental deletion of user objects.</span></span> <span data-ttu-id="554ce-186">När du tar bort ett användarkonto från Azure AD-klienten flyttas motsvarande användarobjekt till papperskorgen.</span><span class="sxs-lookup"><span data-stu-id="554ce-186">When you delete a user account from your Azure AD tenant, the corresponding user object is moved to the Recycle Bin.</span></span> <span data-ttu-id="554ce-187">När den här åtgärden synkroniseras till din hanterade domän gör motsvarande användarkonto markeras som inaktiverade.</span><span class="sxs-lookup"><span data-stu-id="554ce-187">When this delete operation is synchronized to your managed domain, it causes the corresponding user account to be marked disabled.</span></span> <span data-ttu-id="554ce-188">Den här funktionen hjälper dig att återställa eller ångra borttagning användarkontot senare.</span><span class="sxs-lookup"><span data-stu-id="554ce-188">This feature helps you recover or undelete the user account later.</span></span>

<span data-ttu-id="554ce-189">Användarkontot förblir inaktiverad i din hanterade domän, även om du återskapa ett användarkonto med samma UPN i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="554ce-189">The user account remains in the disabled state in your managed domain, even if you re-create a user account with the same UPN in your Azure AD directory.</span></span> <span data-ttu-id="554ce-190">Om du vill ta bort användarkontot från din hanterade domän måste du tvinga tas bort från Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="554ce-190">To remove the user account from your managed domain, you need to force delete it from your Azure AD tenant.</span></span>

<span data-ttu-id="554ce-191">Om du vill ta bort användarkontot helt från din hanterade domän, ta bort användaren permanent från Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="554ce-191">To remove the user account fully from your managed domain, delete the user permanently from your Azure AD tenant.</span></span> <span data-ttu-id="554ce-192">Använd PowerShell-cmdleten Remove-MsolUser med alternativet ”-RemoveFromRecycleBin” enligt beskrivningen i den här [MSDN-artikeln](https://msdn.microsoft.com/library/azure/dn194132.aspx).</span><span class="sxs-lookup"><span data-stu-id="554ce-192">Use the Remove-MsolUser PowerShell cmdlet with the '-RemoveFromRecycleBin' option, as described in this [MSDN article](https://msdn.microsoft.com/library/azure/dn194132.aspx).</span></span>

## <a name="contact-us"></a><span data-ttu-id="554ce-193">Kontakta oss</span><span class="sxs-lookup"><span data-stu-id="554ce-193">Contact Us</span></span>
<span data-ttu-id="554ce-194">Kontakta produktteamet Azure Active Directory Domain Services för att [dela feedback eller support](active-directory-ds-contact-us.md).</span><span class="sxs-lookup"><span data-stu-id="554ce-194">Contact the Azure Active Directory Domain Services product team to [share feedback or for support](active-directory-ds-contact-us.md).</span></span>
