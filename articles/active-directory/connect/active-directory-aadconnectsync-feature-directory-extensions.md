---
title: "Azure AD Connect-synkronisering: katalogtillägg | Microsoft Docs"
description: "Det här avsnittet beskrivs hello directory tillägg-funktionen i Azure AD Connect."
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
ms.openlocfilehash: 31525ae914aa4d9e047ea1515b460a8311d5c815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a><span data-ttu-id="b16b8-103">Azure AD Connect-synkronisering: katalogtillägg</span><span class="sxs-lookup"><span data-stu-id="b16b8-103">Azure AD Connect sync: Directory extensions</span></span>
<span data-ttu-id="b16b8-104">Katalogtillägg tillåter tooextend hello schemat i Azure AD med dina egna attribut från lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b16b8-104">Directory extensions allows you tooextend hello schema in Azure AD with your own attributes from on-premises Active Directory.</span></span> <span data-ttu-id="b16b8-105">Den här funktionen kan du använda attribut du fortsätter toomanage lokala toobuild LOB-appar.</span><span class="sxs-lookup"><span data-stu-id="b16b8-105">This feature allows you toobuild LOB apps consuming attributes you continue toomanage on-premises.</span></span> <span data-ttu-id="b16b8-106">Attributen kan användas via [Azure AD Graph katalogtillägg](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) eller [Microsoft Graph](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="b16b8-106">These attributes can be consumed through [Azure AD Graph directory extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) or [Microsoft Graph](https://graph.microsoft.io/).</span></span> <span data-ttu-id="b16b8-107">Du kan se hello attribut tillgängliga med [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) och [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respektive.</span><span class="sxs-lookup"><span data-stu-id="b16b8-107">You can see hello attributes available using [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) and [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respectively.</span></span>

<span data-ttu-id="b16b8-108">För närvarande förbrukar inga Office 365-arbetsbelastning attributen.</span><span class="sxs-lookup"><span data-stu-id="b16b8-108">At present no Office 365 workload consumes these attributes.</span></span>

<span data-ttu-id="b16b8-109">Du konfigurerar vilka ytterligare attribut som du vill toosynchronize i hello anpassade inställningar sökväg i hello installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="b16b8-109">You configure which additional attributes you want toosynchronize in hello custom settings path in hello installation wizard.</span></span>
<span data-ttu-id="b16b8-110">![Schemat tillägget guiden](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span><span class="sxs-lookup"><span data-stu-id="b16b8-110">![Schema Extension Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span></span>  
<span data-ttu-id="b16b8-111">hello installationen visar hello följande attribut som är giltiga alternativ:</span><span class="sxs-lookup"><span data-stu-id="b16b8-111">hello installation shows hello following attributes, which are valid candidates:</span></span>

* <span data-ttu-id="b16b8-112">Användare och grupp objekttyper</span><span class="sxs-lookup"><span data-stu-id="b16b8-112">User and Group object types</span></span>
* <span data-ttu-id="b16b8-113">Enkelvärdesattribut anges: String, Boolean, heltal, binär</span><span class="sxs-lookup"><span data-stu-id="b16b8-113">Single-valued attributes: String, Boolean, Integer, Binary</span></span>
* <span data-ttu-id="b16b8-114">Flera värden attribut: sträng, binär</span><span class="sxs-lookup"><span data-stu-id="b16b8-114">Multi-valued attributes: String, Binary</span></span>

<span data-ttu-id="b16b8-115">hello lista med attribut som läses från hello schema-cache skapas under installationen av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="b16b8-115">hello list of attributes is read from hello schema cache created during installation of Azure AD Connect.</span></span> <span data-ttu-id="b16b8-116">Om du har utökat hello Active Directory-schemat med ytterligare attribut, sedan hello [schemat måste uppdateras](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) innan dessa nya attribut visas.</span><span class="sxs-lookup"><span data-stu-id="b16b8-116">If you have extended hello Active Directory schema with additional attributes, then hello [schema must be refreshed](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) before these new attributes are visible.</span></span>

<span data-ttu-id="b16b8-117">Ett objekt i Azure AD kan ha upp too100 katalogattribut tillägg.</span><span class="sxs-lookup"><span data-stu-id="b16b8-117">An object in Azure AD can have up too100 directory extensions attributes.</span></span> <span data-ttu-id="b16b8-118">hello maxlängden är 250 tecken.</span><span class="sxs-lookup"><span data-stu-id="b16b8-118">hello max length is 250 characters.</span></span> <span data-ttu-id="b16b8-119">Om ett attributvärde är längre trunkeras det av hello Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="b16b8-119">If an attribute value is longer, then it is truncated by hello sync engine.</span></span>

<span data-ttu-id="b16b8-120">Under installationen av Azure AD Connect registreras ett program när dessa attribut är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b16b8-120">During installation of Azure AD Connect, an application is registered where these attributes are available.</span></span> <span data-ttu-id="b16b8-121">Du kan se det här programmet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b16b8-121">You can see this application in hello Azure portal.</span></span>  
![Schemat tillägget App](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

<span data-ttu-id="b16b8-123">Dessa attribut finns nu tillgängliga diagram:</span><span class="sxs-lookup"><span data-stu-id="b16b8-123">These attributes are now available through Graph:</span></span>  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

<span data-ttu-id="b16b8-125">hello-attribut med tillägget prefixet\_{AppClientId}\_.</span><span class="sxs-lookup"><span data-stu-id="b16b8-125">hello attributes are prefixed with extension\_{AppClientId}\_.</span></span> <span data-ttu-id="b16b8-126">Hej AppClientId har hello samma värde för alla attribut i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="b16b8-126">hello AppClientId has hello same value for all attributes in your Azure AD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b16b8-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b16b8-127">Next steps</span></span>
<span data-ttu-id="b16b8-128">Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b16b8-128">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="b16b8-129">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="b16b8-129">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
