---
title: "aaaAdd användaren-tooa Linux VM i Azure | Microsoft Docs"
description: "Lägg till en användare tooa Linux VM på Azure."
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
ms.openlocfilehash: eed050154adf0cbed2c168e7aa675bd3ded85fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-user-tooan-azure-vm"></a><span data-ttu-id="f96e3-103">Lägg till en användare tooan Azure VM</span><span class="sxs-lookup"><span data-stu-id="f96e3-103">Add a user tooan Azure VM</span></span>
<span data-ttu-id="f96e3-104">En av hello första uppgifter på alla nya Linux-VM är toocreate en ny användare.</span><span class="sxs-lookup"><span data-stu-id="f96e3-104">One of hello first tasks on any new Linux VM is toocreate a new user.</span></span>  <span data-ttu-id="f96e3-105">Vi går igenom skapar ett användarkonto för sudo, ange hello lösenord att lägga till SSH offentliga nycklar i den här artikeln och slutligen använder `visudo` tooallow sudo utan lösenord.</span><span class="sxs-lookup"><span data-stu-id="f96e3-105">In this article, we walk through creating a sudo user account, setting hello password, adding SSH Public Keys, and finally use `visudo` tooallow sudo without a password.</span></span>

<span data-ttu-id="f96e3-106">Krav är: [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/), [offentliga och privata nycklar för SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), en Azure-resursgrupp och hello Azure CLI installerade och växlade tooAzure Resource Manager med hjälp av `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="f96e3-106">Prerequisites are: [an Azure account](https://azure.microsoft.com/pricing/free-trial/), [SSH public and private keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), an Azure resource group, and hello Azure CLI installed and switched tooAzure Resource Manager mode using `azure config mode arm`.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="f96e3-107">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="f96e3-107">Quick Commands</span></span>
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

# Copy hello SSH Public Key toohello new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers tooallow no password
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify hello SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="f96e3-108">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="f96e3-108">Detailed Walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="f96e3-109">Introduktion</span><span class="sxs-lookup"><span data-stu-id="f96e3-109">Introduction</span></span>
<span data-ttu-id="f96e3-110">En av hello första och de vanligaste uppgiften med en ny server är tooadd ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="f96e3-110">One of hello first and most common task with a new server is tooadd a user account.</span></span>  <span data-ttu-id="f96e3-111">Rot-inloggningar ska vara inaktiverade och hello rotkontot själva ska inte användas med Linux-servern, endast sudo.</span><span class="sxs-lookup"><span data-stu-id="f96e3-111">Root logins should be disabled and hello root account itself should not be used with your Linux server, only sudo.</span></span>  <span data-ttu-id="f96e3-112">Ger en användare rot eskalering privilegier med sudo den hello tooadminister på rätt sätt och använda Linux.</span><span class="sxs-lookup"><span data-stu-id="f96e3-112">Giving a user root escalation privileges using sudo it hello proper way tooadminister and use Linux.</span></span>

<span data-ttu-id="f96e3-113">Kommandot hello `useradd` vi lägger till användare konton toohello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="f96e3-113">Using hello command `useradd` we are adding user accounts toohello Linux VM.</span></span>  <span data-ttu-id="f96e3-114">Kör `useradd` ändrar `/etc/passwd`, `/etc/shadow`, `/etc/group`, och `/etc/gshadow`.</span><span class="sxs-lookup"><span data-stu-id="f96e3-114">Running `useradd` modifies `/etc/passwd`, `/etc/shadow`, `/etc/group`, and `/etc/gshadow`.</span></span>  <span data-ttu-id="f96e3-115">Vi lägger till en kommandorad flaggan toohello `useradd` kommandot tooalso lägga till hello ny toohello rätt sudo användargrupp på Linux.</span><span class="sxs-lookup"><span data-stu-id="f96e3-115">We are adding a command-line flag toohello `useradd` command tooalso add hello new user toohello proper sudo group on Linux.</span></span>  <span data-ttu-id="f96e3-116">Även du `useradd` skapas en post i `/etc/passwd` inte får hello nytt användarkonto ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="f96e3-116">Even thou `useradd` creates an entry into `/etc/passwd` it does not give hello new user account a password.</span></span>  <span data-ttu-id="f96e3-117">Vi skapar ett initialt lösenord för hello nya användare med enkel hello `passwd` kommando.</span><span class="sxs-lookup"><span data-stu-id="f96e3-117">We are creating an initial password for hello new user using hello simple `passwd` command.</span></span>  <span data-ttu-id="f96e3-118">hello sista steget är toomodify hello sudo regler tooallow som användaren tooexecute kommandon med sudo-behörighet utan tooenter ett lösenord för varje kommando.</span><span class="sxs-lookup"><span data-stu-id="f96e3-118">hello last step is toomodify hello sudo rules tooallow that user tooexecute commands with sudo privileges without having tooenter a password for every command.</span></span>  <span data-ttu-id="f96e3-119">Logga in med hello privata nyckel vi antar att användarkontot från obehöriga och ska tooallow sudo åtkomst utan lösenord.</span><span class="sxs-lookup"><span data-stu-id="f96e3-119">Logging in using hello Private key we are assuming that user account is safe from bad actors and are going tooallow sudo access without a password.</span></span>  

### <a name="adding-a-single-sudo-user-tooan-azure-vm"></a><span data-ttu-id="f96e3-120">Lägga till en enda sudo användaren tooan Azure VM</span><span class="sxs-lookup"><span data-stu-id="f96e3-120">Adding a single sudo user tooan Azure VM</span></span>
<span data-ttu-id="f96e3-121">Logga in toohello Azure VM med hjälp av SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="f96e3-121">Log in toohello Azure VM using SSH keys.</span></span>  <span data-ttu-id="f96e3-122">Om du inte har installationsprogrammet SSH offentlig nyckel kan slutföra den här artikeln först [med offentlig nyckel autentisering med Azure](http://link.to/article).</span><span class="sxs-lookup"><span data-stu-id="f96e3-122">If you have not setup SSH public key access, complete this article first [Using Public Key Authentication with Azure](http://link.to/article).</span></span>  

<span data-ttu-id="f96e3-123">Hej `useradd` kommandot har slutförts hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="f96e3-123">hello `useradd` command completes hello following tasks:</span></span>

* <span data-ttu-id="f96e3-124">Skapa ett nytt användarkonto</span><span class="sxs-lookup"><span data-stu-id="f96e3-124">create a new user account</span></span>
* <span data-ttu-id="f96e3-125">Skapa en ny användargrupp med hello samma namn</span><span class="sxs-lookup"><span data-stu-id="f96e3-125">create a new user group with hello same name</span></span>
* <span data-ttu-id="f96e3-126">Lägg till en tom post för`/etc/passwd`</span><span class="sxs-lookup"><span data-stu-id="f96e3-126">add a blank entry too`/etc/passwd`</span></span>
* <span data-ttu-id="f96e3-127">Lägg till en tom post för`/etc/gpasswd`</span><span class="sxs-lookup"><span data-stu-id="f96e3-127">add a blank entry too`/etc/gpasswd`</span></span>

<span data-ttu-id="f96e3-128">Hej `-G` kommandoradsverktyget flaggan lägger till hello ny konto toohello rätt Linux användargrupp ger hello nytt användarkonto rot eskalering av privilegier.</span><span class="sxs-lookup"><span data-stu-id="f96e3-128">hello `-G` command-line flag adds hello new user account toohello proper Linux group giving hello new user account root escalation privileges.</span></span>

#### <a name="add-hello-user"></a><span data-ttu-id="f96e3-129">Lägg till hello användare</span><span class="sxs-lookup"><span data-stu-id="f96e3-129">Add hello user</span></span>
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a><span data-ttu-id="f96e3-130">Ange ett lösenord</span><span class="sxs-lookup"><span data-stu-id="f96e3-130">Set a password</span></span>
<span data-ttu-id="f96e3-131">Hej `useradd` kommando skapar hello användare och lägger till en post tooboth `/etc/passwd` och `/etc/gpasswd` men anger inte faktiskt hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="f96e3-131">hello `useradd` command creates hello user and adds an entry tooboth `/etc/passwd` and `/etc/gpasswd` but does not actually set hello password.</span></span>  <span data-ttu-id="f96e3-132">hello lösenord läggs toohello posten med hello `passwd` kommando.</span><span class="sxs-lookup"><span data-stu-id="f96e3-132">hello password is added toohello entry using hello `passwd` command.</span></span>

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

<span data-ttu-id="f96e3-133">Vi har nu en användare med sudo-behörighet på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="f96e3-133">We now have a user with sudo privileges on hello server.</span></span>

### <a name="add-your-ssh-public-key-toohello-new-user-account"></a><span data-ttu-id="f96e3-134">Lägga till en offentlig SSH-nyckel toohello nytt användarkonto</span><span class="sxs-lookup"><span data-stu-id="f96e3-134">Add your SSH Public Key toohello new user account</span></span>
<span data-ttu-id="f96e3-135">Använd hello från din dator `ssh-copy-id` med hello nytt lösenord.</span><span class="sxs-lookup"><span data-stu-id="f96e3-135">From your machine, use hello `ssh-copy-id` command with hello new password.</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-tooallow-sudo-usage-without-a-password"></a><span data-ttu-id="f96e3-136">Med visudo tooallow sudo användning utan lösenord</span><span class="sxs-lookup"><span data-stu-id="f96e3-136">Using visudo tooallow sudo usage without a password</span></span>
<span data-ttu-id="f96e3-137">Med hjälp av `visudo` tooedit hello `/etc/sudoers` filen lägger till några skyddslager mot felaktiga ändringar viktiga filen.</span><span class="sxs-lookup"><span data-stu-id="f96e3-137">Using `visudo` tooedit hello `/etc/sudoers` file adds a few layers of protection against incorrectly modifying this important file.</span></span>  <span data-ttu-id="f96e3-138">Vid körning `visudo`, hello `/etc/sudoers` filen är låst tooensure ingen användare kan göra ändringar när det är aktivt som redigeras.</span><span class="sxs-lookup"><span data-stu-id="f96e3-138">Upon executing `visudo`, hello `/etc/sudoers` file is locked tooensure no other user can make changes while it is actively being edited.</span></span>  <span data-ttu-id="f96e3-139">Hej `/etc/sudoers` också kontrolleras misstag av `visudo` när du försöker toosave eller avsluta så att du kan spara en bruten sudoers-fil.</span><span class="sxs-lookup"><span data-stu-id="f96e3-139">hello `/etc/sudoers` file is also checked for mistakes by `visudo` when you attempt toosave or exit so you cannot save a broken sudoers file.</span></span>

<span data-ttu-id="f96e3-140">Vi har redan användare i hello rätt standardgruppen för sudo-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f96e3-140">We already have users in hello correct default group for sudo access.</span></span>  <span data-ttu-id="f96e3-141">Nu vi tooenable dessa grupper toouse sudo utan lösenord.</span><span class="sxs-lookup"><span data-stu-id="f96e3-141">Now we are going tooenable those groups toouse sudo with no password.</span></span>

```bash
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-hello-user-ssh-keys-and-sudo"></a><span data-ttu-id="f96e3-142">Kontrollera hello användaren ssh-nycklar och sudo</span><span class="sxs-lookup"><span data-stu-id="f96e3-142">Verify hello user, ssh keys, and sudo</span></span>
```bash
# Verify hello SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
