---
title: "aaaSet upp en Windows 10-enhet med Azure AD från inställningar | Microsoft Docs"
description: "Beskriver hur användare kan ansluta till tooAzure AD via hello inställningsmenyn."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: b844e1d9-ad43-4e4a-a398-5c4a43bf2f5c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: d6485228338db571cc01f913c99fbba49e0bb74a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a><span data-ttu-id="c38a4-103">Konfigurera en Windows 10-enhet med Azure AD från inställningar</span><span class="sxs-lookup"><span data-stu-id="c38a4-103">Set up a Windows 10 device with Azure AD from Settings</span></span>
<span data-ttu-id="c38a4-104">Om du redan använder Windows 7 eller Windows 8 och din dator eller enhet har varit uppgraderade tooWindows 10, du kan delta i tooAzure Active Directory (AD Azure) via hello inställningsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c38a4-104">If you are already using Windows 7 or Windows 8, and your computer or device has been upgraded tooWindows 10, you can join tooAzure Active Directory (Azure AD) through hello Settings menu.</span></span>

## <a name="toojoin-tooazure-ad-from-hello-settings-menu"></a><span data-ttu-id="c38a4-105">toojoin tooAzure AD hello inställningsmenyn</span><span class="sxs-lookup"><span data-stu-id="c38a4-105">toojoin tooAzure AD from hello Settings menu</span></span>
1. <span data-ttu-id="c38a4-106">Från hello **starta** -menyn klickar du på hello **inställningar** snabbknappen.</span><span class="sxs-lookup"><span data-stu-id="c38a4-106">From hello **Start** menu, click hello **Settings** charm.</span></span>
2. <span data-ttu-id="c38a4-107">Från **inställningar**väljer **System**->**om**->**ansluta till Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="c38a4-107">From **Settings**, select     **System**->**About**->**Join Azure AD**.</span></span>
   
   <span data-ttu-id="c38a4-108"><center>
   ![Ansluta till Azure AD hello inställningsmenyn](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center></span><span class="sxs-lookup"><span data-stu-id="c38a4-108"><center>
![Join Azure AD from hello Settings menu](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center></span></span>
3. <span data-ttu-id="c38a4-109">Klicka på **Fortsätt** i meddelandefönstret för hello Azure AD Join.</span><span class="sxs-lookup"><span data-stu-id="c38a4-109">Click **Continue** in hello Azure AD Join message window.</span></span>
   
   <span data-ttu-id="c38a4-110"><center>
   ![Anslut till Azure AD-meddelandefönstret](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center></span><span class="sxs-lookup"><span data-stu-id="c38a4-110"><center>
![Join Azure AD message window](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center></span></span>
4. <span data-ttu-id="c38a4-111">Ange dina inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c38a4-111">Provide your sign-in credentials.</span></span> <span data-ttu-id="c38a4-112">Den här inloggningen tas alla hello steg som är nödvändiga toocomplete autentisering.</span><span class="sxs-lookup"><span data-stu-id="c38a4-112">This sign-in experience will include all hello steps that are required toocomplete authentication.</span></span> <span data-ttu-id="c38a4-113">Om du är en del av en extern klient kan ger administratören dig hello federation upplevelse som finns i din organisation.</span><span class="sxs-lookup"><span data-stu-id="c38a4-113">If you are part of a federated tenant, your administrator will provide you with hello federation experience that's hosted by your organization.</span></span>
   <span data-ttu-id="c38a4-114"><center>
   ![Ange inloggningsuppgifter](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center></span><span class="sxs-lookup"><span data-stu-id="c38a4-114"><center>
![Provide sign-in credentials](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center></span></span>
5. <span data-ttu-id="c38a4-115">Om din organisation har konfigurerat Azure Multi-Factor Authentication för att ansluta till tooAzure AD kan du ange hello tvåfaktorsautentisering innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="c38a4-115">If your organization has configured Azure Multi-Factor Authentication for joining tooAzure AD, provide hello second factor before proceeding.</span></span>
6. <span data-ttu-id="c38a4-116">Klicka på **acceptera** på hello **Tillåt den här enheten toobe hanteras** skärmen.</span><span class="sxs-lookup"><span data-stu-id="c38a4-116">Click **Accept** on hello **Allow this device toobe managed** screen.</span></span>
7. <span data-ttu-id="c38a4-117">Du bör se ”enheten är nu ansluten tooyour organisation i Azure AD” hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c38a4-117">You should see hello message "Your device is now joined tooyour organization in Azure AD".</span></span>

## <a name="additional-information"></a><span data-ttu-id="c38a4-118">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="c38a4-118">Additional information</span></span>
* [<span data-ttu-id="c38a4-119">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="c38a4-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="c38a4-120">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="c38a4-120">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="c38a4-121">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="c38a4-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)
* [<span data-ttu-id="c38a4-122">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="c38a4-122">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)

