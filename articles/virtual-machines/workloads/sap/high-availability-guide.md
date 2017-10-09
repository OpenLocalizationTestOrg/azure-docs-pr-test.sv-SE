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
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a>Hög tillgänglighet för SAP NetWeaver på Azure Virtual Machines

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


Virtuella Azure-datorer är hello lösning för organisationer som behöver bearbetning, lagring och nätverksresurser, minimalt, och utan långa inköp cykler. Du kan använda Azure virtuella datorer toodeploy klassiska program som SAP NetWeaver-baserade ABAP, Java och en ABAP + Java-stacken. Utöka tillförlitlighet och tillgänglighet utan ytterligare lokala resurser. Virtuella Azure-datorer stöder korsanslutningar, så du kan integrera Azure virtuella datorer i din organisations lokala domäner, privata moln och SAP system liggande.

I den här artikeln beskriver vi hello stegen för att toodeploy SAP-system med hög tillgänglighet i Azure med hjälp av hello Azure Resource Manager-distributionsmodellen. Vi går du igenom dessa viktiga uppgifter:

* Hitta hello rätt SAP anteckningar och installationen guider i hello [resurser] [ sap-ha-guide-2] avsnitt. Den här artikeln kompletterar SAP dokumentationen och SAP anteckningar, som är hello primära resurser som hjälper dig att installera och distribuera program på vissa plattformar.
* Lär dig hello skillnader mellan hello Azure Resource Manager-distributionsmodellen och hello Azure klassiska distributionsmodellen.
* Läs mer om Windows Server Failover Clustering kvorum läge, så du kan välja hello-modell som passar din Azure-distribution.
* Läs mer om Windows Server Failover Clustering delad lagring i Azure-tjänster.
* Lär dig hur toohelp skydda enskild punkt för fel komponenter som avancerade Business Application Programming (ABAP) SAP centrala tjänster (ASCS) / SAP centrala Services (SCS) och databashanteringssystem (DBMS) och redundanta komponenter som SAP Programserver i Azure.
* Följ ett steg för steg-exempel på en installation och konfiguration av en SAP-systemet för hög tillgänglighet i ett redundanskluster i Windows Server-kluster i Azure med hjälp av Azure Resource Manager.
* Mer information om ytterligare steg krävs toouse Windows Server Failover Clustering i Azure, men som inte behövs i en lokal distribution.

toosimplify distribution och konfiguration i den här artikeln använder vi hello SAP tre skikt hög tillgänglighet Resource Manager-mallar. hello mallar automatisera distributionen av hello hela infrastrukturen som du behöver för ett SAP-system med hög tillgänglighet. hello-infrastrukturen stöder också SAP programmets prestanda Standard (SAP) storleksändring av SAP-system.

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Nödvändiga komponenter
Innan du börjar bör du kontrollera att du uppfyller hello krav som beskrivs i följande avsnitt hello. Dessutom vara säker på att toocheck visas alla resurser i hello [resurser] [ sap-ha-guide-2] avsnitt.

I den här artikeln använder vi Azure Resource Manager-mallar för [trelagers-SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). En bra översikt över mallar finns [SAP Azure Resource Manager-mallar](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Resurser
Dessa artiklar beskriver SAP-distributioner i Azure:

* [Azure virtuella datorer planering och implementering för SAP NetWeaver][planning-guide]
* [Distribution av Azure virtuella datorer för SAP NetWeaver][deployment-guide]
* [Azure virtuella datorer DBMS-distribution för SAP NetWeaver][dbms-guide]
* [Azure virtuella datorer hög tillgänglighet för SAP NetWeaver (den här guiden)][sap-ha-guide]

> [!NOTE]
> När det är möjligt, får du en länk toohello hänvisar SAP installationsguiden (se hello [SAP installationen guider][sap-installation-guides]). Krav och information om hello installationsprocessen, det är en bra idé tooread hello SAP NetWeaver installation hjälper noggrant. Den här artikeln beskriver endast specifika uppgifter för SAP NetWeaver-baserade datorer som du kan använda med Azure Virtual Machines.
>
>

Anteckningarna SAP är relaterade toohello avsnittet SAP i Azure:

| Nummer | Rubrik |
| --- | --- |
| [1928533] |SAP-program i Azure: produkter och storlek som stöds |
| [2015553] |SAP på Microsoft Azure: stöd för krav |
| [1999351] |Förbättrad Azure övervakning för SAP |
| [2178632] |Key som övervakning mått för SAP på Microsoft Azure |
| [1999351] |Virtualisering på Windows: förbättrad övervakning |
| [2243692] |Användning av Azure Premium SSD-lagring för SAP DBMS-instans |

Mer information om hello [begränsningar i Azure-prenumerationer][azure-subscription-service-limits-subscription], inklusive allmänna standardbegränsningar och maximala begränsningar.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Hög tillgänglighet SAP med Azure Resource Manager kontra hello Azure klassiska distributionsmodellen
hello Azure Resource Manager och Azure klassiska distributionsmodeller är olika i hello följande områden:

- Resursgrupper
- Azure interna läsa in belastningsutjämning Beroende hello Azure-resursgrupp
- Stöd för scenarier för SAP multi-SID

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Resursgrupper
I Azure Resource Manager, du kan använda resursen grupper toomanage alla hello-resurser i din Azure-prenumeration. En integrerad lösning i en resursgrupp, alla resurser har hello samma livscykel. Till exempel alla resurser skapas på hello samma tid och de tas bort i hello samtidigt. Läs mer om [resursgrupper](../../../azure-resource-manager/resource-group-overview.md#resource-groups).

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure interna läsa in belastningsutjämning Beroende hello Azure-resursgrupp

Hello Azure klassiska distributionsmodellen finns det ett beroende mellan hello Azure intern belastningsutjämnare (Azure belastningsutjämnare service) och hello tjänstgruppen för molnet. Varje intern belastningsutjämnare måste en cloud service-grupp.

Azure Resource Manager behöver du inte en Azure-resurs grupp toouse Azure belastningsutjämnare. hello-miljö är enklare och mer flexibelt.

### <a name="support-for-sap-multi-sid-scenarios"></a>Stöd för scenarier för SAP multi-SID

Du kan installera flera SAP system-ID (SID) ASCS/SCS instanser i ett kluster i Azure Resource Manager. Multi-SID-instanserna är möjligt tack vare stöd för flera IP-adresser för varje Azure interna belastningsutjämnare.

toouse hello Azure klassiska distributionsmodellen, följ hello procedurerna som beskrivs i [SAP NetWeaver i Azure: Clustering SAP ASCS/SCS instanser med hjälp av Windows Server Failover Clustering i Azure med SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).

> [!IMPORTANT]
> Vi rekommenderar starkt att du använder hello Azure Resource Manager-modellen för SAP-installationer. Den erbjuder många fördelar som inte är tillgängliga i hello klassiska distributionsmodellen. Lär dig mer om Azure [distributionsmodeller][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
>
>

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover-kluster
Windows Server Failover Clustering är hello grunden för en SAP ASCS/SCS-installation för hög tillgänglighet och DBMS i Windows.

Ett redundanskluster är en grupp 1 + n oberoende servrar (noder) som arbetar tillsammans för tooincrease hello tillgängligheten för program och tjänster. Om det inträffar ett nodfel, Windows Server Failover Clustering beräknar hello antalet fel som kan uppstå samtidigt som en felfri kluster tooprovide program och tjänster. Du kan välja mellan olika kvorum lägen tooachieve failover-kluster.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Kvorumlägen
Du kan välja mellan fyra kvorumlägen när du använder Windows Server Failover Clustering:

* **Nodmajoritet**. Varje nod i klustret hello kan rösta. hello klustret fungerar bara med en majoritet av röster, det vill säga med mer än hälften hello röster. Vi rekommenderar att det här alternativet för kluster som har ett ojämnt antal noder. Till exempel tre noder i ett kluster med sju noder kan misslyckas och hello klustret stillbilder uppnår en majoritet och fortsätter toorun.  
* **Nod- och**. Varje nod och en angiven disk (ett diskvittne) i hello klusterlagringen kan rösta när de är tillgängliga och kommunikation. hello klustret fungerar bara med en majoritet av hello röster som är med mer än hälften hello röster. Det här läget är meningsfullt i en klustermiljö med ett jämnt antal noder. Om hälften hello noder och hello disken är online, förblir hello klustret i felfritt tillstånd.
* **Nod- och filresurs majoritet**. Varje nod plus en avsedda filresurs (ett filresursvittne) som Hej administratör skapar kan rösta, oavsett om hello noder och filresursen är tillgänglig och kommunikation. hello klustret fungerar bara med en majoritet av hello röster som är med mer än hälften hello röster. Det här läget är meningsfullt i en klustermiljö med ett jämnt antal noder. Det är liknande toohello nod- och diskmajoritet läge, men den använder en filresurs vittne i stället för en vittnesdisk. Det här läget är enkelt tooimplement, men om hello filresursen själva har inte hög tillgänglighet, så kan det bli en enskild felpunkt.
* **Ingen majoritet: Endast Disk**. hello-klustret har ett kvorum om en nod är tillgänglig och kommunikation med en viss disk i klusterlagringen hello. Endast hello-noder som också är vid kommunikation med disken kan gå hello klustret. Vi rekommenderar att du inte använder det här läget.
 

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server Failover-kluster lokalt
Bild 1 visar ett kluster med två noder. Om hello nätverksanslutningen mellan hello noder misslyckas och båda noderna fortsätta och körs, ett kvorum disk eller filresurs avgör vilken nod fortsätter tooprovide hello klustret program och tjänster. hello-noden som har åtkomst toohello kvorumdisken eller filresurs är hello-noden som säkerställer att tjänster fortsätter.

Eftersom det här exemplet använder ett kluster med två noder kan använda vi hello nod och filresursmajoritet quorum-läget. hello nod- och diskmajoritet är också ett giltigt alternativ. I en produktionsmiljö rekommenderar vi att du använder en kvorumdisk. Du kan använda Nätverks- och system teknik toomake den hög tillgänglighet.

![Bild 1: Exempel på en Windows Server Failover Clustering konfiguration för SAP ASCS/SCS i Azure][sap-ha-guide-figure-1000]

_**Bild 1:** exempel på en Windows Server Failover Clustering konfiguration för SAP ASCS/SCS i Azure_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Delad lagring
Bild 1 visar också ett kluster med två noder delad lagring. I ett kluster för lokala delad lagring identifiera alla noder i klustret hello delad lagring. En mekanism för låsning skyddar hello data från skador. Alla noder kan känna av om en annan nod misslyckas. Om en nod misslyckas hello kvarvarande noden ägarskapet till hello lagringsresurser och garanterar hello tillgängligheten för tjänster.

> [!NOTE]
> Du behöver inte delade diskar för hög tillgänglighet med vissa DBMS-program, precis som med SQL Server. SQL Server alltid aktiverad replikerar DBMS data och loggfilen filer från hello lokal disk i ett kluster nod toohello lokal disk av en annan klusternod. I så fall behöver hello Windows klusterkonfigurationen inte en delad disk.
>
>

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Nätverk och namnmatchning
Klientdatorer når hello klustret via en virtuell IP-adress och ett virtuellt värdnamn som hello DNS-servern ger. hello lokalt noder och hello DNS-server kan hantera flera IP-adresser.

I en typisk konfiguration kan du använda två eller flera nätverksanslutningar:

* En dedicerad anslutning toohello lagring
* En kluster-interna nätverksanslutning för pulsslag hello
* Ett offentligt nätverk som klienter använder tooconnect toohello kluster

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover-kluster i Azure
Jämfört med distribution för toobare operativsystem eller privata moln, virtuella datorer i Azure kräver ytterligare åtgärder tooconfigure Windows Server Failover Clustering. När du skapar en delad klusterdisk måste tooset flera IP-adresser och virtuella värd namn för hello SAP ASCS/SCS-instansen.

I den här artikeln vi beskrivs viktiga begrepp och hello ytterligare steg krävs toobuild ett SAP centrala tjänster med hög tillgänglighet kluster i Azure. Vi visar hur tooset in hello tredjepartsverktyg SIOS DataKeeper och hur tooconfigure hello Azure interna belastningsutjämnare. Du kan använda dessa verktyg toocreate ett redundanskluster i Windows med ett filresursvittne i Azure.

![Bild 2: Windows Server Failover Clustering konfiguration i Azure utan en delad disk][sap-ha-guide-figure-1001]

_**Bild 2:** Windows Server Failover Clustering konfiguration i Azure utan en delad disk_

### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Delad disk i Azure med SIOS DataKeeper
Du måste ingå i klustret delad lagring för en SAP ASCS/SCS-instans med hög tillgänglighet. Som i September 2016 Azure inte erbjuder delad lagring som kan du använda toocreate ett kluster med delad lagring. Du kan använda programvara från tredje part SIOS DataKeeper Cluster Edition toocreate en speglad lagring som simulerar delad klusterlagring. Hej SIOS lösningen innehåller data i realtid synkron replikering. Detta beror på hur du kan skapa en delad diskresurs för ett kluster:

1. Koppla en Azure virtuell hårddisk (VHD) tooeach hello virtuella datorer (VM) i en Windows-klusterkonfiguration.
2. Kör SIOS DataKeeper Cluster Edition på båda noderna för virtuell dator.
3. Konfigurera SIOS DataKeeper Cluster Edition så att den speglar hello innehållet i hello ytterligare virtuell Hårddisk ansluten volym från hello virtuella toohello ytterligare virtuell Hårddisk ansluten källvolymen av hello virtuella måldatorn. SIOS DataKeeper avlägsnar hello käll- och lokala volymer och visar dem sedan tooWindows Server Failover Clustering som en delad disk.

Hämta mer information [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![Bild 3: Windows Server Failover Clustering konfigurationen i Azure med SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Bild 3:** Windows Server Failover Clustering konfigurationen i Azure med SIOS DataKeeper_

> [!NOTE]
> Du behöver inte delade diskar för hög tillgänglighet med vissa DBMS-produkter, t.ex. SQL Server. SQL Server alltid aktiverad replikerar DBMS data och loggfilen filer från hello lokal disk i ett kluster nod toohello lokal disk av en annan klusternod. I det här fallet behöver hello Windows klusterkonfigurationen inte en delad disk.
>
>

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Namnmatchning i Azure
hello Azure molnplattform erbjuder inte hello alternativet tooconfigure virtuella IP-adresser, till exempel flytande IP-adresser. Du måste en alternativ lösning tooset upp en virtuell IP-adress tooreach hello klusterresurs i hello molnet.
Azure har en intern belastningsutjämnare i hello Azure belastningsutjämnare service. Med hello intern belastningsutjämnare nå klienter hello klustret via hello klustrets virtuella IP-adress.
Du måste toodeploy hello interna belastningsutjämnare i hello resursgruppen som innehåller hello klusternoder. Konfigurera sedan alla nödvändiga port vidarebefordran regler med hello avsökningen portar för hello intern belastningsutjämnare.
hello-klienter kan ansluta via hello virtuellt värdnamn. hello DNS-server löser hello klustrets IP-adress och hello interna belastningsutjämnare handtag port vidarebefordran toohello aktiva noden i hello klustret.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver hög tillgänglighet i Azure Infrastructure-as-a-Service (IaaS)
tooachieve SAP-program med hög tillgänglighet som för SAP programvarukomponenter måste tooprotect hello följande komponenter:

* SAP Application Server-instans
* SAP ASCS/SCS-instans
* DBMS-server

Mer information om hur du skyddar SAP komponenter i scenarier med hög tillgänglighet finns [Azure Virtual Machines planering och implementering för SAP NetWeaver](planning-guide.md).

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Hög tillgänglighet SAP-programserver
Vanligtvis behöver du inte en specifik lösning för hög tillgänglighet för hello SAP-programserver och dialogrutan instanser. Du kan uppnå hög tillgänglighet med redundans och konfigurerar du dialogrutan alla instanser i olika instanser av virtuella datorer i Azure. Du bör ha minst två instanser för SAP-program installeras i två instanser av virtuella datorer i Azure.

![Bild 4: Hög tillgänglighet SAP-programserver][sap-ha-guide-figure-2000]

_**Bild 4:** hög tillgänglighet SAP-programserver_

Du måste placera alla virtuella datorer att hello värden SAP Application Server-instanser i samma Azure tillgänglighetsuppsättning. En Azure tillgänglighetsuppsättning säkerställer att:

* Alla virtuella datorer är en del av hello samma domän. En uppgraderingsdomän, till exempel ser till att hello virtuella datorer inte är uppdaterade på hello samtidigt under planerat underhåll driftstopp.
* Alla virtuella datorer är en del av hello samma feldomän. En feldomän, till exempel ser till att virtuella datorer distribueras så att ingen enskild felpunkt påverkar hello tillgängligheten för alla virtuella datorer.

Läs mer om hur för[hantera hello tillgängligheten för virtuella datorer][virtual-machines-manage-availability].

Eftersom hello Azure storage-konto är en potentiell felpunkt, är det viktigt toohave minst två Azure storage-konton som minst två virtuella datorer distribueras. I en perfekt installationsprogrammet skulle hello diskar för varje virtuell dator som kör en instans för SAP-dialogrutan distribueras i ett annat lagringskonto.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Hög tillgänglighet SAP ASCS/SCS instans
Bild 5 är ett exempel på en hög tillgänglighet SAP ASCS/SCS-instans.

![Bild 5: Hög tillgänglighet SAP ASCS/SCS instans][sap-ha-guide-figure-2001]

_**Bild 5:** hög tillgänglighet SAP ASCS/SCS-instans_

#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASCS/SCS instans hög tillgänglighet med Windows Server Failover Clustering i Azure
Jämfört med distribution för toobare operativsystem eller privata moln, virtuella datorer i Azure kräver ytterligare åtgärder tooconfigure Windows Server Failover Clustering. toobuild ett Windows-redundanskluster, behöver du en delad klusterdisk, flera IP-adresser, flera virtuella värdnamn och en Azure intern belastningsutjämnare för kluster för en SAP ASCS/SCS-instans. Detta diskuterar vi i detalj senare i hello artikeln.

![Bild 6: Windows Server Failover Clustering för en SAP ASCS/SCS-konfiguration i Azure med hjälp av SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Bild 6:** Windows Server Failover Clustering för en SAP ASCS/SCS-konfiguration i Azure med SIOS DataKeeper_

### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Hög tillgänglighet DBMS-instans
hello DBMS är också en enda kontaktpunkt i ett SAP-system. Du behöver tooprotect den med hjälp av en lösning för hög tillgänglighet. Bild 7 visar en SQL Server alltid på hög tillgänglighet lösning i Azure, med Windows Server Failover Clustering och hello Azure interna belastningsutjämnare. SQL Server alltid aktiverad replikerar DBMS data och loggfilen filer med hjälp av sin egen DBMS-replikering. I detta fall kan behöver du inte kluster delade diskar som förenklar hello hela installationen.

![Bild 7: Exempel på en SAP-DBMS med hög tillgänglighet, med SQL Server alltid aktiverad][sap-ha-guide-figure-2003]

_**Bild 7:** exempel på ett SAP-DBMS med hög tillgänglighet, med SQL Server alltid aktiverad_

Mer information om kluster och SQL Server i Azure med hjälp av hello Azure Resource Manager-distributionsmodellen finns i de här artiklarna:

* [Konfigurera Always On-tillgänglighetsgrupp i Azure Virtual Machines manuellt med hjälp av hanteraren för filserverresurser][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Konfigurera en Azure intern belastningsutjämnare för en tillgänglighetsgrupp alltid på i Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Distributionsscenarier för slutpunkt till slutpunkt hög tillgänglighet

### <a name="deployment-scenario-using-architectural-template-1"></a>Scenario för distribution med arkitekturen mall 1

Bild 8 visar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **en** SAP-system. Det här scenariot är inställd på följande sätt:

- Ett dedikerat kluster används för hello SAP ASCS/SCS-instans.
- Ett dedikerat kluster används för hello DBMS-instans.
- SAP Application Server-instanser har distribuerats i sina egna dedikerade virtuella datorer.

![Bild 8: SAP hög tillgänglighet arkitektur mall 1, med dedikerade kluster för ASCS/SCS och DBMS][sap-ha-guide-figure-2004]

_**Bild 8:** SAP hög tillgänglighet arkitektur mall 1 dedikerade kluster för ASCS/SCS och DBMS_

### <a name="deployment-scenario-using-architectural-template-2"></a>Scenario för distribution med arkitekturen mall 2

Bild 9 illustrerar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **en** SAP-system. Det här scenariot är inställd på följande sätt:

- Ett dedikerat kluster används för **både** hello SAP ASCS/SCS instansen och hello DBMS.
- SAP Application Server-instanser har distribuerats i egna dedikerade virtuella datorer.

![Bild 9: SAP hög tillgänglighet arkitektur mall 2, med ett kluster för ASCS/SCS och ett kluster för DBMS][sap-ha-guide-figure-2005]

_**Bild 9:** SAP hög tillgänglighet arkitektur mall 2, med ett kluster för ASCS/SCS och ett kluster för DBMS_

### <a name="deployment-scenario-using-architectural-template-3"></a>Scenario för distribution med arkitekturen mall 3

Bild 10 illustrerar ett exempel på en arkitektur för SAP NetWeaver hög tillgänglighet i Azure för **två** SAP-system med &lt;SID1&gt; och &lt;SID2&gt;. Det här scenariot är inställd på följande sätt:

- Ett dedikerat kluster används för **både** hello SAP ASCS/SCS SID1 instans *och* hello SAP ASCS/SCS SID2 instansen (kluster).
- Ett dedikerat kluster används för DBMS SID1 och ett annat dedikerade kluster används för DBMS SID2 (två kluster).
- SAP Application Server-instanser för hello SAP system SID1 har sina egna dedikerade virtuella datorer.
- SAP Application Server-instanser för hello SAP system SID2 har sina egna dedikerade virtuella datorer.

![Figur 10: SAP hög tillgänglighet arkitektur mall 3, med ett kluster för olika ASCS/SCS-instanser][sap-ha-guide-figure-6003]

_**Figur 10:** SAP hög tillgänglighet arkitektur mall 3, med ett kluster för olika ASCS/SCS-instanser_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Förbereda hello-infrastrukturen

### <a name="prepare-hello-infrastructure-for-architectural-template-1"></a>Förbereda hello infrastrukturen för arkitektur mall 1
Azure Resource Manager-mallar för SAP underlätta distributionen av de nödvändiga resurserna.

Hej trelagers-mallar i Azure Resource Manager stöder också scenarier med hög tillgänglighet som i arkitekturen mall 1, som har två kluster. Varje kluster är ett SAP felpunkt för SAP ASCS/SCS och DBMS.

Här är där du kan hämta Azure Resource Manager-mallar för hello exempelscenario som beskrivs i den här artikeln:

* [Azure Marketplace-avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

tooprepare hello infrastruktur för arkitektur mall 1:

- I hello Azure-portalen på hello **parametrar** bladet i hello **SYSTEMAVAILABILITY** väljer **HA**.

  ![Figur 11: Ange parametrar för SAP hög tillgänglighet Azure Resource Manager][sap-ha-guide-figure-3000]

_**Figur 11:** ange parametrar för SAP hög tillgänglighet Azure Resource Manager_


  Skapa hello-mallar:

  * **Virtuella datorer**:
    * SAP programserver virtuella datorer: <*SAPSystemSID*> - di - <*tal*>
    * ASCS/SCS klustra virtuella datorer: <*SAPSystemSID*> - ascs - <*tal*>
    * DBMS-kluster: <*SAPSystemSID*> - db - <*tal*>

  * **Nätverkskort för alla virtuella datorer med tillhörande IP-adresser**:
    * <*SAPSystemSID*> - nic - di - <*tal*>
    * <*SAPSystemSID*> - nic - ascs - <*tal*>
    * <*SAPSystemSID*> - nic - db - <*tal*>

  * **Azure storage-konton**

  * **Tillgänglighetsgrupper** för:
    * SAP programserver virtuella datorer: <*SAPSystemSID*> - avset - di
    * SAP ASCS/SCS klustra virtuella datorer: <*SAPSystemSID*> - avset - ascs
    * DBMS klustra virtuella datorer: <*SAPSystemSID*> - avset - db

  * **Azure intern belastningsutjämnare**:
    * Med alla portar för hello ASCS/SCS-instansen och IP-adressen <*SAPSystemSID*> - lb - ascs
    * Med alla portar för hello DBMS för SQL Server och IP-adressen <*SAPSystemSID*> - lb - db

  * **Nätverkssäkerhetsgruppen**: <*SAPSystemSID*> - nsg - ascs-0  
    * Med en öppen externa Remote Desktop Protocol (RDP) port toohello <*SAPSystemSID*> - ascs - 0 virtuell dator

> [!NOTE]
> Alla IP-adresser för hello nätverkskort och Azure interna belastningsutjämnare **dynamisk** som standard. Ändra dem för**Statiska** IP-adresser. Vi beskriver hur toodo detta hello nedan.
>
>

### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Distribuera virtuella datorer med företagets nätverk anslutningar (anslutningar mellan lokala) toouse i produktion
Distribuera virtuella Azure-datorer med för produktion SAP-system, [företagets nätverksanslutning (mellan platser)] [ planning-guide-2.2] med hjälp av Azure plats-till-plats-VPN eller Azure ExpressRoute.

> [!NOTE]
> Du kan använda Azure Virtual Network-instans. hello virtuellt nätverk och undernät har redan skapats och förberetts.
>
>

1.  I hello Azure-portalen på hello **parametrar** bladet i hello **NEWOREXISTINGSUBNET** väljer **befintliga**.
2.  I hello **SUBNETID** lägger du till hello fullständig sträng med förberedda Azure nätverket SubnetID där du planerar toodeploy Azure virtuella datorer.
3.  tooget en lista över alla undernät som Azure-nätverk, Kör PowerShell-kommando:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  Hej **ID** fältet visar hello **SUBNETID**.
4. tooget en lista över alla **SUBNETID** värden, Kör PowerShell-kommando:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  Hej **SUBNETID** ser ut så här:

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Distribuera endast molnbaserad SAP-instanser för testning och demo
Du kan distribuera din SAP-systemet för hög tillgänglighet i en endast molnbaserad distributionsmodell. Den här typen av distribution är främst användbart för demonstration och testa användningsområden. Det är inte lämpligt för produktion användningsfall.

- I hello Azure-portalen på hello **parametrar** bladet i hello **NEWOREXISTINGSUBNET** väljer **nya**. Lämna hello **SUBNETID** fältet tomt.

  hello SAP Azure Resource Manager mallen automatiskt skapar hello Azure-nätverk och undernät.

> [!NOTE]
> Du måste också toodeploy minst en dedikerad virtuell dator för Active Directory och DNS i hello samma Azure Virtual Network-instans. hello mall skapar inte dessa virtuella datorer.
>
>


### <a name="prepare-hello-infrastructure-for-architectural-template-2"></a>Förbereda hello infrastrukturen för arkitektur mall 2

Du kan använda den här Azure Resource Manager-mallen för SAP toohelp förenkla distributionen av infrastrukturresurser för SAP arkitektur mall 2.

Här är där du kan hämta Azure Resource Manager-mallar för det här distributionsscenariot:

* [Azure Marketplace-avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-hello-infrastructure-for-architectural-template-3"></a>Förbereda hello infrastrukturen för arkitektur mall 3

Du kan förbereda hello-infrastrukturen och konfigurera SAP för **multi-SID**. Du kan exempelvis lägga till en ytterligare SAP ASCS/SCS-instans i en *befintliga* klusterkonfigurationen. Mer information finns i [och konfigurera en ytterligare instans SAP ASCS/SCS till ett befintligt kluster configuration toocreate en SAP multi-SID-konfiguration i Azure Resource Manager][sap-ha-multi-sid-guide].

Om du vill toocreate ett nytt multi-SID-kluster, kan du använda hello multi-SID [snabbstartsmallar på GitHub](https://github.com/Azure/azure-quickstart-templates).
toocreate ett nytt kluster med flera SID måste toodeploy hello följande tre mallar:

* [ASCS/SCS mall](#ASCS-SCS-template)
* [Databasmall för](#database-template)
* [Mall för servrar](#application-servers-template)

hello ha avsnitten mer information om hello mallar och hello-parametrar som du behöver tooprovide i hello mallar.

#### <a name="ASCS-SCS-template"></a>ASCS/SCS mall

Hej ASCS/SCS mallen distribuerar två virtuella datorer som du kan använda toocreate ett redundanskluster i Windows Server som är värd för flera ASCS/SCS-instanser.

tooset hello ASCS/SCS multi-SID-mall i hello [ASCS/SCS multi-SID-mall][sap-templates-3-tier-multisid-xscs-marketplace-image], ange värden för hello följande parametrar:

  - **Resursen prefixet**.  Ange hello resurs prefixet, vilket används tooprefix alla resurser som har skapats under hello-distribution. Hello resurser inte tillhör tooonly en SAP-system, är hello prefixet hello resursen inte eftersom hello SID för en SAP-system.  hello-prefixet måste vara mellan **tre till sex tecken**.
  - **Stacken typen**. Ange hello stack hello SAP-system. Beroende på hello stack typ har Azure belastningsutjämnare en (ABAP eller endast Java) eller två (ABAP + Java) privata IP-adresser per SAP-system.
  -  **OS-typen**. Välj hello operativsystem hello virtuella datorer.
  -  **SAP System antal**. Välj hello nummer SAP-system som du vill tooinstall i det här klustret.
  -  **Filsystemets tillgänglighet**. Välj **HA**.
  -  **Användarnamn och lösenord för Admin administratör**. Skapa en ny användare som kan använda toosign i toohello datorn.
  -  **Ny eller befintlig undernät**. Ange om ett nytt virtuellt nätverk och undernät som ska skapas eller ett befintligt undernät som ska användas. Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer **befintliga**.
  -  **Undernät-Id**. Ange hello-ID för hello undernät toowhich hello virtuella datorer som ska anslutas. Välj hello undernät för virtuellt privat nätverk (VPN) eller ExpressRoute virtuellt nätverk tooconnect hello virtuella tooyour lokalt nätverk. hello ID ser oftast ut så här:

   /subscriptions/ <*prenumerations-id*> /resourceGroups/ <*resursgruppnamn*> /providers/Microsoft.Network/virtualNetworks/ <*virtuella nätverksnamnet*> /subnets/ <*undernätsnamn*>

hello mallen distribuerar en belastningsutjämnare för Azure-instans som har stöd för flera SAP-system.

- Hej ASCS-instanserna är konfigurerade för instansnummer 00, 10, 20...
- hello SCS-instanserna är konfigurerade för instansnummer 01, 11, 21...
- Hej ASCS sätta replikering Server (ERS) (endast Linux)-instanserna är konfigurerade för instansnummer 02, 12, 22...
- hello SCS ÄNDARE (endast Linux)-instanserna är konfigurerade för instansnummer 03, 13, 23...

hello belastningsutjämnaren innehåller 1.2 för Linux VIP(s), 1 x-VIP för ASCS/SCS och 1 x-VIP för ÄNDARE (endast Linux).

hello innehåller följande lista alla belastningsutjämningsregler (där x är hello antalet hello SAP-systemet, till exempel 1, 2, 3...):
- Windows-specifika portar för varje system för SAP: 445, 5985
- ASCS-portar (instansnummer x0): 32 x 0, 36 x 0, 39 x 0, 81 0 x, 5 x 013, 5 x 014, 5 x 016
- SCS portar (instansnummer x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- ASCS ERS portar på Linux (instansnummer x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216
- SCS ERS portar på Linux (instansnummer x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316

hello belastningsutjämning är konfigurerade toouse hello följande avsökningen portar (där x är hello antalet hello SAP-systemet, till exempel 1, 2, 3...):
- ASCS/SCS interna läsa in belastningsutjämning avsökningsport: 620 x 0
- ÄNDARE interna läsa in belastningsutjämning avsökningsport (endast Linux): 621 x 2

#### <a name="database-template"></a>Databasmall för

hello databasen mallen distribuerar en eller två virtuella datorer som du kan använda tooinstall hello relationella databashanteringssystem (RDBMS) för en SAP-system. Till exempel om du distribuerar en ASCS/SCS-mall för fem SAP-system måste toodeploy mallen fem gånger.

tooset hello databasen multi-SID-mall, i hello [multi-SID-mall för databas][sap-templates-3-tier-multisid-db-marketplace-image], ange värden för hello följande parametrar:

  -  **SAP System-Id**. Ange hello SAP system-ID för hello SAP-system som du vill tooinstall. hello-ID ska användas som ett prefix för hello resurser som distribueras.
  -  **OS-typen**. Välj hello operativsystem hello virtuella datorer.
  -  **DbType**. Välj typ av hello hello databasen som du vill tooinstall på hello klustret. Välj **SQL** om du vill tooinstall Microsoft SQL Server. Välj **HANA** om du tänker tooinstall SAP HANA hello virtuella datorer. Kontrollera tooselect hello rätt operativsystem Systemtyp: Välj **Windows** för SQL och välj en Linux-distributionsplats för HANA. hello konfigureras Azure belastningsutjämnare som är anslutna toohello virtuella datorer kommer att toosupport hello valt databastyp:
    * **SQL**. hello belastningsutjämnaren att belastningsutjämna port 1433. Se till att toouse den här porten för din SQL Server Always On-konfiguration.
    * **HANA**. hello belastningsutjämnaren att belastningsutjämna portarna 35015 och 35017. Se till att tooinstall SAP HANA med instansnummer **50**.
    hello belastningsutjämnare använder avsökningsport 62550.
  -  **SAP systemstorlek**. Hello antal SAP hello nya system ger. Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.
  -  **Filsystemets tillgänglighet**. Välj **HA**.
  -  **Användarnamn och lösenord för Admin administratör**. Skapa en ny användare som kan använda toosign i toohello datorn.
  -  **Undernät-Id**. Ange hello ID hello-undernätet som du använde vid hello distribution av hello ASCS/SCS mall eller hello ID hello undernät som har skapats som en del av hello ASCS/SCS malldistribution.

#### <a name="application-servers-template"></a>Mall för servrar

Hej programmall servrar distribuerar två eller flera virtuella datorer som kan användas som SAP Application Server-instanser för en SAP-system. Till exempel om du distribuerar en ASCS/SCS-mall för fem SAP-system måste toodeploy mallen fem gånger.

tooset in hello servrar multi-SID mall för program, i hello [programmall servrar multi-SID][sap-templates-3-tier-multisid-apps-marketplace-image], ange värden för hello följande parametrar:

  -  **SAP System-Id**. Ange hello SAP system-ID för hello SAP-system som du vill tooinstall. hello-ID ska användas som ett prefix för hello resurser som distribueras.
  -  **OS-typen**. Välj hello operativsystem hello virtuella datorer.
  -  **SAP systemstorlek**. hello antalet SAP hello nya system ger. Om du inte är säker på hur många SAP hello system kräver fråga din SAP-teknikpartner eller systemintegreraren.
  -  **Filsystemets tillgänglighet**. Välj **HA**.
  -  **Användarnamn och lösenord för Admin administratör**. Skapa en ny användare som kan använda toosign i toohello datorn.
  -  **Undernät-Id**. Ange hello ID hello-undernätet som du använde vid hello distribution av hello ASCS/SCS mall eller hello ID hello undernät som har skapats som en del av hello ASCS/SCS malldistribution.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuella Azure-nätverket
I vårt exempel är hello adressutrymme hello virtuella Azure-nätverket 10.0.0.0/16. Det finns ett undernät som kallas **undernät**, med en-adressintervallet 10.0.0.0/24. Alla virtuella datorer och interna belastningsutjämnare har distribuerats i det här virtuella nätverket.

> [!IMPORTANT]
> Inte göra några ändringar toohello nätverksinställningar i hello gästoperativsystemet. Detta inkluderar IP-adresser, DNS-servrar och undernät. Konfigurera nätverksinställningarna i Azure. hello DHCP Dynamic Host Configuration Protocol ()-tjänsten sprider dina inställningar.
>
>

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP-adresser

tooset hello krävs DNS IP-adresser, hello följande steg.

1.  I hello Azure-portalen på hello **DNS-servrar** bladet, se till att det virtuella nätverket **DNS-servrar** alternativet anges för**anpassad DNS**.
2.  Välj inställningarna utifrån hello typen av nätverk som du har. Mer information finns i hello följande resurser:
    * [Företagets nätverksanslutning (mellan platser)][planning-guide-2.2]: Lägg till hello IP-adresser för hello lokala DNS-servrar.  
    Du kan utöka lokala DNS-servrar toohello virtuella datorer som körs i Azure. I scenariot, du kan lägga till hello IP-adresser för hello Azure virtuella datorer som du kör hello DNS-tjänsten.
    * [Endast molnbaserad distribution][planning-guide-2.1]: distribuera en ytterligare virtuell dator i hello samma virtuella nätverk-instans som fungerar som en DNS-server. Lägg till hello IP-adresser för hello Azure virtuella datorer som du har konfigurerat toorun DNS-tjänsten.

    ![Figur 12: Konfigurera DNS-servrar för Azure-nätverk][sap-ha-guide-figure-3001]

    _**Figur 12:** konfigurera DNS-servrar för Azure-nätverk_

  > [!NOTE]
  > Om du ändrar hello IP-adresser för hello DNS-servrar du behöver toorestart hello Azure virtuella datorer tooapply hello ändringen och sprida hello nya DNS-servrar.
  >
  >

I vårt exempel har hello DNS-tjänsten installerats och konfigurerats på dessa virtuella Windows-datorer:

| Rollen virtuell dator | Värdnamn för virtuell dator | Namn på nätverkskort | Statisk IP-adress |
| --- | --- | --- | --- |
| Första DNS-servern |domcontr 0 |PR1-nic-domcontr-0 |10.0.0.10 |
| Andra DNS-server |domcontr 1 |PR1-nic-domcontr-1 |10.0.0.11 |

### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Värdnamn och statiska IP-adresser för hello SAP ASCS/SCS klustrade instansen och DBMS klustrad instans

För lokal distribution måste dessa reserverade värdnamn och IP-adresser:

| Virtuell värd namn roll | Virtuella värdnamn | Virtuell statisk IP-adress |
| --- | --- | --- |
| SAP ASCS/SCS första klustret virtuellt värdnamn (för hantering av kluster) |PR1-ascs-vir |10.0.0.42 |
| SAP ASCS/SCS virtuell värd instansnamn |PR1-ascs-sap |10.0.0.43 |
| SAP DBMS andra kluster virtuellt värdnamn (hantering) |PR1-dbms-vir |10.0.0.32 |

När du skapar klustret hello skapa hello virtuella värdnamn **pr1-ascs-vir** och **pr1-dbms-vir** och hello tillhörande IP-adresser som hanterar själva hello-klustret. Information om hur toodo detta, se [samla in klusternoder i en klusterkonfiguration][sap-ha-guide-8.12.1].

Du kan manuellt skapa hello andra två virtuella värdnamn **pr1-ascs-sap** och **pr1-dbms-sap**, och hello tillhörande IP-adresser på hello DNS-server. hello klustrad SAP ASCS/SCS-instans och hello klustrade DBMS instans använder dessa resurser. Information om hur toodo detta, se [skapa ett virtuellt värdnamn för en klustrad instans av SAP ASCS/SCS][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Ange statiska IP-adresser för hello SAP virtuella datorer
När du distribuerar hello toouse för virtuella datorer i klustret måste tooset statiska IP-adresser för alla virtuella datorer. Gör detta i hello Azure Virtual Network configuration och inte i hello gästoperativsystemet.

1.  Välj i hello Azure-portalen, **resursgruppen** > **nätverkskort** > **inställningar** > **IP-adress** .
2.  På hello **IP-adresser** bladet under **tilldelning**väljer **statiska**. I hello **IP-adress** ange hello IP-adress som du vill toouse.

  > [!NOTE]
  > Om du ändrar hello IP-adressen för nätverkskortet för hello måste toorestart hello Azure virtuella datorer tooapply hello ändringen.  
  >
  >

  ![Figur 13: Ange statiska IP-adresser för hello nätverkskort för varje virtuell dator][sap-ha-guide-figure-3002]

  _**Figur 13:** ange statiska IP-adresser för hello nätverkskort för varje virtuell dator_

  Upprepa det här steget för alla nätverksgränssnitt, det vill säga för alla virtuella datorer, inklusive virtuella datorer som du vill toouse för Active Directory/DNS-tjänsten.

I vårt exempel har vi dessa virtuella datorer och statiska IP-adresser:

| Rollen virtuell dator | Värdnamn för virtuell dator | Namn på nätverkskort | Statisk IP-adress |
| --- | --- | --- | --- |
| Första SAP Application Server-instansen |PR1-di-0 |PR1-nic-di-0 |10.0.0.50 |
| Andra SAP Application Server-instans |PR1-di-1 |PR1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Senaste SAP Application Server-instans |PR1-di-5 |PR1-nic-di-5 |10.0.0.55 |
| Första klusternoden för ASCS/SCS-instans |PR1-ascs-0 |PR1-nic-ascs-0 |10.0.0.40 |
| Andra klusternod för ASCS/SCS-instans |PR1-ascs-1 |PR1-nic-ascs-1 |10.0.0.41 |
| Första klusternoden för DBMS-instans |PR1-db-0 |PR1-nic-db-0 |10.0.0.30 |
| Andra klusternod för DBMS-instans |PR1-db-1 |PR1-nic-db-1 |10.0.0.31 |

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Ange en statisk IP-adress för hello Azure interna belastningsutjämnare

hello skapas SAP Azure Resource Manager en Azure intern belastningsutjämnare som används för hello SAP ASCS/SCS-instans och hello DBMS-klustret.

> [!IMPORTANT]
> hello IP-adress för hello virtuella värdnamn hello SAP ASCS/SCS är hello samma som hello IP-adressen för hello SAP ASCS/SCS intern belastningsutjämnare: **pr1-lb-ascs**.
> hello IP-adressen för hello virtuella namnet på hello DBMS är hello samma som hello IP-adressen för hello DBMS intern belastningsutjämnare: **pr1-lb-dbms**.
>
>

tooset en statisk IP-adress för hello Azure interna belastningsutjämnare:

1.  hello första distributionen anger hello interna IP-adressen för belastningsutjämnaren för**dynamiska**. I hello Azure-portalen på hello **IP-adresser** bladet under **tilldelning**väljer **statiska**.
2.  Ange hello IP-adressen för hello intern belastningsutjämnare **pr1-lb-ascs** toohello IP-adress för hello virtuella värdnamn hello SAP ASCS/SCS-instans.
3.  Ange hello IP-adressen för hello intern belastningsutjämnare **pr1-lb-dbms** toohello IP-adress för hello virtuella värdnamn hello DBMS-instans.

  ![Figur 14: Ange statiska IP-adresser för hello intern belastningsutjämnare för hello SAP ASCS/SCS-instans][sap-ha-guide-figure-3003]

  _**Figur 14:** ange statiska IP-adresser för hello intern belastningsutjämnare för hello SAP ASCS/SCS-instans_

I vårt exempel har vi två Azure interna belastningsutjämnare som har dessa statiska IP-adresser:

| Azure interna belastningsutjämnarrollen | Azure interna belastningsutjämnarens namn | Statisk IP-adress |
| --- | --- | --- |
| Interna belastningsutjämnare för SAP ASCS/SCS-instans |ascs-lb-PR1 |10.0.0.43 |
| SAP DBMS intern belastningsutjämnare |PR1-lb-dbms |10.0.0.33 |


### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Standard ASCS/SCS belastningsutjämningsregler för hello Azure interna belastningsutjämnare

hello SAP Azure Resource Manager-mall skapar hello-portar som du behöver:
* En ABAP ASCS-instans med hello standard instansnummer **00**
* En Java-SCS-instans med hello standard instansnummer **01**

När du installerar din SAP ASCS/SCS-instans måste du använda hello standard instansnummer **00** ABAP ASCS-instans och hello standard instans-ID **01** för din Java SCS-instans.

Skapa sedan nödvändiga intern belastningsutjämning slutpunkter för hello SAP NetWeaver portar.

toocreate krävs intern belastningsutjämning slutpunkter, börja med att skapa dessa slutpunkter för hello SAP NetWeaver ABAP ASCS-portar för belastningsutjämning:

| Tjänsten/belastningen belastningsutjämning Regelnamn | Standardportnumren. | Konkret portar för (ASCS-instans med instansnummer 00) (ERS med 10) |
| --- | --- | --- |
| Sätta Server / *lbrule3200* |32 <*InstanceNumber*> |3200 |
| ABAP Message Server / *lbrule3600* |36 <*InstanceNumber*> |3600 |
| Internt ABAP meddelande / *lbrule3900* |39 <*InstanceNumber*> |3900 |
| Serverns HTTP-meddelandet / *Lbrule8100* |81 <*InstanceNumber*> |8100 |
| SAP Start Service ASCS HTTP / *Lbrule50013* |5 <*InstanceNumber*> 13 |50013 |
| SAP Start Service ASCS HTTPS / *Lbrule50014* |5 <*InstanceNumber*> 14 |50014 |
| Sätta replikering / *Lbrule50016* |5 <*InstanceNumber*> 16 |50016 |
| SAP Start Service ÄNDARE HTTP *Lbrule51013* |5 <*InstanceNumber*> 13 |51013 |
| SAP Start Service ÄNDARE HTTP *Lbrule51014* |5 <*InstanceNumber*> 14 |51014 |
| Win RM *Lbrule5985* | |5985 |
| Filresurs *Lbrule445* | |445 |

_**Tabell 1:** portnummer hello SAP NetWeaver ABAP ASCS-instanser_

Skapa sedan dessa slutpunkter för hello SAP NetWeaver Java SCS portar för belastningsutjämning:

| Tjänsten/belastningen belastningsutjämning Regelnamn | Standardportnumren. | Konkret portar för (SCS-instans med instansnummer 01) (ERS med 11) |
| --- | --- | --- |
| Sätta Server / *lbrule3201* |32 <*InstanceNumber*> |3201 |
| Gateway-Server / *lbrule3301* |33 <*InstanceNumber*> |3301 |
| Java Message Server / *lbrule3900* |39 <*InstanceNumber*> |3901 |
| Serverns HTTP-meddelandet / *Lbrule8101* |81 <*InstanceNumber*> |8101 |
| SAP Start Service SCS HTTP / *Lbrule50113* |5 <*InstanceNumber*> 13 |50113 |
| SAP Start Service SCS HTTPS / *Lbrule50114* |5 <*InstanceNumber*> 14 |50114 |
| Sätta replikering / *Lbrule50116* |5 <*InstanceNumber*> 16 |50116 |
| SAP Start Service ÄNDARE HTTP *Lbrule51113* |5 <*InstanceNumber*> 13 |51113 |
| SAP Start Service ÄNDARE HTTP *Lbrule51114* |5 <*InstanceNumber*> 14 |51114 |
| Win RM *Lbrule5985* | |5985 |
| Filresurs *Lbrule445* | |445 |

_**Tabell 2:** portnummer hello SAP NetWeaver Java SCS instanser_

![Figur 15: Standard ASCS/SCS belastningsutjämningsregler för hello Azure interna belastningsutjämnare][sap-ha-guide-figure-3004]

_**Figur 15:** standard ASCS/SCS regler för hello Azure intern belastningsutjämnare för belastningsutjämning_

Ange hello IP-adressen för belastningsutjämnaren hello **pr1-lb-dbms** toohello IP-adress för hello virtuella värdnamn hello DBMS-instans.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Ändra hello ASCS/SCS standard belastningsutjämning regler för hello Azure interna belastningsutjämnare

Om du vill toouse olika nummer för hello SAP ASCS eller SCS instanser, måste du ändra hello namn och värden på deras hamnar från standardvärdena.

1.  Markera i hello Azure-portalen,  **<* SID*> - lb - ascs läsa in belastningsutjämning ** > **regler för att läsa in belastningsutjämning**.
2.  För alla belastningsutjämningsregler som tillhör toohello SAP ASCS eller SCS-instans, ändra dessa värden:

  * Namn
  * Port
  * Backend-port

  Om du vill toochange hello ASCS instans Standardnumret från 00 too31, måste du till exempel toomake hello ändringar för alla portar som anges i tabell 1.

  Här är ett exempel på en uppdatering för port *lbrule3200*.

  ![Bild 16: Ändra hello ASCS/SCS standard belastningsutjämning regler för hello Azure interna belastningsutjämnare][sap-ha-guide-figure-3005]

  _**Bild 16:** ändra hello ASCS/SCS standard belastningsutjämningsregler för hello Azure interna belastningsutjämnare_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Lägg till Windows-datorer toohello domän

När du tilldelar en statisk IP-adress toohello virtuella datorer kan du lägga till hello virtuella datorer toohello domän.

![Figur 17: Lägga till en virtuell dator tooa domän][sap-ha-guide-figure-3006]

_**Figur 17:** lägga till en virtuell dator tooa domän_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Lägg till registerposter på båda klusternoderna för hello SAP ASCS/SCS-instansen

Azure belastningsutjämnare har en intern belastningsutjämnare att stängs anslutningar när hello anslutningar är inaktiva för en viss tid (en timeout för inaktivitet). SAP arbetsprocesser i dialogrutan instanser öppna anslutningar toohello SAP sätta bearbeta så snart hello första sätta/status Created begäran måste toobe skickas. Dessa anslutningar förblir oftast etablerade tills arbetsprocessen för hello eller hello sätta processen startas om. Men om hello anslutning är inaktiv under en angiven tidsperiod, hello Azure interna belastningen belastningsutjämnaren stängs hello anslutningar. Detta är inte ett problem Eftersom hello SAP arbetsprocessen återupprättar hello anslutningsprocessen toohello sätta om den inte längre finns. Dessa aktiviteter finns dokumenterade i hello developer spår av SAP processer, men de skapar en stor mängd extra innehåll i dessa spår. Det är en bra idé toochange hello TCP/IP `KeepAliveTime` och `KeepAliveInterval` på båda klusternoderna. Kombinera ändringarna i hello TCP/IP-parametrar med SAP profil-parametrar som beskrivs senare i hello artikeln.

tooadd registerposter på båda klusternoderna av hello SAP ASCS/SCS instansen, Lägg först till dessa registerposter i Windows på båda klusternoderna i Windows för SAP ASCS/SCS:

| Sökväg | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Variabelnamn |`KeepAliveTime` |
| Variabeltyp |REG_DWORD (decimalt) |
| Värde |120000 |
| Länken toodocumentation |[https://technet.microsoft.com/en-us/library/cc957549.aspx](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

_**Tabell 3:** ändra hello första TCP/IP-parameter_

Lägg sedan till den här Windows-registerposter på båda klusternoderna i Windows för SAP ASCS/SCS:

| Sökväg | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Variabelnamn |`KeepAliveInterval` |
| Variabeltyp |REG_DWORD (decimalt) |
| Värde |120000 |
| Länken toodocumentation |[https://technet.microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

_**Tabell 4:** ändra hello andra TCP/IP-parameter_

**tooapply hello ändringar, starta om båda klusternoderna**.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Konfigurera ett redundanskluster i Windows Server-kluster för en SAP ASCS/SCS-instans

Konfigurera ett redundanskluster i Windows Server-kluster för en SAP ASCS/SCS-instans innebär att dessa uppgifter:

- Samla in hello klusternoder i en klusterkonfiguration
- Konfigurera ett filresursvittne för kluster

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Samla in hello klusternoder i en klusterkonfiguration

1.  Lägga till redundansklustring tooboth klusternoder i hello Lägg till roller och funktioner.
2.  Ställ in hello failover-kluster med hjälp av hanteraren för redundanskluster. I hanteraren för redundanskluster, Välj **Skapa kluster**, och Lägg sedan till endast hello namn hello första kluster nod A. Lägg inte till andra hello-noden ännu; du ska lägga till hello andra noden i ett senare steg.

  ![Figur 18: Lägga till hello server eller virtuella namnet på hello första klusternoden][sap-ha-guide-figure-3007]

  _**Figur 18:** Lägg till hello server eller virtuella namnet på hello första klusternoden_

3.  Ange hello nätverksnamn (virtuellt värdnamn) hello-klustret.

  ![Bild 19: Ange hello klustrets namn][sap-ha-guide-figure-3008]

  _**Bild 19:** RETUR hello klustrets namn_

4.  När du har skapat hello-kluster som kör ett klustervalideringstest.

  ![Figur 20: Kör hello klustret verifieringen][sap-ha-guide-figure-3009]

  _**Figur 20:** kör hello klustret verifieringen_

  Du kan ignorera alla varningar om diskarna nu hello pågår. Du ska lägga till en filresurs och hello SIOS delade diskar senare. I det här skedet behöver du inte tooworry om ha ett kvorum.

  ![Figur 21: Inga kvorumdisken hittas][sap-ha-guide-figure-3010]

  _**Figur 21:** inga kvorumdisken hittas_

  ![Figur 22: Core klusterresurs måste en ny IP-adress][sap-ha-guide-figure-3011]

  _**Figur 22:** Core klusterresurs måste en ny IP-adress_

5.  Ändra hello IP-adressen för hello core klustertjänsten. hello kluster kan inte starta tills du ändrar hello IP-adressen för hello core klustertjänsten, eftersom hello hello-serverns IP-adress pekar tooone hello virtuella noder. Gör detta på hello **egenskaper** sidan hello core klustertjänstens IP-resurs.

  Exempelvis måste tooassign en IP-adress (i vårt exempel **10.0.0.42**) för hello klustrets virtuella värdnamn **pr1-ascs-vir**.

  ![Figur 23: Hello Egenskaper dialogrutan Ändra hello IP-adress][sap-ha-guide-figure-3012]

  _**Figur 23:** i hello **egenskaper** dialogrutan, ändra hello IP-adress_

  ![Figur 24: Tilldela hello IP-adress som är reserverad för hello kluster][sap-ha-guide-figure-3013]

  _**Figur 24:** tilldela hello IP-adress som är reserverad för hello kluster_

6.  Sätta hello virtuell värd klusternamnet.

  ![Bild 25: Core klustertjänsten är igång och körs och med hello rätt IP-adress][sap-ha-guide-figure-3014]

  _**Bild 25:** core klustertjänsten är igång och körs, och med hello rätt IP-adress_

7.  Lägga till andra hello-klusternod.

  Nu när hello core klustertjänsten är igång, du kan lägga till hello andra noder.

  ![Bild 26: Lägga till hello andra klusternod][sap-ha-guide-figure-3015]

  _**Bild 26:** Lägg till hello andra klusternod_

8.  Ange ett namn för hello andra nod klustervärd.

  ![Bild 27: Ange hello andra värden klusternodnamnet][sap-ha-guide-figure-3016]

  _**Bild 27:** RETUR hello andra värden klusternodnamnet_

  > [!IMPORTANT]
  > Måste du se till att hello **lägga till alla tillgängliga lagringsenheter toohello klustret** kryssrutan är **inte** valda.  
  >
  >

  ![Bild 28: Markera inte kryssrutan hello][sap-ha-guide-figure-3017]

  _**Bild 28:** gör **inte** Välj hello kryssrutan_

  Du kan ignorera varningar om kvorum och diskar. Du måste ställa in hello kvorum och dela hello senare, enligt beskrivningen i [installerar SIOS DataKeeper Cluster Edition för SAP ASCS/SCS klusterdisk resursen][sap-ha-guide-8.12.3].

  ![Bild 29: Ignorera varningar om hello disk kvorum][sap-ha-guide-figure-3018]

  _**Bild 29:** Ignorera varningar om hello disk kvorum_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Konfigurera ett filresursvittne för kluster

När du konfigurerar ett kluster filresursvittne inkluderar dessa uppgifter:

- Skapa en filresurs
- Inställningen hello filen filresurs vittne kvorum i hanteraren för redundanskluster

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Skapa en filresurs

1.  Välj ett filresursvittne i stället för en kvorumdisk. SIOS DataKeeper stöder det här alternativet.

  I hello exemplen i den här artikeln är hello filresursvittne på hello Active Directory/DNS-server som körs i Azure. hello filresursvittne kallas **domcontr 0**. Eftersom du skulle har konfigurerat ett VPN-anslutningen tooAzure (via plats-till-plats-VPN eller Azure ExpressRoute) kan dela din Active Directory/DNS-tjänsten är lokalt och inte är en fil med lämpliga toorun vittne.

  > [!NOTE]
  > Om Active Directory/DNS-tjänsten körs bara lokalt, inte konfigurera din filresursvittne på hello Active Directory/DNS-Windows-operativsystem som körs lokalt. Nätverksfördröjningen mellan klusternoder som körs i Azure och Active Directory/DNS lokalt kan vara för stort och orsaka problem med nätverksanslutningen. Vara att tooconfigure hello filresursvittne på en virtuell Azure-dator som kör Stäng toohello klusternod.  
  >
  >

  hello kvorum enhet behöver minst 1 024 MB ledigt utrymme. Vi rekommenderar 2 048 MB ledigt utrymme för hello kvorum enhet.

2.  Lägg till hello klusternamnobjekt.

  ![Bild 30: Tilldela hello behörigheter på hello resurs för hello klusternamnobjekt][sap-ha-guide-figure-3019]

  _**Bild 30:** tilldela hello behörigheter på hello resurs för hello klusternamnobjekt_

  Se till att hello behörigheter inkluderar hello myndigheten toochange data i hello resurs för hello klusternamnobjekt (i vårt exempel **pr1-ascs-vir$**).

3.  tooadd hello klustrets namn toohello objektlistan väljer **Lägg till**. Ändra hello filter toocheck för datorobjekt i tillägg toothose visas i bild 31.

  ![Bild 31: Ändra hello objekttyper tooinclude datorer][sap-ha-guide-figure-3020]

  _**Bild 31:** ändra hello objekttyper tooinclude datorer_

  ![Bild 32: Kryssrutan hello datorer][sap-ha-guide-figure-3021]

  _**Bild 32:** väljer hello **datorer** kryssruta_

4.  Ange hello klusternamnobjekt som visas i bild 31. Eftersom hello post har redan har skapats och kan du ändra hello behörigheter som visas i bild 30.

5.  Välj hello **säkerhet** fliken hello resursen och ange sedan mer detaljerade behörigheter för hello klusternamnobjekt.

  ![Bild 33: Ange hello säkerhetsattribut för hello klusternamnobjekt på hello filen resursen kvorum][sap-ha-guide-figure-3022]

  _**Bild 33:** ange hello säkerhetsattribut för hello klusternamnobjekt på hello filen resursen kvorum_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Ange hello filen filresurs vittne kvorum i hanteraren för redundanskluster

1.  Öppna hello konfigurationsguiden kvorum inställningen.

  ![Bild 34: Starta hello inställningen guiden Konfigurera klusterkvorum][sap-ha-guide-figure-3023]

  _**Bild 34:** Start hello konfigurera inställningen guiden klusterkvorum_

2.  På hello **Välj kvorumkonfigurationen** väljer **Välj kvorumvittne hello**.

  ![Bild 35: Kvorumkonfigurationer som du kan välja mellan][sap-ha-guide-figure-3024]

  _**Bild 35:** kvorumkonfigurationer som du kan välja mellan_

3.  På hello **Välj Kvorumvittne** väljer **konfigurera ett filresursvittne**.

  ![Bild 36: Välj hello filresursvittne][sap-ha-guide-figure-3025]

  _**Bild 36:** Välj hello filresursvittne_

4.  Ange hello UNC-sökvägen toohello filresurs (i vårt exempel \\domcontr 0\FSW). toosee en lista över hello ändringar du kan göra, Välj **nästa**.

  ![Bild 37: Definiera hello filresursplats för hello vittne resurs][sap-ha-guide-figure-3026]

  _**Bild 37:** definiera hello filresursplats för hello vittne resurs_

5.  Välj hello ändringar och välj sedan **nästa**. Du behöver toosuccessfully omkonfigurera hello klusterkonfigurationen som visas i figur 38.  

  ![Bild 38: Bekräftelse att du har konfigurerat om hello kluster][sap-ha-guide-figure-3027]

  _**Bild 38:** bekräftelse att du har konfigurerat om hello kluster_

När du har installerat hello redundanskluster i Windows har förändringar som måste toobe görs toosome tröskelvärden tooadapt redundans identifiering tooconditions i Azure. hello parametrar toobe ändras dokumenteras i den här bloggen: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/. Förutsatt att din två virtuella datorer som skapa hello är Windows klusterkonfigurationen för ASCS/SCS i Hej samma undernät, hello följande parametrar måste toobe ändras toothese värden:
- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Dessa inställningar har testats med kunder och tillhandahålls en bra kompromiss toobe tillräckligt flexibla på ena sidan av hello. På hello andra hand dessa inställningar har att erbjuda snabb tillräckligt med redundans i verkliga felvillkor på SAP programvara eller nod och VM-fel. 

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Installera SIOS DataKeeper Cluster Edition för hello SAP ASCS/SCS klusterdisk resursen

Nu har du en fungerande konfiguration för Windows Server Failover Clustering i Azure. Men tooinstall en SAP ASCS/SCS-instans, behöver du en delad disk-resurs. Du kan inte skapa hello delad diskresurser som du behöver i Azure. SIOS DataKeeper Cluster Edition är en lösning för från tredje part som du kan använda delade toocreate diskresurser.

Installera SIOS DataKeeper Cluster Edition för hello SAP ASCS/SCS omfattar resursen klusterdisk dessa aktiviteter:

- Lägga till hello .NET Framework 3.5
- Installera SIOS DataKeeper
- Ställa in SIOS DataKeeper

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Lägg till hello .NET Framework 3.5
hello Microsoft .NET Framework 3.5 är inte automatiskt aktiverats eller installerats på Windows Server 2012 R2. Eftersom SIOS DataKeeper kräver hello .NET Framework toobe på alla noder som du installerar DataKeeper på, måste du installera hello .NET Framework 3.5 hello gästoperativsystemet för alla virtuella datorer i hello kluster.

Det finns två sätt tooadd hello .NET Framework 3.5:

- Använd guiden hello Lägg till roller och funktioner i Windows som visas i bild 39.

  ![Bild 39: Installera hello .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner][sap-ha-guide-figure-3028]

  _**Bild 39:** hello installera .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner_

  ![Bild 40: Installationen förloppsindikator när du installerar hello .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner][sap-ha-guide-figure-3029]

  _**Bild 40:** Installation förloppsindikator när du installerar hello .NET Framework 3.5 med hjälp av guiden hello Lägg till roller och funktioner_

- Använd hello kommandoradsverktyget dism.exe. För den här typen av installation måste tooaccess hello SxS katalog på hello Windows-installationsmediet. Skriv följande vid en upphöjd kommandotolk:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>Installera SIOS DataKeeper

Installera SIOS DataKeeper Cluster Edition på varje nod i klustret hello. toocreate virtuella delad lagring med SIOS DataKeeper, skapa en synkroniserad spegling och simulera delad klusterlagring.

Innan du installerar hello SIOS programvara skapa hello domänanvändare **DataKeeperSvc**.

> [!NOTE]
> Lägg till hello **DataKeeperSvc** användare toohello **lokal administratör** på båda klusternoderna.
>
>

tooinstall SIOS DataKeeper:

1.  Installera hello SIOS på båda klusternoderna.

  ![SIOS installer][sap-ha-guide-figure-3030]

  ![Bild 41: Första sidan i hello SIOS DataKeeper installation][sap-ha-guide-figure-3031]

  _**Bild 41:** första sidan i hello SIOS DataKeeper installation_

2.  Välj hello dialogrutan visas i bild 42, **Ja**.

  ![Bild 42: DataKeeper informerar dig om att en tjänst kommer att inaktiveras][sap-ha-guide-figure-3032]

  _**Bild 42:** DataKeeper informerar dig om att en tjänst kommer att inaktiveras_

3.  Vi rekommenderar att du väljer hello i dialogrutan visas i bild 43 **domän eller Server konto**.

  ![Bild 43: Användarens val för SIOS DataKeeper][sap-ha-guide-figure-3033]

  _**Bild 43:** användarens val för SIOS DataKeeper_

4.  Ange hello domän kontots användarnamn och lösenord som du skapade för SIOS DataKeeper.

  ![Bild 44: Ange hello domänanvändarnamn och lösenord för hello SIOS DataKeeper installation][sap-ha-guide-figure-3034]

  _**Bild 44:** ange hello domänanvändarnamn och lösenord för hello SIOS DataKeeper installation_

5.  Installera hello licensnyckel för din SIOS DataKeeper-instans som visas i bild 45.

  ![Bild 45: Ange licensnyckeln SIOS DataKeeper][sap-ha-guide-figure-3035]

  _**Bild 45:** Ange licensnyckeln SIOS DataKeeper_

6.  När du uppmanas starta om hello virtuell dator.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Ställ in SIOS DataKeeper

När du installerar SIOS DataKeeper på båda noderna, måste toostart hello konfiguration. hello målet med hello konfiguration är toohave synkron replikering mellan hello ytterligare virtuella hårddiskar anslutna tooeach hello virtuella datorer.

1.  Starta hello DataKeeper hantering och verktyg och välj sedan **ansluta servern**. (I figur 46 det här alternativet är i en röd cirkel.)

  ![Bild 46: SIOS DataKeeper hantering och verktyg][sap-ha-guide-figure-3036]

  _**Bild 46:** SIOS DataKeeper hantering och konfiguration av verktyget_

2.  Ange hello namn eller TCP/IP-adressen för hello första nod hello hantering och konfiguration verktyget ska ansluta till, och i andra steget hello andra noden.

  ![Bild 47: Infoga hello namn eller TCP/IP-adress för hello första nod hello hantering och konfiguration verktyget ska ansluta till, och i andra steget hello andra noden][sap-ha-guide-figure-3037]

  _**Bild 47:** Insert hello namn eller TCP/IP-adress för hello första nod hello hantering och konfiguration verktyget ska ansluta till och i andra steget hello andra noden_

3.  Skapa hello replikering mellan hello två noder.

  ![Bild 48: Skapa ett jobb för replikering][sap-ha-guide-figure-3038]

  _**Bild 48:** skapa ett jobb för replikering_

  En guide hjälper dig att hello processen att skapa ett jobb för replikering.
4.  Definiera hello namn, TCP/IP-adress och diskvolymen för hello Källnoden.

  ![Bild 49: Definiera hello namnet på hello replikering jobb][sap-ha-guide-figure-3039]

  _**Bild 49:** definiera hello namnet på hello replikering jobb_

  ![Bild 50: Definiera hello grunddata för hello nod som ska vara hello aktuella Källnoden][sap-ha-guide-figure-3040]

  _**Bild 50:** definiera hello grunddata för hello nod som ska vara hello aktuella Källnoden_

5.  Definiera hello namn, TCP/IP-adress och volymen av hello målnoden.

  ![Bild av 51: Definiera hello grunddata för hello nod som ska vara hello aktuella målnod][sap-ha-guide-figure-3041]

  _**Bild av 51:** definiera hello grunddata för hello nod som ska vara hello aktuella målnod_

6.  Definiera hello komprimeringsalgoritmer. I vårt exempel rekommenderar vi att du komprimera hello replikering dataströmmen. Särskilt i omsynkroniseringen situationer minskar hello-komprimering av hello replikering dataströmmen kraftigt omsynkroniseringen tid. Observera att komprimering används hello processor och RAM resurser för en virtuell dator. Som hello komprimering ökar, så hello mängden processorresurser som används. Du kan också ändra denna inställning senare.

7.  En annan inställningen du behöver toocheck är om hello replikeringen sker synkront eller asynkront. *När du skyddar SAP ASCS/SCS konfigurationer, måste du använda synkron replikering*.  

  ![Bild 52: Definiera replikeringsinformation][sap-ha-guide-figure-3042]

  _**Bild 52:** definiera replikeringsinformation_

8.  Definiera om hello volym som replikeras av hello replikering jobb ska vara representeras tooa Windows Server Failover Clustering klusterkonfigurationen som en delad disk. Hello SAP ASCS/SCS konfiguration, markera **Ja** så att hello Windows kluster ser hello replikerade volymen som en delad disk som kan användas som en volym i klustret.

  ![Bild 53: Välj Ja tooset hello replikerad volym som en klustervolym][sap-ha-guide-figure-3043]

  _**Bild 53:** Välj **Ja** tooset hello replikeras volymen som en klustervolym_

  När hello volym skapas visar hello DataKeeper hantering och konfiguration verktyget hello replikering jobbet är aktiv.

  ![Bild 54: DataKeeper synkroniserad spegling för hello SAP ASCS/SCS resursen disk är aktiv][sap-ha-guide-figure-3044]

  _**Bild 54:** DataKeeper synkroniserad spegling för hello SAP ASCS/SCS dela disken är aktiv_

  Hanteraren för redundanskluster innehåller nu hello disk som en DataKeeper disk som visas i figur 55.

  ![Bild 55: Klusterhanterare för växling vid fel visar hello-disk som DataKeeper replikeras][sap-ha-guide-figure-3045]

  _**Bild 55:** Klusterhanteraren visar hello disk som DataKeeper replikeras_

## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Installera hello SAP NetWeaver system

Vi kommer inte beskriver hello DBMS installationen eftersom inställningar varierar beroende på hello DBMS system du använder. Men förutsätter vi att få hög tillgänglighet med hello DBMS behandlas med hello funktioner hello olika DBMS-leverantörer som har stöd för Azure. Till exempel alltid på eller databasspegling för SQL Server och Oracle Data Guard för Oracle-databaser. Vi inte lägga till mer skydd toohello DBMS i hello-scenariot som vi använder i den här artikeln.

Det finns inga särskilda överväganden när olika DBMS tjänster interagera med den här typen av klustrad SAP ASCS/SCS konfiguration i Azure.

> [!NOTE]
> hello installationsproceduren för SAP NetWeaver ABAP system, Java-system och ABAP + Java system är nästan identiska. hello viktigaste skillnaden är att ett SAP ABAP system har en ASCS-instans. hello SAP Java system har en SCS-instans. hello SAP ABAP + Java system har en ASCS-instans och en SCS-instans som körs i hello samma klustergrupp för Microsoft-redundans. Installationen skillnader för varje SAP NetWeaver installationen stack anges explicit. Du kan anta att alla andra delar är hello samma.  
>
>

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Installera SAP med en hög tillgänglighet ASCS/SCS-instans

> [!IMPORTANT]
> Glöm inte tooplace sidan fil på DataKeeper speglade volymer. DataKeeper stöder inte speglade volymer. Du kan lämna växlingsfilen på hello tillfälliga enhet D av en virtuell Azure-dator, vilket är hello standard. Om den inte redan finns där, flytta hello Windows sidan filen toodrive D på ditt Azure-datorn.
>
>

Dessa uppgifter omfattar att installera SAP med en hög tillgänglighet ASCS/SCS-instans:

- Skapa ett virtuellt värdnamn för hello klustrade SAP ASCS/SCS-instans
- Installera hello SAP första klusternoden
- Ändra hello SAP profil hello ASCS/SCS instans
- Lägger till en avsökningsport
- Öppna hello avsökningsport för Windows-brandväggen

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Skapa ett virtuellt värdnamn för hello klustrade SAP ASCS/SCS-instans

1.  Skapa en DNS-post för hello virtuella värdnamn för hello ASCS/SCS-instansen i hello Windows DNS-hanteraren.

  > [!IMPORTANT]
  > hello IP-adress som du tilldelar toohello virtuella värdnamn för hello ASCS/SCS instansen måste vara hello samma som hello IP-adress som du tilldelade tooAzure belastningsutjämnare (**<*SID*> - lb - ascs **).  
  >
  >

  Hej IP-adressen för hello virtuella värdnamn för SAP ASCS/SCS (**pr1-ascs-sap**) hello är samma som hello IP-adress för Azure belastningsutjämnare (**pr1-lb-ascs**).

  ![Bild 56: Definiera hello DNS-post för hello SAP ASCS/SCS kluster virtuella namn och TCP/IP-adress][sap-ha-guide-figure-3046]

  _**Bild 56:** definiera hello DNS-post för hello SAP ASCS/SCS kluster virtuella namn och TCP/IP-adress_

2.  toodefine hello IP-adress som tilldelats toohello virtuellt värdnamn, Välj **DNS-hanteraren** > **domän**.

  ![Bild 57: Nya virtuella namn och TCP/IP-adress för SAP ASCS/SCS klusterkonfiguration][sap-ha-guide-figure-3047]

  _**Bild 57:** nya virtuella namn och TCP/IP-adress för SAP ASCS/SCS klusterkonfiguration_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Installera hello SAP första klusternoden

1.  Köra hello första kluster nod alternativet på klusternoden A. Till exempel på hello **pr1-ascs-0** värden.
2.  tookeep hello standardportarna för hello Azure interna belastningsutjämnare, Välj:

  * **ABAP system**: **ASCS** instansen nummer **00**
  * **Java system**: **SCS** instansen nummer **01**
  * **ABAP + Java system**: **ASCS** instansen nummer **00** och **SCS** instansen nummer **01**

  toouse instans siffror än 00 för hello ABAP ASCS-instans och 01 för hello Java SCS instans, måste du först toochange hello Azure interna standard belastningsutjämnare NLB regler som beskrivs i [ändra hello ASCS/SCS standard belastning NLB regler för hello Azure intern belastningsutjämnare][sap-ha-guide-8.9].

hello beskrivs nästa några åtgärder inte i hello dokumentationen standard SAP.

> [!NOTE]
> hello SAP dokumentationen beskriver hur tooinstall hello första ASCS/SCS klusternoden.
>
>

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Ändra hello SAP-profil för hello ASCS/SCS-instansen

Du måste tooadd en ny profil-parameter. hello profil parametern förhindrar anslutningar mellan SAP arbetsprocesser och hello sätta servern stängs när de är inaktiv för länge. Vi har nämnt hello problemet i [lägga till registerposter på båda klusternoderna för hello SAP ASCS/SCS instansen][sap-ha-guide-8.11]. I det avsnittet introducerade vi också två ändringar toosome grundläggande TCP/IP-anslutningsparametrar. I nästa steg behöver du tooset hello sätta server toosend en `keep_alive` signalerar så att hello anslutningar inte nått hello Azure interna belastningsutjämnarens inaktiv tröskelvärdet.

toomodify hello SAP-profilen för hello ASCS/SCS instans:

1.  Lägg till den här profilen parametern toohello SAP ASCS/SCS instans profil:

  ```
  enque/encni/set_so_keepalive = true
  ```
  I vårt exempel är hello sökvägen:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  Till exempel toohello SAP SCS instans profil och motsvarande sökväg:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  tooapply hello ändringar hello SAP ASCS /SCS-instansen startas om.

#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Lägga till en avsökningsport

Använda hello interna belastningsutjämnarens avsökningen funktioner toomake hello hela klustret konfigurationen fungerar med Azure belastningsutjämnare. hello Azure intern belastningsutjämnare distribuerar vanligtvis hello inkommande belastningen jämnt mellan deltagande virtuella datorer. Men fungerar det inte i vissa klusterkonfigurationer eftersom endast en instans är aktiv. hello instanser är passivt och kan inte acceptera alla hello arbetsbelastning. Det hjälper dig att en avsökning funktionalitet när hello Azure interna belastningen belastningsutjämnaren tilldelas fungerar endast tooan aktiva instansen. Med hello avsökningen funktioner kan du identifiera vilka instanser som är aktiva och sedan målinstans endast hello med hello arbetsbelastning hello intern belastningsutjämnare.

tooadd en avsökningsport:

1.  Kontrollera hello aktuella **ProbePort** inställningen genom att köra följande PowerShell-kommando hello. Köra den från inom ett hello virtuella datorer i hello klusterkonfiguration.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  Definiera en avsökningsport. hello standardportnumret för avsökning är **0**. I vårt exempel vi använder avsökningsport **62000**.

  ![Bild 58: hello klustret configuration avsökningsport är 0 som standard][sap-ha-guide-figure-3048]

  _**Bild 58:** hello klustret configuration avsökningen standardporten är 0_

  hello-portnummer har definierats i SAP Azure Resource Manager-mallar. Du kan tilldela hello portnumret i PowerShell.

  tooset ett nytt ProbePort värde för hello  **SAP <*SID*> IP ** klusterresurs, kör hello följande PowerShell-skript. Uppdatera hello PowerShell variabler för din miljö. När hello skriptet körs, kommer du att ange toorestart hello SAP klustret tooactivate hello ändringar.

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

  När du tar hello  **SAP <*SID*> ** rollen online bör du kontrollera att **ProbePort** toohello nytt värde anges.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Bild 59: Avsökning hello klustret port när du ställer in hello nytt värde][sap-ha-guide-figure-3049]

  _**Bild 59:** avsökning hello klustret port när du ställer in hello nytt värde_

#### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Öppna hello avsökningsport för Windows-brandväggen

Du måste tooopen en Windows-brandväggen avsökningsport på båda klusternoderna. Använd följande skript tooopen en avsökning brandväggsport Windows hello. Uppdatera hello PowerShell variabler för din miljö.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

Hej **ProbePort** har angetts för**62000**. Nu kan du komma åt hello filresurs  **\\\ascsha-clsap\sapmnt** från andra värdar, exempel från **ascsha dbas**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Installera hello-databasinstans

tooinstall hello databasen instansen, följ hello process som beskrivs i hello SAP dokumentationen.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Installera hello andra klusternod

tooinstall hello andra kluster, följ hello stegen i hello SAP-installationsguiden.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Ändra hello starttypen till hello SAP ÄNDARE Windows service-instans

Ändra hello starttypen till hello SAP ÄNDARE Windows-tjänst för**automatiskt (förskjuten Start)** på båda klusternoderna.

![Bild 60: Ändra hello service-typen för hello SAP ÄNDARE instans toodelayed automatisk][sap-ha-guide-figure-3050]

_**Bild 60:** ändra hello service-typen för hello SAP ÄNDARE instans toodelayed automatisk_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Installera hello SAP primära programserver

Installera hello primära programmet Server (PROVIDERADRESSER) instans <*SID*> - di - 0 på hello virtuell dator som du har angett toohost hello PROVIDERADRESSER. Det finns inga beroenden på Azure eller DataKeeper-specifika inställningar.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Installera hello SAP ytterligare programserver

Installera en SAP ytterligare program Server (AAS) på alla hello virtuella datorer som du har angett toohost en SAP Application Server-instans. Till exempel på <*SID*> - di - 1 för <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> Detta är klar hello installation av ett system med hög tillgänglighet SAP NetWeaver. Fortsätt sedan med testning av redundans.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testa hello SAP ASCS/SCS instans redundans- och SIOS replikering
Det är enkelt tootest och övervaka en växling vid fel för SAP ASCS/SCS-instans och SIOS diskreplikering med hjälp av hanteraren för redundanskluster och hello SIOS DataKeeper hantering och konfiguration verktyget.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASCS/SCS-instans som körs på en nod i klustret

Hej **SAP PR1** klustergrupp körs på klusternoden A. Till exempel på **pr1-ascs-0**. Tilldela hello delad enhet S, vilket är en del av hello **SAP PR1** grupp och vilken hello ASCS/SCS instans använder toocluster nod A.

![Bild 61: Hanteraren för redundanskluster: hello SAP < SID > klustergrupp körs på en nod i klustret][sap-ha-guide-figure-5000]

_**Bild 61:** Klusterhanteraren: hello SAP <*SID*> klustergrupp körs på en nod i klustret_

I hello SIOS DataKeeper hantering och konfigurationsverktyget, kan du se hello delade disken data replikeras synkront från hello volym källenheten S på klusternoden en toohello volym målenheten S på klusternoden B. Till exempel har replikerats från **pr1-ascs-0 [10.0.0.40]** för**pr1-ascs-1 [10.0.0.41]**.

![Bild 62: I SIOS DataKeeper replikera hello lokal volym från klusternoden toocluster-nod B][sap-ha-guide-figure-5001]

_**Bild 62:** i SIOS DataKeeper replikerar hello lokal volym från klusternoden toocluster-nod B_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Växling vid fel från noden A toonode B

1.  Välj något av dessa alternativ tooinitiate en redundansväxling av hello SAP <*SID*> klustergrupp från klusternoden nod A toocluster B:
  - Använd hanteraren för redundanskluster  
  - Använda PowerShell för Failover-kluster

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  Starta om klustret nod A hello Windows-gästoperativsystem (Detta startar en automatisk redundans hello SAP <*SID*> klustergrupp från noden A toonode B).  
3.  Starta om klustret nod A från hello Azure-portalen (Detta startar en automatisk redundans hello SAP <*SID*> klustergrupp från noden A toonode B).  
4.  Starta om klustret nod A med hjälp av Azure PowerShell (Detta startar en automatisk redundans hello SAP <*SID*> klustergrupp från noden A toonode B).

  Efter redundans hello SAP <*SID*> klustergrupp körs på klusternoden B. Till exempel den körs på **pr1-ascs-1**.

  ![Bild 63: I Klusterhanteraren hello SAP < SID > klustergrupp körs på klusternoden B][sap-ha-guide-figure-5002]

  _**Bild 63**: I Hanteraren för redundanskluster, hello SAP <*SID*> klustergrupp körs på klusternoden B_

  hello delad disk nu monteras på klustret nod B. SIOS DataKeeper replikerar data från volymen källenheten S kluster nod B tootarget volym enhet S på klusternoden A. Till exempel den replikerar från **pr1-ascs-1 [10.0.0.41]** för**pr1-ascs-0 [10.0.0.40]**.

  ![Bild 64: SIOS DataKeeper replikerar hello lokal volym från noden B toocluster klusternod A][sap-ha-guide-figure-5003]

  _**Bild 64:** SIOS DataKeeper replikerar hello lokal volym från klusternoden nod B toocluster A_
