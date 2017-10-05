---
title: Skapa ett program med Application Proxy | Microsoft Docs
description: "Så här felsöker du problem med att skapa programproxy program i Azure AD Admin portal"
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
ms.openlocfilehash: fe56f56162ba7186f1b660a5b44fcef38f1dee9d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problem-creating-an-application-proxy-application"></a><span data-ttu-id="7d376-103">Skapa ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="7d376-103">Problem creating an Application Proxy application</span></span> 

<span data-ttu-id="7d376-104">Nedan visas några vanliga problem personer står inför när du skapar ett nytt program för application proxy.</span><span class="sxs-lookup"><span data-stu-id="7d376-104">Below are some of the common issues people face when creating a new application proxy application.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="7d376-105">Rekommenderade dokument</span><span class="sxs-lookup"><span data-stu-id="7d376-105">Recommended documents</span></span> 

<span data-ttu-id="7d376-106">Mer information om hur du skapar ett program för Application Proxy via Admin Portal finns [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="7d376-106">To learn more about creating an Application Proxy application through the Admin Portal, see [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="7d376-107">Om du följer stegen i dokumentet och får ett fel när programmet finns i felinformationen och förslag om hur du löser programmet.</span><span class="sxs-lookup"><span data-stu-id="7d376-107">If you are following the steps in that document and are getting an error creating the application, see the error details for information and suggestions for how to fix the application.</span></span> <span data-ttu-id="7d376-108">De vanligaste felmeddelanden som innehåller en föreslagen åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7d376-108">Most error messages include a suggested fix.</span></span> 

## <a name="specific-things-to-check"></a><span data-ttu-id="7d376-109">Vissa saker att kontrollera</span><span class="sxs-lookup"><span data-stu-id="7d376-109">Specific things to check</span></span>

<span data-ttu-id="7d376-110">Kontrollera följande för att undvika vanliga fel:</span><span class="sxs-lookup"><span data-stu-id="7d376-110">To avoid common errors, verify:</span></span>

-   <span data-ttu-id="7d376-111">Du är administratör med behörighet att skapa ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="7d376-111">You are an administrator with permission to create an Application Proxy application</span></span>

-   <span data-ttu-id="7d376-112">Intern URL är unikt</span><span class="sxs-lookup"><span data-stu-id="7d376-112">The internal URL is unique</span></span>

-   <span data-ttu-id="7d376-113">Den externa URL: en är unikt</span><span class="sxs-lookup"><span data-stu-id="7d376-113">The external URL is unique</span></span>

-   <span data-ttu-id="7d376-114">URL: er börja med http eller https och sluta med en ”/”</span><span class="sxs-lookup"><span data-stu-id="7d376-114">The URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="7d376-115">URL-Adressen ska vara ett domännamn och inte en IP-adress</span><span class="sxs-lookup"><span data-stu-id="7d376-115">The URL should be a domain name and not an IP address</span></span>

<span data-ttu-id="7d376-116">Felmeddelandet ska visas i det övre högra hörnet när du skapar i program.</span><span class="sxs-lookup"><span data-stu-id="7d376-116">The error message should display in the top right corner when you create the application.</span></span> <span data-ttu-id="7d376-117">Du kan också välja meddelandeikonen att se felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="7d376-117">You can also select the notification icon to see the error messages.</span></span>

   ![Meddelande-fråga](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a><span data-ttu-id="7d376-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d376-119">Next steps</span></span>
[<span data-ttu-id="7d376-120">Aktivera Application Proxy på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7d376-120">Enable Application Proxy in the Azure portal</span></span>](active-directory-application-proxy-enable.md)
