---
title: "aaaAzure instans Metadata Service för Windows | Microsoft Docs"
description: "RESTful-gränssnitt tooget information om Windows VM beräkning, nätverk och kommande underhållshändelser."
services: virtual-machines-windows
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: harijay
ms.openlocfilehash: a33c26b5e9ed650be639598cdb6895fc19ccb605
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-instance-metadata-service-for-windows-vms"></a>Azure-instans Metadata-tjänsten för virtuella Windows-datorer


hello Azure instans Metadata Service innehåller information om kör instanser för virtuella datorer som kan använda toomanage och konfigurera de virtuella datorerna.
Detta omfattar information som SKU, nätverkskonfigurationen och kommande underhållshändelser. Mer information om vilken typ av information är tillgänglig finns [metadatakategorier](#instance-metadata-data-categories).

Azures instans Metadata Service är en REST-slutpunkt tillgänglig tooall IaaS-VM som skapats via hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). hello-slutpunkten är tillgänglig på en välkänd icke-dirigerbara IP-adress (`169.254.169.254`) som kan nås från inom hello VM.



> [!IMPORTANT]
> Den här tjänsten är **allmänt tillgänglig** i globala Azure-regioner. Det är i Public preview för myndigheter, Kina och tyska Azure-molnet. Den tar emot uppdateringar tooexpose ny information om virtuell datorinstans regelbundet. Den här sidan visar hello uppdaterade [datakategorier](#instance-metadata-data-categories) tillgängliga.



## <a name="service-availability"></a>Tjänsttillgänglighet för
hello-tjänsten är tillgänglig i alla allmänt tillgänglig Global Azure-regioner. hello-tjänsten är tillgänglig som förhandsversion i hello myndigheter, Kina eller Tyskland regioner.

Regioner                                        | Tillgänglighet?
-----------------------------------------------|-----------------------------------------------
[Alla allmänt tillgänglig Global Azure-regioner](https://azure.microsoft.com/regions/)     | Allmänt tillgänglig 
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | I förhandsversionen 
[Azure Kina](https://www.azure.cn/)                                                           | I förhandsversionen
[Azure Tyskland](https://azure.microsoft.com/overview/clouds/germany/)                    | I förhandsversionen

Den här tabellen uppdateras när hello tjänsten blir tillgänglig i andra Azure-moln.

tootry ut hello instans Metadata tjänsten, skapa en virtuell dator från [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) eller hello [Azure-portalen](http://portal.azure.com) i hello över regioner och följ hello exemplen nedan.

## <a name="usage"></a>Användning

### <a name="versioning"></a>Versionshantering
hello instans Metadata Service är en ny version. Versioner är obligatoriska och hello aktuella versionen är `2017-04-02`.

> [!NOTE] 
> Tidigare förhandsvisningarna av schemalagda händelser stöds {senaste} som hello api-versionen. Det här formatet stöds inte längre och kommer att inaktualiseras i hello framtida.

När vi lägger till nya versioner kan äldre versioner fortfarande användas för kompatibilitet om skripten har beroenden på specifika dataformat. Observera dock att hello aktuella preview version(2017-03-01) inte kanske tillgänglig när hello-tjänsten är allmänt tillgänglig.

### <a name="using-headers"></a>Med hjälp av rubriker
När du frågar hello instans Metadata tjänsten måste du ange hello huvud `Metadata: true` tooensure hello begäran omdirigerades inte oavsiktligt.

### <a name="retrieving-metadata"></a>Hämta metadata

Metadata för instansen är tillgängliga för att köra virtuella datorer skapas/hanteras med hjälp av [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Komma åt alla datakategorier för en virtuell dator-instans med hello på begäran:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> Alla metadatafrågor för instansen är skiftlägeskänsliga.

### <a name="data-output"></a>Utdata
Som standard hello instans Metadata tjänsten returnerar data i JSON-format (`Content-Type: application/json`). Dock kan olika API: er returnera data i olika format om begärt.
hello är följande tabell en referens för andra dataformat stöder API: er.

API | Standardformatet för Data | Andra format
--------|---------------------|--------------
/Instance | JSON | Text
/scheduledevents | JSON | Ingen

tooaccess ett svarsformat som inte är standard, ange hello begärda formatet som en querystring-parameter i hello-begäran. Exempel:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a>Säkerhet
hello instans Metadata tjänstslutpunkten är enbart tillgänglig från hello som kör virtuella instans på en icke-dirigerbara IP-adress. Dessutom begäran med en `X-Forwarded-For` huvud avvisas av hello-tjänsten.
Vi behöver också begäranden toocontain en `Metadata: true` huvud tooensure som hello faktiska begäran var direkt avsedda och inte en del av oavsiktlig omdirigering. 

### <a name="error"></a>Fel
Om det finns en dataelement som inte hittades eller en felaktig begäran, returnerar hello instans Metadata Service standard HTTP-fel. Exempel:

HTTP-statuskod | Orsak
----------------|-------
200 OK |
400 Felaktig förfrågan | Saknas `Metadata: true` sidhuvud
404 Hittades inte | Hej does't begärt element finns 
405 Metoden tillåts inte | Endast `GET` och `POST` stöds
429 för många begäranden | hello API stöder för närvarande högst 5 frågor per sekund
500 tjänstfel     | Försök igen efter en stund

### <a name="examples"></a>Exempel

> [!NOTE] 
> Alla API-svar är JSON-strängar. Alla följande exempel svar är pretty ut för läsbarhet.

#### <a name="retrieving-network-information"></a>Hämtar nätverksinformation

**Förfrågan**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

**Svar**

> [!NOTE] 
> hello svaret är en JSON-sträng. följande exempelsvar hello är pretty ut för läsbarhet.

```
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Hämta offentlig IP-adress

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Hämta alla metadata för en instans

**Förfrågan**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

**Svar**

> [!NOTE] 
> hello svaret är en JSON-sträng. följande exempelsvar hello är pretty ut för läsbarhet.

```
{
  "compute": {
    "location": "westcentralus",
    "name": "IMDSSample",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "Canonical",
    "sku": "16.04.0-LTS",
    "version": "16.04.201610200",
    "vmId": "5d33a910-a7a0-4443-9f01-6a807801b29b",
    "vmSize": "Standard_A1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.0.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.0.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3AF806EC"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Hämtning av metadata i Windows-dator

**Förfrågan**

Instansen metadata kan hämtas i Windows via hello PowerShell verktyget `curl`: 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

Eller via hello `Invoke-RestMethod` cmdlet:
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

**Svar**

> [!NOTE] 
> hello svaret är en JSON-sträng. följande exempelsvar hello är pretty ut för läsbarhet.

```
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Instansen metadata datakategorier
hello följande datakategorier finns tillgängliga hello instans Metadata tjänsten:

Data | Beskrivning
-----|------------
location | Azure Region hello virtuell dator som körs
namn | Namnet på hello VM 
Erbjudande | Ger information om hello VM-avbildning. Det här värdet finns bara för avbildningar som distribueras från Azure-avbildning gallery.
Publisher | Utgivaren av hello VM-avbildning
SKU | Specifika SKU för hello VM-avbildning  
Version | Version av hello VM-avbildning 
osType | Linux- eller Windows 
platformUpdateDomain |  [Uppdateringsdomän](manage-availability.md) hello VM körs i
platformFaultDomain | [Feldomänen](manage-availability.md) hello VM körs i
vmId | [Unik identifierare](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) för hello VM
vmSize | [VM-storlek](sizes.md)
IPv4/privateIpAddress | Lokala IPv4-adressen för hello VM 
IPv4/publicIpAddress | Offentliga IPv4-adressen för hello VM
undernätet och/eller adress | Undernätsadress av hello VM
undernätsprefixet / | Undernätets prefix, exempel 24
IPv6/IP-adress | Den lokala IPv6-adressen för hello VM
MAC-adress | VM mac-adress 
scheduledevents | För närvarande finns i Public Preview finns [scheduledevents](scheduled-events.md)

## <a name="example-scenarios-for-usage"></a>Exempelscenarier för användning  

### <a name="tracking-vm-running-on-azure"></a>Spårning av virtuell dator som kör på Azure

Du kan behöva tootrack hello antalet virtuella datorer som kör programvaran eller agenter som behöver tootrack unika hello VM som en tjänstprovider. toobe kan tooget ett unikt ID för en virtuell dator, Använd hello `vmId` från Metadata-tjänsten för instansen.

**Förfrågan**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

**Svar**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Placering av behållare, partitioner data baserat fel/uppdatera domän 

För vissa scenarier, placering av olika repliker är av yttersta vikt. Till exempel [HDFS replik placering](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) eller placering av behållare via en [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) kan du kräva tooknow hello `platformFaultDomain` och `platformUpdateDomain` hello virtuella datorn körs på.
Du kan fråga data direkt via hello instans Metadata-tjänsten.

**Förfrågan**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

**Svar**

```
0
```

### <a name="getting-more-information-about-hello-vm-during-support-case"></a>Mer information om hello VM under supportärende 

Som en leverantör får du ett supportsamtal där du vill ha tooknow mer information om hello VM. Frågar hello kunden tooshare innehåller hello beräkning metadata grundläggande information för hello support professional tooknow hello typ av VM på Azure. 

**Förfrågan**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

**Svar**

> [!NOTE] 
> hello svaret är en JSON-sträng. följande exempelsvar hello är pretty ut för läsbarhet.

```
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-hello-vm"></a>Exempel på hur metadatatjänsten som använder olika språk i hello VM 

Språk | Exempel 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB
Gå Lan   | https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go            
python   | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY
C++      | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp
C#       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.CS
Javascript | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH
    

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
1. Jag får hello fel `400 Bad Request, Required metadata header not specified`. Vad innebär det?
   * hello instans Metadata tjänsten kräver hello huvud `Metadata: true` toobe skickades hello-begäran. Skicka det här sidhuvudet i hello REST-anrop kan åtkomst toohello instans Metadata-tjänsten. 
2. Varför inte inträffar beräknings-information för den virtuella datorn?
   * Hello instans Metadata tjänsten stöder för närvarande bara instanser som skapats med Azure Resource Manager. Hello framtida, kan vi lägga till stöd för virtuella datorer för molnet tjänsten.
3. Jag har skapat Min virtuella dator via Azure Resource Manager en tid sedan. Varför kan jag inte se compute metadatainformation?
   * För alla virtuella datorer som skapats efter Sep 2016, lägga till en [taggen](../../azure-resource-manager/resource-group-using-tags.md) toostart Se beräkning metadata. För äldre virtuella datorer (som skapats före Sep 2016), Lägg till/ta bort tillägg eller data diskar toohello VM toorefresh metadata.
4. Varför får hello fel `500 Internal Server Error`?
   * Försök att utföra din begäran utifrån exponentiell tillbaka av systemet. Kontakta Azure-supporten om hello problemet kvarstår.
5. Där delar ytterligare frågor/kommentarer?
   * Skicka kommentarer om http://feedback.azure.com.
7. Fungerar detta för Virtual Machine Scale ange instansen?
   * Ja är Metadata-tjänsten tillgänglig för skala ange instanser. 
6. Hur får jag support för hello tjänsten?
   * tooget stöd för hello-tjänsten, skapa ett supportproblem i Azure portal för hello VM där du inte är kan tooget metadata svar efter lång försök 

   ![Stöd för instans-Metadata](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Nästa steg

- Mer information om hello [schemalagda händelser](scheduled-events.md) API **som förhandsversion** tillhandahålls av hello instans Metadata-tjänsten.
