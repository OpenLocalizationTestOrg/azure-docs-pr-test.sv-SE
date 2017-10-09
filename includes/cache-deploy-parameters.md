
### <a name="cacheskuname"></a>cacheSKUName
hello prisnivå för hello nya Azure Redis-Cache.

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

hello mallen definierar hello värden som tillåts för den här parametern (Basic eller Standard) och tilldelas ett standardvärde (grundläggande), om inget värde anges. Basic innehåller en enda nod med flera storlekar som är tillgängliga för dig too53 GB.
Standarden erbjuder två noder primära/repliken med flera storlekar som är tillgängliga för dig too53 GB och 99,9% SLA.

### <a name="cacheskufamily"></a>cacheSKUFamily
hello-familjen för hello sku.

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


### <a name="cacheskucapacity"></a>cacheSKUCapacity
hello storlek hello ny Azure Redis-Cache-instans. 

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


hello mallen definierar hello värden som tillåts för den här parametern (0, 1, 2, 3, 4, 5 eller 6) och tilldelas ett standardvärde (1) om inget värde anges. Dessa siffror motsvarar toofollowing cache-storlekar: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

