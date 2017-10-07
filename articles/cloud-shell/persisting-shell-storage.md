---
title: "aaaPersist filer i Azure Cloud Shell (förhandsversion) | Microsoft Docs"
description: "Genomgång av hur Azure Cloud Shell kvarstår filer."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: b230453d5551c545553d63559741950206e4f1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a>Spara filer i Azure Cloud Shell
Molnet Shell utnyttjar Azure File storage toopersist filer mellan sessioner.

## <a name="set-up-a-clouddrive-file-share"></a>Konfigurera en `clouddrive` filresurs
På första start efterfrågar molnet Shell tooassociate en ny eller befintlig filresursen toopersist filer mellan sessioner.

### <a name="create-new-storage"></a>Skapa nya lagringsenheter

När du använder grundläggande inställningar och välj endast en prenumeration skapar molnet Shell tre resurser å dina vägnar i hello stöds region som är närmast tooyou:
* Resursgrupp:`cloud-shell-storage-<region>`
* Storage-konto:`cs<uniqueGuid>`
* Filresurs:`cs-<user>-<domain>-com-<uniqueGuid>`

![hello prenumeration inställningen](media/basic-storage.png)

hello filresurs monterar som `clouddrive` i din `$Home` directory. hello filresursen är också använda toostore en 5 GB-avbildning som skapas automatiskt för dig och som uppdateras och kvarstår din `$Home` directory. Detta är en engångsåtgärd och hello filresursen automatiskt monterar i efterföljande sessioner.

### <a name="use-existing-resources"></a>Använda befintliga resurser

Du kan associera befintliga resurser med hjälp av hello Avancerat alternativ. När hello lagring installationsprogrammet uppmanas att välja **visa avancerade inställningar** tooview ytterligare alternativ. Befintliga filresurser får en användare med 5 GB avbildningen toopersist din `$Home` directory. hello nedrullningsbara menyerna filtreras för molntjänster Shell-region och lokala redundant & geo-redundant storage-konton.

![hello resurs grupp-inställning](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Begränsa att skapa en resurs med en princip för Azure-resurs
Storage-konton som du skapar i molnet Shell märks med `ms-resource-usage:azure-cloud-shell`. Om du vill toodisallow användare från att skapa storage-konton i molnet Shell, skapa en [Azure resursprincip för taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) som utlöses av den här specifika taggen.

## <a name="how-cloud-shell-works"></a>Så här fungerar Cloud Shell
Molnet Shell kvarstår filer via båda hello följande metoder:
* Skapa en avbildning av din `$Home` directory toopersist alla innehåll i hello katalog. hello diskavbildning sparas i angivna filresursen som `acc_<User>.img` på `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, och den automatiskt synkroniserar ändringar.

* Montera filresursen angivna som `clouddrive` i din `$Home` katalogen för direkta filinteraktion för resursen. `/Home/<User>/clouddrive`har mappats för`fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Alla filer i din `$Home` katalog, t.ex SSH-nycklar finns kvar i din användare diskavbildning som lagras i monterade filresursen. Tillämpa metodtips när du bevara informationen i dina `$Home` directory och monterade filresurs.

## <a name="use-hello-clouddrive-command"></a>Använd hello `clouddrive` kommando
Moln-gränssnittet kan du köra ett kommando som kallas `clouddrive`, vilket aktiverar du toomanually uppdatering hello-filresurs som monterade tooCloud Shell.
![Köra hello ”clouddrive” kommando](media/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Montera en ny`clouddrive`

### <a name="prerequisites-for-manual-mounting"></a>Krav för manuell montering
Du kan uppdatera hello-filresurs som är kopplad till molnet Shell med hjälp av hello `clouddrive mount` kommando.

Om du monterar en befintlig resurs måste hello storage-konton vara:
* Lokalt redundant lagring eller geo-redundant lagring toosupport filresurser.
* Finns i din tilldelade region. När du är onboarding hello region tilldelas du toois som anges i hello resursgruppens namn `cloud-shell-storage-<region>`.

### <a name="supported-storage-regions"></a>Regioner som stöds
hello Azure filer måste finnas i hello samma region som hello molnet Shell-dator som du montera dem till. Molnet Shell kluster finns för närvarande i hello följande områden:
|Område|Region|
|---|---|
|Nord- och Sydamerika|Östra USA, södra centrala USA, västra USA|
|Europa|Norra Europa, västra Europa|
|Asien och stillahavsområdet|Indien Central, sydöstra Asien|

### <a name="hello-clouddrive-mount-command"></a>Hej `clouddrive mount` kommando

> [!NOTE]
> Om du montera en ny filresurs, skapas en ny användaravbildning för din `$Home` katalogen, eftersom den föregående `$Home` avbildningen sparas i hello tidigare filresurs.

Kör hello `clouddrive mount` med hello följande parametrar:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

tooview mer information, kör `clouddrive mount -h`, som visas här:

![Körs hello ' clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a>Demontera`clouddrive`
Du kan montera en filresurs som monterade tooCloud Shell när som helst. När filresursen har demonterats, kommer du att ange toomount en ny filresursen tidigare tooyour nästa session.

tooremove en fil dela från molnet Shell:
1. Kör `clouddrive unmount`.
2. Bekräfta och bekräfta hello anvisningarna.

Filresursen fortsätter tooexist om du tar bort den manuellt. Molnet Shell efter inte längre filresursen på efterföljande sessioner.

tooview mer information, kör `clouddrive unmount -h`, som visas här:

![Körs hello ' clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> Kör detta kommando raderar inte några resurser. Manuellt tar bort en resursgrupp, storage-konto eller filresurs som är mappad tooCloud Shell tar permanent bort din `$Home` directory avbildningen och andra filer i filresursen. Det går inte att ångra åtgärden.

## <a name="list-clouddrive-file-shares"></a>Lista `clouddrive` filresurser
toodiscover vilka filresursen är monterad som `clouddrive`, kör följande hello `df` kommando. 

hello filen sökvägen tooclouddrive visar dina lagringskontonamnet och filresursen i hello-URL. Till exempel, `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                               1K-blocks     Used Available Use% Mounted on
overlay                                                   30428648 15585636  14826628  52% /
tmpfs                                                       986704        0    986704   0% /dev
tmpfs                                                       986704        0    986704   0% /sys/fs/cgroup
/dev/sda1                                                 30428648 15585636  14826628  52% /etc/hosts
shm                                                          65536        0     65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName        6291456  5242944   1048512  84% /usr/justin/clouddrive
/dev/loop0                                                 5160576   601652   4296780  13% /home/justin
```

## <a name="transfer-local-files-toocloud-shell"></a>Överför lokala filer tooCloud Shell
Hej `clouddrive` directory synkroniseras med hello Azure portal lagring-bladet. Använd det här bladet tootransfer lokala filer tooor från filresursen. Uppdatera filer i molnet Shell avspeglas i hello fillagring GUI när du uppdaterar hello-bladet.

### <a name="download-files"></a>Hämta filer

![Lista över lokala filer](media/download.png)
1. Gå toohello monterade filresurs i hello Azure-portalen.
2. Välj hello målfilen.
3. Välj hello **hämta** knappen.

### <a name="upload-files"></a>Överföra filer

![Toobe för lokala filer har överförts](media/upload.png)
1. Gå tooyour monterade filresurs.
2. Välj hello **överför** knappen.
3. Välj hello eller filer som du vill tooupload.
4. Bekräfta hello överföringen.

Du bör nu se hello-filer som är tillgängliga i din `clouddrive` katalog i molnet Shell.

## <a name="next-steps"></a>Nästa steg
[Molnet Shell Snabbstart](quickstart.md) <br>
[Lär dig mer om Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Lär dig mer om lagring taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
