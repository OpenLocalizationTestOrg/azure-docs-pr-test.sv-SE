---
title: "Lägga till en användare till en Linux-VM i Azure | Microsoft Docs"
description: "Lägga till en användare till en Linux-VM på Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8aa633b-8b75-45d7-b61d-11ac112cedd5
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: v-livech
ms.openlocfilehash: a95157f57c0cbd1f2a9ed68a0fe83140d7c9ec40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-user-to-an-azure-vm"></a><span data-ttu-id="46927-103">Lägg till en användare i en Azure VM</span><span class="sxs-lookup"><span data-stu-id="46927-103">Add a user to an Azure VM</span></span>
<span data-ttu-id="46927-104">En av de första aktiviteterna på alla nya Linux-VM är att skapa en ny användare.</span><span class="sxs-lookup"><span data-stu-id="46927-104">One of the first tasks on any new Linux VM is to create a new user.</span></span>  <span data-ttu-id="46927-105">Vi går igenom hur du skapar ett användarkonto med sudo, ställa in lösenordet lägger till SSH offentliga nycklar i den här artikeln och slutligen använder `visudo` att sudo utan lösenord.</span><span class="sxs-lookup"><span data-stu-id="46927-105">In this article, we walk through creating a sudo user account, setting the password, adding SSH Public Keys, and finally use `visudo` to allow sudo without a password.</span></span>

<span data-ttu-id="46927-106">Krav är: [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/), [offentliga och privata nycklar för SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), en Azure-resursgrupp och Azure CLI installerad och växla till Azure Resource Manager med hjälp av `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="46927-106">Prerequisites are: [an Azure account](https://azure.microsoft.com/pricing/free-trial/), [SSH public and private keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), an Azure resource group, and the Azure CLI installed and switched to Azure Resource Manager mode using `azure config mode arm`.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="46927-107">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="46927-107">Quick Commands</span></span>
```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="46927-108">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="46927-108">Detailed Walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="46927-109">Introduktion</span><span class="sxs-lookup"><span data-stu-id="46927-109">Introduction</span></span>
<span data-ttu-id="46927-110">En av de första och de vanligaste uppgiften med en ny server är att lägga till ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="46927-110">One of the first and most common task with a new server is to add a user account.</span></span>  <span data-ttu-id="46927-111">Rot-inloggningar ska vara inaktiverade och rotkontot själva ska inte användas med Linux-servern, endast sudo.</span><span class="sxs-lookup"><span data-stu-id="46927-111">Root logins should be disabled and the root account itself should not be used with your Linux server, only sudo.</span></span>  <span data-ttu-id="46927-112">Ge en användarrot eskalering av privilegier med sudo den på rätt sätt att administrera och använda Linux.</span><span class="sxs-lookup"><span data-stu-id="46927-112">Giving a user root escalation privileges using sudo it the proper way to administer and use Linux.</span></span>

<span data-ttu-id="46927-113">Med hjälp av kommandot `useradd` vi lägger till användarkonton till Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="46927-113">Using the command `useradd` we are adding user accounts to the Linux VM.</span></span>  <span data-ttu-id="46927-114">Kör `useradd` ändrar `/etc/passwd`, `/etc/shadow`, `/etc/group`, och `/etc/gshadow`.</span><span class="sxs-lookup"><span data-stu-id="46927-114">Running `useradd` modifies `/etc/passwd`, `/etc/shadow`, `/etc/group`, and `/etc/gshadow`.</span></span>  <span data-ttu-id="46927-115">Vi lägger till en flagga som kommandoraden för att den `useradd` kommando för att lägga till den nya användaren till rätt sudo-gruppen på Linux.</span><span class="sxs-lookup"><span data-stu-id="46927-115">We are adding a command-line flag to the `useradd` command to also add the new user to the proper sudo group on Linux.</span></span>  <span data-ttu-id="46927-116">Även du `useradd` skapas en post i `/etc/passwd` inte får det nya användarkontot ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="46927-116">Even thou `useradd` creates an entry into `/etc/passwd` it does not give the new user account a password.</span></span>  <span data-ttu-id="46927-117">Vi skapar ett initialt lösenord för den nya användaren med hjälp av enkla `passwd` kommando.</span><span class="sxs-lookup"><span data-stu-id="46927-117">We are creating an initial password for the new user using the simple `passwd` command.</span></span>  <span data-ttu-id="46927-118">Det sista steget är att ändra sudo-regler för att tillåta användaren att köra kommandon med sudo-behörighet utan att behöva ange ett lösenord för varje kommando.</span><span class="sxs-lookup"><span data-stu-id="46927-118">The last step is to modify the sudo rules to allow that user to execute commands with sudo privileges without having to enter a password for every command.</span></span>  <span data-ttu-id="46927-119">Logga in med den privata nyckeln som vi antar att användarkontot från obehöriga och ska tillåta sudo åtkomst utan lösenord.</span><span class="sxs-lookup"><span data-stu-id="46927-119">Logging in using the Private key we are assuming that user account is safe from bad actors and are going to allow sudo access without a password.</span></span>  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a><span data-ttu-id="46927-120">Lägga till en enda sudo-användare i en Azure VM</span><span class="sxs-lookup"><span data-stu-id="46927-120">Adding a single sudo user to an Azure VM</span></span>
<span data-ttu-id="46927-121">Logga in på den virtuella Azure-datorn med hjälp av SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="46927-121">Log in to the Azure VM using SSH keys.</span></span>  <span data-ttu-id="46927-122">Om du inte har installationsprogrammet SSH offentlig nyckel kan slutföra den här artikeln först [med offentlig nyckel autentisering med Azure](http://link.to/article).</span><span class="sxs-lookup"><span data-stu-id="46927-122">If you have not setup SSH public key access, complete this article first [Using Public Key Authentication with Azure](http://link.to/article).</span></span>  

<span data-ttu-id="46927-123">Den `useradd` kommando utför följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="46927-123">The `useradd` command completes the following tasks:</span></span>

* <span data-ttu-id="46927-124">Skapa ett nytt användarkonto</span><span class="sxs-lookup"><span data-stu-id="46927-124">create a new user account</span></span>
* <span data-ttu-id="46927-125">Skapa en ny användargrupp med samma namn</span><span class="sxs-lookup"><span data-stu-id="46927-125">create a new user group with the same name</span></span>
* <span data-ttu-id="46927-126">Lägg till en tom post till`/etc/passwd`</span><span class="sxs-lookup"><span data-stu-id="46927-126">add a blank entry to `/etc/passwd`</span></span>
* <span data-ttu-id="46927-127">Lägg till en tom post till`/etc/gpasswd`</span><span class="sxs-lookup"><span data-stu-id="46927-127">add a blank entry to `/etc/gpasswd`</span></span>

<span data-ttu-id="46927-128">Den `-G` kommandoradsverktyget flaggan lägger till det nya användarkontot i rätt Linux gruppen och ge det nya användarkontot rot eskalering av privilegier.</span><span class="sxs-lookup"><span data-stu-id="46927-128">The `-G` command-line flag adds the new user account to the proper Linux group giving the new user account root escalation privileges.</span></span>

#### <a name="add-the-user"></a><span data-ttu-id="46927-129">Lägg till användare</span><span class="sxs-lookup"><span data-stu-id="46927-129">Add the user</span></span>
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a><span data-ttu-id="46927-130">Ange ett lösenord</span><span class="sxs-lookup"><span data-stu-id="46927-130">Set a password</span></span>
<span data-ttu-id="46927-131">Den `useradd` kommando skapar användaren och lägger till en post för både `/etc/passwd` och `/etc/gpasswd` men anger inte lösenordet faktiskt.</span><span class="sxs-lookup"><span data-stu-id="46927-131">The `useradd` command creates the user and adds an entry to both `/etc/passwd` and `/etc/gpasswd` but does not actually set the password.</span></span>  <span data-ttu-id="46927-132">Lösenordet har lagts till i en post med hjälp av den `passwd` kommando.</span><span class="sxs-lookup"><span data-stu-id="46927-132">The password is added to the entry using the `passwd` command.</span></span>

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

<span data-ttu-id="46927-133">Vi har nu en användare med sudo-behörighet på servern.</span><span class="sxs-lookup"><span data-stu-id="46927-133">We now have a user with sudo privileges on the server.</span></span>

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a><span data-ttu-id="46927-134">Lägg till en offentlig SSH-nyckel i det nya användarkontot</span><span class="sxs-lookup"><span data-stu-id="46927-134">Add your SSH Public Key to the new user account</span></span>
<span data-ttu-id="46927-135">Från datorn, använder du den `ssh-copy-id` med det nya lösenordet.</span><span class="sxs-lookup"><span data-stu-id="46927-135">From your machine, use the `ssh-copy-id` command with the new password.</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a><span data-ttu-id="46927-136">Med hjälp av visudo så att sudo-användningen utan lösenord</span><span class="sxs-lookup"><span data-stu-id="46927-136">Using visudo to allow sudo usage without a password</span></span>
<span data-ttu-id="46927-137">Med hjälp av `visudo` att redigera den `/etc/sudoers` filen lägger till några skyddslager mot felaktiga ändringar viktiga filen.</span><span class="sxs-lookup"><span data-stu-id="46927-137">Using `visudo` to edit the `/etc/sudoers` file adds a few layers of protection against incorrectly modifying this important file.</span></span>  <span data-ttu-id="46927-138">Vid körning `visudo`, `/etc/sudoers` är filen låst för att säkerställa att ingen användare kan göra ändringar när det är aktivt som redigeras.</span><span class="sxs-lookup"><span data-stu-id="46927-138">Upon executing `visudo`, the `/etc/sudoers` file is locked to ensure no other user can make changes while it is actively being edited.</span></span>  <span data-ttu-id="46927-139">Den `/etc/sudoers` också kontrolleras misstag av `visudo` när du försöker spara eller avsluta så att du kan spara en bruten sudoers-fil.</span><span class="sxs-lookup"><span data-stu-id="46927-139">The `/etc/sudoers` file is also checked for mistakes by `visudo` when you attempt to save or exit so you cannot save a broken sudoers file.</span></span>

<span data-ttu-id="46927-140">Vi har redan användare i rätt standardgruppen för sudo-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="46927-140">We already have users in the correct default group for sudo access.</span></span>  <span data-ttu-id="46927-141">Nu det dags att aktivera dessa grupper om du vill använda sudo utan lösenord.</span><span class="sxs-lookup"><span data-stu-id="46927-141">Now we are going to enable those groups to use sudo with no password.</span></span>

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a><span data-ttu-id="46927-142">Verifiera användaren, ssh-nycklar och sudo</span><span class="sxs-lookup"><span data-stu-id="46927-142">Verify the user, ssh keys, and sudo</span></span>
```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
