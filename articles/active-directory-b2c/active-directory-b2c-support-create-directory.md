---
title: "Azure Active Directory B2C: Felsöka skapar hyresgäster | Microsoft Docs"
description: "Problem och lösningar för att skapa en Azure Active Directory eller Azure Active Directory B2C-klient."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 4bb7ec6ec67d3368a0e36c098f290f582510714a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a><span data-ttu-id="be31c-103">Felsökning: skapa en Azure Active Directory eller Azure Active Directory B2C-klient</span><span class="sxs-lookup"><span data-stu-id="be31c-103">Troubleshoot creating an Azure Active Directory or Azure Active Directory B2C tenant</span></span> 

## <a name="create-an-azure-ad-tenant"></a><span data-ttu-id="be31c-104">Skapa en Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="be31c-104">Create an Azure AD tenant</span></span>
<span data-ttu-id="be31c-105">Om du inte skapa en Azure Active Directory (Azure AD)-klient på hello första försöket, försök igen.</span><span class="sxs-lookup"><span data-stu-id="be31c-105">If you can't create an Azure Active Directory (Azure AD) tenant on hello first attempt, try again.</span></span> <span data-ttu-id="be31c-106">Kontakta Azure-supporten om hello problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="be31c-106">If hello problem persists, contact Azure Support.</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="be31c-107">Skapa en Azure AD B2C-klient</span><span class="sxs-lookup"><span data-stu-id="be31c-107">Create an Azure AD B2C tenant</span></span>
<span data-ttu-id="be31c-108">Om du får problem när du [skapa ett Azure Active Directory B2C-klient (Azure AD B2C)](active-directory-b2c-get-started.md), försök hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="be31c-108">If you encounter issues when you [create an Azure Active Directory B2C (Azure AD B2C) tenant](active-directory-b2c-get-started.md), try hello following options:</span></span>

* <span data-ttu-id="be31c-109">Om hello Azure AD B2C-klient inte visas i din lista över klienter, försök igen toocreate hello-klient.</span><span class="sxs-lookup"><span data-stu-id="be31c-109">If hello Azure AD B2C tenant doesn't show up in your list of tenants, try again toocreate hello tenant.</span></span>
* <span data-ttu-id="be31c-110">Om hello Azure AD B2C-klient visas i listan över klienter och följande felmeddelande hello, ta bort hello klient och skapa den igen:</span><span class="sxs-lookup"><span data-stu-id="be31c-110">If hello Azure AD B2C tenant does show up in your list of tenants and you see hello following  error message, delete hello tenant and create it again:</span></span>

    <span data-ttu-id="be31c-111">”Det gick inte att slutföra hello skapandet av 'contosob2c' hello B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="be31c-111">"Could not complete hello creation of hello B2C tenant 'contosob2c'.</span></span> <span data-ttu-id="be31c-112">Finns det [länken](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) för mer hjälp ”.</span><span class="sxs-lookup"><span data-stu-id="be31c-112">Please visit this [link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) for more guidance."</span></span>
* <span data-ttu-id="be31c-113">Det finns kända problem när du tar bort en befintlig Azure AD B2C-klient och skapa den igen med hjälp av hello samma domännamn.</span><span class="sxs-lookup"><span data-stu-id="be31c-113">There are known issues when you delete an existing Azure AD B2C tenant and re-create it by using hello same domain name.</span></span> <span data-ttu-id="be31c-114">När du skapar en ny Azure AD B2C-klient, måste du använda ett annat domännamn.</span><span class="sxs-lookup"><span data-stu-id="be31c-114">When you create a new Azure AD B2C tenant, you must use a different domain name.</span></span>
* <span data-ttu-id="be31c-115">Kontakta Azure-supporten om dessa lösningar inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="be31c-115">If these resolutions don't work, contact Azure Support.</span></span> <span data-ttu-id="be31c-116">Mer information finns i [filbegäranden stöd för Azure AD B2C](active-directory-b2c-support.md).</span><span class="sxs-lookup"><span data-stu-id="be31c-116">For more information, see [File support requests for Azure AD B2C](active-directory-b2c-support.md).</span></span>

