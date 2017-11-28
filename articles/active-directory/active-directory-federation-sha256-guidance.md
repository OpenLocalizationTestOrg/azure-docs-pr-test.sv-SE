---
title: "aaaChange signaturhash-algoritm för förlitande part för Office 365 | Microsoft Docs"
description: "Den här sidan innehåller riktlinjer för att ändra SHA-algoritmen för federationsförtroende med Office 365"
keywords: "SHA1, SHA256, O365, federation, aadconnect, adfs, ADFS, ändra sha federationsförtroende förtroende för förlitande part"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: samueld
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: 3333d1384aff8bdf6b3bcc894f8c633fd9ccc3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a><span data-ttu-id="90e23-104">Ändra signaturhash-algoritm för Office 365 förtroende för förlitande part</span><span class="sxs-lookup"><span data-stu-id="90e23-104">Change signature hash algorithm for Office 365 relying party trust</span></span>
## <a name="overview"></a><span data-ttu-id="90e23-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="90e23-105">Overview</span></span>
<span data-ttu-id="90e23-106">Active Directory Federation Services (AD FS) loggar dess token tooMicrosoft Azure Active Directory-tooensure som de inte kan brytas.</span><span class="sxs-lookup"><span data-stu-id="90e23-106">Active Directory Federation Services (AD FS) signs its tokens tooMicrosoft Azure Active Directory tooensure that they cannot be tampered with.</span></span> <span data-ttu-id="90e23-107">Denna signatur kan baseras på SHA1 eller SHA256.</span><span class="sxs-lookup"><span data-stu-id="90e23-107">This signature can be based on SHA1 or SHA256.</span></span> <span data-ttu-id="90e23-108">Azure Active Directory har nu stöd för token som signerats med ett SHA256-algoritmen och rekommenderar vi hello tokensignering algoritmen tooSHA256 för hello högsta säkerhetsnivå.</span><span class="sxs-lookup"><span data-stu-id="90e23-108">Azure Active Directory now supports tokens signed with an SHA256 algorithm, and we recommend setting hello token-signing algorithm tooSHA256 for hello highest level of security.</span></span> <span data-ttu-id="90e23-109">Den här artikeln beskriver hello steg behövs tooset hello tokensignering algoritmen toohello säkrare SHA256-nivå.</span><span class="sxs-lookup"><span data-stu-id="90e23-109">This article describes hello steps needed tooset hello token-signing algorithm toohello more secure SHA256 level.</span></span>

>[!NOTE]
><span data-ttu-id="90e23-110">Microsoft rekommenderar användning av SHA256 som hello-algoritm för att signera token som det är säkrare än SHA1 men SHA1 fortfarande är ett alternativ som stöds.</span><span class="sxs-lookup"><span data-stu-id="90e23-110">Microsoft recommends usage of SHA256 as hello algorithm for signing tokens as it is more secure than SHA1 but SHA1 still remains a supported option.</span></span>

## <a name="change-hello-token-signing-algorithm"></a><span data-ttu-id="90e23-111">Ändra certifikat för tokensignering hello-algoritmen</span><span class="sxs-lookup"><span data-stu-id="90e23-111">Change hello token-signing algorithm</span></span>
<span data-ttu-id="90e23-112">När du har angett hello signaturalgoritm med någon av hello två processer nedan, loggar hello token för Office 365 förtroende för förlitande part med SHA256 i AD FS.</span><span class="sxs-lookup"><span data-stu-id="90e23-112">After you have set hello signature algorithm with one of hello two processes below, AD FS signs hello tokens for Office 365 relying party trust with SHA256.</span></span> <span data-ttu-id="90e23-113">Du behöver inte toomake några extra konfigurationsändringar och den här ändringen har ingen inverkan på din möjlighet tooaccess Office 365 eller andra Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="90e23-113">You don't need toomake any extra configuration changes, and this change has no impact on your ability tooaccess Office 365 or other Azure AD applications.</span></span>

### <a name="ad-fs-management-console"></a><span data-ttu-id="90e23-114">AD FS-hanteringskonsolen</span><span class="sxs-lookup"><span data-stu-id="90e23-114">AD FS management console</span></span>
1. <span data-ttu-id="90e23-115">Öppna hello AD FS-hanteringskonsolen på hello primär AD FS-servern.</span><span class="sxs-lookup"><span data-stu-id="90e23-115">Open hello AD FS management console on hello primary AD FS server.</span></span>
2. <span data-ttu-id="90e23-116">Expandera hello AD FS-noden och klicka på **förtroende för förlitande part**.</span><span class="sxs-lookup"><span data-stu-id="90e23-116">Expand hello AD FS node and click **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="90e23-117">Högerklicka på ditt Office 365-/ Azure förlitande part och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="90e23-117">Right-click your Office 365/Azure relying party trust and select **Properties**.</span></span>
4. <span data-ttu-id="90e23-118">Välj hello **Avancerat** fliken och väljer hello säkra hash-algoritm SHA256.</span><span class="sxs-lookup"><span data-stu-id="90e23-118">Select hello **Advanced** tab and select hello secure hash algorithm SHA256.</span></span>
5. <span data-ttu-id="90e23-119">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="90e23-119">Click **OK**.</span></span>

![SHA256 Signeringsalgoritm--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a><span data-ttu-id="90e23-121">AD FS PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="90e23-121">AD FS PowerShell cmdlets</span></span>
1. <span data-ttu-id="90e23-122">Öppna PowerShell med administratörsbehörighet på alla AD FS-servern.</span><span class="sxs-lookup"><span data-stu-id="90e23-122">On any AD FS server, open PowerShell under administrator privileges.</span></span>
2. <span data-ttu-id="90e23-123">Ange hello säkra hash-algoritm med hjälp av hello **Set-AdfsRelyingPartyTrust** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="90e23-123">Set hello secure hash algorithm by using hello **Set-AdfsRelyingPartyTrust** cmdlet.</span></span>
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a><span data-ttu-id="90e23-124">Läs också</span><span class="sxs-lookup"><span data-stu-id="90e23-124">Also read</span></span>
* [<span data-ttu-id="90e23-125">Reparera Office 365 förtroende med Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="90e23-125">Repair Office 365 trust with Azure AD Connect</span></span>](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

