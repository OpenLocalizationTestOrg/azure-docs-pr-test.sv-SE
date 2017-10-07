---
title: "aaaImport och exportera en domänzonen filen tooAzure DNS med hjälp av Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooimport och exportera ett DNS zonen filen tooAzure DNS med hjälp av Azure CLI 1.0"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 4c3163395e151e9934c730349b828c612491016f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-a-dns-zone-file-using-hello-azure-cli-10"></a>Importera och exportera en DNS-zonfilen med hello Azure CLI 1.0 

Den här artikeln får du veta hur tooimport export DNS-zonen i filerna och Azure DNS med hello Azure CLI 1.0.

## <a name="introduction-toodns-zone-migration"></a>Introduktion tooDNS zon migrering

En DNS-zonfilen är en textfil som innehåller information om alla poster för Domain Name System (DNS) i hello zonen. Här följer ett standardformat, vilket gör den lämplig för överföring av DNS-poster mellan DNS-system. Med hjälp av en zonfil är en snabb, tillförlitligt och praktiskt sätt tootransfer en DNS-zon till eller från Azure DNS.

Azure DNS stöder import och export av filer med hjälp av hello Azure-kommandoradsgränssnittet (CLI). Zonen filimport är **inte** stöds för närvarande via Azure PowerShell eller hello Azure-portalen.

hello Azure CLI 1.0 är ett kommandoradsverktyg för flera plattformar som används för att hantera Azure-tjänster. Den är tillgänglig för hello Windows, Mac och Linux-plattformarna från hello [Azure nedladdningar](https://azure.microsoft.com/downloads/). Plattformsoberoende stöd är viktigt för import och export av filer, eftersom hello vanligaste namn serverprogramvara [BINDA](https://www.isc.org/downloads/bind/), vanligtvis körs på Linux.

> [!NOTE]
> Det finns två versioner av hello Azure CLI. CLI1.0 baseras på Node.js och har kommandon som börjar med ”azure”.
> CLI2.0 baseras på Python och har kommandon som börjar med ”az”. När zonen filimport stöds i båda versionerna, bör du använda hello CLI1.0 kommandon som beskrivs i den här sidan.

## <a name="obtain-your-existing-dns-zone-file"></a>Hämta din befintliga DNS-zonfilen

Innan du importerar en DNS-zonfilen till Azure DNS måste tooobtain en kopia av hello zonfilen. hello källan för den här filen är beroende av hello DNS-zon som finns för närvarande.

* Om DNS-zonen finns på en partner-tjänst (till exempel en domänregistrator, dedikerad DNS-värdtjänsten eller alternativa molnprovider), tillhandahålla tjänsten hello möjlighet toodownload hello DNS-zonfilen.
* Om DNS-zonen finns på Windows DNS hello standardmappen för hello filer är **%systemroot%\system32\dns**. hello fullständig sökväg tooeach zonfilen visar även hello **allmänna** för hello DNS-konsolen.
* Om DNS-zonen finns med hjälp av BIND hello platsen för hello-zonfilen för varje zon som anges i konfigurationsfilen för hello BIND **named.conf**.

> [!NOTE]
> Zonen filer som hämtas från GoDaddy har ett avvikande något format. Du behöver toocorrect detta innan du importerar dessa filer till Azure DNS.
>
> DNS-namn i hello RDATA för varje DNS-posten har angetts som ett fullständigt kvalificerat namn, men de inte har en avslutande ””. Det innebär att de tolkas av andra DNS-system som relativa namn. Du behöver tooedit hello zon filen tooappend hello avslutande ””. tootheir namn innan du importerar dem till Azure DNS.
>
> Till exempel hello CNAME-post ”www 3600 i CNAME contoso.com” ska ändras för ”www 3600 IN CNAME contoso.com”.
> (med en avslutande ””.).

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Importera en DNS-zonfilen till Azure DNS

Om du importerar en zonfil skapas en ny zon i Azure DNS om det inte redan finns. Om det redan finns hello zonen måste hello postuppsättningar i hello zonfil slås samman med hello befintliga postuppsättningar.

### <a name="merge-behavior"></a>Sammanfoga beteende

* Som standard kombineras befintliga och nya postuppsättningar. Identiska poster i en sammanfogad postuppsättning är ta bort dubbletter.
* Du kan också genom att ange hello `--force` alternativ, hello importera processen ersätter befintliga postuppsättningar med nya postuppsättningar. Befintliga postuppsättningar som saknar motsvarande post i hello importerade zonfilen är inte tas bort.
* När postuppsättningar sammanfogas används hello toolive TTL (time) redan befintliga uppsättningar av poster. När `--force` är används, hello TTL-värde på hello nya postuppsättning används.
* Start av Authority (SOA)-parametrar (utom `host`) alltid tas från hello importerade zonfilen, oavsett om `--force` används. På samma sätt för hello namnserverposten på hello zonens apex, hämtas hello TTL alltid från hello importerade zonfilen.
* En importerad CNAME-post inte ersätta en befintlig CNAME registrera med samma namn om hello hello `--force` parameter har angetts.
* När en konflikt uppstår mellan en CNAME-post och en annan post med samma namn men olika hello Skriv (oavsett vilket finns en befintlig eller ny), behålls hello befintlig post. Det är oberoende av hello användning av `--force`.

### <a name="additional-information-about-importing"></a>Mer information om hur du importerar

hello ger följande information ytterligare teknisk information om hello zonen import-processen.

* Hej `$TTL` direktiv är valfritt och stöds. När ingen `$TTL` direktiv ges, utan en explicit TTL importeras tooa standard TTL 3600 sekunder. När två poster i hello samma postuppsättning ange olika TTLs, hello lägre värde används.
* Hej `$ORIGIN` direktiv är valfritt och stöds. När ingen `$ORIGIN` anges hello standard-värde som används är hello zonnamnet som anges på kommandoraden för hello (plus avslutar hello ””.).
* Hej `$INCLUDE` och `$GENERATE` direktiven stöds inte.
* Dessa typer stöds: A, AAAA, CNAME, MX, NS, SOA, SRV och TXT.
* hello SOA-post skapas automatiskt av Azure DNS när en zon skapas. När du importerar en zonfil alla SOA-parametrarna hämtas från hello zonfilen *utom* hello `host` parameter. Den här parametern används hello-värde som tillhandahålls av Azure DNS. Det beror på att den här parametern måste referera toohello namn på primär server som ingår i Azure DNS.
* hello namnserverposten på hello zonens apex skapas också automatiskt av Azure DNS när hello zon skapas. Endast importeras hello TTL-värde på den här postuppsättning. Dessa poster innehålla hello servernamn tillhandahålls av Azure DNS. hello registrera data skrivs inte över av hello värden i hello importerade zonfilen.
* Under Public Preview stöder Azure DNS bara en sträng TXT-poster. Flersträngiga TXT-poster är vara sammansatta och trunkerat too255 tecken.

### <a name="cli-format-and-values"></a>CLI-format och värden

hello Azure CLI kommandot tooimport en DNS-zon hello format är:

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

Värden:

* `<resource group>`är hello namn hello resursgruppen för hello zonen i Azure DNS.
* `<zone name>`är hello namnet på hello zonen.
* `<zone file name>`är hello/sökvägsnamn hello zon filen toobe importeras.

Om en zon med det här namnet inte finns i resursgruppen hello, skapas den åt dig. Hello zonen redan hello importerade postuppsättningar slås samman med befintliga postuppsättningar. toooverwrite hello befintliga postuppsättningar, använda hello `--force` alternativet.

tooverify hello filformatet zon utan att faktiskt importera dem, Använd hello `--parse-only` alternativet.

### <a name="step-1-import-a-zone-file"></a>Steg 1. Importera en zonfil

tooimport en zonfil för hello zonen **contoso.com**.

1. Logga in tooyour Azure-prenumeration med hjälp av hello Azure CLI 1.0.

    ```azurecli
    azure login
    ```

2. Välj hello prenumeration där du vill att toocreate ny DNS-zon.

    ```azurecli
    azure account set <subscription name>
    ```

3. Azure DNS är en Azure Resource Manager-only-tjänst så hello Azure CLI måste vara avstängda tooResource Manager-läget.

    ```azurecli
    azure config mode arm
    ```

4. Innan du använder hello Azure DNS-tjänsten måste du registrera din prenumeration toouse hello Microsoft.Network-resursprovidern. (Detta är en engångsåtgärd för varje prenumeration.)

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. Om du inte har någon redan måste du också toocreate en Resource Manager-resursgrupp.

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. tooimport hello zonen **contoso.com** från hello filen **contoso.com.txt** till en ny DNS-zon i hello resursgruppen **myresourcegroup**, kör hello kommando `azure network dns zone import`.<BR>Det här kommandot laddar hello zonfilen och analysera den. hello kommandot Kör en serie kommandon på hello Azure DNS-tjänsten toocreate hello zonen och alla hello postuppsättningar i zonen hello. hello kommandot rapporterar förlopp i hello konsolfönstret, tillsammans med eventuella fel eller varningar. Eftersom postuppsättningar skapas i serien, kan det ta några minuter tooimport en stor zonfil.

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-hello-zone"></a>Steg 2. Kontrollera hello zon

tooverify hello DNS-zonen när du har importerat hello-fil, du kan använda någon av följande metoder hello:

* Du kan ange hello poster med hjälp av hello följande Azure CLI-kommando:

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* Du kan ange hello poster med hjälp av PowerShell-cmdlet för hello `Get-AzureRmDnsRecordSet`.
* Du kan använda `nslookup` tooverify namnmatchning för hello poster. Eftersom hello zonen inte delegerad ännu, måste toospecify hello rätt Azure DNS-namnservrar explicit. hello visar följande exempel hur tooretrieve hello servernamn som tilldelats toohello zon. IT visar även hur tooquery hello ”www” registrera med `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up hello DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Steg 3. Uppdatera DNS-delegering

När du har kontrollerat att hello zonen har importerats korrekt behöver tooupdate hello DNS-delegering toopoint toohello namnservrar Azure DNS. Mer information finns i artikeln hello [uppdatera hello DNS-delegering](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportera en DNS-zonfilen från Azure DNS

hello Azure CLI kommandot tooimport en DNS-zon hello format är:

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

Värden:

* `<resource group>`är hello namn hello resursgruppen för hello zonen i Azure DNS.
* `<zone name>`är hello namnet på hello zonen.
* `<zone file name>`är hello/sökvägsnamn hello zon filen toobe exporteras.

Som med hello zonen import, behöver du först toosign i, väljer din prenumeration och konfigurerar hello Azure CLI toouse Resource Manager-läget.

### <a name="tooexport-a-zone-file"></a>tooexport en zonfil

1. Logga in tooyour Azure-prenumeration med hjälp av hello Azure CLI.

    ```azurecli
    azure login
    ```

2. Välj hello prenumeration där du vill att toocreate DNS-zon.

    ```azurecli
    azure account set <subscription name>
    ```

3. Azure DNS är en Azure Resource Manager-only-tjänst. hello Azure CLI måste vara avstängda tooResource Manager-läget.

    ```azurecli
    azure config mode arm
    ```

4. tooexport hello Azure DNS-zon **contoso.com** i resursgruppen **myresourcegroup** toohello filen **contoso.com.txt** (kör helloaktuellamapp)`azure network dns zone export`. Det här kommandot anrop hello Azure DNS-tjänsten tooenumerate postuppsättningar i zonen hello och exportera zonfilen för hello resultat tooa BIND-kompatibel.

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
