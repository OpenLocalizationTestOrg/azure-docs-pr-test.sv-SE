---
title: "aaaTroubleshoot SSH-anslutningen utfärdar tooan Azure VM | Microsoft Docs"
description: "Hur tootroubleshoot problem, till exempel ”SSH-anslutningen misslyckades' eller 'SSH-anslutningen nekades' för en Azure VM kör Linux."
keywords: SSH anslutningen nekades, ssh fel, azure ssh, SSH-anslutning misslyckades
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: dfb4e75e571c8306edf5f300c4e0f07a5fe7750a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-ssh-connections-tooan-azure-linux-vm-that-fails-errors-out-or-is-refused"></a><span data-ttu-id="50a30-104">Felsökning av SSH-anslutningar tooan virtuella Azure Linux-datorn som misslyckas, fel, eller nekas</span><span class="sxs-lookup"><span data-stu-id="50a30-104">Troubleshoot SSH connections tooan Azure Linux VM that fails, errors out, or is refused</span></span>
<span data-ttu-id="50a30-105">Det finns olika orsaker till att det uppstår fel på SSH (Secure Shell), SSH anslutningsfel eller SSH nekas när du försöker tooconnect tooa virtuell Linux-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="50a30-105">There are various reasons that you encounter Secure Shell (SSH) errors, SSH connection failures, or SSH is refused when you try tooconnect tooa Linux virtual machine (VM).</span></span> <span data-ttu-id="50a30-106">Den här artikeln hjälper dig att hitta och rätt hello problem.</span><span class="sxs-lookup"><span data-stu-id="50a30-106">This article helps you find and correct hello problems.</span></span> <span data-ttu-id="50a30-107">Du kan använda hello Azure-portalen, Azure CLI eller tillägg för virtuell dator åtkomst för Linux tootroubleshoot och lösa anslutningsproblem med.</span><span class="sxs-lookup"><span data-stu-id="50a30-107">You can use hello Azure portal, Azure CLI, or VM Access Extension for Linux tootroubleshoot and resolve connection problems.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="50a30-108">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](http://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="50a30-108">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](http://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="50a30-109">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="50a30-109">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="50a30-110">Gå toohello [Azure supportwebbplats](http://azure.microsoft.com/support/options/) och välj **få support**.</span><span class="sxs-lookup"><span data-stu-id="50a30-110">Go toohello [Azure support site](http://azure.microsoft.com/support/options/) and select **Get support**.</span></span> <span data-ttu-id="50a30-111">Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](http://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="50a30-111">For information about using Azure Support, read hello [Microsoft Azure support FAQ](http://azure.microsoft.com/support/faq/).</span></span>

## <a name="quick-troubleshooting-steps"></a><span data-ttu-id="50a30-112">Snabbsteg för felsökning</span><span class="sxs-lookup"><span data-stu-id="50a30-112">Quick troubleshooting steps</span></span>
<span data-ttu-id="50a30-113">Försöka ansluta igen toohello VM efter varje steg i felsökningen.</span><span class="sxs-lookup"><span data-stu-id="50a30-113">After each troubleshooting step, try reconnecting toohello VM.</span></span>

1. <span data-ttu-id="50a30-114">Återställa hello SSH-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="50a30-114">Reset hello SSH configuration.</span></span>
2. <span data-ttu-id="50a30-115">Återställa hello autentiseringsuppgifter för hello användaren.</span><span class="sxs-lookup"><span data-stu-id="50a30-115">Reset hello credentials for hello user.</span></span>
3. <span data-ttu-id="50a30-116">Kontrollera hello [Nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) regler att SSH-trafik.</span><span class="sxs-lookup"><span data-stu-id="50a30-116">Verify hello [Network Security Group](../../virtual-network/virtual-networks-nsg.md) rules permit SSH traffic.</span></span>
   * <span data-ttu-id="50a30-117">Se till att det finns en Nätverkssäkerhetsgrupp regel toopermit SSH-trafik (som standard, TCP-port 22).</span><span class="sxs-lookup"><span data-stu-id="50a30-117">Ensure that a Network Security Group rule exists toopermit SSH traffic (by default, TCP port 22).</span></span>
   * <span data-ttu-id="50a30-118">Du kan inte använda omdirigering av portar / mappning utan att använda en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="50a30-118">You cannot use port redirection / mapping without using an Azure load balancer.</span></span>
4. <span data-ttu-id="50a30-119">Kontrollera hello [VM resurshälsa](../../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="50a30-119">Check hello [VM resource health](../../resource-health/resource-health-overview.md).</span></span> 
   * <span data-ttu-id="50a30-120">Se till att hello VM rapporter som felfritt.</span><span class="sxs-lookup"><span data-stu-id="50a30-120">Ensure that hello VM reports as being healthy.</span></span>
   * <span data-ttu-id="50a30-121">Om du har aktiverat startdiagnostikinställningar Kontrollera hello VM inte rapporterar startfel i hello loggar.</span><span class="sxs-lookup"><span data-stu-id="50a30-121">If you have boot diagnostics enabled, verify hello VM is not reporting boot errors in hello logs.</span></span>
5. <span data-ttu-id="50a30-122">Starta om hello VM.</span><span class="sxs-lookup"><span data-stu-id="50a30-122">Restart hello VM.</span></span>
6. <span data-ttu-id="50a30-123">Omdistribuera hello VM.</span><span class="sxs-lookup"><span data-stu-id="50a30-123">Redeploy hello VM.</span></span>

<span data-ttu-id="50a30-124">Fortsätt läsa för mer detaljerad felsökning och förklaringar.</span><span class="sxs-lookup"><span data-stu-id="50a30-124">Continue reading for more detailed troubleshooting steps and explanations.</span></span>

## <a name="available-methods-tootroubleshoot-ssh-connection-issues"></a><span data-ttu-id="50a30-125">Tillgängliga metoder tootroubleshoot SSH-anslutningsproblem</span><span class="sxs-lookup"><span data-stu-id="50a30-125">Available methods tootroubleshoot SSH connection issues</span></span>
<span data-ttu-id="50a30-126">Du kan återställa autentiseringsuppgifter eller SSH-konfigurationen med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="50a30-126">You can reset credentials or SSH configuration using one of hello following methods:</span></span>

* <span data-ttu-id="50a30-127">[Azure-portalen](#use-the-azure-portal) – bra om du behöver tooquickly återställa hello SSH-konfigurationen eller SSH-nyckeln och du inte hello Azure-verktyg installerat.</span><span class="sxs-lookup"><span data-stu-id="50a30-127">[Azure portal](#use-the-azure-portal) - great if you need tooquickly reset hello SSH configuration or SSH key and you don't have hello Azure tools installed.</span></span>
* <span data-ttu-id="50a30-128">[Azure CLI 2.0](#use-the-azure-cli-20) - om du redan är på hello kommandoraden snabbt återställa hello SSH-konfigurationen eller autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="50a30-128">[Azure CLI 2.0](#use-the-azure-cli-20) - if you are already on hello command line, quickly reset hello SSH configuration or credentials.</span></span> <span data-ttu-id="50a30-129">Du kan också använda hello [Azure CLI 1.0](#use-the-azure-cli-10)</span><span class="sxs-lookup"><span data-stu-id="50a30-129">You can also use hello [Azure CLI 1.0](#use-the-azure-cli-10)</span></span>
* <span data-ttu-id="50a30-130">[Azure VMAccessForLinux-tillägget](#use-the-vmaccess-extension) – skapa och återanvända json definition filer tooreset hello SSH konfiguration eller autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="50a30-130">[Azure VMAccessForLinux extension](#use-the-vmaccess-extension) - create and reuse json definition files tooreset hello SSH configuration or user credentials.</span></span>

<span data-ttu-id="50a30-131">Försök ansluta tooyour VM igen efter varje steg i felsökningen.</span><span class="sxs-lookup"><span data-stu-id="50a30-131">After each troubleshooting step, try connecting tooyour VM again.</span></span> <span data-ttu-id="50a30-132">Om du fortfarande inte kan ansluta försök hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="50a30-132">If you still cannot connect, try hello next step.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="50a30-133">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="50a30-133">Use hello Azure portal</span></span>
<span data-ttu-id="50a30-134">hello Azure-portalen innehåller ett snabbt sätt tooreset hello SSH-konfigurationen eller autentiseringsuppgifter utan att installera verktyg som på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="50a30-134">hello Azure portal provides a quick way tooreset hello SSH configuration or user credentials without installing any tools on your local computer.</span></span>

<span data-ttu-id="50a30-135">Välj den virtuella datorn i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="50a30-135">Select your VM in hello Azure portal.</span></span> <span data-ttu-id="50a30-136">Bläddra nedåt toohello **stöd + felsökning** avsnittet och väljer **Återställ lösenord** som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="50a30-136">Scroll down toohello **Support + Troubleshooting** section and select **Reset password** as in hello following example:</span></span>

![Återställ SSH-konfigurationen eller autentiseringsuppgifter i hello Azure-portalen](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-hello-ssh-configuration"></a><span data-ttu-id="50a30-138">Återställ hello SSH-konfiguration</span><span class="sxs-lookup"><span data-stu-id="50a30-138">Reset hello SSH configuration</span></span>
<span data-ttu-id="50a30-139">Som ett första steg bör du välja `Reset configuration only` från hello **läge** nedrullningsbara menyn som i föregående skärmbilden hello Klicka hello **återställa** knappen.</span><span class="sxs-lookup"><span data-stu-id="50a30-139">As a first step, select `Reset configuration only` from hello **Mode** drop-down menu as in hello preceding screenshot, then click hello **Reset** button.</span></span> <span data-ttu-id="50a30-140">När den här åtgärden har slutförts, försök tooaccess den virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="50a30-140">Once this action has completed, try tooaccess your VM again.</span></span>

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="50a30-141">Återställ SSH-autentiseringsuppgifter för en användare</span><span class="sxs-lookup"><span data-stu-id="50a30-141">Reset SSH credentials for a user</span></span>
<span data-ttu-id="50a30-142">tooreset hello autentiseringsuppgifterna för en befintlig användare väljer du antingen `Reset SSH public key` eller `Reset password` från hello **läge** nedrullningsbara menyn som hello föregående skärmbild.</span><span class="sxs-lookup"><span data-stu-id="50a30-142">tooreset hello credentials of an existing user, select either `Reset SSH public key` or `Reset password` from hello **Mode** drop-down menu as in hello preceding screenshot.</span></span> <span data-ttu-id="50a30-143">Ange hello användarnamn och en SSH-nyckel eller ett nytt lösenord och klicka sedan på hello **återställa** knappen.</span><span class="sxs-lookup"><span data-stu-id="50a30-143">Specify hello username and an SSH key or new password, then click hello **Reset** button.</span></span>

<span data-ttu-id="50a30-144">Du kan också skapa en användare med sudo-behörighet på hello VM från den här menyn.</span><span class="sxs-lookup"><span data-stu-id="50a30-144">You can also create a user with sudo privileges on hello VM from this menu.</span></span> <span data-ttu-id="50a30-145">Ange ett nytt användarnamn och tillhörande lösenord eller SSH-nyckel och klicka sedan på hello **återställa** knappen.</span><span class="sxs-lookup"><span data-stu-id="50a30-145">Enter a new username and associated password or SSH key, and then click hello **Reset** button.</span></span>

## <a name="use-hello-azure-cli-20"></a><span data-ttu-id="50a30-146">Använd hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50a30-146">Use hello Azure CLI 2.0</span></span>
<span data-ttu-id="50a30-147">Om du inte redan gjort installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="50a30-147">If you haven't already, install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="50a30-148">Om du har skapat och överföra en anpassad avbildning för Linux-disk, se att hello [Microsoft Azure Linux-agenten](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 eller senare är installerat.</span><span class="sxs-lookup"><span data-stu-id="50a30-148">If you created and uploaded a custom Linux disk image, make sure hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="50a30-149">För virtuella datorer skapas med galleriavbildningar kan det här tillägget för åtkomst redan installeras och konfigureras för dig.</span><span class="sxs-lookup"><span data-stu-id="50a30-149">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="50a30-150">Återställ SSH-konfiguration</span><span class="sxs-lookup"><span data-stu-id="50a30-150">Reset SSH configuration</span></span>
<span data-ttu-id="50a30-151">Du kan först Försök återställa hello SSH toodefault konfigurationsvärden och om hello SSH-server på hello VM.</span><span class="sxs-lookup"><span data-stu-id="50a30-151">You can initially try resetting hello SSH configuration toodefault values and rebooting hello SSH server on hello VM.</span></span> <span data-ttu-id="50a30-152">Observera att detta inte ändra hello konto användarnamn, lösenord eller SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="50a30-152">Note that this does not change hello user account name, password, or SSH keys.</span></span>
<span data-ttu-id="50a30-153">hello följande exempel används [az vm användare återställa-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello SSH-konfigurationen på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-153">hello following example uses [az vm user reset-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello SSH configuration on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-154">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-154">Use your own values as follows:</span></span>

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="50a30-155">Återställ SSH-autentiseringsuppgifter för en användare</span><span class="sxs-lookup"><span data-stu-id="50a30-155">Reset SSH credentials for a user</span></span>
<span data-ttu-id="50a30-156">hello följande exempel används [az vm uppdateringen](/cli/azure/vm/user#update) tooreset hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-156">hello following example uses [az vm user update](/cli/azure/vm/user#update) tooreset hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-157">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-157">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

<span data-ttu-id="50a30-158">Om du använder SSH-nyckelautentisering, kan du återställa hello SSH-nyckel för en viss användare.</span><span class="sxs-lookup"><span data-stu-id="50a30-158">If using SSH key authentication, you can reset hello SSH key for a given user.</span></span> <span data-ttu-id="50a30-159">hello följande exempel används **az vm komma åt set-linux-användare** tooupdate hello SSH-nyckel som lagras i `~/.ssh/id_rsa.pub` för hello användare med namnet `myUsername`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-159">hello following example uses **az vm access set-linux-user** tooupdate hello SSH key stored in `~/.ssh/id_rsa.pub` for hello user named `myUsername`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-160">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-160">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-hello-vmaccess-extension"></a><span data-ttu-id="50a30-161">Använd hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="50a30-161">Use hello VMAccess extension</span></span>
<span data-ttu-id="50a30-162">hello VM Access-tillägg för Linux läser i en json-fil som definierar åtgärder toocarry ut. Dessa åtgärder omfattar återställer SSHD, när du återställer en SSH-nyckel eller lägger till en användare.</span><span class="sxs-lookup"><span data-stu-id="50a30-162">hello VM Access Extension for Linux reads in a json file that defines actions toocarry out. These actions include resetting SSHD, resetting an SSH key, or adding a user.</span></span> <span data-ttu-id="50a30-163">Du fortfarande använda hello Azure CLI toocall hello VMAccess-tillägget, men du kan återanvända hello json-filer över flera virtuella datorer om du vill.</span><span class="sxs-lookup"><span data-stu-id="50a30-163">You still use hello Azure CLI toocall hello VMAccess extension, but you can reuse hello json files across multiple VMs if desired.</span></span> <span data-ttu-id="50a30-164">Den här metoden kan du toocreate en databas för json-filer som sedan kan anropas för angivna scenarier.</span><span class="sxs-lookup"><span data-stu-id="50a30-164">This approach allows you toocreate a repository of json files that can then be called for given scenarios.</span></span>

### <a name="reset-sshd"></a><span data-ttu-id="50a30-165">Återställ SSHD</span><span class="sxs-lookup"><span data-stu-id="50a30-165">Reset SSHD</span></span>
<span data-ttu-id="50a30-166">Skapa en fil med namnet `settings.json` med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="50a30-166">Create a file named `settings.json` with hello following content:</span></span>

```json
{  
    "reset_ssh":"True"
}
```

<span data-ttu-id="50a30-167">Använda hello Azure CLI kan du sedan anropa hello `VMAccessForLinux` tillägget tooreset SSHD anslutningen genom att ange en json-fil.</span><span class="sxs-lookup"><span data-stu-id="50a30-167">Using hello Azure CLI, you then call hello `VMAccessForLinux` extension tooreset your SSHD connection by specifying your json file.</span></span> <span data-ttu-id="50a30-168">hello följande exempel används [az vm-tillägget set](/cli/azure/vm/extension#set) tooreset SSHD på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-168">hello following example uses [az vm extension set](/cli/azure/vm/extension#set) tooreset SSHD on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-169">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-169">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="50a30-170">Återställ SSH-autentiseringsuppgifter för en användare</span><span class="sxs-lookup"><span data-stu-id="50a30-170">Reset SSH credentials for a user</span></span>
<span data-ttu-id="50a30-171">Om SSHD visas toofunction korrekt, kan du återställa hello autentiseringsuppgifter för en x-användare.</span><span class="sxs-lookup"><span data-stu-id="50a30-171">If SSHD appears toofunction correctly, you can reset hello credentials for a giver user.</span></span> <span data-ttu-id="50a30-172">tooreset hello lösenord för en användare skapar en fil med namnet `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="50a30-172">tooreset hello password for a user, create a file named `settings.json`.</span></span> <span data-ttu-id="50a30-173">hello följande exempel återställs hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="50a30-173">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`.</span></span> <span data-ttu-id="50a30-174">Ange hello följande rader i din `settings.json` fil, använda egna värden:</span><span class="sxs-lookup"><span data-stu-id="50a30-174">Enter hello following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

<span data-ttu-id="50a30-175">Eller tooreset Hej SSH-nyckel för en användare, först skapa en fil med namnet `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="50a30-175">Or tooreset hello SSH key for a user, first create a file named `settings.json`.</span></span> <span data-ttu-id="50a30-176">hello följande exempel återställs hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-176">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-177">Ange hello följande rader i din `settings.json` fil, använda egna värden:</span><span class="sxs-lookup"><span data-stu-id="50a30-177">Enter hello following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

<span data-ttu-id="50a30-178">När du har skapat en json-fil, använder du hello Azure CLI toocall hello `VMAccessForLinux` tillägget tooreset SSH-användare autentiseringsuppgifter genom att ange en json-fil.</span><span class="sxs-lookup"><span data-stu-id="50a30-178">After creating your json file, use hello Azure CLI toocall hello `VMAccessForLinux` extension tooreset your SSH user credentials by specifying your json file.</span></span> <span data-ttu-id="50a30-179">hello följande exempel återställs autentiseringsuppgifter på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-179">hello following example resets credentials on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-180">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-180">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-hello-azure-cli-10"></a><span data-ttu-id="50a30-181">Använd hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50a30-181">Use hello Azure CLI 1.0</span></span>
<span data-ttu-id="50a30-182">Om du inte redan har gjort [installera hello Azure CLI 1.0 och ansluta tooyour Azure-prenumeration](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="50a30-182">If you haven't already, [install hello Azure CLI 1.0 and connect tooyour Azure subscription](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="50a30-183">Kontrollera att du använder Resource Manager-läge på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="50a30-183">Make sure that you are using Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="50a30-184">Om du har skapat och överföra en anpassad avbildning för Linux-disk, se att hello [Microsoft Azure Linux-agenten](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 eller senare är installerat.</span><span class="sxs-lookup"><span data-stu-id="50a30-184">If you created and uploaded a custom Linux disk image, make sure hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="50a30-185">För virtuella datorer skapas med galleriavbildningar kan det här tillägget för åtkomst redan installeras och konfigureras för dig.</span><span class="sxs-lookup"><span data-stu-id="50a30-185">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="50a30-186">Återställ SSH-konfiguration</span><span class="sxs-lookup"><span data-stu-id="50a30-186">Reset SSH configuration</span></span>
<span data-ttu-id="50a30-187">Hej SSHD configuration själva kan vara felkonfigurerad eller hello tjänsten påträffade ett fel.</span><span class="sxs-lookup"><span data-stu-id="50a30-187">hello SSHD configuration itself may be misconfigured or hello service encountered an error.</span></span> <span data-ttu-id="50a30-188">Du kan återställa SSHD toomake att hello SSH-konfigurationen själva är giltig.</span><span class="sxs-lookup"><span data-stu-id="50a30-188">You can reset SSHD toomake sure hello SSH configuration itself is valid.</span></span> <span data-ttu-id="50a30-189">Återställer SSHD ska hello första åtgärden du vidta.</span><span class="sxs-lookup"><span data-stu-id="50a30-189">Resetting SSHD should be hello first troubleshooting step you take.</span></span>

<span data-ttu-id="50a30-190">hello följande exempel återställs SSHD på en virtuell dator med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-190">hello following example resets SSHD on a VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="50a30-191">Använd din egen VM- och gruppnamn enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-191">Use your own VM and resource group names as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="50a30-192">Återställ SSH-autentiseringsuppgifter för en användare</span><span class="sxs-lookup"><span data-stu-id="50a30-192">Reset SSH credentials for a user</span></span>
<span data-ttu-id="50a30-193">Du kan återställa hello lösenord för en användare som är om SSHD visas toofunction korrekt.</span><span class="sxs-lookup"><span data-stu-id="50a30-193">If SSHD appears toofunction correctly, you can reset hello password for a giver user.</span></span> <span data-ttu-id="50a30-194">hello följande exempel återställs hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-194">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-195">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-195">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

<span data-ttu-id="50a30-196">Om du använder SSH-nyckelautentisering, kan du återställa hello SSH-nyckel för en viss användare.</span><span class="sxs-lookup"><span data-stu-id="50a30-196">If using SSH key authentication, you can reset hello SSH key for a given user.</span></span> <span data-ttu-id="50a30-197">följande exempel uppdateringar hello hello SSH-nyckel som lagras i `~/.ssh/id_rsa.pub` för hello användare med namnet `myUsername`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-197">hello following example updates hello SSH key stored in `~/.ssh/id_rsa.pub` for hello user named `myUsername`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="50a30-198">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-198">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a><span data-ttu-id="50a30-199">Starta om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="50a30-199">Restart a VM</span></span>
<span data-ttu-id="50a30-200">Om du har återställts hello SSH-konfigurationen och användarautentiseringsuppgifter eller påträffade ett fel när detta sker, kan du omstart hello VM tooaddress underliggande beräkning problem.</span><span class="sxs-lookup"><span data-stu-id="50a30-200">If you have reset hello SSH configuration and user credentials, or encountered an error in doing so, you can try restarting hello VM tooaddress underlying compute issues.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="50a30-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="50a30-201">Azure portal</span></span>
<span data-ttu-id="50a30-202">toorestart som en virtuell dator med hjälp av hello Azure portal, Välj den virtuella datorn och klicka på hello **starta om** knappen som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="50a30-202">toorestart a VM using hello Azure portal, select your VM and click hello **Restart** button as in hello following example:</span></span>

![Starta om en virtuell dator i hello Azure-portalen](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="50a30-204">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50a30-204">Azure CLI 1.0</span></span>
<span data-ttu-id="50a30-205">följande exempel omstarter hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-205">hello following example restarts hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="50a30-206">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-206">Use your own values as follows:</span></span>

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="50a30-207">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50a30-207">Azure CLI 2.0</span></span>
<span data-ttu-id="50a30-208">hello följande exempel används [az vm omstart](/cli/azure/vm#restart) toorestart hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-208">hello following example uses [az vm restart](/cli/azure/vm#restart) toorestart hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="50a30-209">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-209">Use your own values as follows:</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a><span data-ttu-id="50a30-210">Distribuera om en VM</span><span class="sxs-lookup"><span data-stu-id="50a30-210">Redeploy a VM</span></span>
<span data-ttu-id="50a30-211">Du kan distribuera en VM tooanother nod i Azure, vilket kan korrigera eventuella underliggande nätverksproblem.</span><span class="sxs-lookup"><span data-stu-id="50a30-211">You can redeploy a VM tooanother node within Azure, which may correct any underlying networking issues.</span></span> <span data-ttu-id="50a30-212">Information om omdistribuera en virtuell dator finns [omdistribuera virtuell dator toonew Azure nod](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="50a30-212">For information about redeploying a VM, see [Redeploy virtual machine toonew Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="50a30-213">När den här åtgärden har slutförts tillfälliga data kommer att gå förlorade och dynamiska IP-adresser som är associerade med hello virtuella datorn kommer att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="50a30-213">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with hello virtual machine will be updated.</span></span>
> 
> 

### <a name="azure-portal"></a><span data-ttu-id="50a30-214">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="50a30-214">Azure portal</span></span>
<span data-ttu-id="50a30-215">tooredeploy som en virtuell dator med hjälp av hello Azure portal, Välj den virtuella datorn och rulla ned toohello **stöd + felsökning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="50a30-215">tooredeploy a VM using hello Azure portal, select your VM and scroll down toohello **Support + Troubleshooting** section.</span></span> <span data-ttu-id="50a30-216">Klicka på hello **omdistribuera** knappen som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="50a30-216">Click hello **Redeploy** button as in hello following example:</span></span>

![Distribuera en virtuell dator i hello Azure-portalen](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="50a30-218">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50a30-218">Azure CLI 1.0</span></span>
<span data-ttu-id="50a30-219">följande exempel återdistribuerar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-219">hello following example redeploys hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="50a30-220">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-220">Use your own values as follows:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="50a30-221">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50a30-221">Azure CLI 2.0</span></span>
<span data-ttu-id="50a30-222">Hej följande exempel används [az vm Omdistributionen](/cli/azure/vm#redeploy) tooredeploy hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="50a30-222">hello following example use [az vm redeploy](/cli/azure/vm#redeploy) tooredeploy hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="50a30-223">Använd egna värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="50a30-223">Use your own values as follows:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-hello-classic-deployment-model"></a><span data-ttu-id="50a30-224">Virtuella datorer som skapats med hjälp av hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="50a30-224">VMs created by using hello Classic deployment model</span></span>
<span data-ttu-id="50a30-225">Försök dessa steg tooresolve hello vanligaste SSH anslutningsfel för virtuella datorer som har skapats med hjälp av hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="50a30-225">Try these steps tooresolve hello most common SSH connection failures for VMs that were created by using hello classic deployment model.</span></span> <span data-ttu-id="50a30-226">Försöka ansluta igen toohello VM efter varje steg.</span><span class="sxs-lookup"><span data-stu-id="50a30-226">After each step, try reconnecting toohello VM.</span></span>

* <span data-ttu-id="50a30-227">Återställ fjärråtkomst från hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="50a30-227">Reset remote access from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="50a30-228">På hello Azure-portalen markerar du den virtuella datorn och på hello **Återställ fjärråtkomst...**  knappen.</span><span class="sxs-lookup"><span data-stu-id="50a30-228">On hello Azure portal, select your VM and click hello **Reset Remote...** button.</span></span>
* <span data-ttu-id="50a30-229">Starta om hello VM.</span><span class="sxs-lookup"><span data-stu-id="50a30-229">Restart hello VM.</span></span> <span data-ttu-id="50a30-230">På hello [Azure-portalen](https://portal.azure.com), Välj den virtuella datorn och på hello **starta om** knappen.</span><span class="sxs-lookup"><span data-stu-id="50a30-230">On hello [Azure portal](https://portal.azure.com), select your VM and click hello **Restart** button.</span></span>
    
* <span data-ttu-id="50a30-231">Omdistribuera hello VM tooa nya Azure noden.</span><span class="sxs-lookup"><span data-stu-id="50a30-231">Redeploy hello VM tooa new Azure node.</span></span> <span data-ttu-id="50a30-232">Information om hur tooredeploy en virtuell dator finns [omdistribuera virtuell dator toonew Azure nod](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="50a30-232">For information about how tooredeploy a VM, see [Redeploy virtual machine toonew Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  
    <span data-ttu-id="50a30-233">När den här åtgärden har slutförts tillfälliga data kommer att gå förlorade och dynamiska IP-adresser som är associerade med hello virtuella datorn kommer att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="50a30-233">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with hello virtual machine will be updated.</span></span>
* <span data-ttu-id="50a30-234">Följ anvisningarna för hello i [hur tooreset lösenord eller SSH för Linux-baserade virtuella datorer](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) till:</span><span class="sxs-lookup"><span data-stu-id="50a30-234">Follow hello instructions in [How tooreset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) to:</span></span>
  
  * <span data-ttu-id="50a30-235">Återställ hello lösenord eller SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="50a30-235">Reset hello password or SSH key.</span></span>
  * <span data-ttu-id="50a30-236">Skapa en *sudo* användarkonto.</span><span class="sxs-lookup"><span data-stu-id="50a30-236">Create a *sudo* user account.</span></span>
  * <span data-ttu-id="50a30-237">Återställa hello SSH-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="50a30-237">Reset hello SSH configuration.</span></span>
* <span data-ttu-id="50a30-238">Kontrollera hello Virtuella datorresursens hälsotillstånd vid plattformsproblem.</span><span class="sxs-lookup"><span data-stu-id="50a30-238">Check hello VM's resource health for any platform issues.</span></span><br>
     <span data-ttu-id="50a30-239">Välj den virtuella datorn och rulla ned **inställningar** > **Kontrollera hälsan**.</span><span class="sxs-lookup"><span data-stu-id="50a30-239">Select your VM and scroll down **Settings** > **Check Health**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="50a30-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="50a30-240">Additional resources</span></span>
* <span data-ttu-id="50a30-241">Om du fortfarande inte tooSSH tooyour VM efter följande hello efter stegen, se [mer detaljerad felsökning](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview ytterligare steg tooresolve problemet.</span><span class="sxs-lookup"><span data-stu-id="50a30-241">If you are still unable tooSSH tooyour VM after following hello after steps, see [more detailed troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview additional steps tooresolve your issue.</span></span>
* <span data-ttu-id="50a30-242">Mer information om hur du felsöker programåtkomst finns [Felsök åtkomst tooan program som körs på en virtuell Azure-dator](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="50a30-242">For more information about troubleshooting application access, see [Troubleshoot access tooan application running on an Azure virtual machine](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="50a30-243">Mer information om felsökning av virtuella datorer som har skapats med hjälp av hello klassiska distributionsmodellen finns [hur tooreset lösenord eller SSH för Linux-baserade virtuella datorer](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="50a30-243">For more information about troubleshooting virtual machines that were created by using hello classic deployment model, see [How tooreset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

