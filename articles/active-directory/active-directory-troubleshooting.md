---
title: "Felsöka: ”Active Directory-objekt är saknas eller är inte tillgänglig | Microsoft Docs"
description: "Vad du gör om menyalternativet för Active Directory inte visas i Azure-hanteringsportalen."
services: active-directory
documentationcenter: na
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: be3a797c4a405fd2f6636e67f4c961dd6d143486
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a><span data-ttu-id="0ba93-103">Felsöka: ”Active Directory-objekt är saknas eller är inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="0ba93-103">Troubleshooting: 'Active Directory' item is missing or not available</span></span>
<span data-ttu-id="0ba93-104">Många av anvisningar för användning av Azure Active Directory-funktioner och tjänster som börjar med ”gå till Azure-hanteringsportalen och klicka på **Active Directory**”.</span><span class="sxs-lookup"><span data-stu-id="0ba93-104">Many of the instructions for using Azure Active Directory features and services begin with "Go to the Azure Management Portal and click **Active Directory**."</span></span> <span data-ttu-id="0ba93-105">Men vad gör du om Active Directory-tillägget eller menyobjektet inte visas eller om den är markerad **saknas**?</span><span class="sxs-lookup"><span data-stu-id="0ba93-105">But what do you do if the Active Directory extension or menu item does not appear or if it is marked **Not Available**?</span></span> <span data-ttu-id="0ba93-106">Det här avsnittet är avsedda att hjälpa.</span><span class="sxs-lookup"><span data-stu-id="0ba93-106">This topic is designed to help.</span></span> <span data-ttu-id="0ba93-107">Beskriver villkor under vilka **Active Directory** visas inte eller är inte tillgängligt och förklarar hur du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="0ba93-107">It describes the conditions under which **Active Directory** does not appear or is unavailable and explains how to proceed.</span></span>

## <a name="active-directory-is-missing"></a><span data-ttu-id="0ba93-108">Active Directory saknas</span><span class="sxs-lookup"><span data-stu-id="0ba93-108">Active Directory is missing</span></span>
<span data-ttu-id="0ba93-109">Normalt en **Active Directory** objektet visas i den vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0ba93-109">Typically, an **Active Directory** item appears in the left navigation menu.</span></span> <span data-ttu-id="0ba93-110">Anvisningarna i Azure Active Directory procedurer förutsätter att det här objektet är i vyn.</span><span class="sxs-lookup"><span data-stu-id="0ba93-110">The instructions in Azure Active Directory procedures assume that this item is in your view.</span></span>

![Skärmbild som visar: Active Directory i Azure](./media/active-directory-troubleshooting/typical-view.png)

<span data-ttu-id="0ba93-112">Active Directory-objektet visas i den vänstra navigeringsmenyn när något av följande villkor är sant.</span><span class="sxs-lookup"><span data-stu-id="0ba93-112">The Active Directory item appears in the left navigation menu when any of the following conditions is true.</span></span> <span data-ttu-id="0ba93-113">Annars visas inte objektet.</span><span class="sxs-lookup"><span data-stu-id="0ba93-113">Otherwise, the item does not appear.</span></span>

* <span data-ttu-id="0ba93-114">Den aktuella användaren har loggat in med ett Microsoft-konto (tidigare känt som Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="0ba93-114">The current user signed on with a Microsoft account (formerly known as a Windows Live ID).</span></span>
  
    <span data-ttu-id="0ba93-115">ELLER</span><span class="sxs-lookup"><span data-stu-id="0ba93-115">OR</span></span>
* <span data-ttu-id="0ba93-116">Azure-klient har en katalog och det aktuella kontot är en katalogadministratör.</span><span class="sxs-lookup"><span data-stu-id="0ba93-116">The Azure tenant has a directory and the current account is a directory administrator.</span></span>
  
    <span data-ttu-id="0ba93-117">ELLER</span><span class="sxs-lookup"><span data-stu-id="0ba93-117">OR</span></span>
* <span data-ttu-id="0ba93-118">Azure-klient har minst en Azure AD-åtkomstkontroll (ACS) namnområde.</span><span class="sxs-lookup"><span data-stu-id="0ba93-118">The Azure tenant has at least one Azure AD Access Control (ACS) namespace.</span></span> <span data-ttu-id="0ba93-119">Mer information finns i [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ba93-119">For more information, see [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span></span>
  
    <span data-ttu-id="0ba93-120">ELLER</span><span class="sxs-lookup"><span data-stu-id="0ba93-120">OR</span></span>
* <span data-ttu-id="0ba93-121">Azure-klient har minst en provider för Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="0ba93-121">The Azure tenant has at least one Azure Multi-Factor Authentication provider.</span></span> <span data-ttu-id="0ba93-122">Mer information finns i [administrera Azure Multi-Factor Authentication-Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="0ba93-122">For more information, see [Administering Azure Multi-Factor Authentication Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>

<span data-ttu-id="0ba93-123">Klicka för att skapa ett namnområde för åtkomstkontroll eller en flerfunktionsautentiseringsleverantör **+ ny** > **Apptjänster** > **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0ba93-123">To create an Access Control namespace or a Multi-Factor Authentication provider, click **+New** > **App Services** > **Active Directory**.</span></span>

<span data-ttu-id="0ba93-124">Om du vill ha administrativ behörighet till en katalog har en administratör tilldela en administratörsroll till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="0ba93-124">To get administrative rights to a directory, have an administrator assign an administrator role to your account.</span></span> <span data-ttu-id="0ba93-125">Mer information finns i [Tilldela administratörsroller](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="0ba93-125">For details, see [Assigning administrator roles](active-directory-assign-admin-roles.md).</span></span>

## <a name="active-directory-is-not-available"></a><span data-ttu-id="0ba93-126">Active Directory är inte tillgängligt</span><span class="sxs-lookup"><span data-stu-id="0ba93-126">Active Directory is not available</span></span>
<span data-ttu-id="0ba93-127">När du klickar på **+ ny** > **Apptjänster**, en **Active Directory** objektet visas.</span><span class="sxs-lookup"><span data-stu-id="0ba93-127">When you click **+New** > **App Services**, an **Active Directory** item appears.</span></span> <span data-ttu-id="0ba93-128">Mer specifikt visas Active Directory-objekt när någon av Active Directory-funktioner, till exempel katalog, Access Control eller leverantör av Multifaktorautent, är tillgängliga för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0ba93-128">Specifically, the Active Directory item appears when any of the Active Directory features, such as Directory, Access Control, or Multi-Factor Auth Provider, are available to the current user.</span></span>

<span data-ttu-id="0ba93-129">Dock medan sidan läses objektet är nedtonad och markeras **inte tillgänglig**.</span><span class="sxs-lookup"><span data-stu-id="0ba93-129">However, while the page is loading, the item is dimmed and is marked **Not Available**.</span></span> <span data-ttu-id="0ba93-130">Detta är ett tillfälligt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0ba93-130">This is a temporary state.</span></span> <span data-ttu-id="0ba93-131">Om du vänta några sekunder blir objektet tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="0ba93-131">If you wait a few seconds, the item becomes available.</span></span> <span data-ttu-id="0ba93-132">Om fördröjningen förlängs, löser uppdatera sidan ofta problemet.</span><span class="sxs-lookup"><span data-stu-id="0ba93-132">If the delay is prolonged, refreshing the web page often resolves the problem.</span></span>

![Skärmbild som visar: Active Directory är inte tillgängligt](./media/active-directory-troubleshooting/not-available.png)

