---
title: aaaVulnerabilities som identifieras av Azure Active Directory Identity Protection | Microsoft Docs
description: "Översikt över hello sårbarheter som identifieras av Azure Active Directory Identity Protection."
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
ms.openlocfilehash: 5e1cb401f8b566a180eb46e3420a090bcfc66767
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a><span data-ttu-id="28a1c-104">Sårbarheter som identifieras av Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="28a1c-104">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="28a1c-105">Säkerhetsrisker är svagheter i din miljö som kan utnyttjas av en angripare.</span><span class="sxs-lookup"><span data-stu-id="28a1c-105">Vulnerabilities are weaknesses in your environment that can be exploited by an attacker.</span></span> <span data-ttu-id="28a1c-106">Vi rekommenderar att åtgärda dessa säkerhetsrisker tooimprove hello säkerhetsläget i din organisation och förhindra att angripare utnyttjar dem.</span><span class="sxs-lookup"><span data-stu-id="28a1c-106">We recommend that you address these vulnerabilities tooimprove hello security posture of your organization, and prevent attackers from exploiting them.</span></span>


<span data-ttu-id="28a1c-107">![säkerhetsproblem](./media/active-directory-identityprotection-vulnerabilities/101.png "säkerhetsrisker")</span><span class="sxs-lookup"><span data-stu-id="28a1c-107">![vulnerabilities](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")</span></span>



<span data-ttu-id="28a1c-108">hello ger följande avsnitt dig en översikt över hello säkerhetsrisker som rapporterats av Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="28a1c-108">hello following sections provide you with an overview of hello vulnerabilities reported by Identity Protection.</span></span>

## <a name="multi-factor-authentication-registration-not-configured"></a><span data-ttu-id="28a1c-109">Multifaktorautentisering registrering har inte konfigurerats</span><span class="sxs-lookup"><span data-stu-id="28a1c-109">Multi-factor authentication registration not configured</span></span>
<span data-ttu-id="28a1c-110">Det här problemet kan du styra hello distribution av Azure Multi-Factor Authentication i din organisation.</span><span class="sxs-lookup"><span data-stu-id="28a1c-110">This vulnerability helps you control hello deployment of Azure Multi-Factor Authentication in your organization.</span></span> 

<span data-ttu-id="28a1c-111">Azure Multi-Factor authentication ger ett andra säkerhetslager toouser autentiseringsmetod för nätverkssäkerhet.</span><span class="sxs-lookup"><span data-stu-id="28a1c-111">Azure multi-factor authentication provides a second layer of security toouser authentication.</span></span> <span data-ttu-id="28a1c-112">Skydda åtkomst toodata och program hjälper samtidigt som du uppfyller efterfrågan från användarna för en process för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="28a1c-112">It helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="28a1c-113">Den ger stark autentisering via en mängd alternativ för enkel verifiering – telefonsamtal, textmeddelande eller mobilapp meddelande eller verifiering kod och 3 part OATH-token.</span><span class="sxs-lookup"><span data-stu-id="28a1c-113">It delivers strong authentication via a range of easy verification options—phone call, text message, or mobile app notification or verification code and 3rd party OATH tokens.</span></span>

<span data-ttu-id="28a1c-114">Vi rekommenderar att du behöver Azure Multi-Factor Authentication för användarinloggningar. Multifaktorautentisering spelar en viktig roll i risk-baserade villkorliga åtkomstprinciper tillgängliga via Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="28a1c-114">We recommend that you require Azure Multi-Factor Authentication for user sign-ins. Multi-factor authentication plays a key role in risk-based conditional access policies available through Identity Protection.</span></span>

<span data-ttu-id="28a1c-115">Mer information finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="28a1c-115">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

## <a name="unmanaged-cloud-apps"></a><span data-ttu-id="28a1c-116">Ohanterad molnappar</span><span class="sxs-lookup"><span data-stu-id="28a1c-116">Unmanaged cloud apps</span></span>
<span data-ttu-id="28a1c-117">Det här problemet kan du identifiera ohanterade molnappar i din organisation.</span><span class="sxs-lookup"><span data-stu-id="28a1c-117">This vulnerability helps you identify unmanaged cloud apps in your organization.</span></span>

<span data-ttu-id="28a1c-118">I moderna företag inte IT-avdelningar ofta känner till alla hello molnprogram att användare i organisationen använder toodo sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="28a1c-118">In modern enterprises, IT departments are often unaware of all hello cloud applications that users in their organization are using toodo their work.</span></span> <span data-ttu-id="28a1c-119">Det är enkelt toosee varför administratörer skulle oroar obehörig åtkomst toocorporate data, möjliga dataläckage och andra säkerhetsrisker.</span><span class="sxs-lookup"><span data-stu-id="28a1c-119">It is easy toosee why administrators would have concerns about unauthorized access toocorporate data, possible data leakage and other security risks.</span></span> 

<span data-ttu-id="28a1c-120">Vi rekommenderar att din organisation distribuerar Cloud App Discovery toodiscover ohanterad molnprogram och toomanage dessa program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="28a1c-120">We recommend that your organization deploy Cloud App Discovery toodiscover unmanaged cloud applications, and toomanage these applications using Azure Active Directory.</span></span>

<span data-ttu-id="28a1c-121">Mer information finns i [hitta ohanterade molnprogram med Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="28a1c-121">For more details, see [Finding unmanaged cloud applications with Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>

## <a name="security-alerts-from-privileged-identity-management"></a><span data-ttu-id="28a1c-122">Säkerhetsaviseringar från Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="28a1c-122">Security Alerts from Privileged Identity Management</span></span>
<span data-ttu-id="28a1c-123">Den här säkerhetsrisken hjälper dig att identifiera och lösa aviseringar om Privilegierade identiteter i din organisation.</span><span class="sxs-lookup"><span data-stu-id="28a1c-123">This vulnerability helps you discover and resolve alerts about privileged identities in your organization.</span></span>  

<span data-ttu-id="28a1c-124">tooenable användare toocarry Privilegierade åtgärder, organisationer behöver toogrant användare tillfälligt eller permanent privilegierad åtkomst i Azure AD Azure eller Office 365 resurser eller andra SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="28a1c-124">tooenable users toocarry out privileged operations, organizations need toogrant users temporary or permanent privileged access in Azure AD, Azure or Office 365 resources, or other SaaS apps.</span></span> <span data-ttu-id="28a1c-125">Var och en av dessa Privilegierade användare ökar hello risken för angrepp på din organisation.</span><span class="sxs-lookup"><span data-stu-id="28a1c-125">Each of these privileged users increases hello attack surface of your organization.</span></span> <span data-ttu-id="28a1c-126">Den här säkerhetsrisken hjälper dig att identifiera användare med onödiga privilegierad åtkomst och vidta lämplig åtgärd tooreduce eller eliminera hello risker de utgör.</span><span class="sxs-lookup"><span data-stu-id="28a1c-126">This vulnerability helps you identify users with unnecessary privileged access, and take appropriate action tooreduce or eliminate hello risk they pose.</span></span> 

<span data-ttu-id="28a1c-127">Vi rekommenderar att din organisation använder Azure AD Privileged Identity Management toomanage, styra och övervaka Privilegierade identiteter och deras åtkomst tooresources i Azure AD samt andra Microsoft online services som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="28a1c-127">We recommend that your organization uses Azure AD Privileged Identity Management toomanage, control, and monitor privileged identities and their access tooresources in Azure AD as well as other Microsoft online services like Office 365 or Microsoft Intune.</span></span>

<span data-ttu-id="28a1c-128">Mer information finns i [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="28a1c-128">For more details, see [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="see-also"></a><span data-ttu-id="28a1c-129">Se även</span><span class="sxs-lookup"><span data-stu-id="28a1c-129">See also</span></span>
* [<span data-ttu-id="28a1c-130">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="28a1c-130">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

