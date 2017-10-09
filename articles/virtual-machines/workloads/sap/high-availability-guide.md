---
title: "aaaAzure hög tillgänglighet för virtuella datorer för SAP NetWeaver | Microsoft Docs"
description: "Hög tillgänglighet guide för SAP NetWeaver på Azure Virtual Machines"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/07/2016
ms.author: goraco
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 927409830065573248a43427eb382448ca07b6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a><span data-ttu-id="d2173-103">Hög tillgänglighet för SAP NetWeaver på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="d2173-103">High availability for SAP NetWeaver on Azure VMs</span></span>

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:https://docs.microsoft.com/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md
[dbms-guide-2.1]:../../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:../../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:../../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:../../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:../../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:../../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:../../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:../../virtual-machines-windows-sap-deployment-guide.md
[deployment-guide-2.2]:../../virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:../../virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:../../virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:../../virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:../../virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:../../virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:../../virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:../../virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:../../virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:../../virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:../../virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:../../virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:../../virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:../../virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:../../virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:../../virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:../../virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:../../virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:../../virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:../../virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:../../virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:../../virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:../../virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:../../virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:../../virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:../../virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:../../virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:../../virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../../../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../../../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:../../virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:../../virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:../../virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:../../virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:high-availability-guide.md
[sap-ha-guide-1]:high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c
[sap-ha-guide-2]:high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-3]:high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1
[sap-ha-guide-3.1]:high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686
[sap-ha-guide-3.2]:high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860
[sap-ha-guide-4]:high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-4.1]:high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2
[sap-ha-guide-5]:high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275
[sap-ha-guide-5.1]:high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592
[sap-ha-guide-5.2]:high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd
[sap-ha-guide-6]:high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a
[sap-ha-guide-6.1]:high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341
[sap-ha-guide-6.2]:high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb
[sap-ha-guide-7]:high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa
[sap-ha-guide-7.1]:high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e
[sap-ha-guide-7.2]:high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db
[sap-ha-guide-7.2.1]:high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51
[sap-ha-guide-7.3]:high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027
[sap-ha-guide-7.4]:high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816
[sap-ha-guide-8]:high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.2]:high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310
[sap-ha-guide-8.3]:high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a
[sap-ha-guide-8.4]:high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e
[sap-ha-guide-8.5]:high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f
[sap-ha-guide-8.6]:high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30
[sap-ha-guide-8.7]:high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e
[sap-ha-guide-8.8]:high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.10]:high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.2]:high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca
[sap-ha-guide-8.12.2.1]:high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855
[sap-ha-guide-8.12.2.2]:high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d
[sap-ha-guide-8.12.3]:high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-ha-guide-9.1.2]:high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0
[sap-ha-guide-9.1.3]:high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-ha-guide-9.1.4]:high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-ha-guide-9.1.5]:high-availability-guide.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-ha-guide-9.2]:high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7
[sap-ha-guide-9.3]:high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748
[sap-ha-guide-9.4]:high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-ha-guide-9.5]:high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5
[sap-ha-guide-9.6]:high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772
[sap-ha-guide-10]:high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9
[sap-ha-guide-10.1]:high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445
[sap-ha-guide-10.2]:high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916
[sap-ha-guide-10.3]:high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5

[sap-ha-multi-sid-guide]:high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


[sap-ha-guide-figure-1000]:media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../../storage/common/storage-premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:../../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../azure-resource-manager/resource-group-overview.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../virtual-machines-windows-sizes.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


<span data-ttu-id="d2173-109">Virtuella Azure-datorer är hello lösning för organisationer som behöver bearbetning, lagring och nätverksresurser, minimalt, och utan långa inköp cykler.</span><span class="sxs-lookup"><span data-stu-id="d2173-109">Azure Virtual Machines is hello solution for organizations that need compute, storage, and network resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="d2173-110">Du kan använda Azure virtuella datorer toodeploy klassiska program som SAP NetWeaver-baserade ABAP, Java och en ABAP + Java-stacken.</span><span class="sxs-lookup"><span data-stu-id="d2173-110">You can use Azure Virtual Machines toodeploy classic applications like SAP NetWeaver-based ABAP, Java, and an ABAP+Java stack.</span></span> <span data-ttu-id="d2173-111">Utöka tillförlitlighet och tillgänglighet utan ytterligare lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="d2173-111">Extend reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="d2173-112">Virtuella Azure-datorer stöder korsanslutningar, så du kan integrera Azure virtuella datorer i din organisations lokala domäner, privata moln och SAP system liggande.</span><span class="sxs-lookup"><span data-stu-id="d2173-112">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="d2173-113">I den här artikeln beskriver vi hello stegen för att toodeploy SAP-system med hög tillgänglighet i Azure med hjälp av hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d2173-113">In this article, we cover hello steps that you can take toodeploy high-availability SAP systems in Azure by using hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="d2173-114">Vi går du igenom dessa viktiga uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d2173-114">We walk you through these major tasks:</span></span>

* <span data-ttu-id="d2173-115">Hitta hello rätt SAP anteckningar och installationen guider i hello [resurser] [ sap-ha-guide-2] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d2173-115">Find hello right SAP Notes and installation guides, listed in hello [Resources][sap-ha-guide-2] section.</span></span> <span data-ttu-id="d2173-116">Den här artikeln kompletterar SAP dokumentationen och SAP anteckningar, som är hello primära resurser som hjälper dig att installera och distribuera program på vissa plattformar.</span><span class="sxs-lookup"><span data-stu-id="d2173-116">This article complements SAP installation documentation and SAP Notes, which are hello primary resources that can help you install and deploy SAP software on specific platforms.</span></span>
* <span data-ttu-id="d2173-117">Lär dig hello skillnader mellan hello Azure Resource Manager-distributionsmodellen och hello Azure klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d2173-117">Learn hello differences between hello Azure Resource Manager deployment model and hello Azure classic deployment model.</span></span>
* <span data-ttu-id="d2173-118">Läs mer om Windows Server Failover Clustering kvorum läge, så du kan välja hello-modell som passar din Azure-distribution.</span><span class="sxs-lookup"><span data-stu-id="d2173-118">Learn about Windows Server Failover Clustering quorum modes, so you can select hello model that is right for your Azure deployment.</span></span>
* <span data-ttu-id="d2173-119">Läs mer om Windows Server Failover Clustering delad lagring i Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d2173-119">Learn about Windows Server Failover Clustering shared storage in Azure services.</span></span>
* <span data-ttu-id="d2173-120">Lär dig hur toohelp skydda enskild punkt för fel komponenter som avancerade Business Application Programming (ABAP) SAP centrala tjänster (ASCS) / SAP centrala Services (SCS) och databashanteringssystem (DBMS) och redundanta komponenter som SAP Programserver i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-120">Learn how toohelp protect single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server, in Azure.</span></span>
* <span data-ttu-id="d2173-121">Följ ett steg för steg-exempel på en installation och konfiguration av en SAP-systemet för hög tillgänglighet i ett redundanskluster i Windows Server-kluster i Azure med hjälp av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d2173-121">Follow a step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure by using Azure Resource Manager.</span></span>
* <span data-ttu-id="d2173-122">Mer information om ytterligare steg krävs toouse Windows Server Failover Clustering i Azure, men som inte behövs i en lokal distribution.</span><span class="sxs-lookup"><span data-stu-id="d2173-122">Learn about additional steps required toouse Windows Server Failover Clustering in Azure, but which are not needed in an on-premises deployment.</span></span>

<span data-ttu-id="d2173-123">toosimplify distribution och konfiguration i den här artikeln använder vi hello SAP tre skikt hög tillgänglighet Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="d2173-123">toosimplify deployment and configuration, in this article, we use hello SAP three-tier high-availability Resource Manager templates.</span></span> <span data-ttu-id="d2173-124">hello mallar automatisera distributionen av hello hela infrastrukturen som du behöver för ett SAP-system med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="d2173-124">hello templates automate deployment of hello entire infrastructure that you need for a high-availability SAP system.</span></span> <span data-ttu-id="d2173-125">hello-infrastrukturen stöder också SAP programmets prestanda Standard (SAP) storleksändring av SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-125">hello infrastructure also supports SAP Application Performance Standard (SAPS) sizing of your SAP system.</span></span>

## <span data-ttu-id="d2173-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="d2173-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Prerequisites</span></span>
<span data-ttu-id="d2173-127">Innan du börjar bör du kontrollera att du uppfyller hello krav som beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="d2173-127">Before you start, make sure that you meet hello prerequisites that are described in hello following sections.</span></span> <span data-ttu-id="d2173-128">Dessutom vara säker på att toocheck visas alla resurser i hello [resurser] [ sap-ha-guide-2] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d2173-128">Also, be sure toocheck all resources listed in hello [Resources][sap-ha-guide-2] section.</span></span>

<span data-ttu-id="d2173-129">I den här artikeln använder vi Azure Resource Manager-mallar för [trelagers-SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span><span class="sxs-lookup"><span data-stu-id="d2173-129">In this article, we use Azure Resource Manager templates for [three-tier SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span></span> <span data-ttu-id="d2173-130">En bra översikt över mallar finns [SAP Azure Resource Manager-mallar](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span><span class="sxs-lookup"><span data-stu-id="d2173-130">For a helpful overview of templates, see [SAP Azure Resource Manager templates](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span></span>

## <span data-ttu-id="d2173-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Resurser</span><span class="sxs-lookup"><span data-stu-id="d2173-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources</span></span>
<span data-ttu-id="d2173-132">Dessa artiklar beskriver SAP-distributioner i Azure:</span><span class="sxs-lookup"><span data-stu-id="d2173-132">These articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="d2173-133">[Azure virtuella datorer planering och implementering för SAP NetWeaver][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="d2173-133">[Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide]</span></span>
* <span data-ttu-id="d2173-134">[Distribution av Azure virtuella datorer för SAP NetWeaver][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="d2173-134">[Azure Virtual Machines deployment for SAP NetWeaver][deployment-guide]</span></span>
* <span data-ttu-id="d2173-135">[Azure virtuella datorer DBMS-distribution för SAP NetWeaver][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="d2173-135">[Azure Virtual Machines DBMS deployment for SAP NetWeaver][dbms-guide]</span></span>
* <span data-ttu-id="d2173-136">[Azure virtuella datorer hög tillgänglighet för SAP NetWeaver (den här guiden)][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="d2173-136">[Azure Virtual Machines high availability for SAP NetWeaver (this guide)][sap-ha-guide]</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-137">När det är möjligt, får du en länk toohello hänvisar SAP installationsguiden (se hello [SAP installationen guider][sap-installation-guides]).</span><span class="sxs-lookup"><span data-stu-id="d2173-137">Whenever possible, we give you a link toohello referring SAP installation guide (see hello [SAP installation guides][sap-installation-guides]).</span></span> <span data-ttu-id="d2173-138">Krav och information om hello installationsprocessen, det är en bra idé tooread hello SAP NetWeaver installation hjälper noggrant.</span><span class="sxs-lookup"><span data-stu-id="d2173-138">For prerequisites and information about hello installation process, it's a good idea tooread hello SAP NetWeaver installation guides carefully.</span></span> <span data-ttu-id="d2173-139">Den här artikeln beskriver endast specifika uppgifter för SAP NetWeaver-baserade datorer som du kan använda med Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="d2173-139">This article covers only specific tasks for SAP NetWeaver-based systems that you can use with Azure Virtual Machines.</span></span>
>
>

<span data-ttu-id="d2173-140">Anteckningarna SAP är relaterade toohello avsnittet SAP i Azure:</span><span class="sxs-lookup"><span data-stu-id="d2173-140">These SAP Notes are related toohello topic of SAP in Azure:</span></span>

| <span data-ttu-id="d2173-141">Nummer</span><span class="sxs-lookup"><span data-stu-id="d2173-141">Note number</span></span> | <span data-ttu-id="d2173-142">Rubrik</span><span class="sxs-lookup"><span data-stu-id="d2173-142">Title</span></span> |
| --- | --- |
| <span data-ttu-id="d2173-143">[1928533]</span><span class="sxs-lookup"><span data-stu-id="d2173-143">[1928533]</span></span> |<span data-ttu-id="d2173-144">SAP-program i Azure: produkter och storlek som stöds</span><span class="sxs-lookup"><span data-stu-id="d2173-144">SAP Applications on Azure: Supported Products and Sizing</span></span> |
| <span data-ttu-id="d2173-145">[2015553]</span><span class="sxs-lookup"><span data-stu-id="d2173-145">[2015553]</span></span> |<span data-ttu-id="d2173-146">SAP på Microsoft Azure: stöd för krav</span><span class="sxs-lookup"><span data-stu-id="d2173-146">SAP on Microsoft Azure: Support Prerequisites</span></span> |
| <span data-ttu-id="d2173-147">[1999351]</span><span class="sxs-lookup"><span data-stu-id="d2173-147">[1999351]</span></span> |<span data-ttu-id="d2173-148">Förbättrad Azure övervakning för SAP</span><span class="sxs-lookup"><span data-stu-id="d2173-148">Enhanced Azure Monitoring for SAP</span></span> |
| <span data-ttu-id="d2173-149">[2178632]</span><span class="sxs-lookup"><span data-stu-id="d2173-149">[2178632]</span></span> |<span data-ttu-id="d2173-150">Key som övervakning mått för SAP på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="d2173-150">Key Monitoring Metrics for SAP on Microsoft Azure</span></span> |
| <span data-ttu-id="d2173-151">[1999351]</span><span class="sxs-lookup"><span data-stu-id="d2173-151">[1999351]</span></span> |<span data-ttu-id="d2173-152">Virtualisering på Windows: förbättrad övervakning</span><span class="sxs-lookup"><span data-stu-id="d2173-152">Virtualization on Windows: Enhanced Monitoring</span></span> |
| <span data-ttu-id="d2173-153">[2243692]</span><span class="sxs-lookup"><span data-stu-id="d2173-153">[2243692]</span></span> |<span data-ttu-id="d2173-154">Användning av Azure Premium SSD-lagring för SAP DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-154">Use of Azure Premium SSD Storage for SAP DBMS Instance</span></span> |

<span data-ttu-id="d2173-155">Mer information om hello [begränsningar i Azure-prenumerationer][azure-subscription-service-limits-subscription], inklusive allmänna standardbegränsningar och maximala begränsningar.</span><span class="sxs-lookup"><span data-stu-id="d2173-155">Learn more about hello [limitations of Azure subscriptions][azure-subscription-service-limits-subscription], including general default limitations and maximum limitations.</span></span>

## <span data-ttu-id="d2173-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Hög tillgänglighet SAP med Azure Resource Manager kontra hello Azure klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="d2173-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>High-availability SAP with Azure Resource Manager vs. hello Azure classic deployment model</span></span>
<span data-ttu-id="d2173-157">hello Azure Resource Manager och Azure klassiska distributionsmodeller är olika i hello följande områden:</span><span class="sxs-lookup"><span data-stu-id="d2173-157">hello Azure Resource Manager and Azure classic deployment models are different in hello following areas:</span></span>

- <span data-ttu-id="d2173-158">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="d2173-158">Resource groups</span></span>
- <span data-ttu-id="d2173-159">Azure interna läsa in belastningsutjämning Beroende hello Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d2173-159">Azure internal load balancer dependency on hello Azure resource group</span></span>
- <span data-ttu-id="d2173-160">Stöd för scenarier för SAP multi-SID</span><span class="sxs-lookup"><span data-stu-id="d2173-160">Support for SAP multi-SID scenarios</span></span>

### <span data-ttu-id="d2173-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="d2173-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resource groups</span></span>
<span data-ttu-id="d2173-162">I Azure Resource Manager, du kan använda resursen grupper toomanage alla hello-resurser i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d2173-162">In Azure Resource Manager, you can use resource groups toomanage all hello application resources in your Azure subscription.</span></span> <span data-ttu-id="d2173-163">En integrerad lösning i en resursgrupp, alla resurser har hello samma livscykel.</span><span class="sxs-lookup"><span data-stu-id="d2173-163">An integrated approach, in a resource group, all resources have hello same life cycle.</span></span> <span data-ttu-id="d2173-164">Till exempel alla resurser skapas på hello samma tid och de tas bort i hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d2173-164">For example, all resources are created at hello same time and they are deleted at hello same time.</span></span> <span data-ttu-id="d2173-165">Läs mer om [resursgrupper](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="d2173-165">Learn more about [resource groups](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

### <span data-ttu-id="d2173-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure interna läsa in belastningsutjämning Beroende hello Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d2173-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure internal load balancer dependency on hello Azure resource group</span></span>

<span data-ttu-id="d2173-167">Hello Azure klassiska distributionsmodellen finns det ett beroende mellan hello Azure intern belastningsutjämnare (Azure belastningsutjämnare service) och hello tjänstgruppen för molnet.</span><span class="sxs-lookup"><span data-stu-id="d2173-167">In hello Azure classic deployment model, there is a dependency between hello Azure internal load balancer (Azure Load Balancer service) and hello cloud service group.</span></span> <span data-ttu-id="d2173-168">Varje intern belastningsutjämnare måste en cloud service-grupp.</span><span class="sxs-lookup"><span data-stu-id="d2173-168">Every internal load balancer needs one cloud service group.</span></span>

<span data-ttu-id="d2173-169">Azure Resource Manager behöver du inte en Azure-resurs grupp toouse Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d2173-169">In Azure Resource Manager, you don't need an Azure resource group toouse Azure Load Balancer.</span></span> <span data-ttu-id="d2173-170">hello-miljö är enklare och mer flexibelt.</span><span class="sxs-lookup"><span data-stu-id="d2173-170">hello environment is simpler and more flexible.</span></span>

### <a name="support-for-sap-multi-sid-scenarios"></a><span data-ttu-id="d2173-171">Stöd för scenarier för SAP multi-SID</span><span class="sxs-lookup"><span data-stu-id="d2173-171">Support for SAP multi-SID scenarios</span></span>

<span data-ttu-id="d2173-172">Du kan installera flera SAP system-ID (SID) ASCS/SCS instanser i ett kluster i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d2173-172">In Azure Resource Manager, you can install multiple SAP system identifier (SID) ASCS/SCS instances in one cluster.</span></span> <span data-ttu-id="d2173-173">Multi-SID-instanserna är möjligt tack vare stöd för flera IP-adresser för varje Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d2173-173">Multi-SID instances are possible because of support for multiple IP addresses for each Azure internal load balancer.</span></span>

<span data-ttu-id="d2173-174">toouse hello Azure klassiska distributionsmodellen, följ hello procedurerna som beskrivs i [SAP NetWeaver i Azure: Clustering SAP ASCS/SCS instanser med hjälp av Windows Server Failover Clustering i Azure med SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span><span class="sxs-lookup"><span data-stu-id="d2173-174">toouse hello Azure classic deployment model, follow hello procedures described in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2173-175">Vi rekommenderar starkt att du använder hello Azure Resource Manager-modellen för SAP-installationer.</span><span class="sxs-lookup"><span data-stu-id="d2173-175">We strongly recommend that you use hello Azure Resource Manager deployment model for your SAP installations.</span></span> <span data-ttu-id="d2173-176">Den erbjuder många fördelar som inte är tillgängliga i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d2173-176">It offers many benefits that are not available in hello classic deployment model.</span></span> <span data-ttu-id="d2173-177">Lär dig mer om Azure [distributionsmodeller][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span><span class="sxs-lookup"><span data-stu-id="d2173-177">Learn more about Azure [deployment models][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span></span>   
>
>

## <span data-ttu-id="d2173-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover-kluster</span><span class="sxs-lookup"><span data-stu-id="d2173-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Failover Clustering</span></span>
<span data-ttu-id="d2173-179">Windows Server Failover Clustering är hello grunden för en SAP ASCS/SCS-installation för hög tillgänglighet och DBMS i Windows.</span><span class="sxs-lookup"><span data-stu-id="d2173-179">Windows Server Failover Clustering is hello foundation of a high-availability SAP ASCS/SCS installation and DBMS in Windows.</span></span>

<span data-ttu-id="d2173-180">Ett redundanskluster är en grupp 1 + n oberoende servrar (noder) som arbetar tillsammans för tooincrease hello tillgängligheten för program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="d2173-180">A failover cluster is a group of 1+n independent servers (nodes) that work together tooincrease hello availability of applications and services.</span></span> <span data-ttu-id="d2173-181">Om det inträffar ett nodfel, Windows Server Failover Clustering beräknar hello antalet fel som kan uppstå samtidigt som en felfri kluster tooprovide program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="d2173-181">If a node failure occurs, Windows Server Failover Clustering calculates hello number of failures that can occur while maintaining a healthy cluster tooprovide applications and services.</span></span> <span data-ttu-id="d2173-182">Du kan välja mellan olika kvorum lägen tooachieve failover-kluster.</span><span class="sxs-lookup"><span data-stu-id="d2173-182">You can choose from different quorum modes tooachieve failover clustering.</span></span>

### <span data-ttu-id="d2173-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Kvorumlägen</span><span class="sxs-lookup"><span data-stu-id="d2173-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum modes</span></span>
<span data-ttu-id="d2173-184">Du kan välja mellan fyra kvorumlägen när du använder Windows Server Failover Clustering:</span><span class="sxs-lookup"><span data-stu-id="d2173-184">You can choose from four quorum modes when you use Windows Server Failover Clustering:</span></span>

* <span data-ttu-id="d2173-185">**Nodmajoritet**.</span><span class="sxs-lookup"><span data-stu-id="d2173-185">**Node Majority**.</span></span> <span data-ttu-id="d2173-186">Varje nod i klustret hello kan rösta.</span><span class="sxs-lookup"><span data-stu-id="d2173-186">Each node of hello cluster can vote.</span></span> <span data-ttu-id="d2173-187">hello klustret fungerar bara med en majoritet av röster, det vill säga med mer än hälften hello röster.</span><span class="sxs-lookup"><span data-stu-id="d2173-187">hello cluster functions only with a majority of votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="d2173-188">Vi rekommenderar att det här alternativet för kluster som har ett ojämnt antal noder.</span><span class="sxs-lookup"><span data-stu-id="d2173-188">We recommend this option for clusters that have an uneven number of nodes.</span></span> <span data-ttu-id="d2173-189">Till exempel tre noder i ett kluster med sju noder kan misslyckas och hello klustret stillbilder uppnår en majoritet och fortsätter toorun.</span><span class="sxs-lookup"><span data-stu-id="d2173-189">For example, three nodes in a seven-node cluster can fail, and hello cluster stills achieves a majority and continues toorun.</span></span>  
* <span data-ttu-id="d2173-190">**Nod- och**.</span><span class="sxs-lookup"><span data-stu-id="d2173-190">**Node and Disk Majority**.</span></span> <span data-ttu-id="d2173-191">Varje nod och en angiven disk (ett diskvittne) i hello klusterlagringen kan rösta när de är tillgängliga och kommunikation.</span><span class="sxs-lookup"><span data-stu-id="d2173-191">Each node and a designated disk (a disk witness) in hello cluster storage can vote when they are available and in communication.</span></span> <span data-ttu-id="d2173-192">hello klustret fungerar bara med en majoritet av hello röster som är med mer än hälften hello röster.</span><span class="sxs-lookup"><span data-stu-id="d2173-192">hello cluster functions only with a majority of hello votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="d2173-193">Det här läget är meningsfullt i en klustermiljö med ett jämnt antal noder.</span><span class="sxs-lookup"><span data-stu-id="d2173-193">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="d2173-194">Om hälften hello noder och hello disken är online, förblir hello klustret i felfritt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d2173-194">If half hello nodes and hello disk are online, hello cluster remains in a healthy state.</span></span>
* <span data-ttu-id="d2173-195">**Nod- och filresurs majoritet**.</span><span class="sxs-lookup"><span data-stu-id="d2173-195">**Node and File Share Majority**.</span></span> <span data-ttu-id="d2173-196">Varje nod plus en avsedda filresurs (ett filresursvittne) som Hej administratör skapar kan rösta, oavsett om hello noder och filresursen är tillgänglig och kommunikation.</span><span class="sxs-lookup"><span data-stu-id="d2173-196">Each node plus a designated file share (a file share witness) that hello administrator creates can vote, regardless of whether hello nodes and file share are available and in communication.</span></span> <span data-ttu-id="d2173-197">hello klustret fungerar bara med en majoritet av hello röster som är med mer än hälften hello röster.</span><span class="sxs-lookup"><span data-stu-id="d2173-197">hello cluster functions only with a majority of hello votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="d2173-198">Det här läget är meningsfullt i en klustermiljö med ett jämnt antal noder.</span><span class="sxs-lookup"><span data-stu-id="d2173-198">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="d2173-199">Det är liknande toohello nod- och diskmajoritet läge, men den använder en filresurs vittne i stället för en vittnesdisk.</span><span class="sxs-lookup"><span data-stu-id="d2173-199">It's similar toohello Node and Disk Majority mode, but it uses a witness file share instead of a witness disk.</span></span> <span data-ttu-id="d2173-200">Det här läget är enkelt tooimplement, men om hello filresursen själva har inte hög tillgänglighet, så kan det bli en enskild felpunkt.</span><span class="sxs-lookup"><span data-stu-id="d2173-200">This mode is easy tooimplement, but if hello file share itself is not highly available, it might become a single point of failure.</span></span>
* <span data-ttu-id="d2173-201">**Ingen majoritet: Endast Disk**.</span><span class="sxs-lookup"><span data-stu-id="d2173-201">**No Majority: Disk Only**.</span></span> <span data-ttu-id="d2173-202">hello-klustret har ett kvorum om en nod är tillgänglig och kommunikation med en viss disk i klusterlagringen hello.</span><span class="sxs-lookup"><span data-stu-id="d2173-202">hello cluster has a quorum if one node is available and in communication with a specific disk in hello cluster storage.</span></span> <span data-ttu-id="d2173-203">Endast hello-noder som också är vid kommunikation med disken kan gå hello klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-203">Only hello nodes that also are in communication with that disk can join hello cluster.</span></span> <span data-ttu-id="d2173-204">Vi rekommenderar att du inte använder det här läget.</span><span class="sxs-lookup"><span data-stu-id="d2173-204">We recommend that you do not use this mode.</span></span>
 

## <span data-ttu-id="d2173-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server Failover-kluster lokalt</span><span class="sxs-lookup"><span data-stu-id="d2173-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Server Failover Clustering on-premises</span></span>
<span data-ttu-id="d2173-206">Bild 1 visar ett kluster med två noder.</span><span class="sxs-lookup"><span data-stu-id="d2173-206">Figure 1 shows a cluster of two nodes.</span></span> <span data-ttu-id="d2173-207">Om hello nätverksanslutningen mellan hello noder misslyckas och båda noderna fortsätta och körs, ett kvorum disk eller filresurs avgör vilken nod fortsätter tooprovide hello klustret program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="d2173-207">If hello network connection between hello nodes fails and both nodes stay up and running, a quorum disk or file share determines which node will continue tooprovide hello cluster's applications and services.</span></span> <span data-ttu-id="d2173-208">hello-noden som har åtkomst toohello kvorumdisken eller filresurs är hello-noden som säkerställer att tjänster fortsätter.</span><span class="sxs-lookup"><span data-stu-id="d2173-208">hello node that has access toohello quorum disk or file share is hello node that ensures that services continue.</span></span>

<span data-ttu-id="d2173-209">Eftersom det här exemplet använder ett kluster med två noder kan använda vi hello nod och filresursmajoritet quorum-läget.</span><span class="sxs-lookup"><span data-stu-id="d2173-209">Because this example uses a two-node cluster, we use hello Node and File Share Majority quorum mode.</span></span> <span data-ttu-id="d2173-210">hello nod- och diskmajoritet är också ett giltigt alternativ.</span><span class="sxs-lookup"><span data-stu-id="d2173-210">hello Node and Disk Majority also is a valid option.</span></span> <span data-ttu-id="d2173-211">I en produktionsmiljö rekommenderar vi att du använder en kvorumdisk.</span><span class="sxs-lookup"><span data-stu-id="d2173-211">In a production environment, we recommend that you use a quorum disk.</span></span> <span data-ttu-id="d2173-212">Du kan använda Nätverks- och system teknik toomake den hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="d2173-212">You can use network and storage system technology toomake it highly available.</span></span>

![Bild 1: Exempel på en Windows Server Failover Clustering konfiguration för SAP ASCS/SCS i Azure][sap-ha-guide-figure-1000]

<span data-ttu-id="d2173-214">_**Bild 1:** exempel på en Windows Server Failover Clustering konfiguration för SAP ASCS/SCS i Azure_</span><span class="sxs-lookup"><span data-stu-id="d2173-214">_**Figure 1:** Example of a Windows Server Failover Clustering configuration for SAP ASCS/SCS in Azure_</span></span>

### <span data-ttu-id="d2173-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Delad lagring</span><span class="sxs-lookup"><span data-stu-id="d2173-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Shared storage</span></span>
<span data-ttu-id="d2173-216">Bild 1 visar också ett kluster med två noder delad lagring.</span><span class="sxs-lookup"><span data-stu-id="d2173-216">Figure 1 also shows a two-node shared storage cluster.</span></span> <span data-ttu-id="d2173-217">I ett kluster för lokala delad lagring identifiera alla noder i klustret hello delad lagring.</span><span class="sxs-lookup"><span data-stu-id="d2173-217">In an on-premises shared storage cluster, all nodes in hello cluster detect shared storage.</span></span> <span data-ttu-id="d2173-218">En mekanism för låsning skyddar hello data från skador.</span><span class="sxs-lookup"><span data-stu-id="d2173-218">A locking mechanism protects hello data from corruption.</span></span> <span data-ttu-id="d2173-219">Alla noder kan känna av om en annan nod misslyckas.</span><span class="sxs-lookup"><span data-stu-id="d2173-219">All nodes can detect if another node fails.</span></span> <span data-ttu-id="d2173-220">Om en nod misslyckas hello kvarvarande noden ägarskapet till hello lagringsresurser och garanterar hello tillgängligheten för tjänster.</span><span class="sxs-lookup"><span data-stu-id="d2173-220">If one node fails, hello remaining node takes ownership of hello storage resources and ensures hello availability of services.</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-221">Du behöver inte delade diskar för hög tillgänglighet med vissa DBMS-program, precis som med SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d2173-221">You don't need shared disks for high availability with some DBMS applications, like with SQL Server.</span></span> <span data-ttu-id="d2173-222">SQL Server alltid aktiverad replikerar DBMS data och loggfilen filer från hello lokal disk i ett kluster nod toohello lokal disk av en annan klusternod.</span><span class="sxs-lookup"><span data-stu-id="d2173-222">SQL Server Always On replicates DBMS data and log files from hello local disk of one cluster node toohello local disk of another cluster node.</span></span> <span data-ttu-id="d2173-223">I så fall behöver hello Windows klusterkonfigurationen inte en delad disk.</span><span class="sxs-lookup"><span data-stu-id="d2173-223">In that case, hello Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="d2173-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Nätverk och namnmatchning</span><span class="sxs-lookup"><span data-stu-id="d2173-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Networking and name resolution</span></span>
<span data-ttu-id="d2173-225">Klientdatorer når hello klustret via en virtuell IP-adress och ett virtuellt värdnamn som hello DNS-servern ger.</span><span class="sxs-lookup"><span data-stu-id="d2173-225">Client computers reach hello cluster over a virtual IP address and a virtual host name that hello DNS server provides.</span></span> <span data-ttu-id="d2173-226">hello lokalt noder och hello DNS-server kan hantera flera IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d2173-226">hello on-premises nodes and hello DNS server can handle multiple IP addresses.</span></span>

<span data-ttu-id="d2173-227">I en typisk konfiguration kan du använda två eller flera nätverksanslutningar:</span><span class="sxs-lookup"><span data-stu-id="d2173-227">In a typical setup, you use two or more network connections:</span></span>

* <span data-ttu-id="d2173-228">En dedicerad anslutning toohello lagring</span><span class="sxs-lookup"><span data-stu-id="d2173-228">A dedicated connection toohello storage</span></span>
* <span data-ttu-id="d2173-229">En kluster-interna nätverksanslutning för pulsslag hello</span><span class="sxs-lookup"><span data-stu-id="d2173-229">A cluster-internal network connection for hello heartbeat</span></span>
* <span data-ttu-id="d2173-230">Ett offentligt nätverk som klienter använder tooconnect toohello kluster</span><span class="sxs-lookup"><span data-stu-id="d2173-230">A public network that clients use tooconnect toohello cluster</span></span>

## <span data-ttu-id="d2173-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="d2173-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="d2173-232">Jämfört med distribution för toobare operativsystem eller privata moln, virtuella datorer i Azure kräver ytterligare åtgärder tooconfigure Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="d2173-232">Compared toobare metal or private cloud deployments, Azure Virtual Machines requires additional steps tooconfigure Windows Server Failover Clustering.</span></span> <span data-ttu-id="d2173-233">När du skapar en delad klusterdisk måste tooset flera IP-adresser och virtuella värd namn för hello SAP ASCS/SCS-instansen.</span><span class="sxs-lookup"><span data-stu-id="d2173-233">When you build a shared cluster disk, you need tooset several IP addresses and virtual host names for hello SAP ASCS/SCS instance.</span></span>

<span data-ttu-id="d2173-234">I den här artikeln vi beskrivs viktiga begrepp och hello ytterligare steg krävs toobuild ett SAP centrala tjänster med hög tillgänglighet kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-234">In this article, we discuss key concepts and hello additional steps required toobuild an SAP high-availability central services cluster in Azure.</span></span> <span data-ttu-id="d2173-235">Vi visar hur tooset in hello tredjepartsverktyg SIOS DataKeeper och hur tooconfigure hello Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d2173-235">We show you how tooset up hello third-party tool SIOS DataKeeper, and how tooconfigure hello Azure internal load balancer.</span></span> <span data-ttu-id="d2173-236">Du kan använda dessa verktyg toocreate ett redundanskluster i Windows med ett filresursvittne i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-236">You can use these tools toocreate a Windows failover cluster with a file share witness in Azure.</span></span>

![Bild 2: Windows Server Failover Clustering konfiguration i Azure utan en delad disk][sap-ha-guide-figure-1001]

<span data-ttu-id="d2173-238">_**Bild 2:** Windows Server Failover Clustering konfiguration i Azure utan en delad disk_</span><span class="sxs-lookup"><span data-stu-id="d2173-238">_**Figure 2:** Windows Server Failover Clustering configuration in Azure without a shared disk_</span></span>

### <span data-ttu-id="d2173-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Delad disk i Azure med SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="d2173-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Shared disk in Azure with SIOS DataKeeper</span></span>
<span data-ttu-id="d2173-240">Du måste ingå i klustret delad lagring för en SAP ASCS/SCS-instans med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="d2173-240">You need cluster shared storage for a high-availability SAP ASCS/SCS instance.</span></span> <span data-ttu-id="d2173-241">Som i September 2016 Azure inte erbjuder delad lagring som kan du använda toocreate ett kluster med delad lagring.</span><span class="sxs-lookup"><span data-stu-id="d2173-241">As of September 2016, Azure doesn't offer shared storage that you can use toocreate a shared storage cluster.</span></span> <span data-ttu-id="d2173-242">Du kan använda programvara från tredje part SIOS DataKeeper Cluster Edition toocreate en speglad lagring som simulerar delad klusterlagring.</span><span class="sxs-lookup"><span data-stu-id="d2173-242">You can use third-party software SIOS DataKeeper Cluster Edition toocreate a mirrored storage that simulates cluster shared storage.</span></span> <span data-ttu-id="d2173-243">Hej SIOS lösningen innehåller data i realtid synkron replikering.</span><span class="sxs-lookup"><span data-stu-id="d2173-243">hello SIOS solution provides real-time synchronous data replication.</span></span> <span data-ttu-id="d2173-244">Detta beror på hur du kan skapa en delad diskresurs för ett kluster:</span><span class="sxs-lookup"><span data-stu-id="d2173-244">This is how you can create a shared disk resource for a cluster:</span></span>

1. <span data-ttu-id="d2173-245">Koppla en Azure virtuell hårddisk (VHD) tooeach hello virtuella datorer (VM) i en Windows-klusterkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d2173-245">Attach an additional Azure virtual hard disk (VHD) tooeach of hello virtual machines (VMs) in a Windows cluster configuration.</span></span>
2. <span data-ttu-id="d2173-246">Kör SIOS DataKeeper Cluster Edition på båda noderna för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2173-246">Run SIOS DataKeeper Cluster Edition on both virtual machine nodes.</span></span>
3. <span data-ttu-id="d2173-247">Konfigurera SIOS DataKeeper Cluster Edition så att den speglar hello innehållet i hello ytterligare virtuell Hårddisk ansluten volym från hello virtuella toohello ytterligare virtuell Hårddisk ansluten källvolymen av hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="d2173-247">Configure SIOS DataKeeper Cluster Edition so that it mirrors hello content of hello additional VHD attached volume from hello source virtual machine toohello additional VHD attached volume of hello target virtual machine.</span></span> <span data-ttu-id="d2173-248">SIOS DataKeeper avlägsnar hello käll- och lokala volymer och visar dem sedan tooWindows Server Failover Clustering som en delad disk.</span><span class="sxs-lookup"><span data-stu-id="d2173-248">SIOS DataKeeper abstracts hello source and target local volumes, and then presents them tooWindows Server Failover Clustering as one shared disk.</span></span>

<span data-ttu-id="d2173-249">Hämta mer information [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span><span class="sxs-lookup"><span data-stu-id="d2173-249">Get more information about [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span></span>

![Bild 3: Windows Server Failover Clustering konfigurationen i Azure med SIOS DataKeeper][sap-ha-guide-figure-1002]

<span data-ttu-id="d2173-251">_**Bild 3:** Windows Server Failover Clustering konfigurationen i Azure med SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="d2173-251">_**Figure 3:** Windows Server Failover Clustering configuration in Azure with SIOS DataKeeper_</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-252">Du behöver inte delade diskar för hög tillgänglighet med vissa DBMS-produkter, t.ex. SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d2173-252">You don't need shared disks for high availability with some DBMS products, like SQL Server.</span></span> <span data-ttu-id="d2173-253">SQL Server alltid aktiverad replikerar DBMS data och loggfilen filer från hello lokal disk i ett kluster nod toohello lokal disk av en annan klusternod.</span><span class="sxs-lookup"><span data-stu-id="d2173-253">SQL Server Always On replicates DBMS data and log files from hello local disk of one cluster node toohello local disk of another cluster node.</span></span> <span data-ttu-id="d2173-254">I det här fallet behöver hello Windows klusterkonfigurationen inte en delad disk.</span><span class="sxs-lookup"><span data-stu-id="d2173-254">In this case, hello Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="d2173-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Namnmatchning i Azure</span><span class="sxs-lookup"><span data-stu-id="d2173-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Name resolution in Azure</span></span>
<span data-ttu-id="d2173-256">hello Azure molnplattform erbjuder inte hello alternativet tooconfigure virtuella IP-adresser, till exempel flytande IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d2173-256">hello Azure cloud platform doesn't offer hello option tooconfigure virtual IP addresses, such as floating IP addresses.</span></span> <span data-ttu-id="d2173-257">Du måste en alternativ lösning tooset upp en virtuell IP-adress tooreach hello klusterresurs i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="d2173-257">You need an alternative solution tooset up a virtual IP address tooreach hello cluster resource in hello cloud.</span></span>
<span data-ttu-id="d2173-258">Azure har en intern belastningsutjämnare i hello Azure belastningsutjämnare service.</span><span class="sxs-lookup"><span data-stu-id="d2173-258">Azure has an internal load balancer in hello Azure Load Balancer service.</span></span> <span data-ttu-id="d2173-259">Med hello intern belastningsutjämnare nå klienter hello klustret via hello klustrets virtuella IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d2173-259">With hello internal load balancer, clients reach hello cluster over hello cluster virtual IP address.</span></span>
<span data-ttu-id="d2173-260">Du måste toodeploy hello interna belastningsutjämnare i hello resursgruppen som innehåller hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="d2173-260">You need toodeploy hello internal load balancer in hello resource group that contains hello cluster nodes.</span></span> <span data-ttu-id="d2173-261">Konfigurera sedan alla nödvändiga port vidarebefordran regler med hello avsökningen portar för hello intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d2173-261">Then, configure all necessary port forwarding rules with hello probe ports of hello internal load balancer.</span></span>
<span data-ttu-id="d2173-262">hello-klienter kan ansluta via hello virtuellt värdnamn.</span><span class="sxs-lookup"><span data-stu-id="d2173-262">hello clients can connect via hello virtual host name.</span></span> <span data-ttu-id="d2173-263">hello DNS-server löser hello klustrets IP-adress och hello interna belastningsutjämnare handtag port vidarebefordran toohello aktiva noden i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-263">hello DNS server resolves hello cluster IP address, and hello internal load balancer handles port forwarding toohello active node of hello cluster.</span></span>

## <span data-ttu-id="d2173-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver hög tillgänglighet i Azure Infrastructure-as-a-Service (IaaS)</span><span class="sxs-lookup"><span data-stu-id="d2173-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver high availability in Azure Infrastructure-as-a-Service (IaaS)</span></span>
<span data-ttu-id="d2173-265">tooachieve SAP-program med hög tillgänglighet som för SAP programvarukomponenter måste tooprotect hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="d2173-265">tooachieve SAP application high availability, such as for SAP software components, you need tooprotect hello following components:</span></span>

* <span data-ttu-id="d2173-266">SAP Application Server-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-266">SAP Application Server instance</span></span>
* <span data-ttu-id="d2173-267">SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-267">SAP ASCS/SCS instance</span></span>
* <span data-ttu-id="d2173-268">DBMS-server</span><span class="sxs-lookup"><span data-stu-id="d2173-268">DBMS server</span></span>

<span data-ttu-id="d2173-269">Mer information om hur du skyddar SAP komponenter i scenarier med hög tillgänglighet finns [Azure Virtual Machines planering och implementering för SAP NetWeaver](planning-guide.md).</span><span class="sxs-lookup"><span data-stu-id="d2173-269">For more information about protecting SAP components in high-availability scenarios, see [Azure Virtual Machines planning and implementation for SAP NetWeaver](planning-guide.md).</span></span>

### <span data-ttu-id="d2173-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Hög tillgänglighet SAP-programserver</span><span class="sxs-lookup"><span data-stu-id="d2173-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> High-availability SAP Application Server</span></span>
<span data-ttu-id="d2173-271">Vanligtvis behöver du inte en specifik lösning för hög tillgänglighet för hello SAP-programserver och dialogrutan instanser.</span><span class="sxs-lookup"><span data-stu-id="d2173-271">You usually don't need a specific high-availability solution for hello SAP Application Server and dialog instances.</span></span> <span data-ttu-id="d2173-272">Du kan uppnå hög tillgänglighet med redundans och konfigurerar du dialogrutan alla instanser i olika instanser av virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-272">You achieve high availability by redundancy, and you'll configure multiple dialog instances in different instances of Azure Virtual Machines.</span></span> <span data-ttu-id="d2173-273">Du bör ha minst två instanser för SAP-program installeras i två instanser av virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-273">You should have at least two SAP application instances installed in two instances of Azure Virtual Machines.</span></span>

![Bild 4: Hög tillgänglighet SAP-programserver][sap-ha-guide-figure-2000]

<span data-ttu-id="d2173-275">_**Bild 4:** hög tillgänglighet SAP-programserver_</span><span class="sxs-lookup"><span data-stu-id="d2173-275">_**Figure 4:** High-availability SAP Application Server_</span></span>

<span data-ttu-id="d2173-276">Du måste placera alla virtuella datorer att hello värden SAP Application Server-instanser i samma Azure tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="d2173-276">You must place all virtual machines that host SAP Application Server instances in hello same Azure availability set.</span></span> <span data-ttu-id="d2173-277">En Azure tillgänglighetsuppsättning säkerställer att:</span><span class="sxs-lookup"><span data-stu-id="d2173-277">An Azure availability set ensures that:</span></span>

* <span data-ttu-id="d2173-278">Alla virtuella datorer är en del av hello samma domän.</span><span class="sxs-lookup"><span data-stu-id="d2173-278">All virtual machines are part of hello same upgrade domain.</span></span> <span data-ttu-id="d2173-279">En uppgraderingsdomän, till exempel ser till att hello virtuella datorer inte är uppdaterade på hello samtidigt under planerat underhåll driftstopp.</span><span class="sxs-lookup"><span data-stu-id="d2173-279">An upgrade domain, for example, makes sure that hello virtual machines aren't updated at hello same time during planned maintenance downtime.</span></span>
* <span data-ttu-id="d2173-280">Alla virtuella datorer är en del av hello samma feldomän.</span><span class="sxs-lookup"><span data-stu-id="d2173-280">All virtual machines are part of hello same fault domain.</span></span> <span data-ttu-id="d2173-281">En feldomän, till exempel ser till att virtuella datorer distribueras så att ingen enskild felpunkt påverkar hello tillgängligheten för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-281">A fault domain, for example, makes sure that virtual machines are deployed so that no single point of failure affects hello availability of all virtual machines.</span></span>

<span data-ttu-id="d2173-282">Läs mer om hur för[hantera hello tillgängligheten för virtuella datorer][virtual-machines-manage-availability].</span><span class="sxs-lookup"><span data-stu-id="d2173-282">Learn more about how too[manage hello availability of virtual machines][virtual-machines-manage-availability].</span></span>

<span data-ttu-id="d2173-283">Eftersom hello Azure storage-konto är en potentiell felpunkt, är det viktigt toohave minst två Azure storage-konton som minst två virtuella datorer distribueras.</span><span class="sxs-lookup"><span data-stu-id="d2173-283">Because hello Azure storage account is a potential single point of failure, it's important toohave at least two Azure storage accounts, in which at least two virtual machines are distributed.</span></span> <span data-ttu-id="d2173-284">I en perfekt installationsprogrammet skulle hello diskar för varje virtuell dator som kör en instans för SAP-dialogrutan distribueras i ett annat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d2173-284">In an ideal setup, hello disks of each virtual machine that is running an SAP dialog instance would be deployed in a different storage account.</span></span>

### <span data-ttu-id="d2173-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Hög tillgänglighet SAP ASCS/SCS instans</span><span class="sxs-lookup"><span data-stu-id="d2173-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> High-availability SAP ASCS/SCS instance</span></span>
<span data-ttu-id="d2173-286">Bild 5 är ett exempel på en hög tillgänglighet SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-286">Figure 5 is an example of a high-availability SAP ASCS/SCS instance.</span></span>

![Bild 5: Hög tillgänglighet SAP ASCS/SCS instans][sap-ha-guide-figure-2001]

<span data-ttu-id="d2173-288">_**Bild 5:** hög tillgänglighet SAP ASCS/SCS-instans_</span><span class="sxs-lookup"><span data-stu-id="d2173-288">_**Figure 5:** High-availability SAP ASCS/SCS instance_</span></span>

#### <span data-ttu-id="d2173-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASCS/SCS instans hög tillgänglighet med Windows Server Failover Clustering i Azure</span><span class="sxs-lookup"><span data-stu-id="d2173-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS instance high availability with Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="d2173-290">Jämfört med distribution för toobare operativsystem eller privata moln, virtuella datorer i Azure kräver ytterligare åtgärder tooconfigure Windows Server Failover Clustering.</span><span class="sxs-lookup"><span data-stu-id="d2173-290">Compared toobare metal or private cloud deployments, Azure Virtual Machines requires additional steps tooconfigure Windows Server Failover Clustering.</span></span> <span data-ttu-id="d2173-291">toobuild ett Windows-redundanskluster, behöver du en delad klusterdisk, flera IP-adresser, flera virtuella värdnamn och en Azure intern belastningsutjämnare för kluster för en SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-291">toobuild a Windows failover cluster, you need a shared cluster disk, several IP addresses, several virtual host names, and an Azure internal load balancer for clustering an SAP ASCS/SCS instance.</span></span> <span data-ttu-id="d2173-292">Detta diskuterar vi i detalj senare i hello artikeln.</span><span class="sxs-lookup"><span data-stu-id="d2173-292">We discuss this in more detail later in hello article.</span></span>

![Bild 6: Windows Server Failover Clustering för en SAP ASCS/SCS-konfiguration i Azure med hjälp av SIOS DataKeeper][sap-ha-guide-figure-1002]

<span data-ttu-id="d2173-294">_**Bild 6:** Windows Server Failover Clustering för en SAP ASCS/SCS-konfiguration i Azure med SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="d2173-294">_**Figure 6:** Windows Server Failover Clustering for an SAP ASCS/SCS configuration in Azure with SIOS DataKeeper_</span></span>

### <span data-ttu-id="d2173-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Hög tillgänglighet DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>High-availability DBMS instance</span></span>
<span data-ttu-id="d2173-296">hello DBMS är också en enda kontaktpunkt i ett SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-296">hello DBMS also is a single point of contact in an SAP system.</span></span> <span data-ttu-id="d2173-297">Du behöver tooprotect den med hjälp av en lösning för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="d2173-297">You need tooprotect it by using a high-availability solution.</span></span> <span data-ttu-id="d2173-298">Bild 7 visar en SQL Server alltid på hög tillgänglighet lösning i Azure, med Windows Server Failover Clustering och hello Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d2173-298">Figure 7 shows a SQL Server Always On high-availability solution in Azure, with Windows Server Failover Clustering and hello Azure internal load balancer.</span></span> <span data-ttu-id="d2173-299">SQL Server alltid aktiverad replikerar DBMS data och loggfilen filer med hjälp av sin egen DBMS-replikering.</span><span class="sxs-lookup"><span data-stu-id="d2173-299">SQL Server Always On replicates DBMS data and log files by using its own DBMS replication.</span></span> <span data-ttu-id="d2173-300">I detta fall kan behöver du inte kluster delade diskar som förenklar hello hela installationen.</span><span class="sxs-lookup"><span data-stu-id="d2173-300">In this case, you don't need cluster shared disks, which simplifies hello entire setup.</span></span>

![Bild 7: Exempel på en SAP-DBMS med hög tillgänglighet, med SQL Server alltid aktiverad][sap-ha-guide-figure-2003]

<span data-ttu-id="d2173-302">_**Bild 7:** exempel på ett SAP-DBMS med hög tillgänglighet, med SQL Server alltid aktiverad_</span><span class="sxs-lookup"><span data-stu-id="d2173-302">_**Figure 7:** Example of a high-availability SAP DBMS, with SQL Server Always On_</span></span>

<span data-ttu-id="d2173-303">Mer information om kluster och SQL Server i Azure med hjälp av hello Azure Resource Manager-distributionsmodellen finns i de här artiklarna:</span><span class="sxs-lookup"><span data-stu-id="d2173-303">For more information about clustering SQL Server in Azure by using hello Azure Resource Manager deployment model, see these articles:</span></span>

* <span data-ttu-id="d2173-304">[Konfigurera Always On-tillgänglighetsgrupp i Azure Virtual Machines manuellt med hjälp av hanteraren för filserverresurser][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span><span class="sxs-lookup"><span data-stu-id="d2173-304">[Configure Always On availability group in Azure Virtual Machines manually by using Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span></span>
* <span data-ttu-id="d2173-305">[Konfigurera en Azure intern belastningsutjämnare för en tillgänglighetsgrupp alltid på i Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span><span class="sxs-lookup"><span data-stu-id="d2173-305">[Configure an Azure internal load balancer for an Always On availability group in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span></span>

## <span data-ttu-id="d2173-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Distributionsscenarier för slutpunkt till slutpunkt hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="d2173-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end high-availability deployment scenarios</span></span>

### <a name="deployment-scenario-using-architectural-template-1"></a><span data-ttu-id="d2173-307">Scenario för distribution med arkitekturen mall 1</span><span class="sxs-lookup"><span data-stu-id="d2173-307">Deployment scenario using Architectural Template 1</span></span>

<span data-ttu-id="d2173-308">Bild 8 visar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **en** SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-308">Figure 8 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="d2173-309">Det här scenariot är inställd på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d2173-309">This scenario is set up as follows:</span></span>

- <span data-ttu-id="d2173-310">Ett dedikerat kluster används för hello SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-310">One dedicated cluster is used for hello SAP ASCS/SCS instance.</span></span>
- <span data-ttu-id="d2173-311">Ett dedikerat kluster används för hello DBMS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-311">One dedicated cluster is used for hello DBMS instance.</span></span>
- <span data-ttu-id="d2173-312">SAP Application Server-instanser har distribuerats i sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-312">SAP Application Server instances are deployed in their own dedicated VMs.</span></span>

![Bild 8: SAP hög tillgänglighet arkitektur mall 1, med dedikerade kluster för ASCS/SCS och DBMS][sap-ha-guide-figure-2004]

<span data-ttu-id="d2173-314">_**Bild 8:** SAP hög tillgänglighet arkitektur mall 1 dedikerade kluster för ASCS/SCS och DBMS_</span><span class="sxs-lookup"><span data-stu-id="d2173-314">_**Figure 8:** SAP high-availability Architectural Template 1, dedicated clusters for ASCS/SCS and DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-2"></a><span data-ttu-id="d2173-315">Scenario för distribution med arkitekturen mall 2</span><span class="sxs-lookup"><span data-stu-id="d2173-315">Deployment scenario using Architectural Template 2</span></span>

<span data-ttu-id="d2173-316">Bild 9 illustrerar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **en** SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-316">Figure 9 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="d2173-317">Det här scenariot är inställd på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d2173-317">This scenario is set up as follows:</span></span>

- <span data-ttu-id="d2173-318">Ett dedikerat kluster används för **både** hello SAP ASCS/SCS instansen och hello DBMS.</span><span class="sxs-lookup"><span data-stu-id="d2173-318">One dedicated cluster is used for **both** hello SAP ASCS/SCS instance and hello DBMS.</span></span>
- <span data-ttu-id="d2173-319">SAP Application Server-instanser har distribuerats i egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-319">SAP Application Server instances are deployed in own dedicated VMs.</span></span>

![Bild 9: SAP hög tillgänglighet arkitektur mall 2, med ett kluster för ASCS/SCS och ett kluster för DBMS][sap-ha-guide-figure-2005]

<span data-ttu-id="d2173-321">_**Bild 9:** SAP hög tillgänglighet arkitektur mall 2, med ett kluster för ASCS/SCS och ett kluster för DBMS_</span><span class="sxs-lookup"><span data-stu-id="d2173-321">_**Figure 9:** SAP high-availability Architectural Template 2, with a dedicated cluster for ASCS/SCS and a dedicated cluster for DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-3"></a><span data-ttu-id="d2173-322">Scenario för distribution med arkitekturen mall 3</span><span class="sxs-lookup"><span data-stu-id="d2173-322">Deployment scenario using Architectural Template 3</span></span>

<span data-ttu-id="d2173-323">Bild 10 illustrerar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **två** SAP-system med &lt;SID1&gt; och &lt;SID2&gt;.</span><span class="sxs-lookup"><span data-stu-id="d2173-323">Figure 10 shows an example of an SAP NetWeaver high-availability architecture in Azure for **two** SAP systems, with &lt;SID1&gt; and &lt;SID2&gt;.</span></span> <span data-ttu-id="d2173-324">Det här scenariot är inställd på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d2173-324">This scenario is set up as follows:</span></span>

- <span data-ttu-id="d2173-325">Ett dedikerat kluster används för **både** hello SAP ASCS/SCS SID1 instans *och* hello SAP ASCS/SCS SID2 instansen (kluster).</span><span class="sxs-lookup"><span data-stu-id="d2173-325">One dedicated cluster is used for **both** hello SAP ASCS/SCS SID1 instance *and* hello SAP ASCS/SCS SID2 instance (one cluster).</span></span>
- <span data-ttu-id="d2173-326">Ett dedikerat kluster används för DBMS SID1 och ett annat dedikerade kluster används för DBMS SID2 (två kluster).</span><span class="sxs-lookup"><span data-stu-id="d2173-326">One dedicated cluster is used for DBMS SID1, and another dedicated cluster is used for DBMS SID2 (two clusters).</span></span>
- <span data-ttu-id="d2173-327">SAP Application Server-instanser för hello SAP system SID1 har sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-327">SAP Application Server instances for hello SAP system SID1 have their own dedicated VMs.</span></span>
- <span data-ttu-id="d2173-328">SAP Application Server-instanser för hello SAP system SID2 har sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-328">SAP Application Server instances for hello SAP system SID2 have their own dedicated VMs.</span></span>

![Figur 10: SAP hög tillgänglighet arkitektur mall 3, med ett kluster för olika ASCS/SCS-instanser][sap-ha-guide-figure-6003]

<span data-ttu-id="d2173-330">_**Figur 10:** SAP hög tillgänglighet arkitektur mall 3, med ett kluster för olika ASCS/SCS-instanser_</span><span class="sxs-lookup"><span data-stu-id="d2173-330">_**Figure 10:** SAP high-availability Architectural Template 3, with a dedicated cluster for different ASCS/SCS instances_</span></span>

## <span data-ttu-id="d2173-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Förbereda hello-infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="d2173-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Prepare hello infrastructure</span></span>

### <a name="prepare-hello-infrastructure-for-architectural-template-1"></a><span data-ttu-id="d2173-332">Förbereda hello infrastrukturen för arkitektur mall 1</span><span class="sxs-lookup"><span data-stu-id="d2173-332">Prepare hello infrastructure for Architectural Template 1</span></span>
<span data-ttu-id="d2173-333">Azure Resource Manager-mallar för SAP underlätta distributionen av de nödvändiga resurserna.</span><span class="sxs-lookup"><span data-stu-id="d2173-333">Azure Resource Manager templates for SAP help simplify deployment of required resources.</span></span>

<span data-ttu-id="d2173-334">Hej trelagers-mallar i Azure Resource Manager stöder också scenarier med hög tillgänglighet som i arkitekturen mall 1, som har två kluster.</span><span class="sxs-lookup"><span data-stu-id="d2173-334">hello three-tier templates in Azure Resource Manager also support high-availability scenarios, such as in Architectural Template 1, which has two clusters.</span></span> <span data-ttu-id="d2173-335">Varje kluster är ett SAP felpunkt för SAP ASCS/SCS och DBMS.</span><span class="sxs-lookup"><span data-stu-id="d2173-335">Each cluster is an SAP single point of failure for SAP ASCS/SCS and DBMS.</span></span>

<span data-ttu-id="d2173-336">Här är där du kan hämta Azure Resource Manager-mallar för hello exempelscenario som beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="d2173-336">Here's where you can get Azure Resource Manager templates for hello example scenario we describe in this article:</span></span>

* [<span data-ttu-id="d2173-337">Azure Marketplace-avbildning</span><span class="sxs-lookup"><span data-stu-id="d2173-337">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [<span data-ttu-id="d2173-338">Anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="d2173-338">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

<span data-ttu-id="d2173-339">tooprepare hello infrastruktur för arkitektur mall 1:</span><span class="sxs-lookup"><span data-stu-id="d2173-339">tooprepare hello infrastructure for Architectural Template 1:</span></span>

- <span data-ttu-id="d2173-340">I hello Azure-portalen på hello **parametrar** bladet i hello **SYSTEMAVAILABILITY** väljer **HA**.</span><span class="sxs-lookup"><span data-stu-id="d2173-340">In hello Azure portal, on hello **Parameters** blade, in hello **SYSTEMAVAILABILITY** box, select **HA**.</span></span>

  ![Figur 11: Ange parametrar för SAP hög tillgänglighet Azure Resource Manager][sap-ha-guide-figure-3000]

<span data-ttu-id="d2173-342">_**Figur 11:** ange parametrar för SAP hög tillgänglighet Azure Resource Manager_</span><span class="sxs-lookup"><span data-stu-id="d2173-342">_**Figure 11:** Set SAP high-availability Azure Resource Manager parameters_</span></span>


  <span data-ttu-id="d2173-343">Skapa hello-mallar:</span><span class="sxs-lookup"><span data-stu-id="d2173-343">hello templates create:</span></span>

  * <span data-ttu-id="d2173-344">**Virtuella datorer**:</span><span class="sxs-lookup"><span data-stu-id="d2173-344">**Virtual machines**:</span></span>
    * <span data-ttu-id="d2173-345">SAP programserver virtuella datorer: <*SAPSystemSID*> - di - <*tal*></span><span class="sxs-lookup"><span data-stu-id="d2173-345">SAP Application Server virtual machines: <*SAPSystemSID*>-di-<*Number*></span></span>
    * <span data-ttu-id="d2173-346">ASCS/SCS klustra virtuella datorer: <*SAPSystemSID*> - ascs - <*tal*></span><span class="sxs-lookup"><span data-stu-id="d2173-346">ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-ascs-<*Number*></span></span>
    * <span data-ttu-id="d2173-347">DBMS-kluster: <*SAPSystemSID*> - db - <*tal*></span><span class="sxs-lookup"><span data-stu-id="d2173-347">DBMS cluster: <*SAPSystemSID*>-db-<*Number*></span></span>

  * <span data-ttu-id="d2173-348">**Nätverkskort för alla virtuella datorer med tillhörande IP-adresser**:</span><span class="sxs-lookup"><span data-stu-id="d2173-348">**Network cards for all virtual machines, with associated IP addresses**:</span></span>
    * <span data-ttu-id="d2173-349"><*SAPSystemSID*> - nic - di - <*tal*></span><span class="sxs-lookup"><span data-stu-id="d2173-349"><*SAPSystemSID*>-nic-di-<*Number*></span></span>
    * <span data-ttu-id="d2173-350"><*SAPSystemSID*> - nic - ascs - <*tal*></span><span class="sxs-lookup"><span data-stu-id="d2173-350"><*SAPSystemSID*>-nic-ascs-<*Number*></span></span>
    * <span data-ttu-id="d2173-351"><*SAPSystemSID*> - nic - db - <*tal*></span><span class="sxs-lookup"><span data-stu-id="d2173-351"><*SAPSystemSID*>-nic-db-<*Number*></span></span>

  * <span data-ttu-id="d2173-352">**Azure storage-konton**</span><span class="sxs-lookup"><span data-stu-id="d2173-352">**Azure storage accounts**</span></span>

  * <span data-ttu-id="d2173-353">**Tillgänglighetsgrupper** för:</span><span class="sxs-lookup"><span data-stu-id="d2173-353">**Availability groups** for:</span></span>
    * <span data-ttu-id="d2173-354">SAP programserver virtuella datorer: <*SAPSystemSID*> - avset - di</span><span class="sxs-lookup"><span data-stu-id="d2173-354">SAP Application Server virtual machines: <*SAPSystemSID*>-avset-di</span></span>
    * <span data-ttu-id="d2173-355">SAP ASCS/SCS klustra virtuella datorer: <*SAPSystemSID*> - avset - ascs</span><span class="sxs-lookup"><span data-stu-id="d2173-355">SAP ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-avset-ascs</span></span>
    * <span data-ttu-id="d2173-356">DBMS klustra virtuella datorer: <*SAPSystemSID*> - avset - db</span><span class="sxs-lookup"><span data-stu-id="d2173-356">DBMS cluster virtual machines: <*SAPSystemSID*>-avset-db</span></span>

  * <span data-ttu-id="d2173-357">**Azure intern belastningsutjämnare**:</span><span class="sxs-lookup"><span data-stu-id="d2173-357">**Azure internal load balancer**:</span></span>
    * <span data-ttu-id="d2173-358">Med alla portar för hello ASCS/SCS-instansen och IP-adressen <*SAPSystemSID*> - lb - ascs</span><span class="sxs-lookup"><span data-stu-id="d2173-358">With all ports for hello ASCS/SCS instance and IP address <*SAPSystemSID*>-lb-ascs</span></span>
    * <span data-ttu-id="d2173-359">Med alla portar för hello DBMS för SQL Server och IP-adressen <*SAPSystemSID*> - lb - db</span><span class="sxs-lookup"><span data-stu-id="d2173-359">With all ports for hello SQL Server DBMS and IP address <*SAPSystemSID*>-lb-db</span></span>

  * <span data-ttu-id="d2173-360">**Nätverkssäkerhetsgruppen**: <*SAPSystemSID*> - nsg - ascs-0</span><span class="sxs-lookup"><span data-stu-id="d2173-360">**Network security group**: <*SAPSystemSID*>-nsg-ascs-0</span></span>  
    * <span data-ttu-id="d2173-361">Med en öppen externa Remote Desktop Protocol (RDP) port toohello <*SAPSystemSID*> - ascs - 0 virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d2173-361">With an open external Remote Desktop Protocol (RDP) port toohello <*SAPSystemSID*>-ascs-0 virtual machine</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-362">Alla IP-adresser för hello nätverkskort och Azure interna belastningsutjämnare **dynamisk** som standard.</span><span class="sxs-lookup"><span data-stu-id="d2173-362">All IP addresses of hello network cards and Azure internal load balancers are **dynamic** by default.</span></span> <span data-ttu-id="d2173-363">Ändra dem för**Statiska** IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d2173-363">Change them too**static** IP addresses.</span></span> <span data-ttu-id="d2173-364">Vi beskriver hur toodo detta hello nedan.</span><span class="sxs-lookup"><span data-stu-id="d2173-364">We describe how toodo this later in hello article.</span></span>
>
>

### <span data-ttu-id="d2173-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Distribuera virtuella datorer med företagets nätverk anslutningar (anslutningar mellan lokala) toouse i produktion</span><span class="sxs-lookup"><span data-stu-id="d2173-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Deploy virtual machines with corporate network connectivity (cross-premises) toouse in production</span></span>
<span data-ttu-id="d2173-366">Distribuera virtuella Azure-datorer med för produktion SAP-system, [företagets nätverksanslutning (mellan platser)] [ planning-guide-2.2] med hjälp av Azure plats-till-plats-VPN eller Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d2173-366">For production SAP systems, deploy Azure virtual machines with [corporate network connectivity (cross-premises)][planning-guide-2.2] by using Azure Site-to-Site VPN or Azure ExpressRoute.</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-367">Du kan använda Azure Virtual Network-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-367">You can use your Azure Virtual Network instance.</span></span> <span data-ttu-id="d2173-368">hello virtuellt nätverk och undernät har redan skapats och förberetts.</span><span class="sxs-lookup"><span data-stu-id="d2173-368">hello virtual network and subnet have already been created and prepared.</span></span>
>
>

1.  <span data-ttu-id="d2173-369">I hello Azure-portalen på hello **parametrar** bladet i hello **NEWOREXISTINGSUBNET** väljer **befintliga**.</span><span class="sxs-lookup"><span data-stu-id="d2173-369">In hello Azure portal, on hello **Parameters** blade, in hello **NEWOREXISTINGSUBNET** box, select **existing**.</span></span>
2.  <span data-ttu-id="d2173-370">I hello **SUBNETID** lägger du till hello fullständig sträng med förberedda Azure nätverket SubnetID där du planerar toodeploy Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-370">In hello **SUBNETID** box, add hello full string of your prepared Azure network SubnetID where you plan toodeploy your Azure virtual machines.</span></span>
3.  <span data-ttu-id="d2173-371">tooget en lista över alla undernät som Azure-nätverk, Kör PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="d2173-371">tooget a list of all Azure network subnets, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  <span data-ttu-id="d2173-372">Hej **ID** fältet visar hello **SUBNETID**.</span><span class="sxs-lookup"><span data-stu-id="d2173-372">hello **ID** field shows hello **SUBNETID**.</span></span>
4. <span data-ttu-id="d2173-373">tooget en lista över alla **SUBNETID** värden, Kör PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="d2173-373">tooget a list of all **SUBNETID** values, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  <span data-ttu-id="d2173-374">Hej **SUBNETID** ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="d2173-374">hello **SUBNETID** looks like this:</span></span>

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <span data-ttu-id="d2173-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Distribuera endast molnbaserad SAP-instanser för testning och demo</span><span class="sxs-lookup"><span data-stu-id="d2173-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Deploy cloud-only SAP instances for test and demo</span></span>
<span data-ttu-id="d2173-376">Du kan distribuera din SAP-systemet för hög tillgänglighet i en endast molnbaserad distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="d2173-376">You can deploy your high-availability SAP system in a cloud-only deployment model.</span></span> <span data-ttu-id="d2173-377">Den här typen av distribution är främst användbart för demonstration och testa användningsområden.</span><span class="sxs-lookup"><span data-stu-id="d2173-377">This kind of deployment primarily is useful for demo and test use cases.</span></span> <span data-ttu-id="d2173-378">Det är inte lämpligt för produktion användningsfall.</span><span class="sxs-lookup"><span data-stu-id="d2173-378">It's not suited for production use cases.</span></span>

- <span data-ttu-id="d2173-379">I hello Azure-portalen på hello **parametrar** bladet i hello **NEWOREXISTINGSUBNET** väljer **nya**.</span><span class="sxs-lookup"><span data-stu-id="d2173-379">In hello Azure portal, on hello **Parameters** blade, in hello **NEWOREXISTINGSUBNET** box, select **new**.</span></span> <span data-ttu-id="d2173-380">Lämna hello **SUBNETID** fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="d2173-380">Leave hello **SUBNETID** field empty.</span></span>

  <span data-ttu-id="d2173-381">hello SAP Azure Resource Manager mallen automatiskt skapar hello Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="d2173-381">hello SAP Azure Resource Manager template automatically creates hello Azure virtual network and subnet.</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-382">Du måste också toodeploy minst en dedikerad virtuell dator för Active Directory och DNS i hello samma Azure Virtual Network-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-382">You also need toodeploy at least one dedicated virtual machine for Active Directory and DNS in hello same Azure Virtual Network instance.</span></span> <span data-ttu-id="d2173-383">hello mall skapar inte dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-383">hello template doesn't create these virtual machines.</span></span>
>
>


### <a name="prepare-hello-infrastructure-for-architectural-template-2"></a><span data-ttu-id="d2173-384">Förbereda hello infrastrukturen för arkitektur mall 2</span><span class="sxs-lookup"><span data-stu-id="d2173-384">Prepare hello infrastructure for Architectural Template 2</span></span>

<span data-ttu-id="d2173-385">Du kan använda den här Azure Resource Manager-mallen för SAP toohelp förenkla distributionen av infrastrukturresurser för SAP arkitektur mall 2.</span><span class="sxs-lookup"><span data-stu-id="d2173-385">You can use this Azure Resource Manager template for SAP toohelp simplify deployment of required infrastructure resources for SAP Architectural Template 2.</span></span>

<span data-ttu-id="d2173-386">Här är där du kan hämta Azure Resource Manager-mallar för det här distributionsscenariot:</span><span class="sxs-lookup"><span data-stu-id="d2173-386">Here's where you can get Azure Resource Manager templates for this deployment scenario:</span></span>

* [<span data-ttu-id="d2173-387">Azure Marketplace-avbildning</span><span class="sxs-lookup"><span data-stu-id="d2173-387">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [<span data-ttu-id="d2173-388">Anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="d2173-388">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-hello-infrastructure-for-architectural-template-3"></a><span data-ttu-id="d2173-389">Förbereda hello infrastrukturen för arkitektur mall 3</span><span class="sxs-lookup"><span data-stu-id="d2173-389">Prepare hello infrastructure for Architectural Template 3</span></span>

<span data-ttu-id="d2173-390">Du kan förbereda hello-infrastrukturen och konfigurera SAP för **multi-SID**.</span><span class="sxs-lookup"><span data-stu-id="d2173-390">You can prepare hello infrastructure and configure SAP for **multi-SID**.</span></span> <span data-ttu-id="d2173-391">Du kan exempelvis lägga till en ytterligare SAP ASCS/SCS-instans i en *befintliga* klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d2173-391">For example, you can add an additional SAP ASCS/SCS instance into an *existing* cluster configuration.</span></span> <span data-ttu-id="d2173-392">Mer information finns i [och konfigurera en ytterligare instans SAP ASCS/SCS till ett befintligt kluster configuration toocreate en SAP multi-SID-konfiguration i Azure Resource Manager][sap-ha-multi-sid-guide].</span><span class="sxs-lookup"><span data-stu-id="d2173-392">For more information, see [Configure an additional SAP ASCS/SCS instance into an existing cluster configuration toocreate an SAP multi-SID configuration in Azure Resource Manager][sap-ha-multi-sid-guide].</span></span>

<span data-ttu-id="d2173-393">Om du vill toocreate ett nytt multi-SID-kluster, kan du använda hello multi-SID [snabbstartsmallar på GitHub](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="d2173-393">If you want toocreate a new multi-SID cluster, you can use hello multi-SID [quickstart templates on GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span>
<span data-ttu-id="d2173-394">toocreate ett nytt kluster med flera SID måste toodeploy hello följande tre mallar:</span><span class="sxs-lookup"><span data-stu-id="d2173-394">toocreate a new multi-SID cluster, you need toodeploy hello following three templates:</span></span>

* [<span data-ttu-id="d2173-395">ASCS/SCS mall</span><span class="sxs-lookup"><span data-stu-id="d2173-395">ASCS/SCS template</span></span>](#ASCS-SCS-template)
* [<span data-ttu-id="d2173-396">Databasmall för</span><span class="sxs-lookup"><span data-stu-id="d2173-396">Database template</span></span>](#database-template)
* [<span data-ttu-id="d2173-397">Mall för servrar</span><span class="sxs-lookup"><span data-stu-id="d2173-397">Application servers template</span></span>](#application-servers-template)

<span data-ttu-id="d2173-398">hello ha avsnitten mer information om hello mallar och hello-parametrar som du behöver tooprovide i hello mallar.</span><span class="sxs-lookup"><span data-stu-id="d2173-398">hello following sections have more details about hello templates and hello parameters you need tooprovide in hello templates.</span></span>

#### <span data-ttu-id="d2173-399"><a name="ASCS-SCS-template"></a>ASCS/SCS mall</span><span class="sxs-lookup"><span data-stu-id="d2173-399"><a name="ASCS-SCS-template"></a> ASCS/SCS template</span></span>

<span data-ttu-id="d2173-400">Hej ASCS/SCS mallen distribuerar två virtuella datorer som du kan använda toocreate ett redundanskluster i Windows Server som är värd för flera ASCS/SCS-instanser.</span><span class="sxs-lookup"><span data-stu-id="d2173-400">hello ASCS/SCS template deploys two virtual machines that you can use toocreate a Windows Server failover cluster that hosts multiple ASCS/SCS instances.</span></span>

<span data-ttu-id="d2173-401">tooset hello ASCS/SCS multi-SID-mall i hello [ASCS/SCS multi-SID-mall][sap-templates-3-tier-multisid-xscs-marketplace-image], ange värden för hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="d2173-401">tooset up hello ASCS/SCS multi-SID template, in hello [ASCS/SCS multi-SID template][sap-templates-3-tier-multisid-xscs-marketplace-image], enter values for hello following parameters:</span></span>

  - <span data-ttu-id="d2173-402">**Resursen prefixet**.</span><span class="sxs-lookup"><span data-stu-id="d2173-402">**Resource Prefix**.</span></span>  <span data-ttu-id="d2173-403">Ange hello resurs prefixet, vilket används tooprefix alla resurser som har skapats under hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="d2173-403">Set hello resource prefix, which is used tooprefix all resources that are created during hello deployment.</span></span> <span data-ttu-id="d2173-404">Hello resurser inte tillhör tooonly en SAP-system, är hello prefixet hello resursen inte eftersom hello SID för en SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-404">Because hello resources do not belong tooonly one SAP system, hello prefix of hello resource is not hello SID of one SAP system.</span></span>  <span data-ttu-id="d2173-405">hello-prefixet måste vara mellan **tre till sex tecken**.</span><span class="sxs-lookup"><span data-stu-id="d2173-405">hello prefix must be between **three and six characters**.</span></span>
  - <span data-ttu-id="d2173-406">**Stacken typen**.</span><span class="sxs-lookup"><span data-stu-id="d2173-406">**Stack Type**.</span></span> <span data-ttu-id="d2173-407">Ange hello stack hello SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-407">Select hello stack type of hello SAP system.</span></span> <span data-ttu-id="d2173-408">Beroende på hello stack typ har Azure belastningsutjämnare en (ABAP eller endast Java) eller två (ABAP + Java) privata IP-adresser per SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-408">Depending on hello stack type, Azure Load Balancer has one (ABAP or Java only) or two (ABAP+Java) private IP addresses per SAP system.</span></span>
  -  <span data-ttu-id="d2173-409">**OS-typen**.</span><span class="sxs-lookup"><span data-stu-id="d2173-409">**OS Type**.</span></span> <span data-ttu-id="d2173-410">Välj hello operativsystem hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-410">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="d2173-411">**SAP System antal**.</span><span class="sxs-lookup"><span data-stu-id="d2173-411">**SAP System Count**.</span></span> <span data-ttu-id="d2173-412">Välj hello nummer SAP-system som du vill tooinstall i det här klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-412">Select hello number of SAP systems you want tooinstall in this cluster.</span></span>
  -  <span data-ttu-id="d2173-413">**Filsystemets tillgänglighet**.</span><span class="sxs-lookup"><span data-stu-id="d2173-413">**System Availability**.</span></span> <span data-ttu-id="d2173-414">Välj **HA**.</span><span class="sxs-lookup"><span data-stu-id="d2173-414">Select **HA**.</span></span>
  -  <span data-ttu-id="d2173-415">**Användarnamn och lösenord för Admin administratör**.</span><span class="sxs-lookup"><span data-stu-id="d2173-415">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="d2173-416">Skapa en ny användare som kan använda toosign i toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="d2173-416">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="d2173-417">**Ny eller befintlig undernät**.</span><span class="sxs-lookup"><span data-stu-id="d2173-417">**New Or Existing Subnet**.</span></span> <span data-ttu-id="d2173-418">Ange om ett nytt virtuellt nätverk och undernät som ska skapas eller ett befintligt undernät som ska användas.</span><span class="sxs-lookup"><span data-stu-id="d2173-418">Set whether a new virtual network and subnet should be created, or an existing subnet should be used.</span></span> <span data-ttu-id="d2173-419">Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.</span><span class="sxs-lookup"><span data-stu-id="d2173-419">If you already have a virtual network that is connected tooyour on-premises network, select **existing**.</span></span>
  -  <span data-ttu-id="d2173-420">**Undernät-Id**. Ange hello-ID för hello undernät toowhich hello virtuella datorer som ska anslutas.</span><span class="sxs-lookup"><span data-stu-id="d2173-420">**Subnet Id**. Set hello ID of hello subnet toowhich hello virtual machines should be connected.</span></span> <span data-ttu-id="d2173-421">Välj hello undernät för virtuellt privat nätverk (VPN) eller ExpressRoute virtuellt nätverk tooconnect hello virtuella tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d2173-421">Select hello subnet of your virtual private network (VPN) or ExpressRoute virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="d2173-422">hello ID ser oftast ut så här:</span><span class="sxs-lookup"><span data-stu-id="d2173-422">hello ID usually looks like this:</span></span>

   <span data-ttu-id="d2173-423">/subscriptions/ <*prenumerations-id*> /resourceGroups/ <*resursgruppnamn*> /providers/Microsoft.Network/virtualNetworks/ <*virtuella nätverksnamnet*> /subnets/ <*undernätsnamn*></span><span class="sxs-lookup"><span data-stu-id="d2173-423">/subscriptions/<*subscription id*>/resourceGroups/<*resource group name*>/providers/Microsoft.Network/virtualNetworks/<*virtual network name*>/subnets/<*subnet name*></span></span>

<span data-ttu-id="d2173-424">hello mallen distribuerar en belastningsutjämnare för Azure-instans som har stöd för flera SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-424">hello template deploys one Azure Load Balancer instance, which supports multiple SAP systems.</span></span>

- <span data-ttu-id="d2173-425">Hej ASCS-instanserna är konfigurerade för instansnummer 00, 10, 20...</span><span class="sxs-lookup"><span data-stu-id="d2173-425">hello ASCS instances are configured for instance number 00, 10, 20...</span></span>
- <span data-ttu-id="d2173-426">hello SCS-instanserna är konfigurerade för instansnummer 01, 11, 21...</span><span class="sxs-lookup"><span data-stu-id="d2173-426">hello SCS instances are configured for instance number 01, 11, 21...</span></span>
- <span data-ttu-id="d2173-427">Hej ASCS sätta replikering Server (ERS) (endast Linux)-instanserna är konfigurerade för instansnummer 02, 12, 22...</span><span class="sxs-lookup"><span data-stu-id="d2173-427">hello ASCS Enqueue Replication Server (ERS) (Linux only) instances are configured for instance number 02, 12, 22...</span></span>
- <span data-ttu-id="d2173-428">hello SCS ÄNDARE (endast Linux)-instanserna är konfigurerade för instansnummer 03, 13, 23...</span><span class="sxs-lookup"><span data-stu-id="d2173-428">hello SCS ERS (Linux only) instances are configured for instance number 03, 13, 23...</span></span>

<span data-ttu-id="d2173-429">hello belastningsutjämnaren innehåller 1.2 för Linux VIP(s), 1 x-VIP för ASCS/SCS och 1 x-VIP för ÄNDARE (endast Linux).</span><span class="sxs-lookup"><span data-stu-id="d2173-429">hello load balancer contains 1 (2 for Linux) VIP(s), 1x VIP for ASCS/SCS and 1x VIP for ERS (Linux only).</span></span>

<span data-ttu-id="d2173-430">hello innehåller följande lista alla belastningsutjämningsregler (där x är hello antalet hello SAP-systemet, till exempel 1, 2, 3...):</span><span class="sxs-lookup"><span data-stu-id="d2173-430">hello following list contains all load balancing rules (where x is hello number of hello SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="d2173-431">Windows-specifika portar för varje system för SAP: 445, 5985</span><span class="sxs-lookup"><span data-stu-id="d2173-431">Windows-specific ports for every SAP system: 445, 5985</span></span>
- <span data-ttu-id="d2173-432">ASCS-portar (instansnummer x0): 32 x 0, 36 x 0, 39 x 0, 81 0 x, 5 x 013, 5 x 014, 5 x 016</span><span class="sxs-lookup"><span data-stu-id="d2173-432">ASCS ports (instance number x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016</span></span>
- <span data-ttu-id="d2173-433">SCS portar (instansnummer x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116</span><span class="sxs-lookup"><span data-stu-id="d2173-433">SCS ports (instance number x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116</span></span>
- <span data-ttu-id="d2173-434">ASCS ERS portar på Linux (instansnummer x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216</span><span class="sxs-lookup"><span data-stu-id="d2173-434">ASCS ERS ports on Linux (instance number x2): 33x2, 5x213, 5x214, 5x216</span></span>
- <span data-ttu-id="d2173-435">SCS ERS portar på Linux (instansnummer x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316</span><span class="sxs-lookup"><span data-stu-id="d2173-435">SCS ERS ports on Linux (instance number x3): 33x3, 5x313, 5x314, 5x316</span></span>

<span data-ttu-id="d2173-436">hello belastningsutjämning är konfigurerade toouse hello följande avsökningen portar (där x är hello antalet hello SAP-systemet, till exempel 1, 2, 3...):</span><span class="sxs-lookup"><span data-stu-id="d2173-436">hello load balancer is configured toouse hello following probe ports (where x is hello number of hello SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="d2173-437">ASCS/SCS interna läsa in belastningsutjämning avsökningsport: 620 x 0</span><span class="sxs-lookup"><span data-stu-id="d2173-437">ASCS/SCS internal load balancer probe port: 620x0</span></span>
- <span data-ttu-id="d2173-438">ÄNDARE interna läsa in belastningsutjämning avsökningsport (endast Linux): 621 x 2</span><span class="sxs-lookup"><span data-stu-id="d2173-438">ERS internal load balancer probe port (Linux only): 621x2</span></span>

#### <span data-ttu-id="d2173-439"><a name="database-template"></a>Databasmall för</span><span class="sxs-lookup"><span data-stu-id="d2173-439"><a name="database-template"></a> Database template</span></span>

<span data-ttu-id="d2173-440">hello databasen mallen distribuerar en eller två virtuella datorer som du kan använda tooinstall hello relationella databashanteringssystem (RDBMS) för en SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-440">hello database template deploys one or two virtual machines that you can use tooinstall hello relational database management system (RDBMS) for one SAP system.</span></span> <span data-ttu-id="d2173-441">Till exempel om du distribuerar en ASCS/SCS-mall för fem SAP-system måste toodeploy mallen fem gånger.</span><span class="sxs-lookup"><span data-stu-id="d2173-441">For example, if you deploy an ASCS/SCS template for five SAP systems, you need toodeploy this template five times.</span></span>

<span data-ttu-id="d2173-442">tooset hello databasen multi-SID-mall, i hello [multi-SID-mall för databas][sap-templates-3-tier-multisid-db-marketplace-image], ange värden för hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="d2173-442">tooset up hello database multi-SID template, in hello [database multi-SID template][sap-templates-3-tier-multisid-db-marketplace-image], enter values for hello following parameters:</span></span>

  -  <span data-ttu-id="d2173-443">**SAP System-Id**. Ange hello SAP system-ID för hello SAP-system som du vill tooinstall.</span><span class="sxs-lookup"><span data-stu-id="d2173-443">**Sap System Id**. Enter hello SAP system ID of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="d2173-444">hello-ID ska användas som ett prefix för hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="d2173-444">hello ID will be used as a prefix for hello resources that are deployed.</span></span>
  -  <span data-ttu-id="d2173-445">**OS-typen**.</span><span class="sxs-lookup"><span data-stu-id="d2173-445">**Os Type**.</span></span> <span data-ttu-id="d2173-446">Välj hello operativsystem hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-446">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="d2173-447">**DbType**.</span><span class="sxs-lookup"><span data-stu-id="d2173-447">**Dbtype**.</span></span> <span data-ttu-id="d2173-448">Välj typ av hello hello databasen som du vill tooinstall på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-448">Select hello type of hello database you want tooinstall on hello cluster.</span></span> <span data-ttu-id="d2173-449">Välj **SQL** om du vill tooinstall Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d2173-449">Select **SQL** if you want tooinstall Microsoft SQL Server.</span></span> <span data-ttu-id="d2173-450">Välj **HANA** om du tänker tooinstall SAP HANA hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-450">Select **HANA** if you plan tooinstall SAP HANA on hello virtual machines.</span></span> <span data-ttu-id="d2173-451">Kontrollera tooselect hello rätt operativsystem Systemtyp: Välj **Windows** för SQL och välj en Linux-distributionsplats för HANA.</span><span class="sxs-lookup"><span data-stu-id="d2173-451">Make sure tooselect hello correct operating system type: select **Windows** for SQL, and select a Linux distribution for HANA.</span></span> <span data-ttu-id="d2173-452">hello konfigureras Azure belastningsutjämnare som är anslutna toohello virtuella datorer kommer att toosupport hello valt databastyp:</span><span class="sxs-lookup"><span data-stu-id="d2173-452">hello Azure Load Balancer that is connected toohello virtual machines will be configured toosupport hello selected database type:</span></span>
    * <span data-ttu-id="d2173-453">**SQL**.</span><span class="sxs-lookup"><span data-stu-id="d2173-453">**SQL**.</span></span> <span data-ttu-id="d2173-454">hello belastningsutjämnaren att belastningsutjämna port 1433.</span><span class="sxs-lookup"><span data-stu-id="d2173-454">hello load balancer will load-balance port 1433.</span></span> <span data-ttu-id="d2173-455">Se till att toouse den här porten för din SQL Server Always On-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d2173-455">Make sure toouse this port for your SQL Server Always On setup.</span></span>
    * <span data-ttu-id="d2173-456">**HANA**.</span><span class="sxs-lookup"><span data-stu-id="d2173-456">**HANA**.</span></span> <span data-ttu-id="d2173-457">hello belastningsutjämnaren att belastningsutjämna portarna 35015 och 35017.</span><span class="sxs-lookup"><span data-stu-id="d2173-457">hello load balancer will load-balance ports 35015 and 35017.</span></span> <span data-ttu-id="d2173-458">Se till att tooinstall SAP HANA med instansnummer **50**.</span><span class="sxs-lookup"><span data-stu-id="d2173-458">Make sure tooinstall SAP HANA with instance number **50**.</span></span>
    <span data-ttu-id="d2173-459">hello belastningsutjämnare använder avsökningsport 62550.</span><span class="sxs-lookup"><span data-stu-id="d2173-459">hello load balancer will use probe port 62550.</span></span>
  -  <span data-ttu-id="d2173-460">**SAP systemstorlek**.</span><span class="sxs-lookup"><span data-stu-id="d2173-460">**Sap System Size**.</span></span> <span data-ttu-id="d2173-461">Hello antal SAP hello nya system ger.</span><span class="sxs-lookup"><span data-stu-id="d2173-461">Set hello number of SAPS hello new system will provide.</span></span> <span data-ttu-id="d2173-462">Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.</span><span class="sxs-lookup"><span data-stu-id="d2173-462">If you are not sure how many SAPS hello system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="d2173-463">**Filsystemets tillgänglighet**.</span><span class="sxs-lookup"><span data-stu-id="d2173-463">**System Availability**.</span></span> <span data-ttu-id="d2173-464">Välj **HA**.</span><span class="sxs-lookup"><span data-stu-id="d2173-464">Select **HA**.</span></span>
  -  <span data-ttu-id="d2173-465">**Användarnamn och lösenord för Admin administratör**.</span><span class="sxs-lookup"><span data-stu-id="d2173-465">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="d2173-466">Skapa en ny användare som kan använda toosign i toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="d2173-466">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="d2173-467">**Undernät-Id**. Ange hello ID hello-undernätet som du använde vid hello distribution av hello ASCS/SCS mall eller hello ID hello undernät som har skapats som en del av hello ASCS/SCS malldistribution.</span><span class="sxs-lookup"><span data-stu-id="d2173-467">**Subnet Id**. Enter hello ID of hello subnet that you used during hello deployment of hello ASCS/SCS template, or hello ID of hello subnet that was created as part of hello ASCS/SCS template deployment.</span></span>

#### <span data-ttu-id="d2173-468"><a name="application-servers-template"></a>Mall för servrar</span><span class="sxs-lookup"><span data-stu-id="d2173-468"><a name="application-servers-template"></a> Application servers template</span></span>

<span data-ttu-id="d2173-469">Hej programmall servrar distribuerar två eller flera virtuella datorer som kan användas som SAP Application Server-instanser för en SAP-system.</span><span class="sxs-lookup"><span data-stu-id="d2173-469">hello application servers template deploys two or more virtual machines that can be used as SAP Application Server instances for one SAP system.</span></span> <span data-ttu-id="d2173-470">Till exempel om du distribuerar en ASCS/SCS-mall för fem SAP-system måste toodeploy mallen fem gånger.</span><span class="sxs-lookup"><span data-stu-id="d2173-470">For example, if you deploy an ASCS/SCS template for five SAP systems, you need toodeploy this template five times.</span></span>

<span data-ttu-id="d2173-471">tooset in hello servrar multi-SID mall för program, i hello [programmall servrar multi-SID][sap-templates-3-tier-multisid-apps-marketplace-image], ange värden för hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="d2173-471">tooset up hello application servers multi-SID template, in hello [application servers multi-SID template][sap-templates-3-tier-multisid-apps-marketplace-image], enter values for hello following parameters:</span></span>

  -  <span data-ttu-id="d2173-472">**SAP System-Id**. Ange hello SAP system-ID för hello SAP-system som du vill tooinstall.</span><span class="sxs-lookup"><span data-stu-id="d2173-472">**Sap System Id**. Enter hello SAP system ID of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="d2173-473">hello-ID ska användas som ett prefix för hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="d2173-473">hello ID will be used as a prefix for hello resources that are deployed.</span></span>
  -  <span data-ttu-id="d2173-474">**OS-typen**.</span><span class="sxs-lookup"><span data-stu-id="d2173-474">**Os Type**.</span></span> <span data-ttu-id="d2173-475">Välj hello operativsystem hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-475">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="d2173-476">**SAP systemstorlek**.</span><span class="sxs-lookup"><span data-stu-id="d2173-476">**Sap System Size**.</span></span> <span data-ttu-id="d2173-477">hello antalet SAP hello nya system ger.</span><span class="sxs-lookup"><span data-stu-id="d2173-477">hello number of SAPS hello new system will provide.</span></span> <span data-ttu-id="d2173-478">Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.</span><span class="sxs-lookup"><span data-stu-id="d2173-478">If you are not sure how many SAPS hello system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="d2173-479">**Filsystemets tillgänglighet**.</span><span class="sxs-lookup"><span data-stu-id="d2173-479">**System Availability**.</span></span> <span data-ttu-id="d2173-480">Välj **HA**.</span><span class="sxs-lookup"><span data-stu-id="d2173-480">Select **HA**.</span></span>
  -  <span data-ttu-id="d2173-481">**Användarnamn och lösenord för Admin administratör**.</span><span class="sxs-lookup"><span data-stu-id="d2173-481">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="d2173-482">Skapa en ny användare som kan använda toosign i toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="d2173-482">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="d2173-483">**Undernät-Id**. Ange hello ID hello-undernätet som du använde vid hello distribution av hello ASCS/SCS mall eller hello ID hello undernät som har skapats som en del av hello ASCS/SCS malldistribution.</span><span class="sxs-lookup"><span data-stu-id="d2173-483">**Subnet Id**. Enter hello ID of hello subnet that you used during hello deployment of hello ASCS/SCS template, or hello ID of hello subnet that was created as part of hello ASCS/SCS template deployment.</span></span>


### <span data-ttu-id="d2173-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuella Azure-nätverket</span><span class="sxs-lookup"><span data-stu-id="d2173-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure virtual network</span></span>
<span data-ttu-id="d2173-485">I vårt exempel är hello adressutrymme hello virtuella Azure-nätverket 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="d2173-485">In our example, hello address space of hello Azure virtual network is 10.0.0.0/16.</span></span> <span data-ttu-id="d2173-486">Det finns ett undernät som kallas **undernät**, med en-adressintervallet 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="d2173-486">There is one subnet called **Subnet**, with an address range of 10.0.0.0/24.</span></span> <span data-ttu-id="d2173-487">Alla virtuella datorer och interna belastningsutjämnare har distribuerats i det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d2173-487">All virtual machines and internal load balancers are deployed in this virtual network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2173-488">Inte göra några ändringar toohello nätverksinställningar i hello gästoperativsystemet.</span><span class="sxs-lookup"><span data-stu-id="d2173-488">Don't make any changes toohello network settings inside hello guest operating system.</span></span> <span data-ttu-id="d2173-489">Detta inkluderar IP-adresser, DNS-servrar och undernät.</span><span class="sxs-lookup"><span data-stu-id="d2173-489">This includes IP addresses, DNS servers, and subnet.</span></span> <span data-ttu-id="d2173-490">Konfigurera nätverksinställningarna i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-490">Configure all your network settings in Azure.</span></span> <span data-ttu-id="d2173-491">hello DHCP Dynamic Host Configuration Protocol ()-tjänsten sprider dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="d2173-491">hello Dynamic Host Configuration Protocol (DHCP) service propagates your settings.</span></span>
>
>

### <span data-ttu-id="d2173-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP-adresser</span><span class="sxs-lookup"><span data-stu-id="d2173-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP addresses</span></span>

<span data-ttu-id="d2173-493">tooset hello krävs DNS IP-adresser, hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="d2173-493">tooset hello required DNS IP addresses, do hello following steps.</span></span>

1.  <span data-ttu-id="d2173-494">I hello Azure-portalen på hello **DNS-servrar** bladet, se till att det virtuella nätverket **DNS-servrar** alternativet anges för**anpassad DNS**.</span><span class="sxs-lookup"><span data-stu-id="d2173-494">In hello Azure portal, on hello **DNS servers** blade, make sure that your virtual network **DNS servers** option is set too**Custom DNS**.</span></span>
2.  <span data-ttu-id="d2173-495">Välj inställningarna utifrån hello typen av nätverk som du har.</span><span class="sxs-lookup"><span data-stu-id="d2173-495">Select your settings based on hello type of network you have.</span></span> <span data-ttu-id="d2173-496">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="d2173-496">For more information, see hello following resources:</span></span>
    * <span data-ttu-id="d2173-497">[Företagets nätverksanslutning (mellan platser)][planning-guide-2.2]: Lägg till hello IP-adresser för hello lokala DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="d2173-497">[Corporate network connectivity (cross-premises)][planning-guide-2.2]: Add hello IP addresses of hello on-premises DNS servers.</span></span>  
    <span data-ttu-id="d2173-498">Du kan utöka lokala DNS-servrar toohello virtuella datorer som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-498">You can extend on-premises DNS servers toohello virtual machines that are running in Azure.</span></span> <span data-ttu-id="d2173-499">I scenariot, du kan lägga till hello IP-adresser för hello Azure virtuella datorer som du kör hello DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2173-499">In that scenario, you can add hello IP addresses of hello Azure virtual machines on which you run hello DNS service.</span></span>
    * <span data-ttu-id="d2173-500">[Endast molnbaserad distribution][planning-guide-2.1]: distribuera en ytterligare virtuell dator i hello samma virtuella nätverk-instans som fungerar som en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="d2173-500">[Cloud-only deployment][planning-guide-2.1]: Deploy an additional virtual machine in hello same Virtual Network instance that serves as a DNS server.</span></span> <span data-ttu-id="d2173-501">Lägg till hello IP-adresser för hello Azure virtuella datorer som du har konfigurerat toorun DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2173-501">Add hello IP addresses of hello Azure virtual machines that you've set up toorun DNS service.</span></span>

    ![Figur 12: Konfigurera DNS-servrar för Azure-nätverk][sap-ha-guide-figure-3001]

    <span data-ttu-id="d2173-503">_**Figur 12:** konfigurera DNS-servrar för Azure-nätverk_</span><span class="sxs-lookup"><span data-stu-id="d2173-503">_**Figure 12:** Configure DNS servers for Azure Virtual Network_</span></span>

  > [!NOTE]
  > <span data-ttu-id="d2173-504">Om du ändrar hello IP-adresser för hello DNS-servrar du behöver toorestart hello Azure virtuella datorer tooapply hello ändringen och sprida hello nya DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="d2173-504">If you change hello IP addresses of hello DNS servers, you need toorestart hello Azure virtual machines tooapply hello change and propagate hello new DNS servers.</span></span>
  >
  >

<span data-ttu-id="d2173-505">I vårt exempel har hello DNS-tjänsten installerats och konfigurerats på dessa virtuella Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="d2173-505">In our example, hello DNS service is installed and configured on these Windows virtual machines:</span></span>

| <span data-ttu-id="d2173-506">Rollen virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d2173-506">Virtual machine role</span></span> | <span data-ttu-id="d2173-507">Värdnamn för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d2173-507">Virtual machine host name</span></span> | <span data-ttu-id="d2173-508">Namn på nätverkskort</span><span class="sxs-lookup"><span data-stu-id="d2173-508">Network card name</span></span> | <span data-ttu-id="d2173-509">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="d2173-509">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d2173-510">Första DNS-servern</span><span class="sxs-lookup"><span data-stu-id="d2173-510">First DNS server</span></span> |<span data-ttu-id="d2173-511">domcontr 0</span><span class="sxs-lookup"><span data-stu-id="d2173-511">domcontr-0</span></span> |<span data-ttu-id="d2173-512">PR1-nic-domcontr-0</span><span class="sxs-lookup"><span data-stu-id="d2173-512">pr1-nic-domcontr-0</span></span> |<span data-ttu-id="d2173-513">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="d2173-513">10.0.0.10</span></span> |
| <span data-ttu-id="d2173-514">Andra DNS-server</span><span class="sxs-lookup"><span data-stu-id="d2173-514">Second DNS server</span></span> |<span data-ttu-id="d2173-515">domcontr 1</span><span class="sxs-lookup"><span data-stu-id="d2173-515">domcontr-1</span></span> |<span data-ttu-id="d2173-516">PR1-nic-domcontr-1</span><span class="sxs-lookup"><span data-stu-id="d2173-516">pr1-nic-domcontr-1</span></span> |<span data-ttu-id="d2173-517">10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="d2173-517">10.0.0.11</span></span> |

### <span data-ttu-id="d2173-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Värdnamn och statiska IP-adresser för hello SAP ASCS/SCS klustrade instansen och DBMS klustrad instans</span><span class="sxs-lookup"><span data-stu-id="d2173-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Host names and static IP addresses for hello SAP ASCS/SCS clustered instance and DBMS clustered instance</span></span>

<span data-ttu-id="d2173-519">För lokal distribution måste dessa reserverade värdnamn och IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="d2173-519">For on-premises deployment, you need these reserved host names and IP addresses:</span></span>

| <span data-ttu-id="d2173-520">Virtuell värd namn roll</span><span class="sxs-lookup"><span data-stu-id="d2173-520">Virtual host name role</span></span> | <span data-ttu-id="d2173-521">Virtuella värdnamn</span><span class="sxs-lookup"><span data-stu-id="d2173-521">Virtual host name</span></span> | <span data-ttu-id="d2173-522">Virtuell statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="d2173-522">Virtual static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d2173-523">SAP ASCS/SCS första klustret virtuellt värdnamn (för hantering av kluster)</span><span class="sxs-lookup"><span data-stu-id="d2173-523">SAP ASCS/SCS first cluster virtual host name (for cluster management)</span></span> |<span data-ttu-id="d2173-524">PR1-ascs-vir</span><span class="sxs-lookup"><span data-stu-id="d2173-524">pr1-ascs-vir</span></span> |<span data-ttu-id="d2173-525">10.0.0.42</span><span class="sxs-lookup"><span data-stu-id="d2173-525">10.0.0.42</span></span> |
| <span data-ttu-id="d2173-526">SAP ASCS/SCS virtuell värd instansnamn</span><span class="sxs-lookup"><span data-stu-id="d2173-526">SAP ASCS/SCS instance virtual host name</span></span> |<span data-ttu-id="d2173-527">PR1-ascs-sap</span><span class="sxs-lookup"><span data-stu-id="d2173-527">pr1-ascs-sap</span></span> |<span data-ttu-id="d2173-528">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="d2173-528">10.0.0.43</span></span> |
| <span data-ttu-id="d2173-529">SAP DBMS andra kluster virtuellt värdnamn (hantering)</span><span class="sxs-lookup"><span data-stu-id="d2173-529">SAP DBMS second cluster virtual host name (cluster management)</span></span> |<span data-ttu-id="d2173-530">PR1-dbms-vir</span><span class="sxs-lookup"><span data-stu-id="d2173-530">pr1-dbms-vir</span></span> |<span data-ttu-id="d2173-531">10.0.0.32</span><span class="sxs-lookup"><span data-stu-id="d2173-531">10.0.0.32</span></span> |

<span data-ttu-id="d2173-532">När du skapar klustret hello skapa hello virtuella värdnamn **pr1-ascs-vir** och **pr1-dbms-vir** och hello tillhörande IP-adresser som hanterar själva hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-532">When you create hello cluster, create hello virtual host names **pr1-ascs-vir** and **pr1-dbms-vir** and hello associated IP addresses that manage hello cluster itself.</span></span> <span data-ttu-id="d2173-533">Information om hur toodo detta, se [samla in klusternoder i en klusterkonfiguration][sap-ha-guide-8.12.1].</span><span class="sxs-lookup"><span data-stu-id="d2173-533">For information about how toodo this, see [Collect cluster nodes in a cluster configuration][sap-ha-guide-8.12.1].</span></span>

<span data-ttu-id="d2173-534">Du kan manuellt skapa hello andra två virtuella värdnamn **pr1-ascs-sap** och **pr1-dbms-sap**, och hello tillhörande IP-adresser på hello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="d2173-534">You can manually create hello other two virtual host names, **pr1-ascs-sap** and **pr1-dbms-sap**, and hello associated IP addresses, on hello DNS server.</span></span> <span data-ttu-id="d2173-535">hello klustrad SAP ASCS/SCS-instans och hello klustrade DBMS instans använder dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="d2173-535">hello clustered SAP ASCS/SCS instance and hello clustered DBMS instance use these resources.</span></span> <span data-ttu-id="d2173-536">Information om hur toodo detta, se [skapa ett virtuellt värdnamn för en klustrad instans av SAP ASCS/SCS][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="d2173-536">For information about how toodo this, see [Create a virtual host name for a clustered SAP ASCS/SCS instance][sap-ha-guide-9.1.1].</span></span>

### <span data-ttu-id="d2173-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Ange statiska IP-adresser för hello SAP virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d2173-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Set static IP addresses for hello SAP virtual machines</span></span>
<span data-ttu-id="d2173-538">När du distribuerar hello toouse för virtuella datorer i klustret måste tooset statiska IP-adresser för alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-538">After you deploy hello virtual machines toouse in your cluster, you need tooset static IP addresses for all virtual machines.</span></span> <span data-ttu-id="d2173-539">Gör detta i hello Azure Virtual Network configuration och inte i hello gästoperativsystemet.</span><span class="sxs-lookup"><span data-stu-id="d2173-539">Do this in hello Azure Virtual Network configuration, and not in hello guest operating system.</span></span>

1.  <span data-ttu-id="d2173-540">Välj i hello Azure-portalen, **resursgruppen** > **nätverkskort** > **inställningar** > **IP-adress** .</span><span class="sxs-lookup"><span data-stu-id="d2173-540">In hello Azure portal, select **Resource Group** > **Network Card** > **Settings** > **IP Address**.</span></span>
2.  <span data-ttu-id="d2173-541">På hello **IP-adresser** bladet under **tilldelning**väljer **statiska**.</span><span class="sxs-lookup"><span data-stu-id="d2173-541">On hello **IP addresses** blade, under **Assignment**, select **Static**.</span></span> <span data-ttu-id="d2173-542">I hello **IP-adress** ange hello IP-adress som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="d2173-542">In hello **IP address** box, enter hello IP address that you want toouse.</span></span>

  > [!NOTE]
  > <span data-ttu-id="d2173-543">Om du ändrar hello IP-adressen för nätverkskortet för hello måste toorestart hello Azure virtuella datorer tooapply hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="d2173-543">If you change hello IP address of hello network card, you need toorestart hello Azure virtual machines tooapply hello change.</span></span>  
  >
  >

  ![Figur 13: Ange statiska IP-adresser för hello nätverkskort för varje virtuell dator][sap-ha-guide-figure-3002]

  <span data-ttu-id="d2173-545">_**Figur 13:** ange statiska IP-adresser för hello nätverkskort för varje virtuell dator_</span><span class="sxs-lookup"><span data-stu-id="d2173-545">_**Figure 13:** Set static IP addresses for hello network card of each virtual machine_</span></span>

  <span data-ttu-id="d2173-546">Upprepa det här steget för alla nätverksgränssnitt, det vill säga för alla virtuella datorer, inklusive virtuella datorer som du vill toouse för Active Directory/DNS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2173-546">Repeat this step for all network interfaces, that is, for all virtual machines, including virtual machines that you want toouse for your Active Directory/DNS service.</span></span>

<span data-ttu-id="d2173-547">I vårt exempel har vi dessa virtuella datorer och statiska IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="d2173-547">In our example, we have these virtual machines and static IP addresses:</span></span>

| <span data-ttu-id="d2173-548">Rollen virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d2173-548">Virtual machine role</span></span> | <span data-ttu-id="d2173-549">Värdnamn för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d2173-549">Virtual machine host name</span></span> | <span data-ttu-id="d2173-550">Namn på nätverkskort</span><span class="sxs-lookup"><span data-stu-id="d2173-550">Network card name</span></span> | <span data-ttu-id="d2173-551">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="d2173-551">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d2173-552">Första SAP Application Server-instansen</span><span class="sxs-lookup"><span data-stu-id="d2173-552">First SAP Application Server instance</span></span> |<span data-ttu-id="d2173-553">PR1-di-0</span><span class="sxs-lookup"><span data-stu-id="d2173-553">pr1-di-0</span></span> |<span data-ttu-id="d2173-554">PR1-nic-di-0</span><span class="sxs-lookup"><span data-stu-id="d2173-554">pr1-nic-di-0</span></span> |<span data-ttu-id="d2173-555">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="d2173-555">10.0.0.50</span></span> |
| <span data-ttu-id="d2173-556">Andra SAP Application Server-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-556">Second SAP Application Server instance</span></span> |<span data-ttu-id="d2173-557">PR1-di-1</span><span class="sxs-lookup"><span data-stu-id="d2173-557">pr1-di-1</span></span> |<span data-ttu-id="d2173-558">PR1-nic-di-1</span><span class="sxs-lookup"><span data-stu-id="d2173-558">pr1-nic-di-1</span></span> |<span data-ttu-id="d2173-559">10.0.0.51</span><span class="sxs-lookup"><span data-stu-id="d2173-559">10.0.0.51</span></span> |
| <span data-ttu-id="d2173-560">...</span><span class="sxs-lookup"><span data-stu-id="d2173-560">...</span></span> |<span data-ttu-id="d2173-561">...</span><span class="sxs-lookup"><span data-stu-id="d2173-561">...</span></span> |<span data-ttu-id="d2173-562">...</span><span class="sxs-lookup"><span data-stu-id="d2173-562">...</span></span> |<span data-ttu-id="d2173-563">...</span><span class="sxs-lookup"><span data-stu-id="d2173-563">...</span></span> |
| <span data-ttu-id="d2173-564">Senaste SAP Application Server-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-564">Last SAP Application Server instance</span></span> |<span data-ttu-id="d2173-565">PR1-di-5</span><span class="sxs-lookup"><span data-stu-id="d2173-565">pr1-di-5</span></span> |<span data-ttu-id="d2173-566">PR1-nic-di-5</span><span class="sxs-lookup"><span data-stu-id="d2173-566">pr1-nic-di-5</span></span> |<span data-ttu-id="d2173-567">10.0.0.55</span><span class="sxs-lookup"><span data-stu-id="d2173-567">10.0.0.55</span></span> |
| <span data-ttu-id="d2173-568">Första klusternoden för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-568">First cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="d2173-569">PR1-ascs-0</span><span class="sxs-lookup"><span data-stu-id="d2173-569">pr1-ascs-0</span></span> |<span data-ttu-id="d2173-570">PR1-nic-ascs-0</span><span class="sxs-lookup"><span data-stu-id="d2173-570">pr1-nic-ascs-0</span></span> |<span data-ttu-id="d2173-571">10.0.0.40</span><span class="sxs-lookup"><span data-stu-id="d2173-571">10.0.0.40</span></span> |
| <span data-ttu-id="d2173-572">Andra klusternod för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-572">Second cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="d2173-573">PR1-ascs-1</span><span class="sxs-lookup"><span data-stu-id="d2173-573">pr1-ascs-1</span></span> |<span data-ttu-id="d2173-574">PR1-nic-ascs-1</span><span class="sxs-lookup"><span data-stu-id="d2173-574">pr1-nic-ascs-1</span></span> |<span data-ttu-id="d2173-575">10.0.0.41</span><span class="sxs-lookup"><span data-stu-id="d2173-575">10.0.0.41</span></span> |
| <span data-ttu-id="d2173-576">Första klusternoden för DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-576">First cluster node for DBMS instance</span></span> |<span data-ttu-id="d2173-577">PR1-db-0</span><span class="sxs-lookup"><span data-stu-id="d2173-577">pr1-db-0</span></span> |<span data-ttu-id="d2173-578">PR1-nic-db-0</span><span class="sxs-lookup"><span data-stu-id="d2173-578">pr1-nic-db-0</span></span> |<span data-ttu-id="d2173-579">10.0.0.30</span><span class="sxs-lookup"><span data-stu-id="d2173-579">10.0.0.30</span></span> |
| <span data-ttu-id="d2173-580">Andra klusternod för DBMS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-580">Second cluster node for DBMS instance</span></span> |<span data-ttu-id="d2173-581">PR1-db-1</span><span class="sxs-lookup"><span data-stu-id="d2173-581">pr1-db-1</span></span> |<span data-ttu-id="d2173-582">PR1-nic-db-1</span><span class="sxs-lookup"><span data-stu-id="d2173-582">pr1-nic-db-1</span></span> |<span data-ttu-id="d2173-583">10.0.0.31</span><span class="sxs-lookup"><span data-stu-id="d2173-583">10.0.0.31</span></span> |

### <span data-ttu-id="d2173-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Ange en statisk IP-adress för hello Azure interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="d2173-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Set a static IP address for hello Azure internal load balancer</span></span>

<span data-ttu-id="d2173-585">hello skapas SAP Azure Resource Manager en Azure intern belastningsutjämnare som används för hello SAP ASCS/SCS-instans och hello DBMS-klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-585">hello SAP Azure Resource Manager template creates an Azure internal load balancer that is used for hello SAP ASCS/SCS instance cluster and hello DBMS cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2173-586">hello IP-adress för hello virtuella värdnamn hello SAP ASCS/SCS är hello samma som hello IP-adressen för hello SAP ASCS/SCS intern belastningsutjämnare: **pr1-lb-ascs**.</span><span class="sxs-lookup"><span data-stu-id="d2173-586">hello IP address of hello virtual host name of hello SAP ASCS/SCS is hello same as hello IP address of hello SAP ASCS/SCS internal load balancer: **pr1-lb-ascs**.</span></span>
> <span data-ttu-id="d2173-587">hello IP-adressen för hello virtuella namnet på hello DBMS är hello samma som hello IP-adressen för hello DBMS intern belastningsutjämnare: **pr1-lb-dbms**.</span><span class="sxs-lookup"><span data-stu-id="d2173-587">hello IP address of hello virtual name of hello DBMS is hello same as hello IP address of hello DBMS internal load balancer: **pr1-lb-dbms**.</span></span>
>
>

<span data-ttu-id="d2173-588">tooset en statisk IP-adress för hello Azure interna belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="d2173-588">tooset a static IP address for hello Azure internal load balancer:</span></span>

1.  <span data-ttu-id="d2173-589">hello första distributionen anger hello interna IP-adressen för belastningsutjämnaren för**dynamiska**.</span><span class="sxs-lookup"><span data-stu-id="d2173-589">hello initial deployment sets hello internal load balancer IP address too**Dynamic**.</span></span> <span data-ttu-id="d2173-590">I hello Azure-portalen på hello **IP-adresser** bladet under **tilldelning**väljer **statiska**.</span><span class="sxs-lookup"><span data-stu-id="d2173-590">In hello Azure portal, on hello **IP addresses** blade, under **Assignment**, select **Static**.</span></span>
2.  <span data-ttu-id="d2173-591">Ange hello IP-adressen för hello intern belastningsutjämnare **pr1-lb-ascs** toohello IP-adress för hello virtuella värdnamn hello SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-591">Set hello IP address of hello internal load balancer **pr1-lb-ascs** toohello IP address of hello virtual host name of hello SAP ASCS/SCS instance.</span></span>
3.  <span data-ttu-id="d2173-592">Ange hello IP-adressen för hello intern belastningsutjämnare **pr1-lb-dbms** toohello IP-adress för hello virtuella värdnamn hello DBMS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-592">Set hello IP address of hello internal load balancer **pr1-lb-dbms** toohello IP address of hello virtual host name of hello DBMS instance.</span></span>

  ![Figur 14: Ange statiska IP-adresser för hello intern belastningsutjämnare för hello SAP ASCS/SCS-instans][sap-ha-guide-figure-3003]

  <span data-ttu-id="d2173-594">_**Figur 14:** ange statiska IP-adresser för hello intern belastningsutjämnare för hello SAP ASCS/SCS-instans_</span><span class="sxs-lookup"><span data-stu-id="d2173-594">_**Figure 14:** Set static IP addresses for hello internal load balancer for hello SAP ASCS/SCS instance_</span></span>

<span data-ttu-id="d2173-595">I vårt exempel har vi två Azure interna belastningsutjämnare som har dessa statiska IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="d2173-595">In our example, we have two Azure internal load balancers that have these static IP addresses:</span></span>

| <span data-ttu-id="d2173-596">Azure interna belastningsutjämnarrollen</span><span class="sxs-lookup"><span data-stu-id="d2173-596">Azure internal load balancer role</span></span> | <span data-ttu-id="d2173-597">Azure interna belastningsutjämnarens namn</span><span class="sxs-lookup"><span data-stu-id="d2173-597">Azure internal load balancer name</span></span> | <span data-ttu-id="d2173-598">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="d2173-598">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d2173-599">Interna belastningsutjämnare för SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-599">SAP ASCS/SCS instance internal load balancer</span></span> |<span data-ttu-id="d2173-600">ascs-lb-PR1</span><span class="sxs-lookup"><span data-stu-id="d2173-600">pr1-lb-ascs</span></span> |<span data-ttu-id="d2173-601">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="d2173-601">10.0.0.43</span></span> |
| <span data-ttu-id="d2173-602">SAP DBMS intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="d2173-602">SAP DBMS internal load balancer</span></span> |<span data-ttu-id="d2173-603">PR1-lb-dbms</span><span class="sxs-lookup"><span data-stu-id="d2173-603">pr1-lb-dbms</span></span> |<span data-ttu-id="d2173-604">10.0.0.33</span><span class="sxs-lookup"><span data-stu-id="d2173-604">10.0.0.33</span></span> |


### <span data-ttu-id="d2173-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Standard ASCS/SCS belastningsutjämningsregler för hello Azure interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="d2173-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Default ASCS/SCS load balancing rules for hello Azure internal load balancer</span></span>

<span data-ttu-id="d2173-606">hello SAP Azure Resource Manager-mall skapar hello-portar som du behöver:</span><span class="sxs-lookup"><span data-stu-id="d2173-606">hello SAP Azure Resource Manager template creates hello ports you need:</span></span>
* <span data-ttu-id="d2173-607">En ABAP ASCS-instans med hello standard instansnummer **00**</span><span class="sxs-lookup"><span data-stu-id="d2173-607">An ABAP ASCS instance, with hello default instance number **00**</span></span>
* <span data-ttu-id="d2173-608">En Java-SCS-instans med hello standard instansnummer **01**</span><span class="sxs-lookup"><span data-stu-id="d2173-608">A Java SCS instance, with hello default instance number **01**</span></span>

<span data-ttu-id="d2173-609">När du installerar din SAP ASCS/SCS-instans måste du använda hello standard instansnummer **00** ABAP ASCS-instans och hello standard instans-ID **01** för din Java SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-609">When you install your SAP ASCS/SCS instance, you must use hello default instance number **00** for your ABAP ASCS instance and hello default instance number **01** for your Java SCS instance.</span></span>

<span data-ttu-id="d2173-610">Skapa sedan nödvändiga intern belastningsutjämning slutpunkter för hello SAP NetWeaver portar.</span><span class="sxs-lookup"><span data-stu-id="d2173-610">Next, create required internal load balancing endpoints for hello SAP NetWeaver ports.</span></span>

<span data-ttu-id="d2173-611">toocreate krävs intern belastningsutjämning slutpunkter, börja med att skapa dessa slutpunkter för hello SAP NetWeaver ABAP ASCS-portar för belastningsutjämning:</span><span class="sxs-lookup"><span data-stu-id="d2173-611">toocreate required internal load balancing endpoints, first, create these load balancing endpoints for hello SAP NetWeaver ABAP ASCS ports:</span></span>

| <span data-ttu-id="d2173-612">Tjänsten/belastningen belastningsutjämning Regelnamn</span><span class="sxs-lookup"><span data-stu-id="d2173-612">Service/load balancing rule name</span></span> | <span data-ttu-id="d2173-613">Standardportnumren.</span><span class="sxs-lookup"><span data-stu-id="d2173-613">Default port numbers</span></span> | <span data-ttu-id="d2173-614">Konkret portar för (ASCS-instans med instansnummer 00) (ERS med 10)</span><span class="sxs-lookup"><span data-stu-id="d2173-614">Concrete ports for (ASCS instance with instance number 00) (ERS with 10)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d2173-615">Sätta Server / *lbrule3200*</span><span class="sxs-lookup"><span data-stu-id="d2173-615">Enqueue Server / *lbrule3200*</span></span> |<span data-ttu-id="d2173-616">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-616">32<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-617">3200</span><span class="sxs-lookup"><span data-stu-id="d2173-617">3200</span></span> |
| <span data-ttu-id="d2173-618">ABAP Message Server / *lbrule3600*</span><span class="sxs-lookup"><span data-stu-id="d2173-618">ABAP Message Server / *lbrule3600*</span></span> |<span data-ttu-id="d2173-619">36 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-619">36<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-620">3600</span><span class="sxs-lookup"><span data-stu-id="d2173-620">3600</span></span> |
| <span data-ttu-id="d2173-621">Internt ABAP meddelande / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="d2173-621">Internal ABAP Message / *lbrule3900*</span></span> |<span data-ttu-id="d2173-622">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-622">39<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-623">3900</span><span class="sxs-lookup"><span data-stu-id="d2173-623">3900</span></span> |
| <span data-ttu-id="d2173-624">Serverns HTTP-meddelandet / *Lbrule8100*</span><span class="sxs-lookup"><span data-stu-id="d2173-624">Message Server HTTP / *Lbrule8100*</span></span> |<span data-ttu-id="d2173-625">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-625">81<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-626">8100</span><span class="sxs-lookup"><span data-stu-id="d2173-626">8100</span></span> |
| <span data-ttu-id="d2173-627">SAP Start Service ASCS HTTP / *Lbrule50013*</span><span class="sxs-lookup"><span data-stu-id="d2173-627">SAP Start Service ASCS HTTP / *Lbrule50013*</span></span> |<span data-ttu-id="d2173-628">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="d2173-628">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="d2173-629">50013</span><span class="sxs-lookup"><span data-stu-id="d2173-629">50013</span></span> |
| <span data-ttu-id="d2173-630">SAP Start Service ASCS HTTPS / *Lbrule50014*</span><span class="sxs-lookup"><span data-stu-id="d2173-630">SAP Start Service ASCS HTTPS / *Lbrule50014*</span></span> |<span data-ttu-id="d2173-631">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="d2173-631">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="d2173-632">50014</span><span class="sxs-lookup"><span data-stu-id="d2173-632">50014</span></span> |
| <span data-ttu-id="d2173-633">Sätta replikering / *Lbrule50016*</span><span class="sxs-lookup"><span data-stu-id="d2173-633">Enqueue Replication / *Lbrule50016*</span></span> |<span data-ttu-id="d2173-634">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="d2173-634">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="d2173-635">50016</span><span class="sxs-lookup"><span data-stu-id="d2173-635">50016</span></span> |
| <span data-ttu-id="d2173-636">SAP Start Service ÄNDARE HTTP *Lbrule51013*</span><span class="sxs-lookup"><span data-stu-id="d2173-636">SAP Start Service ERS HTTP *Lbrule51013*</span></span> |<span data-ttu-id="d2173-637">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="d2173-637">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="d2173-638">51013</span><span class="sxs-lookup"><span data-stu-id="d2173-638">51013</span></span> |
| <span data-ttu-id="d2173-639">SAP Start Service ÄNDARE HTTP *Lbrule51014*</span><span class="sxs-lookup"><span data-stu-id="d2173-639">SAP Start Service ERS HTTP *Lbrule51014*</span></span> |<span data-ttu-id="d2173-640">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="d2173-640">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="d2173-641">51014</span><span class="sxs-lookup"><span data-stu-id="d2173-641">51014</span></span> |
| <span data-ttu-id="d2173-642">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="d2173-642">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="d2173-643">5985</span><span class="sxs-lookup"><span data-stu-id="d2173-643">5985</span></span> |
| <span data-ttu-id="d2173-644">Filresurs *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="d2173-644">File Share *Lbrule445*</span></span> | |<span data-ttu-id="d2173-645">445</span><span class="sxs-lookup"><span data-stu-id="d2173-645">445</span></span> |

<span data-ttu-id="d2173-646">_**Tabell 1:** portnummer hello SAP NetWeaver ABAP ASCS-instanser_</span><span class="sxs-lookup"><span data-stu-id="d2173-646">_**Table 1:** Port numbers of hello SAP NetWeaver ABAP ASCS instances_</span></span>

<span data-ttu-id="d2173-647">Skapa sedan dessa slutpunkter för hello SAP NetWeaver Java SCS portar för belastningsutjämning:</span><span class="sxs-lookup"><span data-stu-id="d2173-647">Then, create these load balancing endpoints for hello SAP NetWeaver Java SCS ports:</span></span>

| <span data-ttu-id="d2173-648">Tjänsten/belastningen belastningsutjämning Regelnamn</span><span class="sxs-lookup"><span data-stu-id="d2173-648">Service/load balancing rule name</span></span> | <span data-ttu-id="d2173-649">Standardportnumren.</span><span class="sxs-lookup"><span data-stu-id="d2173-649">Default port numbers</span></span> | <span data-ttu-id="d2173-650">Konkret portar för (SCS-instans med instansnummer 01) (ERS med 11)</span><span class="sxs-lookup"><span data-stu-id="d2173-650">Concrete ports for (SCS instance with instance number 01) (ERS with 11)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d2173-651">Sätta Server / *lbrule3201*</span><span class="sxs-lookup"><span data-stu-id="d2173-651">Enqueue Server / *lbrule3201*</span></span> |<span data-ttu-id="d2173-652">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-652">32<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-653">3201</span><span class="sxs-lookup"><span data-stu-id="d2173-653">3201</span></span> |
| <span data-ttu-id="d2173-654">Gateway-Server / *lbrule3301*</span><span class="sxs-lookup"><span data-stu-id="d2173-654">Gateway Server / *lbrule3301*</span></span> |<span data-ttu-id="d2173-655">33 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-655">33<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-656">3301</span><span class="sxs-lookup"><span data-stu-id="d2173-656">3301</span></span> |
| <span data-ttu-id="d2173-657">Java Message Server / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="d2173-657">Java Message Server / *lbrule3900*</span></span> |<span data-ttu-id="d2173-658">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-658">39<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-659">3901</span><span class="sxs-lookup"><span data-stu-id="d2173-659">3901</span></span> |
| <span data-ttu-id="d2173-660">Serverns HTTP-meddelandet / *Lbrule8101*</span><span class="sxs-lookup"><span data-stu-id="d2173-660">Message Server HTTP / *Lbrule8101*</span></span> |<span data-ttu-id="d2173-661">81 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="d2173-661">81<*InstanceNumber*></span></span> |<span data-ttu-id="d2173-662">8101</span><span class="sxs-lookup"><span data-stu-id="d2173-662">8101</span></span> |
| <span data-ttu-id="d2173-663">SAP Start Service SCS HTTP / *Lbrule50113*</span><span class="sxs-lookup"><span data-stu-id="d2173-663">SAP Start Service SCS HTTP / *Lbrule50113*</span></span> |<span data-ttu-id="d2173-664">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="d2173-664">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="d2173-665">50113</span><span class="sxs-lookup"><span data-stu-id="d2173-665">50113</span></span> |
| <span data-ttu-id="d2173-666">SAP Start Service SCS HTTPS / *Lbrule50114*</span><span class="sxs-lookup"><span data-stu-id="d2173-666">SAP Start Service SCS HTTPS / *Lbrule50114*</span></span> |<span data-ttu-id="d2173-667">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="d2173-667">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="d2173-668">50114</span><span class="sxs-lookup"><span data-stu-id="d2173-668">50114</span></span> |
| <span data-ttu-id="d2173-669">Sätta replikering / *Lbrule50116*</span><span class="sxs-lookup"><span data-stu-id="d2173-669">Enqueue Replication / *Lbrule50116*</span></span> |<span data-ttu-id="d2173-670">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="d2173-670">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="d2173-671">50116</span><span class="sxs-lookup"><span data-stu-id="d2173-671">50116</span></span> |
| <span data-ttu-id="d2173-672">SAP Start Service ÄNDARE HTTP *Lbrule51113*</span><span class="sxs-lookup"><span data-stu-id="d2173-672">SAP Start Service ERS HTTP *Lbrule51113*</span></span> |<span data-ttu-id="d2173-673">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="d2173-673">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="d2173-674">51113</span><span class="sxs-lookup"><span data-stu-id="d2173-674">51113</span></span> |
| <span data-ttu-id="d2173-675">SAP Start Service ÄNDARE HTTP *Lbrule51114*</span><span class="sxs-lookup"><span data-stu-id="d2173-675">SAP Start Service ERS HTTP *Lbrule51114*</span></span> |<span data-ttu-id="d2173-676">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="d2173-676">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="d2173-677">51114</span><span class="sxs-lookup"><span data-stu-id="d2173-677">51114</span></span> |
| <span data-ttu-id="d2173-678">Win RM *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="d2173-678">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="d2173-679">5985</span><span class="sxs-lookup"><span data-stu-id="d2173-679">5985</span></span> |
| <span data-ttu-id="d2173-680">Filresurs *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="d2173-680">File Share *Lbrule445*</span></span> | |<span data-ttu-id="d2173-681">445</span><span class="sxs-lookup"><span data-stu-id="d2173-681">445</span></span> |

<span data-ttu-id="d2173-682">_**Tabell 2:** portnummer hello SAP NetWeaver Java SCS instanser_</span><span class="sxs-lookup"><span data-stu-id="d2173-682">_**Table 2:** Port numbers of hello SAP NetWeaver Java SCS instances_</span></span>

![Figur 15: Standard ASCS/SCS belastningsutjämningsregler för hello Azure interna belastningsutjämnare][sap-ha-guide-figure-3004]

<span data-ttu-id="d2173-684">_**Figur 15:** standard ASCS/SCS regler för hello Azure intern belastningsutjämnare för belastningsutjämning_</span><span class="sxs-lookup"><span data-stu-id="d2173-684">_**Figure 15:** Default ASCS/SCS load balancing rules for hello Azure internal load balancer_</span></span>

<span data-ttu-id="d2173-685">Ange hello IP-adressen för belastningsutjämnaren hello **pr1-lb-dbms** toohello IP-adress för hello virtuella värdnamn hello DBMS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-685">Set hello IP address of hello load balancer **pr1-lb-dbms** toohello IP address of hello virtual host name of hello DBMS instance.</span></span>

### <span data-ttu-id="d2173-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Ändra hello ASCS/SCS standard belastningsutjämning regler för hello Azure interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="d2173-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer</span></span>

<span data-ttu-id="d2173-687">Om du vill toouse olika nummer för hello SAP ASCS eller SCS instanser, måste du ändra hello namn och värden på deras hamnar från standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="d2173-687">If you want toouse different numbers for hello SAP ASCS or SCS instances, you must change hello names and values of their ports from default values.</span></span>

1.  <span data-ttu-id="d2173-688">Markera i hello Azure-portalen,  **<* SID*> - lb - ascs läsa in belastningsutjämning ** > **regler för att läsa in belastningsutjämning**.</span><span class="sxs-lookup"><span data-stu-id="d2173-688">In hello Azure portal, select **<*SID*>-lb-ascs load balancer** > **Load Balancing Rules**.</span></span>
2.  <span data-ttu-id="d2173-689">För alla belastningsutjämningsregler som tillhör toohello SAP ASCS eller SCS-instans, ändra dessa värden:</span><span class="sxs-lookup"><span data-stu-id="d2173-689">For all load balancing rules that belong toohello SAP ASCS or SCS instance, change these values:</span></span>

  * <span data-ttu-id="d2173-690">Namn</span><span class="sxs-lookup"><span data-stu-id="d2173-690">Name</span></span>
  * <span data-ttu-id="d2173-691">Port</span><span class="sxs-lookup"><span data-stu-id="d2173-691">Port</span></span>
  * <span data-ttu-id="d2173-692">Backend-port</span><span class="sxs-lookup"><span data-stu-id="d2173-692">Back-end port</span></span>

  <span data-ttu-id="d2173-693">Om du vill toochange hello ASCS instans Standardnumret från 00 too31, måste du till exempel toomake hello ändringar för alla portar som anges i tabell 1.</span><span class="sxs-lookup"><span data-stu-id="d2173-693">For example, if you want toochange hello default ASCS instance number from 00 too31, you need toomake hello changes for all ports listed in Table 1.</span></span>

  <span data-ttu-id="d2173-694">Här är ett exempel på en uppdatering för port *lbrule3200*.</span><span class="sxs-lookup"><span data-stu-id="d2173-694">Here's an example of an update for port *lbrule3200*.</span></span>

  ![Bild 16: Ändra hello ASCS/SCS standard belastningsutjämning regler för hello Azure interna belastningsutjämnare][sap-ha-guide-figure-3005]

  <span data-ttu-id="d2173-696">_**Bild 16:** ändra hello ASCS/SCS standard belastningsutjämningsregler för hello Azure interna belastningsutjämnare_</span><span class="sxs-lookup"><span data-stu-id="d2173-696">_**Figure 16:** Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer_</span></span>

### <span data-ttu-id="d2173-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Lägg till Windows-datorer toohello domän</span><span class="sxs-lookup"><span data-stu-id="d2173-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Add Windows virtual machines toohello domain</span></span>

<span data-ttu-id="d2173-698">När du tilldelar en statisk IP-adress toohello virtuella datorer kan du lägga till hello virtuella datorer toohello domän.</span><span class="sxs-lookup"><span data-stu-id="d2173-698">After you assign a static IP address toohello virtual machines, add hello virtual machines toohello domain.</span></span>

![Figur 17: Lägga till en virtuell dator tooa domän][sap-ha-guide-figure-3006]

<span data-ttu-id="d2173-700">_**Figur 17:** lägga till en virtuell dator tooa domän_</span><span class="sxs-lookup"><span data-stu-id="d2173-700">_**Figure 17:** Add a virtual machine tooa domain_</span></span>

### <span data-ttu-id="d2173-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Lägg till registerposter på båda klusternoderna för hello SAP ASCS/SCS-instansen</span><span class="sxs-lookup"><span data-stu-id="d2173-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Add registry entries on both cluster nodes of hello SAP ASCS/SCS instance</span></span>

<span data-ttu-id="d2173-702">Azure belastningsutjämnare har en intern belastningsutjämnare att stängs anslutningar när hello anslutningar är inaktiva för en viss tid (en timeout för inaktivitet).</span><span class="sxs-lookup"><span data-stu-id="d2173-702">Azure Load Balancer has an internal load balancer that closes connections when hello connections are idle for a set period of time (an idle timeout).</span></span> <span data-ttu-id="d2173-703">SAP arbetsprocesser i dialogrutan instanser öppna anslutningar toohello SAP sätta bearbeta så snart hello första sätta/status Created begäran måste toobe skickas.</span><span class="sxs-lookup"><span data-stu-id="d2173-703">SAP work processes in dialog instances open connections toohello SAP enqueue process as soon as hello first enqueue/dequeue request needs toobe sent.</span></span> <span data-ttu-id="d2173-704">Dessa anslutningar förblir oftast etablerade tills arbetsprocessen för hello eller hello sätta processen startas om.</span><span class="sxs-lookup"><span data-stu-id="d2173-704">These connections usually remain established until hello work process or hello enqueue process restarts.</span></span> <span data-ttu-id="d2173-705">Men om hello anslutning är inaktiv under en angiven tidsperiod, hello Azure interna belastningen belastningsutjämnaren stängs hello anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d2173-705">However, if hello connection is idle for a set period of time, hello Azure internal load balancer closes hello connections.</span></span> <span data-ttu-id="d2173-706">Detta är inte ett problem Eftersom hello SAP arbetsprocessen återupprättar hello anslutningsprocessen toohello sätta om den inte längre finns.</span><span class="sxs-lookup"><span data-stu-id="d2173-706">This isn't a problem because hello SAP work process reestablishes hello connection toohello enqueue process if it no longer exists.</span></span> <span data-ttu-id="d2173-707">Dessa aktiviteter finns dokumenterade i hello developer spår av SAP processer, men de skapar en stor mängd extra innehåll i dessa spår.</span><span class="sxs-lookup"><span data-stu-id="d2173-707">These activities are documented in hello developer traces of SAP processes, but they create a large amount of extra content in those traces.</span></span> <span data-ttu-id="d2173-708">Det är en bra idé toochange hello TCP/IP `KeepAliveTime` och `KeepAliveInterval` på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="d2173-708">It's a good idea toochange hello TCP/IP `KeepAliveTime` and `KeepAliveInterval` on both cluster nodes.</span></span> <span data-ttu-id="d2173-709">Kombinera ändringarna i hello TCP/IP-parametrar med SAP profil-parametrar som beskrivs senare i hello artikeln.</span><span class="sxs-lookup"><span data-stu-id="d2173-709">Combine these changes in hello TCP/IP parameters with SAP profile parameters, described later in hello article.</span></span>

<span data-ttu-id="d2173-710">tooadd registerposter på båda klusternoderna av hello SAP ASCS/SCS instansen, Lägg först till dessa registerposter i Windows på båda klusternoderna i Windows för SAP ASCS/SCS:</span><span class="sxs-lookup"><span data-stu-id="d2173-710">tooadd registry entries on both cluster nodes of hello SAP ASCS/SCS instance, first, add these Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="d2173-711">Sökväg</span><span class="sxs-lookup"><span data-stu-id="d2173-711">Path</span></span> | <span data-ttu-id="d2173-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="d2173-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="d2173-713">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="d2173-713">Variable name</span></span> |`KeepAliveTime` |
| <span data-ttu-id="d2173-714">Variabeltyp</span><span class="sxs-lookup"><span data-stu-id="d2173-714">Variable type</span></span> |<span data-ttu-id="d2173-715">REG_DWORD (decimalt)</span><span class="sxs-lookup"><span data-stu-id="d2173-715">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="d2173-716">Värde</span><span class="sxs-lookup"><span data-stu-id="d2173-716">Value</span></span> |<span data-ttu-id="d2173-717">120000</span><span class="sxs-lookup"><span data-stu-id="d2173-717">120000</span></span> |
| <span data-ttu-id="d2173-718">Länken toodocumentation</span><span class="sxs-lookup"><span data-stu-id="d2173-718">Link toodocumentation</span></span> |[<span data-ttu-id="d2173-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span><span class="sxs-lookup"><span data-stu-id="d2173-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

<span data-ttu-id="d2173-720">_**Tabell 3:** ändra hello första TCP/IP-parameter_</span><span class="sxs-lookup"><span data-stu-id="d2173-720">_**Table 3:** Change hello first TCP/IP parameter_</span></span>

<span data-ttu-id="d2173-721">Lägg sedan till den här Windows-registerposter på båda klusternoderna i Windows för SAP ASCS/SCS:</span><span class="sxs-lookup"><span data-stu-id="d2173-721">Then, add this Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="d2173-722">Sökväg</span><span class="sxs-lookup"><span data-stu-id="d2173-722">Path</span></span> | <span data-ttu-id="d2173-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="d2173-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="d2173-724">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="d2173-724">Variable name</span></span> |`KeepAliveInterval` |
| <span data-ttu-id="d2173-725">Variabeltyp</span><span class="sxs-lookup"><span data-stu-id="d2173-725">Variable type</span></span> |<span data-ttu-id="d2173-726">REG_DWORD (decimalt)</span><span class="sxs-lookup"><span data-stu-id="d2173-726">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="d2173-727">Värde</span><span class="sxs-lookup"><span data-stu-id="d2173-727">Value</span></span> |<span data-ttu-id="d2173-728">120000</span><span class="sxs-lookup"><span data-stu-id="d2173-728">120000</span></span> |
| <span data-ttu-id="d2173-729">Länken toodocumentation</span><span class="sxs-lookup"><span data-stu-id="d2173-729">Link toodocumentation</span></span> |[<span data-ttu-id="d2173-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span><span class="sxs-lookup"><span data-stu-id="d2173-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

<span data-ttu-id="d2173-731">_**Tabell 4:** ändra hello andra TCP/IP-parameter_</span><span class="sxs-lookup"><span data-stu-id="d2173-731">_**Table 4:** Change hello second TCP/IP parameter_</span></span>

<span data-ttu-id="d2173-732">**tooapply hello ändringar, starta om båda klusternoderna**.</span><span class="sxs-lookup"><span data-stu-id="d2173-732">**tooapply hello changes, restart both cluster nodes**.</span></span>

### <span data-ttu-id="d2173-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Konfigurera ett redundanskluster i Windows Server-kluster för en SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Set up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance</span></span>

<span data-ttu-id="d2173-734">Konfigurera ett redundanskluster i Windows Server-kluster för en SAP ASCS/SCS-instans innebär att dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d2173-734">Setting up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="d2173-735">Samla in hello klusternoder i en klusterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="d2173-735">Collecting hello cluster nodes in a cluster configuration</span></span>
- <span data-ttu-id="d2173-736">Konfigurera ett filresursvittne för kluster</span><span class="sxs-lookup"><span data-stu-id="d2173-736">Configuring a cluster file share witness</span></span>

#### <span data-ttu-id="d2173-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Samla in hello klusternoder i en klusterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="d2173-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Collect hello cluster nodes in a cluster configuration</span></span>

1.  <span data-ttu-id="d2173-738">Lägga till redundansklustring tooboth klusternoder i hello Lägg till roller och funktioner.</span><span class="sxs-lookup"><span data-stu-id="d2173-738">In hello Add Role and Features Wizard, add failover clustering tooboth cluster nodes.</span></span>
2.  <span data-ttu-id="d2173-739">Ställ in hello failover-kluster med hjälp av hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="d2173-739">Set up hello failover cluster by using Failover Cluster Manager.</span></span> <span data-ttu-id="d2173-740">I hanteraren för redundanskluster, Välj **Skapa kluster**, och Lägg sedan till endast hello namn hello första kluster nod A. Lägg inte till andra hello-noden ännu; du ska lägga till hello andra noden i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="d2173-740">In Failover Cluster Manager, select **Create Cluster**, and then add only hello name of hello first cluster, node A. Do not add hello second node yet; you'll add hello second node in a later step.</span></span>

  ![Figur 18: Lägga till hello server eller virtuella namnet på hello första klusternoden][sap-ha-guide-figure-3007]

  <span data-ttu-id="d2173-742">_**Figur 18:** Lägg till hello server eller virtuella namnet på hello första klusternoden_</span><span class="sxs-lookup"><span data-stu-id="d2173-742">_**Figure 18:** Add hello server or virtual machine name of hello first cluster node_</span></span>

3.  <span data-ttu-id="d2173-743">Ange hello nätverksnamn (virtuellt värdnamn) hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-743">Enter hello network name (virtual host name) of hello cluster.</span></span>

  ![Bild 19: Ange hello klustrets namn][sap-ha-guide-figure-3008]

  <span data-ttu-id="d2173-745">_**Bild 19:** RETUR hello klustrets namn_</span><span class="sxs-lookup"><span data-stu-id="d2173-745">_**Figure 19:** Enter hello cluster name_</span></span>

4.  <span data-ttu-id="d2173-746">När du har skapat hello-kluster som kör ett klustervalideringstest.</span><span class="sxs-lookup"><span data-stu-id="d2173-746">After you've created hello cluster, run a cluster validation test.</span></span>

  ![Figur 20: Kör hello klustret verifieringen][sap-ha-guide-figure-3009]

  <span data-ttu-id="d2173-748">_**Figur 20:** kör hello klustret verifieringen_</span><span class="sxs-lookup"><span data-stu-id="d2173-748">_**Figure 20:** Run hello cluster validation check_</span></span>

  <span data-ttu-id="d2173-749">Du kan ignorera alla varningar om diskarna nu hello pågår.</span><span class="sxs-lookup"><span data-stu-id="d2173-749">You can ignore any warnings about disks at this point in hello process.</span></span> <span data-ttu-id="d2173-750">Du ska lägga till en filresurs och hello SIOS delade diskar senare.</span><span class="sxs-lookup"><span data-stu-id="d2173-750">You'll add a file share witness and hello SIOS shared disks later.</span></span> <span data-ttu-id="d2173-751">I det här skedet behöver du inte tooworry om ha ett kvorum.</span><span class="sxs-lookup"><span data-stu-id="d2173-751">At this stage, you don't need tooworry about having a quorum.</span></span>

  ![Figur 21: Inga kvorumdisken hittas][sap-ha-guide-figure-3010]

  <span data-ttu-id="d2173-753">_**Figur 21:** inga kvorumdisken hittas_</span><span class="sxs-lookup"><span data-stu-id="d2173-753">_**Figure 21:** No quorum disk is found_</span></span>

  ![Figur 22: Core klusterresurs måste en ny IP-adress][sap-ha-guide-figure-3011]

  <span data-ttu-id="d2173-755">_**Figur 22:** Core klusterresurs måste en ny IP-adress_</span><span class="sxs-lookup"><span data-stu-id="d2173-755">_**Figure 22:** Core cluster resource needs a new IP address_</span></span>

5.  <span data-ttu-id="d2173-756">Ändra hello IP-adressen för hello core klustertjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2173-756">Change hello IP address of hello core cluster service.</span></span> <span data-ttu-id="d2173-757">hello kluster kan inte starta tills du ändrar hello IP-adressen för hello core klustertjänsten, eftersom hello hello-serverns IP-adress pekar tooone hello virtuella noder.</span><span class="sxs-lookup"><span data-stu-id="d2173-757">hello cluster can't start until you change hello IP address of hello core cluster service, because hello IP address of hello server points tooone of hello virtual machine nodes.</span></span> <span data-ttu-id="d2173-758">Gör detta på hello **egenskaper** sidan hello core klustertjänstens IP-resurs.</span><span class="sxs-lookup"><span data-stu-id="d2173-758">Do this on hello **Properties** page of hello core cluster service's IP resource.</span></span>

  <span data-ttu-id="d2173-759">Exempelvis måste tooassign en IP-adress (i vårt exempel **10.0.0.42**) för hello klustrets virtuella värdnamn **pr1-ascs-vir**.</span><span class="sxs-lookup"><span data-stu-id="d2173-759">For example, we need tooassign an IP address (in our example, **10.0.0.42**) for hello cluster virtual host name **pr1-ascs-vir**.</span></span>

  ![Figur 23: Hello Egenskaper dialogrutan Ändra hello IP-adress][sap-ha-guide-figure-3012]

  <span data-ttu-id="d2173-761">_**Figur 23:** i hello **egenskaper** dialogrutan, ändra hello IP-adress_</span><span class="sxs-lookup"><span data-stu-id="d2173-761">_**Figure 23:** In hello **Properties** dialog box, change hello IP address_</span></span>

  ![Figur 24: Tilldela hello IP-adress som är reserverad för hello kluster][sap-ha-guide-figure-3013]

  <span data-ttu-id="d2173-763">_**Figur 24:** tilldela hello IP-adress som är reserverad för hello kluster_</span><span class="sxs-lookup"><span data-stu-id="d2173-763">_**Figure 24:** Assign hello IP address that is reserved for hello cluster_</span></span>

6.  <span data-ttu-id="d2173-764">Sätta hello virtuell värd klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="d2173-764">Bring hello cluster virtual host name online.</span></span>

  ![Bild 25: Core klustertjänsten är igång och körs och med hello rätt IP-adress][sap-ha-guide-figure-3014]

  <span data-ttu-id="d2173-766">_**Bild 25:** core klustertjänsten är igång och körs, och med hello rätt IP-adress_</span><span class="sxs-lookup"><span data-stu-id="d2173-766">_**Figure 25:** Cluster core service is up and running, and with hello correct IP address_</span></span>

7.  <span data-ttu-id="d2173-767">Lägga till andra hello-klusternod.</span><span class="sxs-lookup"><span data-stu-id="d2173-767">Add hello second cluster node.</span></span>

  <span data-ttu-id="d2173-768">Nu när hello core klustertjänsten är igång, du kan lägga till hello andra noder.</span><span class="sxs-lookup"><span data-stu-id="d2173-768">Now that hello core cluster service is up and running, you can add hello second cluster node.</span></span>

  ![Bild 26: Lägga till hello andra klusternod][sap-ha-guide-figure-3015]

  <span data-ttu-id="d2173-770">_**Bild 26:** Lägg till hello andra klusternod_</span><span class="sxs-lookup"><span data-stu-id="d2173-770">_**Figure 26:** Add hello second cluster node_</span></span>

8.  <span data-ttu-id="d2173-771">Ange ett namn för hello andra nod klustervärd.</span><span class="sxs-lookup"><span data-stu-id="d2173-771">Enter a name for hello second cluster node host.</span></span>

  ![Bild 27: Ange hello andra värden klusternodnamnet][sap-ha-guide-figure-3016]

  <span data-ttu-id="d2173-773">_**Bild 27:** RETUR hello andra värden klusternodnamnet_</span><span class="sxs-lookup"><span data-stu-id="d2173-773">_**Figure 27:** Enter hello second cluster node host name_</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="d2173-774">Måste du se till att hello **lägga till alla tillgängliga lagringsenheter toohello klustret** kryssrutan är **inte** valda.</span><span class="sxs-lookup"><span data-stu-id="d2173-774">Be sure that hello **Add all eligible storage toohello cluster** check box is **NOT** selected.</span></span>  
  >
  >

  ![Bild 28: Markera inte kryssrutan hello][sap-ha-guide-figure-3017]

  <span data-ttu-id="d2173-776">_**Bild 28:** gör **inte** Välj hello kryssrutan_</span><span class="sxs-lookup"><span data-stu-id="d2173-776">_**Figure 28:** Do **not** select hello check box_</span></span>

  <span data-ttu-id="d2173-777">Du kan ignorera varningar om kvorum och diskar.</span><span class="sxs-lookup"><span data-stu-id="d2173-777">You can ignore warnings about quorum and disks.</span></span> <span data-ttu-id="d2173-778">Du måste ställa in hello kvorum och dela hello senare, enligt beskrivningen i [installerar SIOS DataKeeper Cluster Edition för SAP ASCS/SCS klusterdisk resursen][sap-ha-guide-8.12.3].</span><span class="sxs-lookup"><span data-stu-id="d2173-778">You'll set hello quorum and share hello disk later, as described in [Installing SIOS DataKeeper Cluster Edition for SAP ASCS/SCS cluster share disk][sap-ha-guide-8.12.3].</span></span>

  ![Bild 29: Ignorera varningar om hello disk kvorum][sap-ha-guide-figure-3018]

  <span data-ttu-id="d2173-780">_**Bild 29:** Ignorera varningar om hello disk kvorum_</span><span class="sxs-lookup"><span data-stu-id="d2173-780">_**Figure 29:** Ignore warnings about hello disk quorum_</span></span>


#### <span data-ttu-id="d2173-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Konfigurera ett filresursvittne för kluster</span><span class="sxs-lookup"><span data-stu-id="d2173-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Configure a cluster file share witness</span></span>

<span data-ttu-id="d2173-782">När du konfigurerar ett kluster filresursvittne inkluderar dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="d2173-782">Configuring a cluster file share witness involves these tasks:</span></span>

- <span data-ttu-id="d2173-783">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="d2173-783">Creating a file share</span></span>
- <span data-ttu-id="d2173-784">Inställningen hello filen filresurs vittne kvorum i hanteraren för redundanskluster</span><span class="sxs-lookup"><span data-stu-id="d2173-784">Setting hello file share witness quorum in Failover Cluster Manager</span></span>

##### <span data-ttu-id="d2173-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="d2173-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Create a file share</span></span>

1.  <span data-ttu-id="d2173-786">Välj ett filresursvittne i stället för en kvorumdisk.</span><span class="sxs-lookup"><span data-stu-id="d2173-786">Select a file share witness instead of a quorum disk.</span></span> <span data-ttu-id="d2173-787">SIOS DataKeeper stöder det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="d2173-787">SIOS DataKeeper supports this option.</span></span>

  <span data-ttu-id="d2173-788">I hello exemplen i den här artikeln är hello filresursvittne på hello Active Directory/DNS-server som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-788">In hello examples in this article, hello file share witness is on hello Active Directory/DNS server that is running in Azure.</span></span> <span data-ttu-id="d2173-789">hello filresursvittne kallas **domcontr 0**.</span><span class="sxs-lookup"><span data-stu-id="d2173-789">hello file share witness is called **domcontr-0**.</span></span> <span data-ttu-id="d2173-790">Eftersom du skulle har konfigurerat ett VPN-anslutningen tooAzure (via plats-till-plats-VPN eller Azure ExpressRoute) kan dela din Active Directory/DNS-tjänsten är lokalt och inte är en fil med lämpliga toorun vittne.</span><span class="sxs-lookup"><span data-stu-id="d2173-790">Because you would have configured a VPN connection tooAzure (via Site-to-Site VPN or Azure ExpressRoute), your Active Directory/DNS service is on-premises and isn't suitable toorun a file share witness.</span></span>

  > [!NOTE]
  > <span data-ttu-id="d2173-791">Om Active Directory/DNS-tjänsten körs bara lokalt, inte konfigurera din filresursvittne på hello Active Directory/DNS-Windows-operativsystem som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="d2173-791">If your Active Directory/DNS service runs only on-premises, don't configure your file share witness on hello Active Directory/DNS Windows operating system that is running on-premises.</span></span> <span data-ttu-id="d2173-792">Nätverksfördröjningen mellan klusternoder som körs i Azure och Active Directory/DNS lokalt kan vara för stort och orsaka problem med nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="d2173-792">Network latency between cluster nodes running in Azure and Active Directory/DNS on-premises might be too large and cause connectivity issues.</span></span> <span data-ttu-id="d2173-793">Vara att tooconfigure hello filresursvittne på en virtuell Azure-dator som kör Stäng toohello klusternod.</span><span class="sxs-lookup"><span data-stu-id="d2173-793">Be sure tooconfigure hello file share witness on an Azure virtual machine that is running close toohello cluster node.</span></span>  
  >
  >

  <span data-ttu-id="d2173-794">hello kvorum enhet behöver minst 1 024 MB ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="d2173-794">hello quorum drive needs at least 1,024 MB of free space.</span></span> <span data-ttu-id="d2173-795">Vi rekommenderar 2 048 MB ledigt utrymme för hello kvorum enhet.</span><span class="sxs-lookup"><span data-stu-id="d2173-795">We recommend 2,048 MB of free space for hello quorum drive.</span></span>

2.  <span data-ttu-id="d2173-796">Lägg till hello klusternamnobjekt.</span><span class="sxs-lookup"><span data-stu-id="d2173-796">Add hello cluster name object.</span></span>

  ![Bild 30: Tilldela hello behörigheter på hello resurs för hello klusternamnobjekt][sap-ha-guide-figure-3019]

  <span data-ttu-id="d2173-798">_**Bild 30:** tilldela hello behörigheter på hello resurs för hello klusternamnobjekt_</span><span class="sxs-lookup"><span data-stu-id="d2173-798">_**Figure 30:** Assign hello permissions on hello share for hello cluster name object_</span></span>

  <span data-ttu-id="d2173-799">Se till att hello behörigheter inkluderar hello myndigheten toochange data i hello resurs för hello klusternamnobjekt (i vårt exempel **pr1-ascs-vir$**).</span><span class="sxs-lookup"><span data-stu-id="d2173-799">Be sure that hello permissions include hello authority toochange data in hello share for hello cluster name object (in our example, **pr1-ascs-vir$**).</span></span>

3.  <span data-ttu-id="d2173-800">tooadd hello klustrets namn toohello objektlistan väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2173-800">tooadd hello cluster name object toohello list, select **Add**.</span></span> <span data-ttu-id="d2173-801">Ändra hello filter toocheck för datorobjekt i tillägg toothose visas i bild 31.</span><span class="sxs-lookup"><span data-stu-id="d2173-801">Change hello filter toocheck for computer objects, in addition toothose shown in Figure 31.</span></span>

  ![Bild 31: Ändra hello objekttyper tooinclude datorer][sap-ha-guide-figure-3020]

  <span data-ttu-id="d2173-803">_**Bild 31:** ändra hello objekttyper tooinclude datorer_</span><span class="sxs-lookup"><span data-stu-id="d2173-803">_**Figure 31:** Change hello Object Types tooinclude computers_</span></span>

  ![Bild 32: Kryssrutan hello datorer][sap-ha-guide-figure-3021]

  <span data-ttu-id="d2173-805">_**Bild 32:** väljer hello **datorer** kryssruta_</span><span class="sxs-lookup"><span data-stu-id="d2173-805">_**Figure 32:** Select hello **Computers** check box_</span></span>

4.  <span data-ttu-id="d2173-806">Ange hello klusternamnobjekt som visas i bild 31.</span><span class="sxs-lookup"><span data-stu-id="d2173-806">Enter hello cluster name object as shown in Figure 31.</span></span> <span data-ttu-id="d2173-807">Eftersom hello post har redan har skapats och kan du ändra hello behörigheter som visas i bild 30.</span><span class="sxs-lookup"><span data-stu-id="d2173-807">Because hello record has already been created, you can change hello permissions, as shown in Figure 30.</span></span>

5.  <span data-ttu-id="d2173-808">Välj hello **säkerhet** fliken hello resursen och ange sedan mer detaljerade behörigheter för hello klusternamnobjekt.</span><span class="sxs-lookup"><span data-stu-id="d2173-808">Select hello **Security** tab of hello share, and then set more detailed permissions for hello cluster name object.</span></span>

  ![Bild 33: Ange hello säkerhetsattribut för hello klusternamnobjekt på hello filen resursen kvorum][sap-ha-guide-figure-3022]

  <span data-ttu-id="d2173-810">_**Bild 33:** ange hello säkerhetsattribut för hello klusternamnobjekt på hello filen resursen kvorum_</span><span class="sxs-lookup"><span data-stu-id="d2173-810">_**Figure 33:** Set hello security attributes for hello cluster name object on hello file share quorum_</span></span>

##### <span data-ttu-id="d2173-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Ange hello filen filresurs vittne kvorum i hanteraren för redundanskluster</span><span class="sxs-lookup"><span data-stu-id="d2173-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Set hello file share witness quorum in Failover Cluster Manager</span></span>

1.  <span data-ttu-id="d2173-812">Öppna hello konfigurationsguiden kvorum inställningen.</span><span class="sxs-lookup"><span data-stu-id="d2173-812">Open hello Configure Quorum Setting Wizard.</span></span>

  ![Bild 34: Starta hello inställningen guiden Konfigurera klusterkvorum][sap-ha-guide-figure-3023]

  <span data-ttu-id="d2173-814">_**Bild 34:** Start hello konfigurera inställningen guiden klusterkvorum_</span><span class="sxs-lookup"><span data-stu-id="d2173-814">_**Figure 34:** Start hello Configure Cluster Quorum Setting Wizard_</span></span>

2.  <span data-ttu-id="d2173-815">På hello **Välj kvorumkonfigurationen** väljer **Välj kvorumvittne hello**.</span><span class="sxs-lookup"><span data-stu-id="d2173-815">On hello **Select Quorum Configuration** page, select **Select hello quorum witness**.</span></span>

  ![Bild 35: Kvorumkonfigurationer som du kan välja mellan][sap-ha-guide-figure-3024]

  <span data-ttu-id="d2173-817">_**Bild 35:** kvorumkonfigurationer som du kan välja mellan_</span><span class="sxs-lookup"><span data-stu-id="d2173-817">_**Figure 35:** Quorum configurations you can choose from_</span></span>

3.  <span data-ttu-id="d2173-818">På hello **Välj Kvorumvittne** väljer **konfigurera ett filresursvittne**.</span><span class="sxs-lookup"><span data-stu-id="d2173-818">On hello **Select Quorum Witness** page, select **Configure a file share witness**.</span></span>

  ![Bild 36: Välj hello filresursvittne][sap-ha-guide-figure-3025]

  <span data-ttu-id="d2173-820">_**Bild 36:** Välj hello filresursvittne_</span><span class="sxs-lookup"><span data-stu-id="d2173-820">_**Figure 36:** Select hello file share witness_</span></span>

4.  <span data-ttu-id="d2173-821">Ange hello UNC-sökvägen toohello filresurs (i vårt exempel \\domcontr 0\FSW).</span><span class="sxs-lookup"><span data-stu-id="d2173-821">Enter hello UNC path toohello file share (in our example, \\domcontr-0\FSW).</span></span> <span data-ttu-id="d2173-822">toosee en lista över hello ändringar du kan göra, Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d2173-822">toosee a list of hello changes you can make, select **Next**.</span></span>

  ![Bild 37: Definiera hello filresursplats för hello vittne resurs][sap-ha-guide-figure-3026]

  <span data-ttu-id="d2173-824">_**Bild 37:** definiera hello filresursplats för hello vittne resurs_</span><span class="sxs-lookup"><span data-stu-id="d2173-824">_**Figure 37:** Define hello file share location for hello witness share_</span></span>

5.  <span data-ttu-id="d2173-825">Välj hello ändringar och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d2173-825">Select hello changes you want, and then select **Next**.</span></span> <span data-ttu-id="d2173-826">Du behöver toosuccessfully omkonfigurera hello klusterkonfigurationen som visas i figur 38.</span><span class="sxs-lookup"><span data-stu-id="d2173-826">You need toosuccessfully reconfigure hello cluster configuration as shown in Figure 38.</span></span>  

  ![Bild 38: Bekräftelse att du har konfigurerat om hello kluster][sap-ha-guide-figure-3027]

  <span data-ttu-id="d2173-828">_**Bild 38:** bekräftelse att du har konfigurerat om hello kluster_</span><span class="sxs-lookup"><span data-stu-id="d2173-828">_**Figure 38:** Confirmation that you've reconfigured hello cluster_</span></span>

<span data-ttu-id="d2173-829">När du har installerat hello redundanskluster i Windows har förändringar som måste toobe görs toosome tröskelvärden tooadapt redundans identifiering tooconditions i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-829">After installing hello Windows Failover Cluster successfully, changes need toobe made toosome thresholds tooadapt failover detection tooconditions in Azure.</span></span> <span data-ttu-id="d2173-830">hello parametrar toobe ändras dokumenteras i den här bloggen: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/.</span><span class="sxs-lookup"><span data-stu-id="d2173-830">hello parameters toobe changed are documented in this blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ .</span></span> <span data-ttu-id="d2173-831">Förutsatt att din två virtuella datorer som skapa hello är Windows klusterkonfigurationen för ASCS/SCS i Hej samma undernät, hello följande parametrar måste toobe ändras toothese värden:</span><span class="sxs-lookup"><span data-stu-id="d2173-831">Assuming that your two VMs that build hello Windows Cluster Configuration for ASCS/SCS are in hello same SubNet, hello following parameters need toobe changed toothese values:</span></span>
- <span data-ttu-id="d2173-832">SameSubNetDelay = 2</span><span class="sxs-lookup"><span data-stu-id="d2173-832">SameSubNetDelay = 2</span></span>
- <span data-ttu-id="d2173-833">SameSubNetThreshold = 15</span><span class="sxs-lookup"><span data-stu-id="d2173-833">SameSubNetThreshold = 15</span></span>

<span data-ttu-id="d2173-834">Dessa inställningar har testats med kunder och tillhandahålls en bra kompromiss toobe tillräckligt flexibla på ena sidan av hello.</span><span class="sxs-lookup"><span data-stu-id="d2173-834">These settings were tested with customers and provided a good compromise toobe resilient enough on hello one side.</span></span> <span data-ttu-id="d2173-835">På hello andra hand dessa inställningar har att erbjuda snabb tillräckligt med redundans i verkliga felvillkor på SAP programvara eller nod och VM-fel.</span><span class="sxs-lookup"><span data-stu-id="d2173-835">On hello other hand those settings were providing fast enough failover in real error conditions on SAP software or node/VM failure.</span></span> 

### <span data-ttu-id="d2173-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Installera SIOS DataKeeper Cluster Edition för hello SAP ASCS/SCS klusterdisk resursen</span><span class="sxs-lookup"><span data-stu-id="d2173-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Install SIOS DataKeeper Cluster Edition for hello SAP ASCS/SCS cluster share disk</span></span>

<span data-ttu-id="d2173-837">Nu har du en fungerande konfiguration för Windows Server Failover Clustering i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-837">You now have a working Windows Server Failover Clustering configuration in Azure.</span></span> <span data-ttu-id="d2173-838">Men tooinstall en SAP ASCS/SCS-instans, behöver du en delad disk-resurs.</span><span class="sxs-lookup"><span data-stu-id="d2173-838">But, tooinstall an SAP ASCS/SCS instance, you need a shared disk resource.</span></span> <span data-ttu-id="d2173-839">Du kan inte skapa hello delad diskresurser som du behöver i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-839">You cannot create hello shared disk resources you need in Azure.</span></span> <span data-ttu-id="d2173-840">SIOS DataKeeper Cluster Edition är en lösning för från tredje part som du kan använda delade toocreate diskresurser.</span><span class="sxs-lookup"><span data-stu-id="d2173-840">SIOS DataKeeper Cluster Edition is a third-party solution you can use toocreate shared disk resources.</span></span>

<span data-ttu-id="d2173-841">Installera SIOS DataKeeper Cluster Edition för hello SAP ASCS/SCS omfattar resursen klusterdisk dessa aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="d2173-841">Installing SIOS DataKeeper Cluster Edition for hello SAP ASCS/SCS cluster share disk involves these tasks:</span></span>

- <span data-ttu-id="d2173-842">Lägga till hello .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="d2173-842">Adding hello .NET Framework 3.5</span></span>
- <span data-ttu-id="d2173-843">Installera SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="d2173-843">Installing SIOS DataKeeper</span></span>
- <span data-ttu-id="d2173-844">Ställa in SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="d2173-844">Setting up SIOS DataKeeper</span></span>

#### <span data-ttu-id="d2173-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Lägg till hello .NET Framework 3.5</span><span class="sxs-lookup"><span data-stu-id="d2173-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Add hello .NET Framework 3.5</span></span>
<span data-ttu-id="d2173-846">hello Microsoft .NET Framework 3.5 är inte automatiskt aktiverats eller installerats på Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="d2173-846">hello Microsoft .NET Framework 3.5 isn't automatically activated or installed on Windows Server 2012 R2.</span></span> <span data-ttu-id="d2173-847">Eftersom SIOS DataKeeper kräver hello .NET Framework toobe på alla noder som du installerar DataKeeper på, måste du installera hello .NET Framework 3.5 hello gästoperativsystemet för alla virtuella datorer i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="d2173-847">Because SIOS DataKeeper requires hello .NET Framework toobe on all nodes that you install DataKeeper on, you must install hello .NET Framework 3.5 on hello guest operating system of all virtual machines in hello cluster.</span></span>

<span data-ttu-id="d2173-848">Det finns två sätt tooadd hello .NET Framework 3.5:</span><span class="sxs-lookup"><span data-stu-id="d2173-848">There are two ways tooadd hello .NET Framework 3.5:</span></span>

- <span data-ttu-id="d2173-849">Använd guiden hello Lägg till roller och funktioner i Windows som visas i bild 39.</span><span class="sxs-lookup"><span data-stu-id="d2173-849">Use hello Add Roles and Features Wizard in Windows as shown in Figure 39.</span></span>

  ![Bild 39: Installera hello .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner][sap-ha-guide-figure-3028]

  <span data-ttu-id="d2173-851">_**Bild 39:** hello installera .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner_</span><span class="sxs-lookup"><span data-stu-id="d2173-851">_**Figure 39:** Install hello .NET Framework 3.5 by using hello Add Roles and Features Wizard_</span></span>

  ![Bild 40: Installationen förloppsindikator när du installerar hello .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner][sap-ha-guide-figure-3029]

  <span data-ttu-id="d2173-853">_**Bild 40:** Installation förloppsindikator när du installerar hello .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner_</span><span class="sxs-lookup"><span data-stu-id="d2173-853">_**Figure 40:** Installation progress bar when you install hello .NET Framework 3.5 by using hello Add Roles and Features Wizard_</span></span>

- <span data-ttu-id="d2173-854">Använd hello kommandoradsverktyget dism.exe.</span><span class="sxs-lookup"><span data-stu-id="d2173-854">Use hello command-line tool dism.exe.</span></span> <span data-ttu-id="d2173-855">För den här typen av installation måste tooaccess hello SxS katalog på hello Windows-installationsmediet.</span><span class="sxs-lookup"><span data-stu-id="d2173-855">For this type of installation, you need tooaccess hello SxS directory on hello Windows installation media.</span></span> <span data-ttu-id="d2173-856">Skriv följande vid en upphöjd kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="d2173-856">At an elevated command prompt, type:</span></span>

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <span data-ttu-id="d2173-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a>Installera SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="d2173-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper</span></span>

<span data-ttu-id="d2173-858">Installera SIOS DataKeeper Cluster Edition på varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="d2173-858">Install SIOS DataKeeper Cluster Edition on each node in hello cluster.</span></span> <span data-ttu-id="d2173-859">toocreate virtuella delad lagring med SIOS DataKeeper, skapa en synkroniserad spegling och simulera delad klusterlagring.</span><span class="sxs-lookup"><span data-stu-id="d2173-859">toocreate virtual shared storage with SIOS DataKeeper, create a synced mirror and then simulate cluster shared storage.</span></span>

<span data-ttu-id="d2173-860">Innan du installerar hello SIOS programvara skapa hello domänanvändare **DataKeeperSvc**.</span><span class="sxs-lookup"><span data-stu-id="d2173-860">Before you install hello SIOS software, create hello domain user **DataKeeperSvc**.</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-861">Lägg till hello **DataKeeperSvc** användare toohello **lokal administratör** på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="d2173-861">Add hello **DataKeeperSvc** user toohello **Local Administrator** group on both cluster nodes.</span></span>
>
>

<span data-ttu-id="d2173-862">tooinstall SIOS DataKeeper:</span><span class="sxs-lookup"><span data-stu-id="d2173-862">tooinstall SIOS DataKeeper:</span></span>

1.  <span data-ttu-id="d2173-863">Installera hello SIOS på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="d2173-863">Install hello SIOS software on both cluster nodes.</span></span>

  ![SIOS installer][sap-ha-guide-figure-3030]

  ![Bild 41: Första sidan i hello SIOS DataKeeper installation][sap-ha-guide-figure-3031]

  <span data-ttu-id="d2173-866">_**Bild 41:** första sidan i hello SIOS DataKeeper installation_</span><span class="sxs-lookup"><span data-stu-id="d2173-866">_**Figure 41:** First page of hello SIOS DataKeeper installation_</span></span>

2.  <span data-ttu-id="d2173-867">Välj hello dialogrutan visas i bild 42, **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d2173-867">In hello dialog box shown in Figure 42, select **Yes**.</span></span>

  ![Bild 42: DataKeeper informerar dig om att en tjänst kommer att inaktiveras][sap-ha-guide-figure-3032]

  <span data-ttu-id="d2173-869">_**Bild 42:** DataKeeper informerar dig om att en tjänst kommer att inaktiveras_</span><span class="sxs-lookup"><span data-stu-id="d2173-869">_**Figure 42:** DataKeeper informs you that a service will be disabled_</span></span>

3.  <span data-ttu-id="d2173-870">Vi rekommenderar att du väljer hello i dialogrutan visas i bild 43 **domän eller Server konto**.</span><span class="sxs-lookup"><span data-stu-id="d2173-870">In hello dialog box shown in Figure 43, we recommend that you select **Domain or Server account**.</span></span>

  ![Bild 43: Användarens val för SIOS DataKeeper][sap-ha-guide-figure-3033]

  <span data-ttu-id="d2173-872">_**Bild 43:** användarens val för SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="d2173-872">_**Figure 43:** User selection for SIOS DataKeeper_</span></span>

4.  <span data-ttu-id="d2173-873">Ange hello domän kontots användarnamn och lösenord som du skapade för SIOS DataKeeper.</span><span class="sxs-lookup"><span data-stu-id="d2173-873">Enter hello domain account user name and passwords that you created for SIOS DataKeeper.</span></span>

  ![Bild 44: Ange hello domänanvändarnamn och lösenord för hello SIOS DataKeeper installation][sap-ha-guide-figure-3034]

  <span data-ttu-id="d2173-875">_**Bild 44:** ange hello domänanvändarnamn och lösenord för hello SIOS DataKeeper installation_</span><span class="sxs-lookup"><span data-stu-id="d2173-875">_**Figure 44:** Enter hello domain user name and password for hello SIOS DataKeeper installation_</span></span>

5.  <span data-ttu-id="d2173-876">Installera hello licensnyckel för din SIOS DataKeeper-instans som visas i bild 45.</span><span class="sxs-lookup"><span data-stu-id="d2173-876">Install hello license key for your SIOS DataKeeper instance as shown in Figure 45.</span></span>

  ![Bild 45: Ange licensnyckeln SIOS DataKeeper][sap-ha-guide-figure-3035]

  <span data-ttu-id="d2173-878">_**Bild 45:** Ange licensnyckeln SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="d2173-878">_**Figure 45:** Enter your SIOS DataKeeper license key_</span></span>

6.  <span data-ttu-id="d2173-879">När du uppmanas starta om hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2173-879">When prompted, restart hello virtual machine.</span></span>

#### <span data-ttu-id="d2173-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Ställ in SIOS DataKeeper</span><span class="sxs-lookup"><span data-stu-id="d2173-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Set up SIOS DataKeeper</span></span>

<span data-ttu-id="d2173-881">När du installerar SIOS DataKeeper på båda noderna, måste toostart hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d2173-881">After you install SIOS DataKeeper on both nodes, you need toostart hello configuration.</span></span> <span data-ttu-id="d2173-882">hello målet med hello konfiguration är toohave synkron replikering mellan hello ytterligare virtuella hårddiskar anslutna tooeach hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-882">hello goal of hello configuration is toohave synchronous data replication between hello additional VHDs attached tooeach of hello virtual machines.</span></span>

1.  <span data-ttu-id="d2173-883">Starta hello DataKeeper hantering och verktyg och välj sedan **ansluta servern**.</span><span class="sxs-lookup"><span data-stu-id="d2173-883">Start hello DataKeeper Management and Configuration tool, and then select **Connect Server**.</span></span> <span data-ttu-id="d2173-884">(I figur 46 det här alternativet är i en röd cirkel.)</span><span class="sxs-lookup"><span data-stu-id="d2173-884">(In Figure 46, this option is circled in red.)</span></span>

  ![Bild 46: SIOS DataKeeper hantering och verktyg][sap-ha-guide-figure-3036]

  <span data-ttu-id="d2173-886">_**Bild 46:** SIOS DataKeeper hantering och konfiguration av verktyget_</span><span class="sxs-lookup"><span data-stu-id="d2173-886">_**Figure 46:** SIOS DataKeeper Management and Configuration tool_</span></span>

2.  <span data-ttu-id="d2173-887">Ange hello namn eller TCP/IP-adressen för hello första nod hello hantering och konfiguration verktyget ska ansluta till, och i andra steget hello andra noden.</span><span class="sxs-lookup"><span data-stu-id="d2173-887">Enter hello name or TCP/IP address of hello first node hello Management and Configuration tool should connect to, and, in a second step, hello second node.</span></span>

  ![Bild 47: Infoga hello namn eller TCP/IP-adress för hello första nod hello hantering och konfiguration verktyget ska ansluta till, och i andra steget hello andra noden][sap-ha-guide-figure-3037]

  <span data-ttu-id="d2173-889">_**Bild 47:** Insert hello namn eller TCP/IP-adress för hello första nod hello hantering och konfiguration verktyget ska ansluta till och i andra steget hello andra noden_</span><span class="sxs-lookup"><span data-stu-id="d2173-889">_**Figure 47:** Insert hello name or TCP/IP address of hello first node hello Management and Configuration tool should connect to, and in a second step, hello second node_</span></span>

3.  <span data-ttu-id="d2173-890">Skapa hello replikering mellan hello två noder.</span><span class="sxs-lookup"><span data-stu-id="d2173-890">Create hello replication job between hello two nodes.</span></span>

  ![Bild 48: Skapa ett jobb för replikering][sap-ha-guide-figure-3038]

  <span data-ttu-id="d2173-892">_**Bild 48:** skapa ett jobb för replikering_</span><span class="sxs-lookup"><span data-stu-id="d2173-892">_**Figure 48:** Create a replication job_</span></span>

  <span data-ttu-id="d2173-893">En guide hjälper dig att hello processen att skapa ett jobb för replikering.</span><span class="sxs-lookup"><span data-stu-id="d2173-893">A wizard guides you through hello process of creating a replication job.</span></span>
4.  <span data-ttu-id="d2173-894">Definiera hello namn, TCP/IP-adress och diskvolymen för hello Källnoden.</span><span class="sxs-lookup"><span data-stu-id="d2173-894">Define hello name, TCP/IP address, and disk volume of hello source node.</span></span>

  ![Bild 49: Definiera hello namnet på hello replikering jobb][sap-ha-guide-figure-3039]

  <span data-ttu-id="d2173-896">_**Bild 49:** definiera hello namnet på hello replikering jobb_</span><span class="sxs-lookup"><span data-stu-id="d2173-896">_**Figure 49:** Define hello name of hello replication job_</span></span>

  ![Bild 50: Definiera hello grunddata för hello nod som ska vara hello aktuella Källnoden][sap-ha-guide-figure-3040]

  <span data-ttu-id="d2173-898">_**Bild 50:** definiera hello grunddata för hello nod som ska vara hello aktuella Källnoden_</span><span class="sxs-lookup"><span data-stu-id="d2173-898">_**Figure 50:** Define hello base data for hello node, which should be hello current source node_</span></span>

5.  <span data-ttu-id="d2173-899">Definiera hello namn, TCP/IP-adress och volymen av hello målnoden.</span><span class="sxs-lookup"><span data-stu-id="d2173-899">Define hello name, TCP/IP address, and disk volume of hello target node.</span></span>

  ![Bild av 51: Definiera hello grunddata för hello nod som ska vara hello aktuella målnod][sap-ha-guide-figure-3041]

  <span data-ttu-id="d2173-901">_**Bild av 51:** definiera hello grunddata för hello nod som ska vara hello aktuella målnod_</span><span class="sxs-lookup"><span data-stu-id="d2173-901">_**Figure 51:** Define hello base data for hello node, which should be hello current target node_</span></span>

6.  <span data-ttu-id="d2173-902">Definiera hello komprimeringsalgoritmer.</span><span class="sxs-lookup"><span data-stu-id="d2173-902">Define hello compression algorithms.</span></span> <span data-ttu-id="d2173-903">I vårt exempel rekommenderar vi att du komprimera hello replikering dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="d2173-903">In our example, we recommend that you compress hello replication stream.</span></span> <span data-ttu-id="d2173-904">Särskilt i omsynkroniseringen situationer minskar hello-komprimering av hello replikering dataströmmen kraftigt omsynkroniseringen tid.</span><span class="sxs-lookup"><span data-stu-id="d2173-904">Especially in resynchronization situations, hello compression of hello replication stream dramatically reduces resynchronization time.</span></span> <span data-ttu-id="d2173-905">Observera att komprimering används hello processor och RAM resurser för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2173-905">Note that compression uses hello CPU and RAM resources of a virtual machine.</span></span> <span data-ttu-id="d2173-906">Som hello komprimering ökar, så hello mängden processorresurser som används.</span><span class="sxs-lookup"><span data-stu-id="d2173-906">As hello compression rate increases, so does hello volume of CPU resources used.</span></span> <span data-ttu-id="d2173-907">Du kan också ändra denna inställning senare.</span><span class="sxs-lookup"><span data-stu-id="d2173-907">You also can adjust this setting later.</span></span>

7.  <span data-ttu-id="d2173-908">En annan inställningen du behöver toocheck är om hello replikeringen sker synkront eller asynkront.</span><span class="sxs-lookup"><span data-stu-id="d2173-908">Another setting you need toocheck is whether hello replication occurs asynchronously or synchronously.</span></span> <span data-ttu-id="d2173-909">*När du skyddar SAP ASCS/SCS konfigurationer, måste du använda synkron replikering*.</span><span class="sxs-lookup"><span data-stu-id="d2173-909">*When you protect SAP ASCS/SCS configurations, you must use synchronous replication*.</span></span>  

  ![Bild 52: Definiera replikeringsinformation][sap-ha-guide-figure-3042]

  <span data-ttu-id="d2173-911">_**Bild 52:** definiera replikeringsinformation_</span><span class="sxs-lookup"><span data-stu-id="d2173-911">_**Figure 52:** Define replication details_</span></span>

8.  <span data-ttu-id="d2173-912">Definiera om hello volym som replikeras av hello replikering jobb ska vara representeras tooa Windows Server Failover Clustering klusterkonfigurationen som en delad disk.</span><span class="sxs-lookup"><span data-stu-id="d2173-912">Define whether hello volume that is replicated by hello replication job should be represented tooa Windows Server Failover Clustering cluster configuration as a shared disk.</span></span> <span data-ttu-id="d2173-913">Hello SAP ASCS/SCS konfiguration, markera **Ja** så att hello Windows kluster ser hello replikerade volymen som en delad disk som kan användas som en volym i klustret.</span><span class="sxs-lookup"><span data-stu-id="d2173-913">For hello SAP ASCS/SCS configuration, select **Yes** so that hello Windows cluster sees hello replicated volume as a shared disk that it can use as a cluster volume.</span></span>

  ![Bild 53: Välj Ja tooset hello replikerad volym som en klustervolym][sap-ha-guide-figure-3043]

  <span data-ttu-id="d2173-915">_**Bild 53:** Välj **Ja** tooset hello replikeras volymen som en klustervolym_</span><span class="sxs-lookup"><span data-stu-id="d2173-915">_**Figure 53:** Select **Yes** tooset hello replicated volume as a cluster volume_</span></span>

  <span data-ttu-id="d2173-916">När hello volym skapas visar hello DataKeeper hantering och konfiguration verktyget hello replikering jobbet är aktiv.</span><span class="sxs-lookup"><span data-stu-id="d2173-916">After hello volume is created, hello DataKeeper Management and Configuration tool shows that hello replication job is active.</span></span>

  ![Bild 54: DataKeeper synkroniserad spegling för hello SAP ASCS/SCS resursen disk är aktiv][sap-ha-guide-figure-3044]

  <span data-ttu-id="d2173-918">_**Bild 54:** DataKeeper synkroniserad spegling för hello SAP ASCS/SCS dela disken är aktiv_</span><span class="sxs-lookup"><span data-stu-id="d2173-918">_**Figure 54:** DataKeeper synchronous mirroring for hello SAP ASCS/SCS share disk is active_</span></span>

  <span data-ttu-id="d2173-919">Hanteraren för redundanskluster innehåller nu hello disk som en DataKeeper disk som visas i figur 55.</span><span class="sxs-lookup"><span data-stu-id="d2173-919">Failover Cluster Manager now shows hello disk as a DataKeeper disk, as shown in Figure 55.</span></span>

  ![Bild 55: Klusterhanterare för växling vid fel visar hello-disk som DataKeeper replikeras][sap-ha-guide-figure-3045]

  <span data-ttu-id="d2173-921">_**Bild 55:** Klusterhanteraren visar hello disk som DataKeeper replikeras_</span><span class="sxs-lookup"><span data-stu-id="d2173-921">_**Figure 55:** Failover Cluster Manager shows hello disk that DataKeeper replicated_</span></span>

## <span data-ttu-id="d2173-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Installera hello SAP NetWeaver system</span><span class="sxs-lookup"><span data-stu-id="d2173-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Install hello SAP NetWeaver system</span></span>

<span data-ttu-id="d2173-923">Vi kommer inte beskriver hello DBMS installationen eftersom inställningar varierar beroende på hello DBMS system du använder.</span><span class="sxs-lookup"><span data-stu-id="d2173-923">We won’t describe hello DBMS setup because setups vary depending on hello DBMS system you use.</span></span> <span data-ttu-id="d2173-924">Men förutsätter vi att få hög tillgänglighet med hello DBMS behandlas med hello funktioner hello olika DBMS-leverantörer som har stöd för Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-924">However, we assume that high-availability concerns with hello DBMS are addressed with hello functionalities hello different DBMS vendors support for Azure.</span></span> <span data-ttu-id="d2173-925">Till exempel alltid på eller databasspegling för SQL Server och Oracle Data Guard för Oracle-databaser.</span><span class="sxs-lookup"><span data-stu-id="d2173-925">For example, Always On or database mirroring for SQL Server, and Oracle Data Guard for Oracle databases.</span></span> <span data-ttu-id="d2173-926">Vi inte lägga till mer skydd toohello DBMS i hello-scenariot som vi använder i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d2173-926">In hello scenario we use in this article, we didn't add more protection toohello DBMS.</span></span>

<span data-ttu-id="d2173-927">Det finns inga särskilda överväganden när olika DBMS tjänster interagera med den här typen av klustrad SAP ASCS/SCS konfiguration i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2173-927">There are no special considerations when different DBMS services interact with this kind of clustered SAP ASCS/SCS configuration in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-928">hello installationsproceduren för SAP NetWeaver ABAP system, Java-system och ABAP + Java system är nästan identiska.</span><span class="sxs-lookup"><span data-stu-id="d2173-928">hello installation procedures of SAP NetWeaver ABAP systems, Java systems, and ABAP+Java systems are almost identical.</span></span> <span data-ttu-id="d2173-929">hello viktigaste skillnaden är att ett SAP ABAP system har en ASCS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-929">hello most significant difference is that an SAP ABAP system has one ASCS instance.</span></span> <span data-ttu-id="d2173-930">hello SAP Java system har en SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-930">hello SAP Java system has one SCS instance.</span></span> <span data-ttu-id="d2173-931">hello SAP ABAP + Java system har en ASCS-instans och en SCS-instans som körs i hello samma klustergrupp för Microsoft-redundans.</span><span class="sxs-lookup"><span data-stu-id="d2173-931">hello SAP ABAP+Java system has one ASCS instance and one SCS instance running in hello same Microsoft failover cluster group.</span></span> <span data-ttu-id="d2173-932">Installationen skillnader för varje SAP NetWeaver installationen stack anges explicit.</span><span class="sxs-lookup"><span data-stu-id="d2173-932">Any installation differences for each SAP NetWeaver installation stack are explicitly mentioned.</span></span> <span data-ttu-id="d2173-933">Du kan anta att alla andra delar är hello samma.</span><span class="sxs-lookup"><span data-stu-id="d2173-933">You can assume that all other parts are hello same.</span></span>  
>
>

### <span data-ttu-id="d2173-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Installera SAP med en hög tillgänglighet ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Install SAP with a high-availability ASCS/SCS instance</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2173-935">Glöm inte tooplace sidan fil på DataKeeper speglade volymer.</span><span class="sxs-lookup"><span data-stu-id="d2173-935">Be sure not tooplace your page file on DataKeeper mirrored volumes.</span></span> <span data-ttu-id="d2173-936">DataKeeper stöder inte speglade volymer.</span><span class="sxs-lookup"><span data-stu-id="d2173-936">DataKeeper does not support mirrored volumes.</span></span> <span data-ttu-id="d2173-937">Du kan lämna växlingsfilen på hello tillfälliga enhet D av en virtuell Azure-dator, vilket är hello standard.</span><span class="sxs-lookup"><span data-stu-id="d2173-937">You can leave your page file on hello temporary drive D of an Azure virtual machine, which is hello default.</span></span> <span data-ttu-id="d2173-938">Om den inte redan finns där, flytta hello Windows sidan filen toodrive D på ditt Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="d2173-938">If it's not already there, move hello Windows page file toodrive D of your Azure virtual machine.</span></span>
>
>

<span data-ttu-id="d2173-939">Dessa uppgifter omfattar att installera SAP med en hög tillgänglighet ASCS/SCS-instans:</span><span class="sxs-lookup"><span data-stu-id="d2173-939">Installing SAP with a high-availability ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="d2173-940">Skapa ett virtuellt värdnamn för hello klustrade SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-940">Creating a virtual host name for hello clustered SAP ASCS/SCS instance</span></span>
- <span data-ttu-id="d2173-941">Installera hello SAP första klusternoden</span><span class="sxs-lookup"><span data-stu-id="d2173-941">Installing hello SAP first cluster node</span></span>
- <span data-ttu-id="d2173-942">Ändra hello SAP profil hello ASCS/SCS instans</span><span class="sxs-lookup"><span data-stu-id="d2173-942">Modifying hello SAP profile of hello ASCS/SCS instance</span></span>
- <span data-ttu-id="d2173-943">Lägger till en avsökningsport</span><span class="sxs-lookup"><span data-stu-id="d2173-943">Adding a probe port</span></span>
- <span data-ttu-id="d2173-944">Öppna hello avsökningsport för Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="d2173-944">Opening hello Windows firewall probe port</span></span>

#### <span data-ttu-id="d2173-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Skapa ett virtuellt värdnamn för hello klustrade SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Create a virtual host name for hello clustered SAP ASCS/SCS instance</span></span>

1.  <span data-ttu-id="d2173-946">Skapa en DNS-post för hello virtuella värdnamn för hello ASCS/SCS-instansen i hello Windows DNS-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="d2173-946">In hello Windows DNS manager, create a DNS entry for hello virtual host name of hello ASCS/SCS instance.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="d2173-947">hello IP-adress som du tilldelar toohello virtuella värdnamn för hello ASCS/SCS instansen måste vara hello samma som hello IP-adress som du tilldelade tooAzure belastningsutjämnare (**<*SID*> - lb - ascs **).</span><span class="sxs-lookup"><span data-stu-id="d2173-947">hello IP address that you assign toohello virtual host name of hello ASCS/SCS instance must be hello same as hello IP address that you assigned tooAzure Load Balancer (**<*SID*>-lb-ascs**).</span></span>  
  >
  >

  <span data-ttu-id="d2173-948">Hej IP-adressen för hello virtuella värdnamn för SAP ASCS/SCS (**pr1-ascs-sap**) hello är samma som hello IP-adress för Azure belastningsutjämnare (**pr1-lb-ascs**).</span><span class="sxs-lookup"><span data-stu-id="d2173-948">hello IP address of hello virtual SAP ASCS/SCS host name (**pr1-ascs-sap**) is hello same as hello IP address of Azure Load Balancer (**pr1-lb-ascs**).</span></span>

  ![Bild 56: Definiera hello DNS-post för hello SAP ASCS/SCS kluster virtuella namn och TCP/IP-adress][sap-ha-guide-figure-3046]

  <span data-ttu-id="d2173-950">_**Bild 56:** definiera hello DNS-post för hello SAP ASCS/SCS kluster virtuella namn och TCP/IP-adress_</span><span class="sxs-lookup"><span data-stu-id="d2173-950">_**Figure 56:** Define hello DNS entry for hello SAP ASCS/SCS cluster virtual name and TCP/IP address_</span></span>

2.  <span data-ttu-id="d2173-951">toodefine hello IP-adress som tilldelats toohello virtuellt värdnamn, Välj **DNS-hanteraren** > **domän**.</span><span class="sxs-lookup"><span data-stu-id="d2173-951">toodefine hello IP address assigned toohello virtual host name, select **DNS Manager** > **Domain**.</span></span>

  ![Bild 57: Nya virtuella namn och TCP/IP-adress för SAP ASCS/SCS klusterkonfiguration][sap-ha-guide-figure-3047]

  <span data-ttu-id="d2173-953">_**Bild 57:** nya virtuella namn och TCP/IP-adress för SAP ASCS/SCS klusterkonfiguration_</span><span class="sxs-lookup"><span data-stu-id="d2173-953">_**Figure 57:** New virtual name and TCP/IP address for SAP ASCS/SCS cluster configuration_</span></span>

#### <span data-ttu-id="d2173-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Installera hello SAP första klusternoden</span><span class="sxs-lookup"><span data-stu-id="d2173-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Install hello SAP first cluster node</span></span>

1.  <span data-ttu-id="d2173-955">Köra hello första kluster nod alternativet på klusternoden A. Till exempel på hello **pr1-ascs-0** värden.</span><span class="sxs-lookup"><span data-stu-id="d2173-955">Execute hello first cluster node option on cluster node A. For example, on hello **pr1-ascs-0** host.</span></span>
2.  <span data-ttu-id="d2173-956">tookeep hello standardportarna för hello Azure interna belastningsutjämnare, Välj:</span><span class="sxs-lookup"><span data-stu-id="d2173-956">tookeep hello default ports for hello Azure internal load balancer, select:</span></span>

  * <span data-ttu-id="d2173-957">**ABAP system**: **ASCS** instansen nummer **00**</span><span class="sxs-lookup"><span data-stu-id="d2173-957">**ABAP system**: **ASCS** instance number **00**</span></span>
  * <span data-ttu-id="d2173-958">**Java system**: **SCS** instansen nummer **01**</span><span class="sxs-lookup"><span data-stu-id="d2173-958">**Java system**: **SCS** instance number **01**</span></span>
  * <span data-ttu-id="d2173-959">**ABAP + Java system**: **ASCS** instansen nummer **00** och **SCS** instansen nummer **01**</span><span class="sxs-lookup"><span data-stu-id="d2173-959">**ABAP+Java system**: **ASCS** instance number **00** and **SCS** instance number **01**</span></span>

  <span data-ttu-id="d2173-960">toouse instans siffror än 00 för hello ABAP ASCS-instans och 01 för hello Java SCS instans, måste du först toochange hello Azure interna standard belastningsutjämnare NLB regler som beskrivs i [ändra hello ASCS/SCS standard belastning NLB regler för hello Azure intern belastningsutjämnare][sap-ha-guide-8.9].</span><span class="sxs-lookup"><span data-stu-id="d2173-960">toouse instance numbers other than 00 for hello ABAP ASCS instance and 01 for hello Java SCS instance, first you need toochange hello Azure internal load balancer default load balancing rules, described in [Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer][sap-ha-guide-8.9].</span></span>

<span data-ttu-id="d2173-961">hello beskrivs nästa några åtgärder inte i hello dokumentationen standard SAP.</span><span class="sxs-lookup"><span data-stu-id="d2173-961">hello next few tasks aren't described in hello standard SAP installation documentation.</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-962">hello SAP dokumentationen beskriver hur tooinstall hello första ASCS/SCS klusternoden.</span><span class="sxs-lookup"><span data-stu-id="d2173-962">hello SAP installation documentation describes how tooinstall hello first ASCS/SCS cluster node.</span></span>
>
>

#### <span data-ttu-id="d2173-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Ändra hello SAP-profil för hello ASCS/SCS-instansen</span><span class="sxs-lookup"><span data-stu-id="d2173-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Modify hello SAP profile of hello ASCS/SCS instance</span></span>

<span data-ttu-id="d2173-964">Du måste tooadd en ny profil-parameter.</span><span class="sxs-lookup"><span data-stu-id="d2173-964">You need tooadd a new profile parameter.</span></span> <span data-ttu-id="d2173-965">hello profil parametern förhindrar anslutningar mellan SAP arbetsprocesser och hello sätta servern stängs när de är inaktiv för länge.</span><span class="sxs-lookup"><span data-stu-id="d2173-965">hello profile parameter prevents connections between SAP work processes and hello enqueue server from closing when they are idle for too long.</span></span> <span data-ttu-id="d2173-966">Vi har nämnt hello problemet i [lägga till registerposter på båda klusternoderna för hello SAP ASCS/SCS instansen][sap-ha-guide-8.11].</span><span class="sxs-lookup"><span data-stu-id="d2173-966">We mentioned hello problem scenario in [Add registry entries on both cluster nodes of hello SAP ASCS/SCS instance][sap-ha-guide-8.11].</span></span> <span data-ttu-id="d2173-967">I det avsnittet introducerade vi också två ändringar toosome grundläggande TCP/IP-anslutningsparametrar.</span><span class="sxs-lookup"><span data-stu-id="d2173-967">In that section, we also introduced two changes toosome basic TCP/IP connection parameters.</span></span> <span data-ttu-id="d2173-968">I nästa steg behöver du tooset hello sätta server toosend en `keep_alive` signalerar så att hello anslutningar inte nått hello Azure interna belastningsutjämnarens inaktiv tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="d2173-968">In a second step, you need tooset hello enqueue server toosend a `keep_alive` signal so that hello connections don't hit hello Azure internal load balancer's idle threshold.</span></span>

<span data-ttu-id="d2173-969">toomodify hello SAP-profilen för hello ASCS/SCS instans:</span><span class="sxs-lookup"><span data-stu-id="d2173-969">toomodify hello SAP profile of hello ASCS/SCS instance:</span></span>

1.  <span data-ttu-id="d2173-970">Lägg till den här profilen parametern toohello SAP ASCS/SCS instans profil:</span><span class="sxs-lookup"><span data-stu-id="d2173-970">Add this profile parameter toohello SAP ASCS/SCS instance profile:</span></span>

  ```
  enque/encni/set_so_keepalive = true
  ```
  <span data-ttu-id="d2173-971">I vårt exempel är hello sökvägen:</span><span class="sxs-lookup"><span data-stu-id="d2173-971">In our example, hello path is:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  <span data-ttu-id="d2173-972">Till exempel toohello SAP SCS instans profil och motsvarande sökväg:</span><span class="sxs-lookup"><span data-stu-id="d2173-972">For example, toohello SAP SCS instance profile and corresponding path:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  <span data-ttu-id="d2173-973">tooapply hello ändringar hello SAP ASCS /SCS-instansen startas om.</span><span class="sxs-lookup"><span data-stu-id="d2173-973">tooapply hello changes, restart hello SAP ASCS /SCS instance.</span></span>

#### <span data-ttu-id="d2173-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Lägga till en avsökningsport</span><span class="sxs-lookup"><span data-stu-id="d2173-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Add a probe port</span></span>

<span data-ttu-id="d2173-975">Använda hello interna belastningsutjämnarens avsökningen funktioner toomake hello hela klustret konfigurationen fungerar med Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d2173-975">Use hello internal load balancer's probe functionality toomake hello entire cluster configuration work with Azure Load Balancer.</span></span> <span data-ttu-id="d2173-976">hello Azure intern belastningsutjämnare distribuerar vanligtvis hello inkommande belastningen jämnt mellan deltagande virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d2173-976">hello Azure internal load balancer usually distributes hello incoming workload equally between participating virtual machines.</span></span> <span data-ttu-id="d2173-977">Men fungerar det inte i vissa klusterkonfigurationer eftersom endast en instans är aktiv.</span><span class="sxs-lookup"><span data-stu-id="d2173-977">However, this won't work in some cluster configurations because only one instance is active.</span></span> <span data-ttu-id="d2173-978">hello instanser är passivt och kan inte acceptera alla hello arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="d2173-978">hello other instance is passive and can’t accept any of hello workload.</span></span> <span data-ttu-id="d2173-979">Det hjälper dig att en avsökning funktionalitet när hello Azure interna belastningen belastningsutjämnaren tilldelas fungerar endast tooan aktiva instansen.</span><span class="sxs-lookup"><span data-stu-id="d2173-979">A probe functionality helps when hello Azure internal load balancer assigns work only tooan active instance.</span></span> <span data-ttu-id="d2173-980">Med hello avsökningen funktioner kan du identifiera vilka instanser som är aktiva och sedan målinstans endast hello med hello arbetsbelastning hello intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d2173-980">With hello probe functionality, hello internal load balancer can detect which instances are active, and then target only hello instance with hello workload.</span></span>

<span data-ttu-id="d2173-981">tooadd en avsökningsport:</span><span class="sxs-lookup"><span data-stu-id="d2173-981">tooadd a probe port:</span></span>

1.  <span data-ttu-id="d2173-982">Kontrollera hello aktuella **ProbePort** inställningen genom att köra följande PowerShell-kommando hello.</span><span class="sxs-lookup"><span data-stu-id="d2173-982">Check hello current **ProbePort** setting by running hello following PowerShell command.</span></span> <span data-ttu-id="d2173-983">Köra den från inom ett hello virtuella datorer i hello klusterkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d2173-983">Execute it from within one of hello virtual machines in hello cluster configuration.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  <span data-ttu-id="d2173-984">Definiera en avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="d2173-984">Define a probe port.</span></span> <span data-ttu-id="d2173-985">hello standardportnumret för avsökning är **0**.</span><span class="sxs-lookup"><span data-stu-id="d2173-985">hello default probe port number is **0**.</span></span> <span data-ttu-id="d2173-986">I vårt exempel vi använder avsökningsport **62000**.</span><span class="sxs-lookup"><span data-stu-id="d2173-986">In our example, we use probe port **62000**.</span></span>

  ![Bild 58: hello klustret configuration avsökningsport är 0 som standard][sap-ha-guide-figure-3048]

  <span data-ttu-id="d2173-988">_**Bild 58:** hello klustret configuration avsökningen standardporten är 0_</span><span class="sxs-lookup"><span data-stu-id="d2173-988">_**Figure 58:** hello default cluster configuration probe port is 0_</span></span>

  <span data-ttu-id="d2173-989">hello-portnummer har definierats i SAP Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="d2173-989">hello port number is defined in SAP Azure Resource Manager templates.</span></span> <span data-ttu-id="d2173-990">Du kan tilldela hello portnumret i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d2173-990">You can assign hello port number in PowerShell.</span></span>

  <span data-ttu-id="d2173-991">tooset ett nytt ProbePort värde för hello  **SAP <*SID*> IP ** klusterresurs, kör hello följande PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="d2173-991">tooset a new ProbePort value for hello **SAP <*SID*> IP** cluster resource, run hello following PowerShell script.</span></span> <span data-ttu-id="d2173-992">Uppdatera hello PowerShell variabler för din miljö.</span><span class="sxs-lookup"><span data-stu-id="d2173-992">Update hello PowerShell variables for your environment.</span></span> <span data-ttu-id="d2173-993">När hello skriptet körs, kommer du att ange toorestart hello SAP klustret tooactivate hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="d2173-993">After hello script runs, you'll be prompted toorestart hello SAP cluster group tooactivate hello changes.</span></span>

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

  Clear-Host
  $SAPClusterRoleName = "SAP $SAPSID"
  $SAPIPresourceName = "SAP $SAPSID IP"
  $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
  $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
  $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
  $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
  $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
  $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
  $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

  $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

  Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

  Write-Host
  Write-Host "Current probe port property of hello SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting hello new probe port property of hello SAP cluster resource '$SAPIPresourceName' too'$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want tootake restart SAP cluster role '$SAPClusterRoleName', tooactivate hello changes (yes/no)?"

  if($ActivateChanges -eq "yes"){
  Write-Host
  Write-Host "Activating changes..." -ForegroundColor Cyan

  Write-Host
  write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
  Stop-ClusterResource -Name $SAPIPresourceName
  sleep 5

  Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
  Start-ClusterGroup -Name $SAPClusterRoleName

  Write-Host "New ProbePort parameter is active." -ForegroundColor Green
  Write-Host

  Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
  Write-Host
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
  }else
  {
  Write-Host "Changes are not activated."
  }
  ```

  <span data-ttu-id="d2173-994">När du tar hello  **SAP <*SID*> ** rollen online bör du kontrollera att **ProbePort** toohello nytt värde anges.</span><span class="sxs-lookup"><span data-stu-id="d2173-994">After you bring hello **SAP <*SID*>** cluster role online, verify that **ProbePort** is set toohello new value.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Bild 59: Avsökning hello klustret port när du ställer in hello nytt värde][sap-ha-guide-figure-3049]

  <span data-ttu-id="d2173-996">_**Bild 59:** avsökning hello klustret port när du ställer in hello nytt värde_</span><span class="sxs-lookup"><span data-stu-id="d2173-996">_**Figure 59:** Probe hello cluster port after you set hello new value_</span></span>

#### <span data-ttu-id="d2173-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Öppna hello avsökningsport för Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="d2173-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open hello Windows firewall probe port</span></span>

<span data-ttu-id="d2173-998">Du måste tooopen en Windows-brandväggen avsökningsport på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="d2173-998">You need tooopen a Windows firewall probe port on both cluster nodes.</span></span> <span data-ttu-id="d2173-999">Använd följande skript tooopen en avsökning brandväggsport Windows hello.</span><span class="sxs-lookup"><span data-stu-id="d2173-999">Use hello following script tooopen a Windows firewall probe port.</span></span> <span data-ttu-id="d2173-1000">Uppdatera hello PowerShell variabler för din miljö.</span><span class="sxs-lookup"><span data-stu-id="d2173-1000">Update hello PowerShell variables for your environment.</span></span>

  ```PowerShell
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

<span data-ttu-id="d2173-1001">Hej **ProbePort** har angetts för**62000**.</span><span class="sxs-lookup"><span data-stu-id="d2173-1001">hello **ProbePort** is set too**62000**.</span></span> <span data-ttu-id="d2173-1002">Nu kan du komma åt hello filresurs  **\\\ascsha-clsap\sapmnt** från andra värdar, exempel från **ascsha dbas**.</span><span class="sxs-lookup"><span data-stu-id="d2173-1002">Now you can access hello file share **\\\ascsha-clsap\sapmnt** from other hosts, such as from **ascsha-dbas**.</span></span>

### <span data-ttu-id="d2173-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Installera hello-databasinstans</span><span class="sxs-lookup"><span data-stu-id="d2173-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Install hello database instance</span></span>

<span data-ttu-id="d2173-1004">tooinstall hello databasen instansen, följ hello process som beskrivs i hello SAP dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="d2173-1004">tooinstall hello database instance, follow hello process described in hello SAP installation documentation.</span></span>

### <span data-ttu-id="d2173-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Installera hello andra klusternod</span><span class="sxs-lookup"><span data-stu-id="d2173-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Install hello second cluster node</span></span>

<span data-ttu-id="d2173-1006">tooinstall hello andra kluster, följ hello stegen i hello SAP-installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="d2173-1006">tooinstall hello second cluster, follow hello steps in hello SAP installation guide.</span></span>

### <span data-ttu-id="d2173-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Ändra hello starttypen till hello SAP ÄNDARE Windows service-instans</span><span class="sxs-lookup"><span data-stu-id="d2173-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Change hello start type of hello SAP ERS Windows service instance</span></span>

<span data-ttu-id="d2173-1008">Ändra hello starttypen till hello SAP ÄNDARE Windows-tjänst för**automatiskt (förskjuten Start)** på båda klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="d2173-1008">Change hello start type of hello SAP ERS Windows service too**Automatic (Delayed Start)** on both cluster nodes.</span></span>

![Bild 60: Ändra hello service-typen för hello SAP ÄNDARE instans toodelayed automatisk][sap-ha-guide-figure-3050]

<span data-ttu-id="d2173-1010">_**Bild 60:** ändra hello service-typen för hello SAP ÄNDARE instans toodelayed automatisk_</span><span class="sxs-lookup"><span data-stu-id="d2173-1010">_**Figure 60:** Change hello service type for hello SAP ERS instance toodelayed automatic_</span></span>

### <span data-ttu-id="d2173-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Installera hello SAP primära programserver</span><span class="sxs-lookup"><span data-stu-id="d2173-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Install hello SAP Primary Application Server</span></span>

<span data-ttu-id="d2173-1012">Installera hello primära programmet Server (PROVIDERADRESSER) instans <*SID*> - di - 0 på hello virtuell dator som du har angett toohost hello PROVIDERADRESSER.</span><span class="sxs-lookup"><span data-stu-id="d2173-1012">Install hello Primary Application Server (PAS) instance <*SID*>-di-0 on hello virtual machine that you've designated toohost hello PAS.</span></span> <span data-ttu-id="d2173-1013">Det finns inga beroenden på Azure eller DataKeeper-specifika inställningar.</span><span class="sxs-lookup"><span data-stu-id="d2173-1013">There are no dependencies on Azure or DataKeeper-specific settings.</span></span>

### <span data-ttu-id="d2173-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Installera hello SAP ytterligare programserver</span><span class="sxs-lookup"><span data-stu-id="d2173-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Install hello SAP Additional Application Server</span></span>

<span data-ttu-id="d2173-1015">Installera en SAP ytterligare program Server (AAS) på alla hello virtuella datorer som du har angett toohost en SAP Application Server-instans.</span><span class="sxs-lookup"><span data-stu-id="d2173-1015">Install an SAP Additional Application Server (AAS) on all hello virtual machines that you've designated toohost an SAP Application Server instance.</span></span> <span data-ttu-id="d2173-1016">Till exempel på <*SID*> - di - 1 för <*SID*> - di -&lt;n&gt;.</span><span class="sxs-lookup"><span data-stu-id="d2173-1016">For example, on <*SID*>-di-1 too<*SID*>-di-&lt;n&gt;.</span></span>

> [!NOTE]
> <span data-ttu-id="d2173-1017">Detta är klar hello installation av ett system med hög tillgänglighet SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="d2173-1017">This finishes hello installation of a high-availability SAP NetWeaver system.</span></span> <span data-ttu-id="d2173-1018">Fortsätt sedan med testning av redundans.</span><span class="sxs-lookup"><span data-stu-id="d2173-1018">Next, proceed with failover testing.</span></span>
>


## <span data-ttu-id="d2173-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testa hello SAP ASCS/SCS instans redundans- och SIOS replikering</span><span class="sxs-lookup"><span data-stu-id="d2173-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Test hello SAP ASCS/SCS instance failover and SIOS replication</span></span>
<span data-ttu-id="d2173-1020">Det är enkelt tootest och övervaka en växling vid fel för SAP ASCS/SCS-instans och SIOS diskreplikering med hjälp av hanteraren för redundanskluster och hello SIOS DataKeeper hantering och konfiguration verktyget.</span><span class="sxs-lookup"><span data-stu-id="d2173-1020">It's easy tootest and monitor an SAP ASCS/SCS instance failover and SIOS disk replication by using Failover Cluster Manager and hello SIOS DataKeeper Management and Configuration tool.</span></span>

### <span data-ttu-id="d2173-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASCS/SCS-instans som körs på en nod i klustret</span><span class="sxs-lookup"><span data-stu-id="d2173-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS instance is running on cluster node A</span></span>

<span data-ttu-id="d2173-1022">Hej **SAP PR1** klustergrupp körs på klusternoden A. Till exempel på **pr1-ascs-0**.</span><span class="sxs-lookup"><span data-stu-id="d2173-1022">hello **SAP PR1** cluster group is running on cluster node A. For example, on **pr1-ascs-0**.</span></span> <span data-ttu-id="d2173-1023">Tilldela hello delad enhet S, vilket är en del av hello **SAP PR1** grupp och vilken hello ASCS/SCS instans använder toocluster nod A.</span><span class="sxs-lookup"><span data-stu-id="d2173-1023">Assign hello shared disk drive S, which is part of hello **SAP PR1** cluster group, and which hello ASCS/SCS instance uses, toocluster node A.</span></span>

![Bild 61: Hanteraren för redundanskluster: hello SAP < SID > klustergrupp körs på en nod i klustret][sap-ha-guide-figure-5000]

<span data-ttu-id="d2173-1025">_**Bild 61:** Klusterhanteraren: hello SAP <*SID*> klustergrupp körs på en nod i klustret_</span><span class="sxs-lookup"><span data-stu-id="d2173-1025">_**Figure 61:** Failover Cluster Manager: hello SAP <*SID*> cluster group is running on cluster node A_</span></span>

<span data-ttu-id="d2173-1026">I hello SIOS DataKeeper hantering och konfigurationsverktyget, kan du se hello delade disken data replikeras synkront från hello volym källenheten S på klusternoden en toohello volym målenheten S på klusternoden B. Till exempel har replikerats från **pr1-ascs-0 [10.0.0.40]** för**pr1-ascs-1 [10.0.0.41]**.</span><span class="sxs-lookup"><span data-stu-id="d2173-1026">In hello SIOS DataKeeper Management and Configuration tool, you can see that hello shared disk data is synchronously replicated from hello source volume drive S on cluster node A toohello target volume drive S on cluster node B. For example, it's replicated from **pr1-ascs-0 [10.0.0.40]** too**pr1-ascs-1 [10.0.0.41]**.</span></span>

![Bild 62: I SIOS DataKeeper replikera hello lokal volym från klusternoden toocluster-nod B][sap-ha-guide-figure-5001]

<span data-ttu-id="d2173-1028">_**Bild 62:** i SIOS DataKeeper replikerar hello lokal volym från klusternoden toocluster-nod B_</span><span class="sxs-lookup"><span data-stu-id="d2173-1028">_**Figure 62:** In SIOS DataKeeper, replicate hello local volume from cluster node A toocluster node B_</span></span>

### <span data-ttu-id="d2173-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Växling vid fel från noden A toonode B</span><span class="sxs-lookup"><span data-stu-id="d2173-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover from node A toonode B</span></span>

1.  <span data-ttu-id="d2173-1030">Välj något av dessa alternativ tooinitiate en redundansväxling av hello SAP <*SID*> klustergrupp från klusternoden nod A toocluster B:</span><span class="sxs-lookup"><span data-stu-id="d2173-1030">Choose one of these options tooinitiate a failover of hello SAP <*SID*> cluster group from cluster node A toocluster node B:</span></span>
  - <span data-ttu-id="d2173-1031">Använd hanteraren för redundanskluster</span><span class="sxs-lookup"><span data-stu-id="d2173-1031">Use Failover Cluster Manager</span></span>  
  - <span data-ttu-id="d2173-1032">Använda PowerShell för Failover-kluster</span><span class="sxs-lookup"><span data-stu-id="d2173-1032">Use Failover Cluster PowerShell</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  <span data-ttu-id="d2173-1033">Starta om klustret nod A hello Windows-gästoperativsystem (Detta startar en automatisk redundans hello SAP <*SID*> klustergrupp från noden A toonode B).</span><span class="sxs-lookup"><span data-stu-id="d2173-1033">Restart cluster node A within hello Windows guest operating system (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>  
3.  <span data-ttu-id="d2173-1034">Starta om klustret nod A från hello Azure-portalen (Detta startar en automatisk redundans hello SAP <*SID*> klustergrupp från noden A toonode B).</span><span class="sxs-lookup"><span data-stu-id="d2173-1034">Restart cluster node A from hello Azure portal (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>  
4.  <span data-ttu-id="d2173-1035">Starta om klustret nod A med hjälp av Azure PowerShell (Detta startar en automatisk redundans hello SAP <*SID*> klustergrupp från noden A toonode B).</span><span class="sxs-lookup"><span data-stu-id="d2173-1035">Restart cluster node A by using Azure PowerShell (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>

  <span data-ttu-id="d2173-1036">Efter redundans hello SAP <*SID*> klustergrupp körs på klusternoden B. Till exempel den körs på **pr1-ascs-1**.</span><span class="sxs-lookup"><span data-stu-id="d2173-1036">After failover, hello SAP <*SID*> cluster group is running on cluster node B. For example, it's running on **pr1-ascs-1**.</span></span>

  ![Bild 63: I Klusterhanteraren hello SAP < SID > klustergrupp körs på klusternoden B][sap-ha-guide-figure-5002]

  <span data-ttu-id="d2173-1038">_**Bild 63**: I Hanteraren för redundanskluster, hello SAP <*SID*> klustergrupp körs på klusternoden B_</span><span class="sxs-lookup"><span data-stu-id="d2173-1038">_**Figure 63**: In Failover Cluster Manager, hello SAP <*SID*> cluster group is running on cluster node B_</span></span>

  <span data-ttu-id="d2173-1039">hello delad disk nu monteras på klustret nod B. SIOS DataKeeper replikerar data från volymen källenheten S kluster nod B tootarget volym enhet S på klusternoden A. Till exempel den replikerar från **pr1-ascs-1 [10.0.0.41]** för**pr1-ascs-0 [10.0.0.40]**.</span><span class="sxs-lookup"><span data-stu-id="d2173-1039">hello shared disk is now mounted on cluster node B. SIOS DataKeeper is replicating data from source volume drive S on cluster node B tootarget volume drive S on cluster node A. For example, it's replicating from **pr1-ascs-1 [10.0.0.41]** too**pr1-ascs-0 [10.0.0.40]**.</span></span>

  ![Bild 64: SIOS DataKeeper replikerar hello lokal volym från noden B toocluster klusternod A][sap-ha-guide-figure-5003]

  <span data-ttu-id="d2173-1041">_**Bild 64:** SIOS DataKeeper replikerar hello lokal volym från klusternoden nod B toocluster A_</span><span class="sxs-lookup"><span data-stu-id="d2173-1041">_**Figure 64:** SIOS DataKeeper replicates hello local volume from cluster node B toocluster node A_</span></span>
