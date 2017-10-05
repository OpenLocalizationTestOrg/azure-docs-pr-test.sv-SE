---
title: "Ändra signaturhash-algoritm för förlitande part för Office 365 | Microsoft Docs"
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
ms.openlocfilehash: c581b1468630a9f28204592c936360b72f42f0d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a><span data-ttu-id="94674-104">Ändra signaturhash-algoritm för Office 365 förtroende för förlitande part</span><span class="sxs-lookup"><span data-stu-id="94674-104">Change signature hash algorithm for Office 365 relying party trust</span></span>
## <a name="overview"></a><span data-ttu-id="94674-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="94674-105">Overview</span></span>
<span data-ttu-id="94674-106">Active Directory Federation Services (AD FS) loggar dess token till Microsoft Azure Active Directory så att de inte kan brytas.</span><span class="sxs-lookup"><span data-stu-id="94674-106">Active Directory Federation Services (AD FS) signs its tokens to Microsoft Azure Active Directory to ensure that they cannot be tampered with.</span></span> <span data-ttu-id="94674-107">Denna signatur kan baseras på SHA1 eller SHA256.</span><span class="sxs-lookup"><span data-stu-id="94674-107">This signature can be based on SHA1 or SHA256.</span></span> <span data-ttu-id="94674-108">Azure Active Directory har nu stöd för token som signerats med ett SHA256-algoritmen och vi rekommenderar att du anger algoritmen tokensignering till SHA256 för den högsta säkerhetsnivån.</span><span class="sxs-lookup"><span data-stu-id="94674-108">Azure Active Directory now supports tokens signed with an SHA256 algorithm, and we recommend setting the token-signing algorithm to SHA256 for the highest level of security.</span></span> <span data-ttu-id="94674-109">Den här artikeln beskriver de steg som krävs för att ange algoritmen tokensignering säkrare SHA256 nivå.</span><span class="sxs-lookup"><span data-stu-id="94674-109">This article describes the steps needed to set the token-signing algorithm to the more secure SHA256 level.</span></span>

>[!NOTE]
><span data-ttu-id="94674-110">Microsoft rekommenderar användning av SHA256 som algoritmen för att signera token som det är säkrare än SHA1 men SHA1 fortfarande är ett alternativ som stöds.</span><span class="sxs-lookup"><span data-stu-id="94674-110">Microsoft recommends usage of SHA256 as the algorithm for signing tokens as it is more secure than SHA1 but SHA1 still remains a supported option.</span></span>

## <a name="change-the-token-signing-algorithm"></a><span data-ttu-id="94674-111">Ändra certifikat för tokensignering-algoritmen</span><span class="sxs-lookup"><span data-stu-id="94674-111">Change the token-signing algorithm</span></span>
<span data-ttu-id="94674-112">När du har angett signaturalgoritm med någon av de två processerna nedan, loggar AD FS tokens för Office 365 förtroende för förlitande part med SHA256.</span><span class="sxs-lookup"><span data-stu-id="94674-112">After you have set the signature algorithm with one of the two processes below, AD FS signs the tokens for Office 365 relying party trust with SHA256.</span></span> <span data-ttu-id="94674-113">Du behöver inte göra några extra konfigurationsändringar och den här ändringen påverkar inte din förmåga att få åtkomst till Office 365 eller andra Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="94674-113">You don't need to make any extra configuration changes, and this change has no impact on your ability to access Office 365 or other Azure AD applications.</span></span>

### <a name="ad-fs-management-console"></a><span data-ttu-id="94674-114">AD FS-hanteringskonsolen</span><span class="sxs-lookup"><span data-stu-id="94674-114">AD FS management console</span></span>
1. <span data-ttu-id="94674-115">Öppna hanteringskonsolen för AD FS på den primära AD FS-servern.</span><span class="sxs-lookup"><span data-stu-id="94674-115">Open the AD FS management console on the primary AD FS server.</span></span>
2. <span data-ttu-id="94674-116">Expandera noden AD FS och klicka på **förtroende för förlitande part**.</span><span class="sxs-lookup"><span data-stu-id="94674-116">Expand the AD FS node and click **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="94674-117">Högerklicka på ditt Office 365-/ Azure förlitande part och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="94674-117">Right-click your Office 365/Azure relying party trust and select **Properties**.</span></span>
4. <span data-ttu-id="94674-118">Välj den **Avancerat** fliken och markera den säkra hash-algoritmen SHA256.</span><span class="sxs-lookup"><span data-stu-id="94674-118">Select the **Advanced** tab and select the secure hash algorithm SHA256.</span></span>
5. <span data-ttu-id="94674-119">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="94674-119">Click **OK**.</span></span>

![SHA256 Signeringsalgoritm--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a><span data-ttu-id="94674-121">AD FS PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="94674-121">AD FS PowerShell cmdlets</span></span>
1. <span data-ttu-id="94674-122">Öppna PowerShell med administratörsbehörighet på alla AD FS-servern.</span><span class="sxs-lookup"><span data-stu-id="94674-122">On any AD FS server, open PowerShell under administrator privileges.</span></span>
2. <span data-ttu-id="94674-123">Ange den säkra hash-algoritmen med hjälp av den **Set-AdfsRelyingPartyTrust** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94674-123">Set the secure hash algorithm by using the **Set-AdfsRelyingPartyTrust** cmdlet.</span></span>
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a><span data-ttu-id="94674-124">Läs också</span><span class="sxs-lookup"><span data-stu-id="94674-124">Also read</span></span>
* [<span data-ttu-id="94674-125">Reparera Office 365 förtroende med Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="94674-125">Repair Office 365 trust with Azure AD Connect</span></span>](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

