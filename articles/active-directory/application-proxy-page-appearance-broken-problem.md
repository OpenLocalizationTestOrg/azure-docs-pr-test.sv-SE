---
title: "Sidan program visas inte korrekt för ett program med Application Proxy | Microsoft Docs"
description: "Vägledning när sidan inte visas korrekt i ett Application Proxy-program som du har integrerat med Azure AD"
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
ms.openlocfilehash: cac4c333e59ef9a0f28a2f93a7afee22eeafd54e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a><span data-ttu-id="c60ac-103">Sidan program visas inte korrekt för ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="c60ac-103">Application page does not display correctly for an Application Proxy application</span></span>

<span data-ttu-id="c60ac-104">Den här artikeln hjälper dig att felsöka problem med Azure Active Directory Application Proxy-program när du navigerar till sidan, men något på sidan ser inte rätt.</span><span class="sxs-lookup"><span data-stu-id="c60ac-104">This article help you to troubleshoot issues with Azure Active Directory Application Proxy applications when you navigate to the page, but something on the page doesn't look correct.</span></span>

## <a name="overview"></a><span data-ttu-id="c60ac-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="c60ac-105">Overview</span></span>
<span data-ttu-id="c60ac-106">När du publicerar en Application Proxy-app är bara sidor under din roten tillgängliga vid åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="c60ac-106">When you publish an Application Proxy app, only pages under your root are accessible when accessing the application.</span></span> <span data-ttu-id="c60ac-107">Om sidan inte visas korrekt, kan den interna rot-URL som används för programmet saknas några resurser för sidan.</span><span class="sxs-lookup"><span data-stu-id="c60ac-107">If the page isn’t displaying correctly, the root internal URL used for the application may be missing some page resources.</span></span> <span data-ttu-id="c60ac-108">För att lösa, se till att du har publicerat *alla* resurser för sidan som en del av ditt program.</span><span class="sxs-lookup"><span data-stu-id="c60ac-108">To resolve, make sure you have published *all* the resources for the page as part of your application.</span></span>

<span data-ttu-id="c60ac-109">Du kan kontrollera detta är problemet genom att öppna nätverk-spårning (till exempel Fiddler eller F12 verktyg i Internet Explorer/kant), sidan lästes in och söker efter 404-fel.</span><span class="sxs-lookup"><span data-stu-id="c60ac-109">You can verify this is the issue by opening your network tracker (such as Fiddler, or F12 tools in Internet Explorer/Edge), loading the page, and looking for 404 errors.</span></span> <span data-ttu-id="c60ac-110">Värde som anger de sidor som för närvarande går inte att hitta och du kan fortfarande publiceras.</span><span class="sxs-lookup"><span data-stu-id="c60ac-110">That indicates the pages that currently cannot be found and may still need to be published.</span></span>

<span data-ttu-id="c60ac-111">Som ett exempel på det här fallet förutsätter att du har publicerat ett utgifter program med hjälp av en intern URL för <http://myapps/expenses>, men att appen använder formatmallen <http://myapps/style.css>.</span><span class="sxs-lookup"><span data-stu-id="c60ac-111">As an example of this case, assume you have published an expenses application using an internal URL of <http://myapps/expenses>, but the app uses the stylesheet <http://myapps/style.css>.</span></span> <span data-ttu-id="c60ac-112">I det här fallet publiceras inte formatmallen i ditt program så inläsning appen utgifter utlösa ett 404-fel vid försök att läsa in style.css.</span><span class="sxs-lookup"><span data-stu-id="c60ac-112">In this case, the stylesheet is not published in your application, so loading the expenses app throw a 404 error while trying to load style.css.</span></span> <span data-ttu-id="c60ac-113">I det här exemplet skulle att lösa problemet genom att publicera program med en intern URL <http://myapp/> i stället.</span><span class="sxs-lookup"><span data-stu-id="c60ac-113">In this example, the problem would be resolved by publishing the application with an internal URL of <http://myapp/> instead.</span></span>

## <a name="problems-with-publishing-as-one-application"></a><span data-ttu-id="c60ac-114">Problem med publicering som ett program</span><span class="sxs-lookup"><span data-stu-id="c60ac-114">Problems with publishing as one application</span></span>

<span data-ttu-id="c60ac-115">Om det inte går att publicera alla resurser inom samma program, måste du publicera flera program och aktivera länkar mellan dem.</span><span class="sxs-lookup"><span data-stu-id="c60ac-115">If it is not possible to publish all resources within the same application, you need to publish multiple applications and enable links between them.</span></span>

<span data-ttu-id="c60ac-116">Om du vill göra det, bör du använda den [anpassade domäner](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) lösning.</span><span class="sxs-lookup"><span data-stu-id="c60ac-116">To do so, we recommend using the [custom domains](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) solution.</span></span> <span data-ttu-id="c60ac-117">Denna lösning kräver dock att du äger certifikatet för din domän och ditt program använder fullständigt kvalificerade domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="c60ac-117">However, this solution requires that you own the certificate for your domain and your applications use fully qualified domain names (FQDNs).</span></span> <span data-ttu-id="c60ac-118">Andra alternativ finns på [felsöka brutna länkar dokumentationen](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span><span class="sxs-lookup"><span data-stu-id="c60ac-118">For other options, see the [troubleshoot broken links documentation](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c60ac-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c60ac-119">Next steps</span></span>
[<span data-ttu-id="c60ac-120">Publicera program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="c60ac-120">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
