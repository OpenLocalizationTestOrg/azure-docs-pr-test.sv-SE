---
title: "Felsöka: ”Active Directory-objekt är saknas eller är inte tillgänglig | Microsoft Docs"
description: "Vilka toodo när menyalternativet för Active Directory inte visas i hello Azure-hanteringsportalen."
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
ms.openlocfilehash: d7355a4e39141f0b09272dc5615c309b23c8c70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a><span data-ttu-id="dbc41-103">Felsöka: ”Active Directory-objekt är saknas eller är inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="dbc41-103">Troubleshooting: 'Active Directory' item is missing or not available</span></span>
<span data-ttu-id="dbc41-104">Många av hello anvisningar för användning av Azure Active Directory-funktioner och tjänster som börjar med ”gå toohello Azure-hanteringsportalen och klicka på **Active Directory**”.</span><span class="sxs-lookup"><span data-stu-id="dbc41-104">Many of hello instructions for using Azure Active Directory features and services begin with "Go toohello Azure Management Portal and click **Active Directory**."</span></span> <span data-ttu-id="dbc41-105">Men vad gör du om hello Active Directory-tillägget eller menyobjektet inte visas eller om den är markerad **saknas**?</span><span class="sxs-lookup"><span data-stu-id="dbc41-105">But what do you do if hello Active Directory extension or menu item does not appear or if it is marked **Not Available**?</span></span> <span data-ttu-id="dbc41-106">Det här avsnittet är utformad toohelp.</span><span class="sxs-lookup"><span data-stu-id="dbc41-106">This topic is designed toohelp.</span></span> <span data-ttu-id="dbc41-107">Beskriver hello villkor under vilka **Active Directory** visas inte eller är inte tillgängligt och förklarar hur tooproceed.</span><span class="sxs-lookup"><span data-stu-id="dbc41-107">It describes hello conditions under which **Active Directory** does not appear or is unavailable and explains how tooproceed.</span></span>

## <a name="active-directory-is-missing"></a><span data-ttu-id="dbc41-108">Active Directory saknas</span><span class="sxs-lookup"><span data-stu-id="dbc41-108">Active Directory is missing</span></span>
<span data-ttu-id="dbc41-109">Normalt en **Active Directory** objektet visas i hello vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="dbc41-109">Typically, an **Active Directory** item appears in hello left navigation menu.</span></span> <span data-ttu-id="dbc41-110">hello-instruktioner i Azure Active Directory procedurer förutsätter att det här objektet är i vyn.</span><span class="sxs-lookup"><span data-stu-id="dbc41-110">hello instructions in Azure Active Directory procedures assume that this item is in your view.</span></span>

![Skärmbild som visar: Active Directory i Azure](./media/active-directory-troubleshooting/typical-view.png)

<span data-ttu-id="dbc41-112">hello Active Directory-objektet visas i hello vänstra navigeringsmenyn när någon av hello följande villkor är sant.</span><span class="sxs-lookup"><span data-stu-id="dbc41-112">hello Active Directory item appears in hello left navigation menu when any of hello following conditions is true.</span></span> <span data-ttu-id="dbc41-113">Annars visas inte hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="dbc41-113">Otherwise, hello item does not appear.</span></span>

* <span data-ttu-id="dbc41-114">hello aktuell användare har loggat in med ett Microsoft-konto (tidigare känt som Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="dbc41-114">hello current user signed on with a Microsoft account (formerly known as a Windows Live ID).</span></span>
  
    <span data-ttu-id="dbc41-115">ELLER</span><span class="sxs-lookup"><span data-stu-id="dbc41-115">OR</span></span>
* <span data-ttu-id="dbc41-116">hello Azure-klient har en katalog och hello aktuella kontot är en directory-administratör.</span><span class="sxs-lookup"><span data-stu-id="dbc41-116">hello Azure tenant has a directory and hello current account is a directory administrator.</span></span>
  
    <span data-ttu-id="dbc41-117">ELLER</span><span class="sxs-lookup"><span data-stu-id="dbc41-117">OR</span></span>
* <span data-ttu-id="dbc41-118">hello Azure-klient har minst en Azure AD-åtkomstkontroll (ACS) namnområde.</span><span class="sxs-lookup"><span data-stu-id="dbc41-118">hello Azure tenant has at least one Azure AD Access Control (ACS) namespace.</span></span> <span data-ttu-id="dbc41-119">Mer information finns i [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span><span class="sxs-lookup"><span data-stu-id="dbc41-119">For more information, see [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span></span>
  
    <span data-ttu-id="dbc41-120">ELLER</span><span class="sxs-lookup"><span data-stu-id="dbc41-120">OR</span></span>
* <span data-ttu-id="dbc41-121">hello Azure-klient har minst en provider för Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="dbc41-121">hello Azure tenant has at least one Azure Multi-Factor Authentication provider.</span></span> <span data-ttu-id="dbc41-122">Mer information finns i [administrera Azure Multi-Factor Authentication-Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="dbc41-122">For more information, see [Administering Azure Multi-Factor Authentication Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>

<span data-ttu-id="dbc41-123">toocreate ett namnområde för åtkomstkontroll eller en Multi-Factor Authentication-leverantör, klickar du på **+ ny** > **Apptjänster** > **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="dbc41-123">toocreate an Access Control namespace or a Multi-Factor Authentication provider, click **+New** > **App Services** > **Active Directory**.</span></span>

<span data-ttu-id="dbc41-124">tooget administratörsbehörighet tooa directory har en administratör tilldela administratör rollen tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="dbc41-124">tooget administrative rights tooa directory, have an administrator assign an administrator role tooyour account.</span></span> <span data-ttu-id="dbc41-125">Mer information finns i [Tilldela administratörsroller](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="dbc41-125">For details, see [Assigning administrator roles](active-directory-assign-admin-roles.md).</span></span>

## <a name="active-directory-is-not-available"></a><span data-ttu-id="dbc41-126">Active Directory är inte tillgängligt</span><span class="sxs-lookup"><span data-stu-id="dbc41-126">Active Directory is not available</span></span>
<span data-ttu-id="dbc41-127">När du klickar på **+ ny** > **Apptjänster**, en **Active Directory** objektet visas.</span><span class="sxs-lookup"><span data-stu-id="dbc41-127">When you click **+New** > **App Services**, an **Active Directory** item appears.</span></span> <span data-ttu-id="dbc41-128">Mer specifikt visas hello Active Directory-objekt när någon av hello Active Directory-funktioner, till exempel katalog, Access Control eller leverantör av Multifaktorautent, är tillgängliga toohello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="dbc41-128">Specifically, hello Active Directory item appears when any of hello Active Directory features, such as Directory, Access Control, or Multi-Factor Auth Provider, are available toohello current user.</span></span>

<span data-ttu-id="dbc41-129">Dock medan hello sidan läses hello-objektet är nedtonad och markeras **inte tillgänglig**.</span><span class="sxs-lookup"><span data-stu-id="dbc41-129">However, while hello page is loading, hello item is dimmed and is marked **Not Available**.</span></span> <span data-ttu-id="dbc41-130">Detta är ett tillfälligt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="dbc41-130">This is a temporary state.</span></span> <span data-ttu-id="dbc41-131">Om du vänta några sekunder blir hello objektet tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="dbc41-131">If you wait a few seconds, hello item becomes available.</span></span> <span data-ttu-id="dbc41-132">Om hello fördröjning förlängs löser uppdaterar hello webbsida ofta hello problem.</span><span class="sxs-lookup"><span data-stu-id="dbc41-132">If hello delay is prolonged, refreshing hello web page often resolves hello problem.</span></span>

![Skärmbild som visar: Active Directory är inte tillgängligt](./media/active-directory-troubleshooting/not-available.png)

