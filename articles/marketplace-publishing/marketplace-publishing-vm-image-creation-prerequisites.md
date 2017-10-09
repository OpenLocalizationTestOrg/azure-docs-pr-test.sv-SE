---
title: "aaaTechnical förutsättningar för att skapa en avbildning av virtuell dator för hello Azure Marketplace | Microsoft Docs"
description: "Förstå hello kraven för att skapa och distribuera en virtuell dator på en bild-toohello Azure Marketplace för andra toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: fcae4d9e052581e3c1dfe7962e6d50ec31040419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-hello-azure-marketplace"></a>Tekniska krav för att skapa en avbildning av virtuell dator för hello Azure Marketplace
Läs hello processen noggrant innan du börjar och förstå varför och där varje steg utförs. Så mycket som möjligt du ska förbereda företagets information och andra data, hämta nödvändiga verktyg och skapa tekniska komponenter innan du börjar hello erbjudande skapas. Dessa objekt bör vara klart från granska den här artikeln.  

## <a name="download-needed-tools--applications"></a>Hämta nödvändiga verktyg och program
Du bör ha följande till hands innan du börjar hello hello:

* Beroende på vilket operativsystem du inriktar dig, installera hello [Azure PowerShell-cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) eller [Linux kommandoradsgränssnittet verktyget](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) från hello [Azure hämtar](https://azure.microsoft.com/downloads/) sidan.
* Installera Azure Lagringsutforskaren från CodePlex.
* Hämta och installera hello certifikatutfärdare-verktyget för Azure certifierade:
  * [http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Du behöver ett Windows-baserad dator toorun hello certifikatutfärdare verktyg. Om du inte har en Windows-baserad dator som är tillgängliga kan du köra hello verktyget med en Windows-baserad virtuell dator i Azure.

## <a name="platforms-supported"></a>Plattformar som stöds
Du kan utveckla Azure-baserade virtuella datorer i Windows eller Linux. Vissa delar av hello publiceringsprocessen – till exempel skapa en Azure-kompatibel virtuell hårddisk (VHD)--Använd olika verktyg och steg beroende på vilket operativsystem du använder:  

* Om du använder Linux finns toohello ”skapa en Azure-kompatibel virtuell Hårddisk (Linux-baserade)” avsnittet av hello [virtuella avbildningen publiceringsguide](marketplace-publishing-vm-image-creation.md).
* Om du använder Windows finns toohello ”skapa en Azure-kompatibel virtuell Hårddisk (Windows-baserade)” avsnittet av hello [virtuella avbildningen publiceringsguide](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> Du behöver komma åt tooa Windows-baserade datorn till:
> 
> * Kör verktyget för validering av hello certifikatutfärdare.
> * Skapa hello VHD delad åtkomst signatur URL för hello VHD certifikatutfärdare ansökan.
> 
> 

## <a name="develop-your-vhd"></a>Utveckla den virtuella Hårddisken
Du kan utveckla Azure virtuella hårddiskar i hello molnet eller lokalt:

* Molnbaserad utveckling innebär alla development steg utförs via fjärranslutning på en virtuell Hårddisk på Azure.
* Lokal utveckling kräver hämtning av en virtuell Hårddisk och utveckla den med hjälp av lokal infrastruktur. Även om det är möjligt, rekommenderas inte den. Observera att utveckla för Windows eller SQL lokalt kräver du toohave hello relevanta lokalt licensnycklar. Du kan inte innehålla eller installera SQL Server när du har skapat en virtuell dator. Erbjudandet måste också baseras på en godkända SQL-avbildning från hello Azure-portalen. Om du väljer toodevelop på lokalt, måste du utföra några steg annorlunda än om du skapar i hello molnet. Du kan hitta relevant information i [skapa en lokal VM-avbildning](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
