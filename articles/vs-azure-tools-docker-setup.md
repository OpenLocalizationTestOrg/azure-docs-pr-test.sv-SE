---
title: "aaaConfigure en Docker-värd med VirtualBox | Microsoft Docs"
description: Stegvisa instruktioner tooconfigure standard Docker-instans med Docker-datorn och VirtualBox
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 1df2da4482444a803d05e413e019edcc57269062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Konfigurera en Docker-värd med VirtualBox
## <a name="overview"></a>Översikt
Den här artikeln hjälper dig att konfigurera en standardinstans Docker med Docker-datorn och VirtualBox. Om du använder hello [Docker för Windows beta](http://beta.docker.com/), behövs inte den här konfigurationen.

## <a name="prerequisites"></a>Krav
hello måste följande verktyg toobe installerad.

* [Docker verktygslådan](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-hello-docker-client-with-windows-powershell"></a>Konfigurera hello Docker-klienten med Windows PowerShell
tooconfigure en Docker-klient helt enkelt öppna Windows PowerShell och utför hello följande steg:

1. Skapa en standardinstans docker värden.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Kontrollera hello standardinstansen är konfigurerad och körs. (Du bör se en instans med namnet 'default' körs.
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker-datorn ls-utdata][0]
3. Ange standard som hello aktuella värden och konfigurera gränssnittet.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. Visa hello active Docker-behållare. hello-listan ska vara tom.
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps-utdata][1]

> [!NOTE]
> Varje gång du startar om utvecklingsdatorn måste toorestart lokala docker-värden.
> toodo detta, problemet hello följande kommando i Kommandotolken: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
