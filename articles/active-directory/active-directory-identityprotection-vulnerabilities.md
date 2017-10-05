---
title: "Sårbarheter som identifieras av Azure Active Directory Identity Protection | Microsoft Docs"
description: "Översikt över sårbarheter som identifieras av Azure Active Directory Identity Protection."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 364873ff54099a6123e40b12e819d1745751f285
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a><span data-ttu-id="d2e94-104">Sårbarheter som identifieras av Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="d2e94-104">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="d2e94-105">Säkerhetsrisker är svagheter i din miljö som kan utnyttjas av en angripare.</span><span class="sxs-lookup"><span data-stu-id="d2e94-105">Vulnerabilities are weaknesses in your environment that can be exploited by an attacker.</span></span> <span data-ttu-id="d2e94-106">Vi rekommenderar att åtgärda dessa problem för att förbättra säkerhetsläget i din organisation och förhindrar att angripare utnyttjar dem.</span><span class="sxs-lookup"><span data-stu-id="d2e94-106">We recommend that you address these vulnerabilities to improve the security posture of your organization, and prevent attackers from exploiting them.</span></span>


<span data-ttu-id="d2e94-107">![säkerhetsproblem](./media/active-directory-identityprotection-vulnerabilities/101.png "säkerhetsrisker")</span><span class="sxs-lookup"><span data-stu-id="d2e94-107">![vulnerabilities](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")</span></span>



<span data-ttu-id="d2e94-108">Följande avsnitt ger dig en översikt över de säkerhetsrisker som rapporterats av Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="d2e94-108">The following sections provide you with an overview of the vulnerabilities reported by Identity Protection.</span></span>

## <a name="multi-factor-authentication-registration-not-configured"></a><span data-ttu-id="d2e94-109">Multifaktorautentisering registrering har inte konfigurerats</span><span class="sxs-lookup"><span data-stu-id="d2e94-109">Multi-factor authentication registration not configured</span></span>
<span data-ttu-id="d2e94-110">Den här säkerhetsrisken hjälper dig att styra distributionen av Azure Multi-Factor Authentication i din organisation.</span><span class="sxs-lookup"><span data-stu-id="d2e94-110">This vulnerability helps you control the deployment of Azure Multi-Factor Authentication in your organization.</span></span> 

<span data-ttu-id="d2e94-111">Azure Multi-Factor authentication ger ett andra säkerhetslager för autentisering av användare.</span><span class="sxs-lookup"><span data-stu-id="d2e94-111">Azure multi-factor authentication provides a second layer of security to user authentication.</span></span> <span data-ttu-id="d2e94-112">Det hjälper dig att skydda åtkomst till data och program och uppfyller efterfrågan från användarna för en process för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d2e94-112">It helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="d2e94-113">Den ger stark autentisering via en mängd alternativ för enkel verifiering – telefonsamtal, textmeddelande eller mobilapp meddelande eller verifiering kod och 3 part OATH-token.</span><span class="sxs-lookup"><span data-stu-id="d2e94-113">It delivers strong authentication via a range of easy verification options—phone call, text message, or mobile app notification or verification code and 3rd party OATH tokens.</span></span>

<span data-ttu-id="d2e94-114">Vi rekommenderar att du behöver Azure Multi-Factor Authentication för användarinloggningar.</span><span class="sxs-lookup"><span data-stu-id="d2e94-114">We recommend that you require Azure Multi-Factor Authentication for user sign-ins.</span></span> <span data-ttu-id="d2e94-115">Multifaktorautentisering spelar en viktig roll i risk-baserade villkorliga åtkomstprinciper tillgängliga via Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="d2e94-115">Multi-factor authentication plays a key role in risk-based conditional access policies available through Identity Protection.</span></span>

<span data-ttu-id="d2e94-116">Mer information finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="d2e94-116">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

## <a name="unmanaged-cloud-apps"></a><span data-ttu-id="d2e94-117">Ohanterad molnappar</span><span class="sxs-lookup"><span data-stu-id="d2e94-117">Unmanaged cloud apps</span></span>
<span data-ttu-id="d2e94-118">Det här problemet kan du identifiera ohanterade molnappar i din organisation.</span><span class="sxs-lookup"><span data-stu-id="d2e94-118">This vulnerability helps you identify unmanaged cloud apps in your organization.</span></span>

<span data-ttu-id="d2e94-119">I moderna företag inte IT-avdelningar ofta känner till alla molnprogram som användare i organisationen använder för att utföra sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="d2e94-119">In modern enterprises, IT departments are often unaware of all the cloud applications that users in their organization are using to do their work.</span></span> <span data-ttu-id="d2e94-120">Det är enkelt att se varför administratörer skulle oroar obehörig åtkomst till företagsdata, möjliga dataläckage och andra säkerhetsrisker.</span><span class="sxs-lookup"><span data-stu-id="d2e94-120">It is easy to see why administrators would have concerns about unauthorized access to corporate data, possible data leakage and other security risks.</span></span> 

<span data-ttu-id="d2e94-121">Vi rekommenderar att din organisation distribuerar Cloud App Discovery att identifiera ohanterade molnprogram och hantera dessa program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d2e94-121">We recommend that your organization deploy Cloud App Discovery to discover unmanaged cloud applications, and to manage these applications using Azure Active Directory.</span></span>

<span data-ttu-id="d2e94-122">Mer information finns i [hitta ohanterade molnprogram med Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d2e94-122">For more details, see [Finding unmanaged cloud applications with Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>

## <a name="security-alerts-from-privileged-identity-management"></a><span data-ttu-id="d2e94-123">Säkerhetsaviseringar från Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="d2e94-123">Security Alerts from Privileged Identity Management</span></span>
<span data-ttu-id="d2e94-124">Den här säkerhetsrisken hjälper dig att identifiera och lösa aviseringar om Privilegierade identiteter i din organisation.</span><span class="sxs-lookup"><span data-stu-id="d2e94-124">This vulnerability helps you discover and resolve alerts about privileged identities in your organization.</span></span>  

<span data-ttu-id="d2e94-125">Om du vill att användare ska kunna utföra Privilegierade åtgärder organisationer behöver ge användare tillfälligt eller permanent privilegierad åtkomst i Azure AD Azure eller Office 365 resurser eller andra SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d2e94-125">To enable users to carry out privileged operations, organizations need to grant users temporary or permanent privileged access in Azure AD, Azure or Office 365 resources, or other SaaS apps.</span></span> <span data-ttu-id="d2e94-126">Var och en av dessa Privilegierade användare ökar risken för angrepp på din organisation.</span><span class="sxs-lookup"><span data-stu-id="d2e94-126">Each of these privileged users increases the attack surface of your organization.</span></span> <span data-ttu-id="d2e94-127">Den här säkerhetsrisken hjälper dig att identifiera användare med onödiga privilegierad åtkomst och vidta lämpliga åtgärder för att minska eller eliminera risken de utgör.</span><span class="sxs-lookup"><span data-stu-id="d2e94-127">This vulnerability helps you identify users with unnecessary privileged access, and take appropriate action to reduce or eliminate the risk they pose.</span></span> 

<span data-ttu-id="d2e94-128">Vi rekommenderar att din organisation använder Azure AD Privileged Identity Management för att hantera, styr och övervaka Privilegierade identiteter och deras åtkomst till resurser i Azure AD samt andra Microsoft online services som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="d2e94-128">We recommend that your organization uses Azure AD Privileged Identity Management to manage, control, and monitor privileged identities and their access to resources in Azure AD as well as other Microsoft online services like Office 365 or Microsoft Intune.</span></span>

<span data-ttu-id="d2e94-129">Mer information finns i [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="d2e94-129">For more details, see [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="see-also"></a><span data-ttu-id="d2e94-130">Se även</span><span class="sxs-lookup"><span data-stu-id="d2e94-130">See also</span></span>
* [<span data-ttu-id="d2e94-131">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="d2e94-131">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

