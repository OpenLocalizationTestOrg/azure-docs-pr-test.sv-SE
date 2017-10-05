---
title: "Tvåstegsverifiering och AD FS – Azure MFA | Microsoft Docs"
description: "Det här är sidan om Azure Multi-Factor Authentication som beskriver hur du kommer igång med Azure MFA och AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: 28aede545c738137ff04257214e4a3f42792d85c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a><span data-ttu-id="25ab8-103">Komma igång med Azure Multi-Factor Authentication och Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="25ab8-103">Getting started with Azure Multi-Factor Authentication and Active Directory Federation Services</span></span>
<span data-ttu-id="25ab8-104"><center>![Moln](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center></span><span class="sxs-lookup"><span data-stu-id="25ab8-104"><center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center></span></span>

<span data-ttu-id="25ab8-105">Om din organisation har federerat det lokala Active Directory med Azure Active Directory med hjälp av AD FS finns det två Azure Multi-Factor Authentication-alternativ tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="25ab8-105">If your organization has federated your on-premises Active Directory with Azure Active Directory using AD FS, there are two options for using Azure Multi-Factor Authentication.</span></span>

* <span data-ttu-id="25ab8-106">Skydda molnresurser med Azure Multi-Factor Authentication eller Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="25ab8-106">Secure cloud resources using Azure Multi-Factor Authentication or Active Directory Federation Services</span></span>
* <span data-ttu-id="25ab8-107">Skydda molnet och lokala resurser med Azure Multi-Factor Authentication Server</span><span class="sxs-lookup"><span data-stu-id="25ab8-107">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server</span></span>

<span data-ttu-id="25ab8-108">Följande tabell sammanfattar verifieringsupplevelsen när resurser skyddas med Azure Multi-Factor Authentication och AD FS</span><span class="sxs-lookup"><span data-stu-id="25ab8-108">The following table summarizes the verification experience between securing resources with Azure Multi-Factor Authentication and AD FS</span></span>

| <span data-ttu-id="25ab8-109">Verifieringsupplevelse – webbläsarbaserade appar</span><span class="sxs-lookup"><span data-stu-id="25ab8-109">Verification Experience - Browser-based Apps</span></span> | <span data-ttu-id="25ab8-110">Verifieringsupplevelse – appar som inte är webbläsarbaserade</span><span class="sxs-lookup"><span data-stu-id="25ab8-110">Verification Experience - Non-Browser-based Apps</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="25ab8-111">Skydda Azure AD-resurser med hjälp av Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="25ab8-111">Securing Azure AD resources using Azure Multi-Factor Authentication</span></span> |<li><span data-ttu-id="25ab8-112">Det första verifieringssteget utförs lokalt med hjälp av AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ab8-112">The first verification step is performed on-premises using AD FS.</span></span></li> <li><span data-ttu-id="25ab8-113">Det andra steget är en telefonbaserad metod som utförs med hjälp av molnautentisering.</span><span class="sxs-lookup"><span data-stu-id="25ab8-113">The second step is a phone-based method carried out using cloud authentication.</span></span></li> |
| <span data-ttu-id="25ab8-114">Skydda Azure AD-resurser med hjälp av Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="25ab8-114">Securing Azure AD resources using Active Directory Federation Services</span></span> |<li><span data-ttu-id="25ab8-115">Det första verifieringssteget utförs lokalt med hjälp av AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ab8-115">The first verification step is performed on-premises using AD FS.</span></span></li><li><span data-ttu-id="25ab8-116">Det andra steget utförs lokalt genom att anspråket tillämpas.</span><span class="sxs-lookup"><span data-stu-id="25ab8-116">The second step is performed on-premises by honoring the claim.</span></span></li> |

<span data-ttu-id="25ab8-117">Varningar med applösenord för federerade användare:</span><span class="sxs-lookup"><span data-stu-id="25ab8-117">Caveats with app passwords for federated users:</span></span>

* <span data-ttu-id="25ab8-118">Applösenord verifieras med molnautentisering och kringgår därför federation.</span><span class="sxs-lookup"><span data-stu-id="25ab8-118">App passwords are verified using cloud authentication, so they bypass federation.</span></span> <span data-ttu-id="25ab8-119">Federation används endast aktivt när applösenorden konfigureras.</span><span class="sxs-lookup"><span data-stu-id="25ab8-119">Federation is only actively used when setting up an app password.</span></span>
* <span data-ttu-id="25ab8-120">Inställningar för lokal klientåtkomstkontroll respekteras inte av applösenord.</span><span class="sxs-lookup"><span data-stu-id="25ab8-120">On-premises Client Access Control settings are not honored by app passwords.</span></span>
* <span data-ttu-id="25ab8-121">Du kan inte använda lokal autentiseringsloggning med applösenord.</span><span class="sxs-lookup"><span data-stu-id="25ab8-121">You lose on-premises authentication-logging capability for app passwords.</span></span>
* <span data-ttu-id="25ab8-122">Inaktiveringen eller borttagningen av konton kan ta upp till tre timmar för katalogsynkronisering, vilket försenar inaktiveringen eller borttagningen av applösenord i molnidentiteten.</span><span class="sxs-lookup"><span data-stu-id="25ab8-122">Account disable/deletion may take up to three hours for directory sync, delaying disable/deletion of app passwords in the cloud identity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25ab8-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25ab8-123">Next steps</span></span>
<span data-ttu-id="25ab8-124">Information om hur du konfigurerar Azure Multi-Factor Authentication eller Azure Multi-Factor Authentication Server med AD FS finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="25ab8-124">For information on setting up either Azure Multi-Factor Authentication or the Azure Multi-Factor Authentication Server with AD FS, see the following articles:</span></span>

* [<span data-ttu-id="25ab8-125">Skydda molnresurser med hjälp av Azure Multi-Factor Authentication och AD FS</span><span class="sxs-lookup"><span data-stu-id="25ab8-125">Secure cloud resources using Azure Multi-Factor Authentication and AD FS</span></span>](multi-factor-authentication-get-started-adfs-cloud.md)
* [<span data-ttu-id="25ab8-126">Skydda molnresurser och lokala resurser med Azure Multi-Factor Authentication Server med Windows Server 2012 R2 AD FS</span><span class="sxs-lookup"><span data-stu-id="25ab8-126">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server with Windows Server 2012 R2 AD FS</span></span>](multi-factor-authentication-get-started-adfs-w2k12.md)
* [<span data-ttu-id="25ab8-127">Skydda molnet och lokala resurser med Azure Multi-Factor Authentication Server med AD FS 2.0</span><span class="sxs-lookup"><span data-stu-id="25ab8-127">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server with AD FS 2.0</span></span>](multi-factor-authentication-get-started-adfs-adfs2.md)
