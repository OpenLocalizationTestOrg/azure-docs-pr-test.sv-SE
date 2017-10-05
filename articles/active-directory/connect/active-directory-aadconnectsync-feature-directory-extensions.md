---
title: "Azure AD Connect-synkronisering: katalogtillägg | Microsoft Docs"
description: "Det här avsnittet beskriver katalogfunktionen i Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8ab9944437443a6d796345782f4f798aec96a0f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a><span data-ttu-id="4b1a5-103">Azure AD Connect-synkronisering: katalogtillägg</span><span class="sxs-lookup"><span data-stu-id="4b1a5-103">Azure AD Connect sync: Directory extensions</span></span>
<span data-ttu-id="4b1a5-104">Katalogtillägg kan du utöka schemat i Azure AD med dina egna attribut från lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-104">Directory extensions allows you to extend the schema in Azure AD with your own attributes from on-premises Active Directory.</span></span> <span data-ttu-id="4b1a5-105">Den här funktionen kan du skapa LOB-appar som förbrukar attribut du fortsätta att hantera lokalt.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-105">This feature allows you to build LOB apps consuming attributes you continue to manage on-premises.</span></span> <span data-ttu-id="4b1a5-106">Attributen kan användas via [Azure AD Graph katalogtillägg](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) eller [Microsoft Graph](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="4b1a5-106">These attributes can be consumed through [Azure AD Graph directory extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) or [Microsoft Graph](https://graph.microsoft.io/).</span></span> <span data-ttu-id="4b1a5-107">Du kan se den attribut tillgängliga med hjälp av [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) och [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respektive.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-107">You can see the attributes available using [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) and [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respectively.</span></span>

<span data-ttu-id="4b1a5-108">För närvarande förbrukar inga Office 365-arbetsbelastning attributen.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-108">At present no Office 365 workload consumes these attributes.</span></span>

<span data-ttu-id="4b1a5-109">Du kan konfigurera vilka ytterligare attribut som du vill synkronisera i sökvägen för anpassade inställningar i installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-109">You configure which additional attributes you want to synchronize in the custom settings path in the installation wizard.</span></span>
<span data-ttu-id="4b1a5-110">![Schemat tillägget guiden](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span><span class="sxs-lookup"><span data-stu-id="4b1a5-110">![Schema Extension Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span></span>  
<span data-ttu-id="4b1a5-111">Installationen visar följande attribut som är giltig kandidater:</span><span class="sxs-lookup"><span data-stu-id="4b1a5-111">The installation shows the following attributes, which are valid candidates:</span></span>

* <span data-ttu-id="4b1a5-112">Användare och grupp objekttyper</span><span class="sxs-lookup"><span data-stu-id="4b1a5-112">User and Group object types</span></span>
* <span data-ttu-id="4b1a5-113">Enkelvärdesattribut anges: String, Boolean, heltal, binär</span><span class="sxs-lookup"><span data-stu-id="4b1a5-113">Single-valued attributes: String, Boolean, Integer, Binary</span></span>
* <span data-ttu-id="4b1a5-114">Flera värden attribut: sträng, binär</span><span class="sxs-lookup"><span data-stu-id="4b1a5-114">Multi-valued attributes: String, Binary</span></span>

<span data-ttu-id="4b1a5-115">I listan med attribut läses från schema-cache skapas under installationen av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-115">The list of attributes is read from the schema cache created during installation of Azure AD Connect.</span></span> <span data-ttu-id="4b1a5-116">Om du har utökat Active Directory-schemat med ytterligare attribut i [schemat måste uppdateras](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) innan dessa nya attribut visas.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-116">If you have extended the Active Directory schema with additional attributes, then the [schema must be refreshed](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) before these new attributes are visible.</span></span>

<span data-ttu-id="4b1a5-117">Ett objekt i Azure AD kan ha upp till 100 katalogattribut tillägg.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-117">An object in Azure AD can have up to 100 directory extensions attributes.</span></span> <span data-ttu-id="4b1a5-118">Maxlängden är 250 tecken.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-118">The max length is 250 characters.</span></span> <span data-ttu-id="4b1a5-119">Om ett attributvärde är längre trunkeras det av Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-119">If an attribute value is longer, then it is truncated by the sync engine.</span></span>

<span data-ttu-id="4b1a5-120">Under installationen av Azure AD Connect registreras ett program när dessa attribut är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-120">During installation of Azure AD Connect, an application is registered where these attributes are available.</span></span> <span data-ttu-id="4b1a5-121">Du kan se det här programmet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-121">You can see this application in the Azure portal.</span></span>  
![Schemat tillägget App](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

<span data-ttu-id="4b1a5-123">Dessa attribut finns nu tillgängliga diagram:</span><span class="sxs-lookup"><span data-stu-id="4b1a5-123">These attributes are now available through Graph:</span></span>  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

<span data-ttu-id="4b1a5-125">Attribut med tillägget prefixet\_{AppClientId}\_.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-125">The attributes are prefixed with extension\_{AppClientId}\_.</span></span> <span data-ttu-id="4b1a5-126">AppClientId har samma värde för alla attribut i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-126">The AppClientId has the same value for all attributes in your Azure AD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b1a5-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b1a5-127">Next steps</span></span>
<span data-ttu-id="4b1a5-128">Lär dig mer om den [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4b1a5-128">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="4b1a5-129">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="4b1a5-129">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
