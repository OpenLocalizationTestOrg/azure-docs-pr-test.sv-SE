---
title: aaaMove en Windows AWS VMs tooAzure | Microsoft Docs
description: "Flytta en Amazon Web Services (AWS) EC2 Windows instans tooAzure virtuella datorer med hjälp av Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: cynthn
ms.openlocfilehash: f912c28d3ffe585162c3add715a1318ac3cd4643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-tooazure-using-powershell"></a>Flytta en virtuell Windows-dator från Amazon Web Services (AWS) tooAzure med hjälp av PowerShell

Om du utvärderar virtuella datorer i Azure som värd för din arbetsbelastning, kan du exportera en befintlig Amazon Web Services (AWS) EC2 Windows VM-instans sedan överför hello virtuell hårddisk (VHD) tooAzure. En gång hello VHD överförs, du kan skapa en ny virtuell dator i Azure från hello VHD. 

Det här avsnittet beskriver flyttar en enda virtuell dator från AWS tooAzure. Om du vill toomove virtuella datorer från AWS tooAzure i skala, se [migrera virtuella datorer i Amazon Web Services (AWS) tooAzure med Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-hello-vm"></a>Förbereda hello VM 
 
Du kan ladda upp tooAzure både generaliserad och specialiserade virtuella hårddiskar. Varje typ måste du förbereda hello VM innan du exporterar från AWS. 

- **Generaliserad virtuell Hårddisk** -en generaliserad virtuell Hårddisk har haft all personlig information bort med hjälp av Sysprep. Om du avser toouse hello VHD som en bild toocreate nya virtuella datorer från bör du: 
 
    * [Förbereda Windows VM](prepare-for-upload-vhd-image.md).  
    * Generalisera hello virtuell dator med hjälp av Sysprep.  

 
- **Specialiserad VHD** – en särskild virtuell Hårddisk underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn. Om du avser toouse hello VHD som-är toocreate en ny virtuell dator, kontrollera hello följande steg har slutförts.  
    * [Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md). **Inte** generalisera hello VM med hjälp av Sysprep. 
    * Ta bort gäst virtualiseringsverktyg och agenter som installerats på hello VM (d.v.s. VMware-verktyg). 
    * Se till att hello VM är konfigurerade toopull dess IP-adress och DNS-inställningarna via DHCP. Detta säkerställer att hello-servern hämtar en IP-adress inom hello VNet när den startas.  


## <a name="export-and-download-hello-vhd"></a>Exportera och hämta hello VHD 

Exportera hello EC2 instans tooa VHD i en Amazon S3-bucket. Följ hello stegen som beskrivs i avsnittet för hello Amazon dokumentationen [och exportera en instans som en virtuell dator med hjälp av VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) och kör hello [skapa-instans-export-aktivitet](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) kommandot tooexport hello EC2 instansen tooa VHD-filen. 

hello sparas exporterade VHD-filen i hello Amazon S3-bucket som du anger. hello grundläggande syntaxen för export av hello VHD visas nedan, bara ersätta hello platshållartexten i <brackets> med din information.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

När hello VHD har exporterats, följer du anvisningarna för hello i [hur hämtar jag ett objekt från ett S3-Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) toodownload hello VHD-filen från hello S3-bucket. 

> [!IMPORTANT]
> AWS debiterar data transfer avgifter för att ladda ned hello VHD. Se [Amazon S3 priser](https://aws.amazon.com/s3/pricing/) för mer information.


## <a name="next-steps"></a>Nästa steg

Nu kan du överföra hello VHD tooAzure och skapa en ny virtuell dator. 

- Om du har kört Sysprep på källfilerna för**generalisera** den innan du exporterar, se [överför en generaliserad virtuell Hårddisk och använda toocreate en nya virtuella datorer i Azure](upload-generalized-managed.md)
- Om du inte har kört Sysprep innan du exporterar hello VHD anses **specialiserade**, se [överför en särskild VHD-tooAzure och skapa en ny virtuell dator](create-vm-specialized.md)

 
