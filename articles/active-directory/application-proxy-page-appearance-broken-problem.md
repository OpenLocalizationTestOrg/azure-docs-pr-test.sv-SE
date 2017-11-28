---
title: "aaaApplication sidan visas inte korrekt för ett program med Application Proxy | Microsoft Docs"
description: "Vägledning när hello sidan inte visas korrekt i ett Application Proxy-program som du har integrerat med Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: f4abaa4e94c512868f2085affe59cac443784a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a><span data-ttu-id="f2fcd-103">Sidan program visas inte korrekt för ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="f2fcd-103">Application page does not display correctly for an Application Proxy application</span></span>

<span data-ttu-id="f2fcd-104">Den här artikeln hjälpa tootroubleshoot problem med Azure Active Directory Application Proxy-program när du navigerar toohello sidan, men något på hello sidan ser inte rätt.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-104">This article help you tootroubleshoot issues with Azure Active Directory Application Proxy applications when you navigate toohello page, but something on hello page doesn't look correct.</span></span>

## <a name="overview"></a><span data-ttu-id="f2fcd-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="f2fcd-105">Overview</span></span>
<span data-ttu-id="f2fcd-106">När du publicerar en Application Proxy-app är bara sidor under din roten tillgängliga vid åtkomst till programmet hello.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-106">When you publish an Application Proxy app, only pages under your root are accessible when accessing hello application.</span></span> <span data-ttu-id="f2fcd-107">Om hello sidan inte visas korrekt, kan hello rot Intern URL som används för programmet hello saknas några resurser för sidan.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-107">If hello page isn’t displaying correctly, hello root internal URL used for hello application may be missing some page resources.</span></span> <span data-ttu-id="f2fcd-108">tooresolve, kontrollera att du har publicerat *alla* hello resurser för hello sida som en del av ditt program.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-108">tooresolve, make sure you have published *all* hello resources for hello page as part of your application.</span></span>

<span data-ttu-id="f2fcd-109">Du kan kontrollera detta är hello problemet genom att öppna nätverk-spårning (till exempel Fiddler eller F12 verktyg i Internet Explorer/kant) ladda hello sida och söker efter 404-fel.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-109">You can verify this is hello issue by opening your network tracker (such as Fiddler, or F12 tools in Internet Explorer/Edge), loading hello page, and looking for 404 errors.</span></span> <span data-ttu-id="f2fcd-110">Värde som anger hello sidor som för närvarande går inte att hitta och du kan fortfarande toobe publiceras.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-110">That indicates hello pages that currently cannot be found and may still need toobe published.</span></span>

<span data-ttu-id="f2fcd-111">Som ett exempel på det här fallet förutsätter att du har publicerat ett utgifter program med hjälp av en intern URL för <http://myapps/expenses>, men hello appen använder hello stylesheet <http://myapps/style.css>.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-111">As an example of this case, assume you have published an expenses application using an internal URL of <http://myapps/expenses>, but hello app uses hello stylesheet <http://myapps/style.css>.</span></span> <span data-ttu-id="f2fcd-112">I det här fallet publiceras inte hello stylesheet i ditt program så inläsning hello utgifter app utlösa ett 404-fel vid försök tooload style.css.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-112">In this case, hello stylesheet is not published in your application, so loading hello expenses app throw a 404 error while trying tooload style.css.</span></span> <span data-ttu-id="f2fcd-113">I det här exemplet hello skulle lösa problem genom att publicera hello program med en intern URL <http://myapp/> i stället.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-113">In this example, hello problem would be resolved by publishing hello application with an internal URL of <http://myapp/> instead.</span></span>

## <a name="problems-with-publishing-as-one-application"></a><span data-ttu-id="f2fcd-114">Problem med publicering som ett program</span><span class="sxs-lookup"><span data-stu-id="f2fcd-114">Problems with publishing as one application</span></span>

<span data-ttu-id="f2fcd-115">Om det inte är möjligt toopublish alla resurser inom Hej samma program, du behöver toopublish flera program och aktivera länkar mellan dem.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-115">If it is not possible toopublish all resources within hello same application, you need toopublish multiple applications and enable links between them.</span></span>

<span data-ttu-id="f2fcd-116">toodo så vi rekommenderar att du använder hello [anpassade domäner](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) lösning.</span><span class="sxs-lookup"><span data-stu-id="f2fcd-116">toodo so, we recommend using hello [custom domains](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) solution.</span></span> <span data-ttu-id="f2fcd-117">Denna lösning kräver dock att du äger hello certifikat för din domän och ditt program använder fullständigt kvalificerade domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="f2fcd-117">However, this solution requires that you own hello certificate for your domain and your applications use fully qualified domain names (FQDNs).</span></span> <span data-ttu-id="f2fcd-118">Andra alternativ finns hello [felsöka brutna länkar dokumentationen](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span><span class="sxs-lookup"><span data-stu-id="f2fcd-118">For other options, see hello [troubleshoot broken links documentation](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2fcd-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2fcd-119">Next steps</span></span>
[<span data-ttu-id="f2fcd-120">Publicera program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="f2fcd-120">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
