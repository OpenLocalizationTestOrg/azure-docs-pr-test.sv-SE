---
title: "Konfigurera en Windows 10-enhet med Azure AD från inställningar | Microsoft Docs"
description: "Beskriver hur användare kan ansluta till Azure AD via menyn Inställningar."
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
ms.openlocfilehash: 5a3963f16b471ad1ca8681b22a1a027935400465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a><span data-ttu-id="8e9ef-103">Konfigurera en Windows 10-enhet med Azure AD från inställningar</span><span class="sxs-lookup"><span data-stu-id="8e9ef-103">Set up a Windows 10 device with Azure AD from Settings</span></span>
<span data-ttu-id="8e9ef-104">Om du redan använder Windows 7 eller Windows 8 och din dator eller enhet har uppgraderats till Windows 10, kan du ansluta till Azure Active Directory (AD Azure) via menyn Inställningar.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-104">If you are already using Windows 7 or Windows 8, and your computer or device has been upgraded to Windows 10, you can join to Azure Active Directory (Azure AD) through the Settings menu.</span></span>

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a><span data-ttu-id="8e9ef-105">Att ansluta till Azure AD från menyn Inställningar</span><span class="sxs-lookup"><span data-stu-id="8e9ef-105">To join to Azure AD from the Settings menu</span></span>
1. <span data-ttu-id="8e9ef-106">Från den **starta** -menyn klickar du på den **inställningar** snabbknappen.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-106">From the **Start** menu, click the **Settings** charm.</span></span>
2. <span data-ttu-id="8e9ef-107">Från **inställningar**väljer **System**->**om**->**ansluta till Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-107">From **Settings**, select     **System**->**About**->**Join Azure AD**.</span></span>
   
   <span data-ttu-id="8e9ef-108"><center>
   ![Ansluta till Azure AD från menyn inställningar](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center></span><span class="sxs-lookup"><span data-stu-id="8e9ef-108"><center>
![Join Azure AD from the Settings menu](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center></span></span>
3. <span data-ttu-id="8e9ef-109">Klicka på **Fortsätt** i meddelandefönstret Azure AD Join.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-109">Click **Continue** in the Azure AD Join message window.</span></span>
   
   <span data-ttu-id="8e9ef-110"><center>
   ![Anslut till Azure AD-meddelandefönstret](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center></span><span class="sxs-lookup"><span data-stu-id="8e9ef-110"><center>
![Join Azure AD message window](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center></span></span>
4. <span data-ttu-id="8e9ef-111">Ange dina inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-111">Provide your sign-in credentials.</span></span> <span data-ttu-id="8e9ef-112">Den här inloggningen innehåller de steg som krävs för att Slutför autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-112">This sign-in experience will include all the steps that are required to complete authentication.</span></span> <span data-ttu-id="8e9ef-113">Om du är en del av en extern klient kan ger administratören dig federation-upplevelse som finns i din organisation.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-113">If you are part of a federated tenant, your administrator will provide you with the federation experience that's hosted by your organization.</span></span>
   <span data-ttu-id="8e9ef-114"><center>
   ![Ange inloggningsuppgifter](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center></span><span class="sxs-lookup"><span data-stu-id="8e9ef-114"><center>
![Provide sign-in credentials](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center></span></span>
5. <span data-ttu-id="8e9ef-115">Om din organisation har konfigurerat Azure Multi-Factor Authentication för att ansluta till Azure AD kan du ange tvåfaktorsautentisering innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-115">If your organization has configured Azure Multi-Factor Authentication for joining to Azure AD, provide the second factor before proceeding.</span></span>
6. <span data-ttu-id="8e9ef-116">Klicka på **acceptera** på den **Tillåt att den här enheten hanteras** skärmen.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-116">Click **Accept** on the **Allow this device to be managed** screen.</span></span>
7. <span data-ttu-id="8e9ef-117">Du bör se meddelandet ”enheten är nu ansluten till din organisation i Azure AD”.</span><span class="sxs-lookup"><span data-stu-id="8e9ef-117">You should see the message "Your device is now joined to your organization in Azure AD".</span></span>

## <a name="additional-information"></a><span data-ttu-id="8e9ef-118">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="8e9ef-118">Additional information</span></span>
* [<span data-ttu-id="8e9ef-119">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="8e9ef-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="8e9ef-120">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="8e9ef-120">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="8e9ef-121">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="8e9ef-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)
* [<span data-ttu-id="8e9ef-122">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="8e9ef-122">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)

