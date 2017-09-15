
### <a name="cacheskuname"></a><span data-ttu-id="85370-101">cacheSKUName</span><span class="sxs-lookup"><span data-stu-id="85370-101">cacheSKUName</span></span>
<span data-ttu-id="85370-102">Prisnivån för den nya Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="85370-102">The pricing tier of the new Azure Redis Cache.</span></span>

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Redis Cache."
      }
    },

<span data-ttu-id="85370-103">Mallen definierar de värden som tillåts för den här parametern (Basic eller Standard), och tilldelas ett standardvärde (grundläggande), om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="85370-103">The template defines the values that are permitted for this parameter (Basic or Standard), and assigns a default value (Basic) if no value is specified.</span></span> <span data-ttu-id="85370-104">Basic tillhandahåller en enda nod med flera storlekar som är tillgängliga för upp till 53 GB.</span><span class="sxs-lookup"><span data-stu-id="85370-104">Basic provides a single node with multiple sizes available up to 53 GB.</span></span>
<span data-ttu-id="85370-105">Standarden erbjuder två noder primära/repliken med flera storlekar som är tillgängliga för dig av 53 GB och 99,9% serviceavtal.</span><span class="sxs-lookup"><span data-stu-id="85370-105">Standard provides two-node Primary/Replica with multiple sizes available up to 53 GB and 99.9% SLA.</span></span>

### <a name="cacheskufamily"></a><span data-ttu-id="85370-106">cacheSKUFamily</span><span class="sxs-lookup"><span data-stu-id="85370-106">cacheSKUFamily</span></span>
<span data-ttu-id="85370-107">Familj för SKU: n.</span><span class="sxs-lookup"><span data-stu-id="85370-107">The family for the sku.</span></span>

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a><span data-ttu-id="85370-108">cacheSKUCapacity</span><span class="sxs-lookup"><span data-stu-id="85370-108">cacheSKUCapacity</span></span>
<span data-ttu-id="85370-109">Storleken på den nya Azure Redis-Cache-instansen.</span><span class="sxs-lookup"><span data-stu-id="85370-109">The size of the new Azure Redis Cache instance.</span></span> 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Redis Cache instance. "
      }
    }


<span data-ttu-id="85370-110">Mallen definierar de värden som tillåts för den här parametern (0, 1, 2, 3, 4, 5 eller 6) och tilldelar ett standardvärde (1) om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="85370-110">The template defines the values that are permitted for this parameter (0, 1, 2, 3, 4, 5 or 6), and assigns a default value (1) if no value is specified.</span></span> <span data-ttu-id="85370-111">Dessa siffror motsvarar följande cache-storlekar: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span><span class="sxs-lookup"><span data-stu-id="85370-111">Those numbers correspond to following cache sizes: 0 = 250 MB, 1 = 1 GB, 2 = 2.5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span></span>

