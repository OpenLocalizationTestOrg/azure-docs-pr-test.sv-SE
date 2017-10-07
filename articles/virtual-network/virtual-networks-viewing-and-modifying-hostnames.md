---
title: "aaaViewing och ändra värdnamn | Microsoft Docs"
description: "Hur tooview och ändra värdnamn för Azure-datorer, webb- och arbetsroller för namnmatchning"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 17d0dd7911754a94db3f37b924b4687da1c70aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="viewing-and-modifying-hostnames"></a>Visa och ändra värdnamn
tooallow din roll instanser toobe refereras av värdnamn, måste du ange hello värde för hello värdnamn i hello tjänstkonfigurationsfilen för varje roll. Det gör du genom att lägga till hello önskade värden namnet toohello **vmName** attribut för hello **rollen** element. Hej värdet för hello **vmName** attributet används som bas för hello värdnamnet för varje rollinstans. Till exempel om **vmName** är *webrole* och det finns tre instanser av rollen, hello värdnamn hello instanser *webrole0*, *webrole1 *, och *webrole2*. Du behöver inte toospecify ett värdnamn för virtuella datorer i hello konfigurationsfilen eftersom hello värdnamnet för en virtuell dator har fyllts i baserat på hello namn på virtuell dator. Mer information om hur du konfigurerar en Microsoft Azure-tjänst finns [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Visa värdnamn
Du kan visa hello värdnamn för virtuella datorer och rollinstanser i en molntjänst med hjälp av hello verktygen nedan.

### <a name="azure-portal"></a>Azure Portal
Du kan använda hello [Azure-portalen](http://portal.azure.com) tooview hello värdnamn för virtuella datorer på hello översikt bladet för en virtuell dator. Tänk på att hello bladet visar ett värde för **namn** och **värdnamn**. Trots att de ursprungligen hello samma ändra hello värdnamnet inte att ändra hello namnet på hello virtuell dator eller rollinstans.

Rollinstanser kan även visas i hello Azure-portalen, men när du listar hello-instanser i en molnbaserad tjänst hello värdnamn visas inte. Du ser ett namn för varje instans, men det här namnet representerar inte hello värdnamn.

### <a name="service-configuration-file"></a>Tjänstkonfigurationsfil
Du kan hämta hello tjänstkonfigurationsfilen för en distribuerad tjänst från hello **konfigurera** bladet hello tjänsterna i hello Azure-portalen. Du kan sedan söka efter hello **vmName** attribut för hello **rollnamn** elementet toosee hello värdnamn. Tänk på att det här värdnamnet används som bas för hello värdnamnet för varje rollinstans. Till exempel om **vmName** är *webrole* och det finns tre instanser av rollen, hello värdnamn hello instanser *webrole0*, *webrole1 *, och *webrole2*.

### <a name="remote-desktop"></a>Fjärrskrivbord
När du aktiverar fjärrskrivbord (Windows), Windows PowerShell-fjärrkommunikation (Windows) eller SSH (Linux och Windows) anslutningar tooyour virtuella datorer eller rollinstanser, kan du visa hello värdnamn från en aktiv anslutning till fjärrskrivbord på olika sätt:

* Skriv värdnamn vid hello kommandotolk eller SSH-terminal.
* Skriv ipconfig/alla i hello kommandot Kommandotolken (endast Windows).
* Visa hello datornamn i hello system inställningar (Windows).

### <a name="azure-service-management-rest-api"></a>Azure Service Management REST API
Följ dessa instruktioner från en REST-klient:

1. Se till att du har en klient certifikat tooconnect toohello Azure-portalen. tooobtain ett klientcertifikat gör hello presenteras i [så här: hämta och importera Publiceringsinställningar och prenumerationsinformation](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Ange en rubrikpost med namnet x-ms-version med värdet 2013-11-01.
3. Skicka en begäran i hello följande format: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<Tjänstenamn\>? bädda in detail = true
4. Leta efter hello **värdnamn** element för varje **RoleInstance** element.

> [!WARNING]
> Du kan också visa hello interna domänsuffixet för din molntjänst från hello REST-anrop svar genom att kontrollera hello **InternalDnsSuffix** elementet eller genom att köra ipconfig/allt från en kommandotolk i en fjärrskrivbordssession (Windows) eller genom att köra cat /etc/resolv.conf från en SSH-terminal (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Ändra ett värdnamn
Du kan ändra hello värdnamn för virtuell dator eller rollinstans genom att ladda upp en ändrade tjänstkonfigurationsfilen eller genom att byta namn på hello dator från en fjärrskrivbordssession.

## <a name="next-steps"></a>Nästa steg
[Namnmatchning (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Konfigurationsschemat för Azure-tjänsten (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Virtuella Azure-nätverket Konfigurationsschemat](http://go.microsoft.com/fwlink/?LinkId=248093)

[Ange DNS-inställningar med hjälp av network configuration-filer](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

