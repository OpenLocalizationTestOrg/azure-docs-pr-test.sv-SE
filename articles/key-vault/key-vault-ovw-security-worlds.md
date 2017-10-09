---
ms.assetid: 
title: "aaaAzure Key Vault säkerhetsvärldar | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 1995119c9e9886829d6c50af921f275d10e382f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a><span data-ttu-id="05ae6-102">Azure Key Vault-säkerhetsvärldar och geografisk gränser</span><span class="sxs-lookup"><span data-stu-id="05ae6-102">Azure Key Vault security worlds and geographic boundaries</span></span>

<span data-ttu-id="05ae6-103">Azure Key Vault är en tjänst med flera klienter och använder en pool med Maskinvarusäkerhetsmoduler (HSM) i varje Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="05ae6-103">Azure Key Vault is a multi-tenant service and uses a pool of Hardware Security Modules (HSMs) in each Azure location.</span></span> 

<span data-ttu-id="05ae6-104">Alla HSM: er på Azure platser i hello hello samma geografiska region delar samma kryptografiska gräns (Thales Säkerhetsvärld).</span><span class="sxs-lookup"><span data-stu-id="05ae6-104">All HSMs at Azure locations in hello same geographic region share hello same cryptographic boundary (Thales Security World).</span></span> <span data-ttu-id="05ae6-105">Till exempel hello östra USA och västra USA delar samma säkerhetsvärld eftersom de tillhör toohello USA geoplats.</span><span class="sxs-lookup"><span data-stu-id="05ae6-105">For example, East US and West US share hello same security world because they belong toohello US geo location.</span></span> <span data-ttu-id="05ae6-106">På liknande sätt kan alla platser i Japan resurs som Azure hello samma säkerhetsvärld och alla Azure platser i Australien Indien, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="05ae6-106">Similarly, all Azure locations in Japan share hello same security world and all Azure locations in Australia, India, and so on.</span></span> 

## <a name="backup-and-restore-behavior"></a><span data-ttu-id="05ae6-107">Beteende för säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="05ae6-107">Backup and restore behavior</span></span>

<span data-ttu-id="05ae6-108">En säkerhetskopia som gjorts av en nyckel från nyckelvalvet i ett Azure-plats kan vara återställts tooa nyckelvalvet i en annan Azure-plats, förutsatt att båda dessa villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="05ae6-108">A backup taken of a key from a key vault in one Azure location can be restored tooa key vault in another Azure location, as long as both of these conditions are true:</span></span>

- <span data-ttu-id="05ae6-109">Både hello Azure platser tillhör toohello samma geografiska plats</span><span class="sxs-lookup"><span data-stu-id="05ae6-109">Both of hello Azure locations belong toohello same geographical location</span></span>
- <span data-ttu-id="05ae6-110">Både hello nyckelvalv tillhör toohello samma Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="05ae6-110">Both of hello key vaults belong toohello same Azure subscription</span></span>

<span data-ttu-id="05ae6-111">Till exempel en säkerhetskopia som gjorts av en viss prenumeration på en nyckel i nyckelvalvet i västra Indien endast kan återställda tooanother nyckelvalvet i hello samma prenumeration och geoplats; Västra Indien, centrala Indien eller södra Indien.</span><span class="sxs-lookup"><span data-stu-id="05ae6-111">For example, a backup taken by a given subscription of a key in a key vault in West India, can only be restored tooanother key vault in hello same subscription and geo location; West India, Central India or South India.</span></span>

## <a name="regions-and-products"></a><span data-ttu-id="05ae6-112">Regioner och produkter</span><span class="sxs-lookup"><span data-stu-id="05ae6-112">Regions and products</span></span>

- [<span data-ttu-id="05ae6-113">Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="05ae6-113">Azure regions</span></span>](https://azure.microsoft.com/regions/)
- [<span data-ttu-id="05ae6-114">Microsoft-produkter efter region</span><span class="sxs-lookup"><span data-stu-id="05ae6-114">Microsoft products by region</span></span>](https://azure.microsoft.com/regions/services/)

<span data-ttu-id="05ae6-115">Regioner är mappade toosecurity världar visas som större rubriker i hello tabeller:</span><span class="sxs-lookup"><span data-stu-id="05ae6-115">Regions are mapped toosecurity worlds, shown as major headings in hello tables:</span></span>

<span data-ttu-id="05ae6-116">I hello produkter efter region artikel, till exempel hello **Americas** fliken innehåller ÖSTRA USA, centrala USA, västra USA alla mappning toohello Americas område.</span><span class="sxs-lookup"><span data-stu-id="05ae6-116">In hello products by region article, for example, hello **Americas** tab contains EAST US, CENTRAL US, WEST US all mapping toohello Americas region.</span></span> 

>[!NOTE]
><span data-ttu-id="05ae6-117">Ett undantag är att oss DOD ÖST och oss DOD centrala har sina egna säkerhetsvärldar.</span><span class="sxs-lookup"><span data-stu-id="05ae6-117">An exception is that US DOD EAST and US DOD CENTRAL have their own security worlds.</span></span> 

<span data-ttu-id="05ae6-118">På liknande sätt på hello **Europa** fliken NORRA Europa och VÄSTEUROPA mappar båda toohello Europa region.</span><span class="sxs-lookup"><span data-stu-id="05ae6-118">Similarly, on hello **Europe** tab, NORTH EUROPE and WEST EUROPE both map toohello Europe region.</span></span> <span data-ttu-id="05ae6-119">hello gäller också på hello **Stillahavsområdet** fliken.</span><span class="sxs-lookup"><span data-stu-id="05ae6-119">hello same is also true on hello **Asia Pacific** tab.</span></span>



