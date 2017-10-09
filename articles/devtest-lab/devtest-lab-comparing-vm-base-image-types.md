---
title: aaaComparing anpassade avbildningar och formler i DevTest Labs | Microsoft Docs
description: "Läs mer om hello skillnaderna mellan anpassade avbildningar och formler som VM baserar så att du kan bestämma vilket som passar din miljö."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tarcher
ms.openlocfilehash: 3c1d88dfe0ff94b8e825bb7a0b4aca3341c9330d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Jämföra anpassade avbildningar och formler i DevTest Labs
Båda [anpassade avbildningar](devtest-lab-create-template.md) och [formler](devtest-lab-manage-formulas.md) kan användas som grund för [skapa nya virtuella datorer](devtest-lab-add-vm-with-artifacts.md). Hello viktiga åtskillnad mellan anpassade avbildningar och formler är dock att en anpassad avbildning är helt enkelt en avbildning baserat på en virtuell Hårddisk, medan en formel är en avbildning baserat på en virtuell Hårddisk *förutom* förkonfigurerade inställningar – till exempel VM-storlek, virtuella nätverk undernätet och artefakter. Inställningarna förinställda ställs in med standardvärden som kan åsidosättas när hello dags att skapa en virtuell dator. Den här artikeln förklarar några av fördelarna hello (tekniker) och nackdelar (nackdelar) toousing anpassade avbildningar jämfört med formler.

## <a name="custom-image-pros-and-cons"></a>Anpassad bild- och nackdelar
Anpassade avbildningar innehåller en statisk, ändras sätt toocreate virtuella datorer från en önskad miljö. 

**Tekniker**

* Etablering från en anpassad avbildning av virtuell dator är snabb eftersom inget ändras efter hello VM är de från hello avbildningen. Det finns med andra ord inga inställningar tooapply som hello anpassad avbildning är en bild utan inställningar. 
* Virtuella datorer skapas från en anpassad bild är identiska.

**Nackdelar**

* Om du behöver tooupdate någon aspekt av hello anpassad avbildning måste du återskapa hello avbildningen.  

## <a name="formula-pros-and-cons"></a>Formeln- och nackdelar
Formler innehåller ett dynamiskt sätt toocreate virtuella datorer från hello önskad konfigurationsinställningar.

**Tekniker**

* Ändringar i hello miljö kan hämtas på hello direkt via artefakter. Till exempel om du vill att en virtuell dator som installerats med hello senaste bits från din pipeline versionen eller registrera hello senaste koden från din lagringsplats du bara ange en artefakt som distribuerar hello senaste bitar eller anlitar hello senaste koden i hello formeln tillsammans med en mål basavbildning. När den här formeln används toocreate virtuella datorer är hello senaste bits/kod distribueras/registrerad toohello VM. 
* Formler kan definiera standardinställningar som anpassade avbildningar inte kan tillhandahålla - som VM-storlekar och inställningarna för virtuella nätverk. 
* hello inställningarna sparas i en formel som standardvärden, men kan ändras när hello VM skapas. 

**Nackdelar**

* Skapa en virtuell dator från en formel kan det ta längre tid än att skapa en virtuell dator från en anpassad avbildning.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Relaterade blogginlägg
* [Anpassade avbildningar eller formler?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Nästa steg
- [DevTest Labs vanliga frågor och svar](devtest-lab-faq.md)