---
title: Installera Elasticsearch på en virtuell utvecklingsdator i Azure
description: Självstudier – Installera Elastic Stack på en virtuell utvecklingsdator med Linux i Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rloutlaw
manager: justhe
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 10/11/2017
ms.author: routlaw
ms.openlocfilehash: 5b0b51504478cc0d501a89760ccd60808a69ccbd
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2018
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a><span data-ttu-id="b5e45-103">Installera Elastic Stack på en virtuell dator i Azure VM</span><span class="sxs-lookup"><span data-stu-id="b5e45-103">Install the Elastic Stack on an Azure VM</span></span>

<span data-ttu-id="b5e45-104">Den här artikeln beskriver hur du installerar [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash) och [Kibana](https://www.elastic.co/products/kibana) på en virtuell dator med Ubuntu i Azure.</span><span class="sxs-lookup"><span data-stu-id="b5e45-104">This article walks you through how to deploy [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), and [Kibana](https://www.elastic.co/products/kibana), on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="b5e45-105">Om du vill se hur Elastic Stack fungerar i praktiken kan du ansluta till Kibana och arbeta med en del exempeldata.</span><span class="sxs-lookup"><span data-stu-id="b5e45-105">To see the Elastic Stack in action, you can optionally connect to Kibana  and work with some sample logging data.</span></span> 

<span data-ttu-id="b5e45-106">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="b5e45-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5e45-107">Skapa en virtuell dator med Ubuntu i en Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b5e45-107">Create an Ubuntu VM in an Azure resource group</span></span>
> * <span data-ttu-id="b5e45-108">Installera Elasticsearch, Logstash och Kibana på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5e45-108">Install Elasticsearch, Logstash, and Kibana on the VM</span></span>
> * <span data-ttu-id="b5e45-109">Skicka exempeldata till Elasticsearch med Logstash</span><span class="sxs-lookup"><span data-stu-id="b5e45-109">Send sample data to Elasticsearch with Logstash</span></span> 
> * <span data-ttu-id="b5e45-110">Öppna portar och arbeta med data i Kibana-konsolen</span><span class="sxs-lookup"><span data-stu-id="b5e45-110">Open ports and work with data in the Kibana console</span></span>


 <span data-ttu-id="b5e45-111">Denna installation är lämplig för grundläggande utveckling med Elastic Stack.</span><span class="sxs-lookup"><span data-stu-id="b5e45-111">This deployment is suitable for basic development with the Elastic Stack.</span></span> <span data-ttu-id="b5e45-112">Mer information om Elastic Stack samt rekommendationer för en produktionsmiljö finns i [Elastic-dokumentationen](https://www.elastic.co/guide/index.html) och [Azure Architecture Center](/azure/architecture/elasticsearch/).</span><span class="sxs-lookup"><span data-stu-id="b5e45-112">For more on the Elastic Stack, including recommendations for a production environment, see the [Elastic documentation](https://www.elastic.co/guide/index.html) and the [Azure Architecture Center](/azure/architecture/elasticsearch/).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b5e45-113">Om du väljer att installera och använda CLI lokalt kräver de här självstudierna att du kör Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b5e45-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b5e45-114">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="b5e45-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="b5e45-115">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b5e45-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="b5e45-116">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b5e45-116">Create a resource group</span></span>

<span data-ttu-id="b5e45-117">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b5e45-117">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b5e45-118">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="b5e45-118">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="b5e45-119">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *eastus*.</span><span class="sxs-lookup"><span data-stu-id="b5e45-119">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="b5e45-120">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b5e45-120">Create a virtual machine</span></span>

<span data-ttu-id="b5e45-121">Skapa en virtuell dator med kommandot [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="b5e45-121">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="b5e45-122">Följande exempel skapar en virtuell dator som heter *myVM*, och SSH-nycklar skapas om de inte redan finns på en standardnyckelplats.</span><span class="sxs-lookup"><span data-stu-id="b5e45-122">The following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="b5e45-123">Om du vill använda en specifik uppsättning nycklar använder du alternativet `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="b5e45-123">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="b5e45-124">När den virtuella datorn har skapats visar Azure CLI information som ser ut ungefär som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="b5e45-124">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="b5e45-125">Anteckna `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="b5e45-125">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="b5e45-126">Den här adressen används för att få åtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b5e45-126">This address is used to access the VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="b5e45-127">SSH till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5e45-127">SSH into your VM</span></span>

<span data-ttu-id="b5e45-128">Om du inte vet den offentliga IP-adressen för den virtuella datorn kör du kommandot [az network public-ip list](/cli/azure/network/public-ip#list):</span><span class="sxs-lookup"><span data-stu-id="b5e45-128">If you don't already know the public IP address of your VM, run the [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="b5e45-129">Använd följande kommando för att skapa en SSH-session med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b5e45-129">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="b5e45-130">Ersätt med den korrekta offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b5e45-130">Substitute the correct public IP address of your virtual machine.</span></span> <span data-ttu-id="b5e45-131">I det här exemplet är IP-adressen *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="b5e45-131">In this example, the IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a><span data-ttu-id="b5e45-132">Installera Elastic Stack</span><span class="sxs-lookup"><span data-stu-id="b5e45-132">Install the Elastic Stack</span></span>

<span data-ttu-id="b5e45-133">Importera signeringsnyckeln för Elasticsearch och uppdatera APT-källistan så den innehåller paketdatabasen för Elastic:</span><span class="sxs-lookup"><span data-stu-id="b5e45-133">Import the Elasticsearch signing key and update your APT sources list to include the Elastic package repository:</span></span>

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

<span data-ttu-id="b5e45-134">Installera Java Virtual Machine på den virtuella datorn och konfigurera variabeln JAVA_HOME – Elastic Stack-komponenterna behöver detta för att kunna köras.</span><span class="sxs-lookup"><span data-stu-id="b5e45-134">Install the Java Virtual on the VM and configure the JAVA_HOME variable-this is necessary for the Elastic Stack components to run.</span></span>

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

<span data-ttu-id="b5e45-135">Kör följande kommandon för att uppdatera Ubuntu-paketkällorna och installera Elasticsearch, Kibana och Logstash.</span><span class="sxs-lookup"><span data-stu-id="b5e45-135">Run the following commands to update Ubuntu package sources and install Elasticsearch, Kibana, and Logstash.</span></span>

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> <span data-ttu-id="b5e45-136">Detaljerade installationsanvisningar, inklusive katalogstrukturer och inledande konfiguration finns i [Elastics dokumentation](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)</span><span class="sxs-lookup"><span data-stu-id="b5e45-136">Detailed installation instructions, including directory layouts and initial configuration, are maintained in [Elastic's documentation](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)</span></span>

## <a name="start-elasticsearch"></a><span data-ttu-id="b5e45-137">Starta Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="b5e45-137">Start Elasticsearch</span></span> 

<span data-ttu-id="b5e45-138">Starta Elasticsearch på den virtuella datorn med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b5e45-138">Start Elasticsearch on your VM with the following command:</span></span>

```bash
sudo systemctl start elasticsearch.service
```

<span data-ttu-id="b5e45-139">Detta kommando genererar inga utdata. Du kontrollerar därför att Elasticsearch körs på den virtuella datorn med detta `curl`-kommando:</span><span class="sxs-lookup"><span data-stu-id="b5e45-139">This command produces no output, so verify that Elasticsearch is running on the VM with this `curl` command:</span></span>

```bash
curl -XGET 'localhost:9200/'
```

<span data-ttu-id="b5e45-140">Om Elasticsearch körs ser du utdata som liknar dessa:</span><span class="sxs-lookup"><span data-stu-id="b5e45-140">If Elasticsearch is running, you see output like the following:</span></span>

```json
{
  "name" : "w6Z4NwR",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "SDzCajBoSK2EkXmHvJVaDQ",
  "version" : {
    "number" : "5.6.3",
    "build_hash" : "1a2f265",
    "build_date" : "2017-10-06T20:33:39.012Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```

## <a name="start-logstash-and-add-data-to-elasticsearch"></a><span data-ttu-id="b5e45-141">Starta Logstash och lägga till data i Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="b5e45-141">Start Logstash and add data to Elasticsearch</span></span>

<span data-ttu-id="b5e45-142">Starta Logstash med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b5e45-142">Start Logstash with the following command:</span></span>

```bash 
sudo systemctl start logstash.service
```

<span data-ttu-id="b5e45-143">Testa Logstash i interaktivt läge och kontrollera att det fungerar korrekt:</span><span class="sxs-lookup"><span data-stu-id="b5e45-143">Test Logstash in interactive mode to make sure it's working correctly:</span></span>

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

<span data-ttu-id="b5e45-144">Det här är en grundläggande logstash-[pipeline](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) som skickar standardindata till standardutdata.</span><span class="sxs-lookup"><span data-stu-id="b5e45-144">This is a basic logstash [pipeline](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) that echoes standard input to standard output.</span></span> 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

<span data-ttu-id="b5e45-145">Konfigurera Logstash så att kernel-meddelanden från den här virtuella dator skickas till Elasticsearch.</span><span class="sxs-lookup"><span data-stu-id="b5e45-145">Set up Logstash to forward the kernel messages from this VM to Elasticsearch.</span></span> <span data-ttu-id="b5e45-146">Skapa en ny fil i en tom katalog med namnet `vm-syslog-logstash.conf` och klistra in följande Logstash-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="b5e45-146">Create a new file in an empty directory called `vm-syslog-logstash.conf` and paste in the following Logstash configuration:</span></span>

```Logstash
input {
    stdin {
        type => "stdin-type"
    }

    file {
        type => "syslog"
        path => [ "/var/log/*.log", "/var/log/*/*.log", "/var/log/messages", "/var/log/syslog" ]
        start_position => "beginning"
    }
}

output {

    stdout {
        codec => rubydebug
    }
    elasticsearch {
        hosts  => "localhost:9200"
    }
}
```

<span data-ttu-id="b5e45-147">Testa denna konfiguration och skicka syslog-data till Elasticsearch:</span><span class="sxs-lookup"><span data-stu-id="b5e45-147">Test this configuration and send the syslog data to Elasticsearch:</span></span>

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

<span data-ttu-id="b5e45-148">Syslog-posterna visas i terminalen allteftersom de skickas till Elasticsearch.</span><span class="sxs-lookup"><span data-stu-id="b5e45-148">You see the syslog entries in your terminal echoed as they are sent to Elasticsearch.</span></span> <span data-ttu-id="b5e45-149">Använd `CTRL+C` och lämna Logstash när du skickat lite data.</span><span class="sxs-lookup"><span data-stu-id="b5e45-149">Use `CTRL+C` to exit out of Logstash once you've sent some data.</span></span>

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a><span data-ttu-id="b5e45-150">Starta Kibana och visualisera data i Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="b5e45-150">Start Kibana and visualize the data in Elasticsearch</span></span>

<span data-ttu-id="b5e45-151">Redigera `/etc/kibana/kibana.yml` och ändra IP-adressen som Kibana lyssnar på så att du kommer åt den från webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="b5e45-151">Edit `/etc/kibana/kibana.yml` and change the IP address Kibana listens on so you can access it from your web browser.</span></span>

```bash
server.host:"0.0.0.0"
```

<span data-ttu-id="b5e45-152">Starta Kibana med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b5e45-152">Start Kibana with the following command:</span></span>

```bash
sudo systemctl start kibana.service
```

<span data-ttu-id="b5e45-153">Öppna port 5601 från Azure CLI och tillåt fjärråtkomst till Kibana-konsolen:</span><span class="sxs-lookup"><span data-stu-id="b5e45-153">Open port 5601 from the Azure CLI to allow remote access to the Kibana console:</span></span>

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b5e45-154">Öppna Kibana-konsolen och välj **Skapa** för att generera ett standardindex baserat på de syslog-data som skickades till Elasticsearch tidigare.</span><span class="sxs-lookup"><span data-stu-id="b5e45-154">Open up the Kibana console and select **Create** to generate a default index based on the syslog data you sent to Elasticsearch earlier.</span></span> 

![Bläddra i Syslog-händelser i Kibana](media/elasticsearch-install/kibana-index.png)

<span data-ttu-id="b5e45-156">Välj **Utforska** på Kibana-konsolen och sök, bläddra och filtrera syslog-händelserna.</span><span class="sxs-lookup"><span data-stu-id="b5e45-156">Select **Discover** on the Kibana console to search, browse, and filter through the syslog events.</span></span>

![Bläddra i Syslog-händelser i Kibana](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a><span data-ttu-id="b5e45-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5e45-158">Next steps</span></span>

<span data-ttu-id="b5e45-159">I den här självstudien installerade du Elastic Stack på en virtuell utvecklingsdator i Azure.</span><span class="sxs-lookup"><span data-stu-id="b5e45-159">In this tutorial, you deployed the Elastic Stack into a development VM in Azure.</span></span> <span data-ttu-id="b5e45-160">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="b5e45-160">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5e45-161">Skapa en virtuell dator med Ubuntu i en Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b5e45-161">Create an Ubuntu VM in an Azure resource group</span></span>
> * <span data-ttu-id="b5e45-162">Installera Elasticsearch, Logstash och Kibana på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5e45-162">Install Elasticsearch, Logstash, and Kibana on the VM</span></span>
> * <span data-ttu-id="b5e45-163">Skicka exempeldata till Elasticsearch från Logstash</span><span class="sxs-lookup"><span data-stu-id="b5e45-163">Send sample data to Elasticsearch from Logstash</span></span> 
> * <span data-ttu-id="b5e45-164">Öppna portar och arbeta med data i Kibana-konsolen</span><span class="sxs-lookup"><span data-stu-id="b5e45-164">Open ports and work with data in the Kibana console</span></span>