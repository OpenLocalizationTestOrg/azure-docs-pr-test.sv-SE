---
ms.assetid: 
title: "Azure Key Vault-säkerhetsvärldar | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 921bbd109c9ea98d8b5c286a7512bed026412d26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a><span data-ttu-id="f5a16-102">Azure Key Vault-säkerhetsvärldar och geografisk gränser</span><span class="sxs-lookup"><span data-stu-id="f5a16-102">Azure Key Vault security worlds and geographic boundaries</span></span>

<span data-ttu-id="f5a16-103">Azure Key Vault är en tjänst med flera klienter och använder en pool med Maskinvarusäkerhetsmoduler (HSM) i varje Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="f5a16-103">Azure Key Vault is a multi-tenant service and uses a pool of Hardware Security Modules (HSMs) in each Azure location.</span></span> 

<span data-ttu-id="f5a16-104">Alla HSM: er på Azure platser i samma geografiska region dela samma kryptografiska gräns (Thales Säkerhetsvärld).</span><span class="sxs-lookup"><span data-stu-id="f5a16-104">All HSMs at Azure locations in the same geographic region share the same cryptographic boundary (Thales Security World).</span></span> <span data-ttu-id="f5a16-105">Till exempel dela östra USA och västra USA samma säkerhetsvärlden eftersom de tillhör geoplats USA.</span><span class="sxs-lookup"><span data-stu-id="f5a16-105">For example, East US and West US share the same security world because they belong to the US geo location.</span></span> <span data-ttu-id="f5a16-106">På liknande sätt kan dela alla Azure platser i Japan samma säkerhetsvärlden och alla Azure platser i Australien Indien och så vidare.</span><span class="sxs-lookup"><span data-stu-id="f5a16-106">Similarly, all Azure locations in Japan share the same security world and all Azure locations in Australia, India, and so on.</span></span> 

## <a name="backup-and-restore-behavior"></a><span data-ttu-id="f5a16-107">Beteende för säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="f5a16-107">Backup and restore behavior</span></span>

<span data-ttu-id="f5a16-108">En säkerhetskopia som gjorts av en nyckel från nyckelvalvet i en Azure-plats kan återställas till nyckelvalvet i en annan Azure-plats, förutsatt att båda dessa villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="f5a16-108">A backup taken of a key from a key vault in one Azure location can be restored to a key vault in another Azure location, as long as both of these conditions are true:</span></span>

- <span data-ttu-id="f5a16-109">Båda av platserna som Azure tillhör samma geografiska plats</span><span class="sxs-lookup"><span data-stu-id="f5a16-109">Both of the Azure locations belong to the same geographical location</span></span>
- <span data-ttu-id="f5a16-110">Både nyckelvalv tillhör samma Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="f5a16-110">Both of the key vaults belong to the same Azure subscription</span></span>

<span data-ttu-id="f5a16-111">Till exempel kan en säkerhetskopia som gjorts av en viss prenumeration på en nyckel i nyckelvalvet i västra Indien bara återställas till en annan nyckelvalvet i samma prenumeration och geoplats; Västra Indien, centrala Indien eller södra Indien.</span><span class="sxs-lookup"><span data-stu-id="f5a16-111">For example, a backup taken by a given subscription of a key in a key vault in West India, can only be restored to another key vault in the same subscription and geo location; West India, Central India or South India.</span></span>

## <a name="regions-and-products"></a><span data-ttu-id="f5a16-112">Regioner och produkter</span><span class="sxs-lookup"><span data-stu-id="f5a16-112">Regions and products</span></span>

- [<span data-ttu-id="f5a16-113">Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="f5a16-113">Azure regions</span></span>](https://azure.microsoft.com/regions/)
- [<span data-ttu-id="f5a16-114">Microsoft-produkter efter region</span><span class="sxs-lookup"><span data-stu-id="f5a16-114">Microsoft products by region</span></span>](https://azure.microsoft.com/regions/services/)

<span data-ttu-id="f5a16-115">Regioner är mappade till säkerhetsvärldar visas som större rubriker i tabeller:</span><span class="sxs-lookup"><span data-stu-id="f5a16-115">Regions are mapped to security worlds, shown as major headings in the tables:</span></span>

<span data-ttu-id="f5a16-116">I produkter efter region artikel, till exempel den **Americas** fliken innehåller ÖSTRA USA, centrala USA, västra USA all mappning för regionen USA.</span><span class="sxs-lookup"><span data-stu-id="f5a16-116">In the products by region article, for example, the **Americas** tab contains EAST US, CENTRAL US, WEST US all mapping to the Americas region.</span></span> 

>[!NOTE]
><span data-ttu-id="f5a16-117">Ett undantag är att oss DOD ÖST och oss DOD centrala har sina egna säkerhetsvärldar.</span><span class="sxs-lookup"><span data-stu-id="f5a16-117">An exception is that US DOD EAST and US DOD CENTRAL have their own security worlds.</span></span> 

<span data-ttu-id="f5a16-118">På liknande sätt på den **Europa** fliken NORRA Europa och VÄSTEUROPA mappar båda till Europa region.</span><span class="sxs-lookup"><span data-stu-id="f5a16-118">Similarly, on the **Europe** tab, NORTH EUROPE and WEST EUROPE both map to the Europe region.</span></span> <span data-ttu-id="f5a16-119">Samma gäller även på den **Stillahavsområdet** fliken.</span><span class="sxs-lookup"><span data-stu-id="f5a16-119">The same is also true on the **Asia Pacific** tab.</span></span>



