---
title: aaaMove App webbresurser tooanother resursgruppen.
description: "Beskriver hello scenarier där du kan flytta webbprogram och Apptjänster från en resursgrupp tooanother."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 931012fee656b7f8a4b2c225c38739a9171d3b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="supported-move-configurations"></a>Konfigurationer som stöds flytta
Du kan flytta Azure Web App resurser med hjälp av hello [Resource Manager flytta resurser API](../azure-resource-manager/resource-group-move-resources.md).

Azure Web Apps stöder för närvarande hello efter flytten scenarier:

* Flytta hello hela innehållet i en resursgrupp (webbappar, apptjänstplaner och certifikat) tooanother resursgruppen. 
   > [!Note]
   > hello mål resursgruppens namn får inte innehålla några Microsoft.Web resurser i det här scenariot.

* Flytta enskilda web apps tooa annan resursgrupp, medan du fortfarande värd dem i deras aktuella apptjänstplan (hello app service-plan förblir i hello gamla resursgrupp).


