---
title: aaaProblem skapar ett program med Application Proxy | Microsoft Docs
description: "Hur tootroubleshoot utfärdar skapar programproxyn program i hello Azure AD Admin portal"
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
ms.openlocfilehash: 24fab83c38a49a9e5754854acf2f9711e374e559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-creating-an-application-proxy-application"></a><span data-ttu-id="9bf1f-103">Skapa ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="9bf1f-103">Problem creating an Application Proxy application</span></span> 

<span data-ttu-id="9bf1f-104">Nedan visas några vanliga problem med hello personer står inför när du skapar ett nytt program för application proxy.</span><span class="sxs-lookup"><span data-stu-id="9bf1f-104">Below are some of hello common issues people face when creating a new application proxy application.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="9bf1f-105">Rekommenderade dokument</span><span class="sxs-lookup"><span data-stu-id="9bf1f-105">Recommended documents</span></span> 

<span data-ttu-id="9bf1f-106">toolearn mer om hur du skapar ett program för Application Proxy via hello Admin Portal finns [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="9bf1f-106">toolearn more about creating an Application Proxy application through hello Admin Portal, see [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="9bf1f-107">Om du följande hello i detta dokument och får ett fel när programmet hello finns hello felinformationen och förslag på hur toofix hello program.</span><span class="sxs-lookup"><span data-stu-id="9bf1f-107">If you are following hello steps in that document and are getting an error creating hello application, see hello error details for information and suggestions for how toofix hello application.</span></span> <span data-ttu-id="9bf1f-108">De vanligaste felmeddelanden som innehåller en föreslagen åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9bf1f-108">Most error messages include a suggested fix.</span></span> 

## <a name="specific-things-toocheck"></a><span data-ttu-id="9bf1f-109">Vissa saker toocheck</span><span class="sxs-lookup"><span data-stu-id="9bf1f-109">Specific things toocheck</span></span>

<span data-ttu-id="9bf1f-110">tooavoid vanliga fel, kontrollera:</span><span class="sxs-lookup"><span data-stu-id="9bf1f-110">tooavoid common errors, verify:</span></span>

-   <span data-ttu-id="9bf1f-111">Du är administratör med behörigheten toocreate ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="9bf1f-111">You are an administrator with permission toocreate an Application Proxy application</span></span>

-   <span data-ttu-id="9bf1f-112">hello Intern URL är unikt</span><span class="sxs-lookup"><span data-stu-id="9bf1f-112">hello internal URL is unique</span></span>

-   <span data-ttu-id="9bf1f-113">hello externa URL: en är unikt</span><span class="sxs-lookup"><span data-stu-id="9bf1f-113">hello external URL is unique</span></span>

-   <span data-ttu-id="9bf1f-114">Hej URL: er starta med http eller https och sluta med en ”/”</span><span class="sxs-lookup"><span data-stu-id="9bf1f-114">hello URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="9bf1f-115">hello Webbadress måste vara ett domännamn och inte en IP-adress</span><span class="sxs-lookup"><span data-stu-id="9bf1f-115">hello URL should be a domain name and not an IP address</span></span>

<span data-ttu-id="9bf1f-116">hello felmeddelande ska visas i hello övre högra hörnet när du skapar hello program.</span><span class="sxs-lookup"><span data-stu-id="9bf1f-116">hello error message should display in hello top right corner when you create hello application.</span></span> <span data-ttu-id="9bf1f-117">Du kan också välja hello ikonen toosee hello felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="9bf1f-117">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Meddelande-fråga](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a><span data-ttu-id="9bf1f-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bf1f-119">Next steps</span></span>
[<span data-ttu-id="9bf1f-120">Aktivera Application Proxy i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9bf1f-120">Enable Application Proxy in hello Azure portal</span></span>](active-directory-application-proxy-enable.md)
