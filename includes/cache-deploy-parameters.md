
### <a name="cacheskuname"></a><span data-ttu-id="c0889-101">cacheSKUName</span><span class="sxs-lookup"><span data-stu-id="c0889-101">cacheSKUName</span></span>
<span data-ttu-id="c0889-102">hello prisnivå för hello nya Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="c0889-102">hello pricing tier of hello new Azure Redis Cache.</span></span>

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "hello pricing tier of hello new Azure Redis Cache."
      }
    },

<span data-ttu-id="c0889-103">hello mallen definierar hello värden som tillåts för den här parametern (Basic eller Standard) och tilldelas ett standardvärde (grundläggande), om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="c0889-103">hello template defines hello values that are permitted for this parameter (Basic or Standard), and assigns a default value (Basic) if no value is specified.</span></span> <span data-ttu-id="c0889-104">Basic innehåller en enda nod med flera storlekar som är tillgängliga för dig too53 GB.</span><span class="sxs-lookup"><span data-stu-id="c0889-104">Basic provides a single node with multiple sizes available up too53 GB.</span></span>
<span data-ttu-id="c0889-105">Standarden erbjuder två noder primära/repliken med flera storlekar som är tillgängliga för dig too53 GB och 99,9% SLA.</span><span class="sxs-lookup"><span data-stu-id="c0889-105">Standard provides two-node Primary/Replica with multiple sizes available up too53 GB and 99.9% SLA.</span></span>

### <a name="cacheskufamily"></a><span data-ttu-id="c0889-106">cacheSKUFamily</span><span class="sxs-lookup"><span data-stu-id="c0889-106">cacheSKUFamily</span></span>
<span data-ttu-id="c0889-107">hello-familjen för hello sku.</span><span class="sxs-lookup"><span data-stu-id="c0889-107">hello family for hello sku.</span></span>

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "hello family for hello sku."
      }
    },


### <a name="cacheskucapacity"></a><span data-ttu-id="c0889-108">cacheSKUCapacity</span><span class="sxs-lookup"><span data-stu-id="c0889-108">cacheSKUCapacity</span></span>
<span data-ttu-id="c0889-109">hello storlek hello ny Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="c0889-109">hello size of hello new Azure Redis Cache instance.</span></span> 

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
        "description": "hello size of hello new Azure Redis Cache instance. "
      }
    }


<span data-ttu-id="c0889-110">hello mallen definierar hello värden som tillåts för den här parametern (0, 1, 2, 3, 4, 5 eller 6) och tilldelas ett standardvärde (1) om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="c0889-110">hello template defines hello values that are permitted for this parameter (0, 1, 2, 3, 4, 5 or 6), and assigns a default value (1) if no value is specified.</span></span> <span data-ttu-id="c0889-111">Dessa siffror motsvarar toofollowing cache-storlekar: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span><span class="sxs-lookup"><span data-stu-id="c0889-111">Those numbers correspond toofollowing cache sizes: 0 = 250 MB, 1 = 1 GB, 2 = 2.5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span></span>

