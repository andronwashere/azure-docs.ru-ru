---
title: Не удается подключиться по протоколу RDP к виртуальной машине Windows в Azure | Документация Майкрософт
description: Устранение неполадок, возникающих при подключении к виртуальной машине Windows в Azure через удаленный рабочий стол
keywords: Ошибка удаленного рабочего стола, ошибка подключения к удаленному рабочему столу, не удается подключиться к виртуальной машине, диагностика удаленного рабочего стола
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: gwallace
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: roiyz
ms.openlocfilehash: 711c5cb8211de8b5ec27cfd76f12c34c84676e64
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710499"
---
# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Устранение неполадок с подключением к виртуальной машине Azure через удаленный рабочий стол
Подключение протокола удаленного рабочего стола (RDP) к виртуальной машине Azure под управлением Windows может завершиться неудачно по нескольким причинам. В этом случае доступ к виртуальной машине будет невозможен. Эта проблема может быть вызвана службой удаленного рабочего стола на виртуальной машине, сетевым подключением или клиентом удаленного рабочего стола на главном компьютере. В этой статье описываются некоторые из наиболее распространенных методов устранения проблем подключения по протоколу RDP. 

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните **Получить поддержку**.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Действия по устранению неполадок
После выполнения каждого шага снова попробуйте подключиться к виртуальной машине.

1. Выполните сброс конфигурации удаленного рабочего стола.
2. Проверьте правила групп безопасности сети или конечные точки облачных служб.
3. Проверьте журналы консоли виртуальной машины.
4. Сбросьте конфигурацию сетевой карты для виртуальной машины.
5. Проверьте работоспособность ресурсов виртуальной машины.
6. Сбросьте пароль виртуальной машины.
7. Перезапустите виртуальную машину.
8. Заново разверните виртуальную машину.

Читайте статью дальше, если вам нужны более подробные инструкции или пояснения. Убедитесь, что локальное сетевое оборудование, такое как маршрутизаторы и брандмауэры, не блокирует исходящий TCP-порт 3389, как указано в [подробных сценариях устранения неполадок RDP](detailed-troubleshoot-rdp.md).

> [!TIP]
> Если кнопка **Подключить** для вашей виртуальной машины на портале неактивна и вы не используете канал [Express Route](../../expressroute/expressroute-introduction.md) или [VPN-подключение типа "сеть — сеть"](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) для подключения к Azure, то прежде всего следует создать общедоступный IP-адрес и назначить его виртуальной машине. Только после этого вы сможете использовать протокол RDP. Дополнительные сведения см. в статье [IP-адреса в Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Способы устранения неполадок при подключении по протоколу RDP
Для устранения неполадок на виртуальных машинах, созданных с помощью модели развертывания Resource Manager, можно воспользоваться одним из следующих методов:

* Портал Azure отлично подходит, когда нужно быстро сбросить конфигурацию удаленного рабочего стола или учетные данные пользователей, а у вас не установлены инструменты Azure.
* Azure PowerShell подходит, если вам удобней работать с командной строкой PowerShell. Выполняйте быстрый сброс конфигурации удаленного рабочего стола или учетных данных пользователей с помощью командлетов Azure PowerShell.

Вы также можете ознакомиться с шагами по устранению неполадок на виртуальных машинах, созданных с помощью [классической модели развертывания](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-the-azure-portal"></a>Устранение неполадок с помощью портала Azure
После выполнения каждого шага устранения неполадок попробуйте подключиться к виртуальной машине еще раз. Если все еще не удается подключиться, то попробуйте выполнить следующее действие.

1. **Сбросьте подключение к удаленному рабочему столу**. Это действие позволяет сбросить конфигурацию удаленного рабочего стола, например, если удаленные соединения отключены или правила брандмауэра Windows блокируют подключения по RDP.
   
    Выберите свою виртуальную машину на портале Azure. Прокрутите область параметров вниз до раздела **Поддержка и устранение неполадок** в нижней части списка. Нажмите кнопку **Сбросить пароль**. В разделе **Режим** выберите **Сбросить только конфигурацию** и нажмите кнопку **Обновить**:
   
    ![Сброс конфигурации удаленного рабочего стола на портале Azure](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Проверьте правила группы безопасности сети**. Используйте [проверку потока для IP-адреса](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md), чтобы определить, блокирует ли правило в группе безопасности сети входящий или исходящий трафик виртуальной машины. Вы можете также просмотреть действующие правила группы безопасности и убедиться, что для порта RDP (3389 по умолчанию) существует и является приоритетным правило NSG, разрешающее входящий трафик. Дополнительные сведения см. в разделе [Использование действующих правил безопасности для устранения проблем с потоком трафика в виртуальной машине](../../virtual-network/diagnose-network-traffic-filter-problem.md).

3. **Просмотрите данные диагностики загрузки виртуальной машины**. Это действие позволяет проверить журналы консоли виртуальной машины и определить, не сообщает ли виртуальная машина о неполадках. Не на всех виртуальных машинах включена диагностика загрузки, поэтому это действие может быть необязательным.
   
    Определенные действия по устранению неполадок выходят за рамки данной статьи, но они могут указать на более широкую проблему, влияющую на подключение к удаленному рабочему столу. Дополнительные сведения о проверке журналов консоли и снимок экрана виртуальной машины см. в статье [Boot Diagnostics for VMs](boot-diagnostics.md) (Диагностика загрузки для виртуальных машин).

4. **Сбросьте конфигурацию сетевой карты для виртуальной машины**. Дополнительные сведения см. в статье [How to reset network interface for Azure Windows VM](../windows/reset-network-interface.md) (Как сбросить конфигурацию сетевого интерфейса для виртуальной машины Windows в Azure).
5. **Проверьте работоспособность ресурсов виртуальной машины**. Это действие проверяет, чтобы в работе платформы Azure не было известных неполадок, которые бы могли повлиять на подключение к виртуальной машине.
   
    Выберите свою виртуальную машину на портале Azure. Прокрутите область параметров вниз до раздела **Поддержка и устранение неполадок** в нижней части списка. Нажмите кнопку **Работоспособность ресурсов**. Если виртуальная машина работоспособна, то отобразится состояние **Доступно**:
   
    ![Проверка работоспособности ресурсов виртуальной машины на портале Azure](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Сбросьте учетные данные пользователей**. Это действие позволяет сбросить пароль учетной записи локального администратора, если вы не уверены или забыли учетные данные.  После входа на виртуальную машину сбросьте пароль для этого пользователя.
   
    Выберите свою виртуальную машину на портале Azure. Прокрутите область параметров вниз до раздела **Поддержка и устранение неполадок** в нижней части списка. Нажмите кнопку **Сбросить пароль**. Убедитесь, что в разделе **Режим** выбрано значение **Сброс пароля**, а затем введите свое имя пользователя и новый пароль. В конце нажмите кнопку **Обновить**:
   
    ![Сброс учетных данных пользователя на портале Azure](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Перезапустите виртуальную машину**. Это действие может исправить внутренние неполадки, возникшие в самой виртуальной машине.
   
    Выберите свою виртуальную машину на портале Azure и щелкните вкладку **Обзор**. Нажмите кнопку **Перезапустить**:
   
    ![Перезапуск виртуальной машины на портале Azure](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Заново разверните виртуальную машину**. Это действие выполняет повторное развертывание виртуальной машины на другом узле Azure для исправления каких-либо внутренних неполадок в работе платформы или сети.
   
    Выберите свою виртуальную машину на портале Azure. Прокрутите область параметров вниз до раздела **Поддержка и устранение неполадок** в нижней части списка. Выберите пункт **Повторное развертывание**, а затем нажмите кнопку **Повторить развертывание**:
   
    ![Повторное развертывание виртуальной машины на портале Azure](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Обратите внимание, что после этой операции будут потеряны данные на временном диске, а также изменятся динамические IP-адреса, связанные с виртуальной машиной.

9. **Проверьте маршрутизацию**. Убедитесь, что маршрут не препятствуют маршрутизации трафика на виртуальную машину или с нее, воспользовавшись возможностью [Следующий прыжок](../../network-watcher/network-watcher-check-next-hop-portal.md) в службе "Наблюдатель за сетями". Кроме того, вы можете просмотреть фактические маршруты для сетевого интерфейса. Дополнительные сведения см. в статье об [использовании фактических маршрутов для устранения проблем с потоком трафика на виртуальной машине](../../virtual-network/diagnose-network-routing-problem.md).

10. Убедитесь, что все локальные брандмауэры или брандмауэр на вашем компьютере разрешают исходящий трафик TCP 3389 в Azure.

Если у вас по-прежнему возникают проблемы с протоколом RDP, то вы можете [отправить запрос в службу поддержки](https://azure.microsoft.com/support/options/) или прочитать [более подробные сведения об основных понятиях и этапах устранения неполадок RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Устранение неполадок с помощью Azure PowerShell
[Установите и настройте последнюю версию Azure PowerShell](/powershell/azure/overview), если у вас ее еще нет.

В следующих примерах используются переменные, такие как `myResourceGroup`, `myVM` и `myVMAccessExtension`. Замените имена этих переменных и расположения собственными значениями.

> [!NOTE]
> Для сброса учетных данных пользователей и конфигурации удаленного рабочего стола используется командлет PowerShell [Set-AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension). В следующих примерах `myVMAccessExtension` — это имя, которое указывается в ходе процесса. Если вы уже работали с агентом VMAccessAgent, то можете получить имя существующего расширения с помощью командлета `Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"`, который возвращает свойства виртуальной машины. Просмотреть это имя можно в разделе "Extensions" в выходных данных.

После выполнения каждого шага устранения неполадок попробуйте подключиться к виртуальной машине еще раз. Если все еще не удается подключиться, то попробуйте выполнить следующее действие.

1. **Сбросьте подключение к удаленному рабочему столу**. Это действие позволяет сбросить конфигурацию удаленного рабочего стола, например, если удаленные соединения отключены или правила брандмауэра Windows блокируют подключения по RDP.
   
    В следующем примере выполняется сброс подключения RDP к виртуальной машине `myVM` в расположении `WestUS` и в группе ресурсов `myResourceGroup`:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Проверьте правила группы безопасности сети**. Это действие проверяет наличие в группе безопасности сети правила, разрешающего трафик RDP. По умолчанию в качестве порта RDP используется TCP-порт 3389. Правило, разрешающее трафик RDP, может не создаваться автоматически при создании виртуальной машины.
   
    Во-первых, назначьте переменной `$rules` все данные конфигурации для своей группы безопасности сети. В следующем примере извлекаются сведения о группе безопасности сети `myNetworkSecurityGroup` в группе ресурсов `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Теперь просмотрите правила, которые настроены для этой группы безопасности сети. Убедитесь, что существует правило, разрешающее использовать TCP-порт 3389 для входящих подключений:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    В следующем примере показано допустимое правило безопасности, разрешающее трафик RDP. Можно увидеть, что параметры `Protocol`, `DestinationPortRange`, `Access` и `Direction` настроены правильно:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    Если у вас нет правила, разрешающего трафик RDP, [создайте правило группы безопасности сети](../windows/nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Разрешите использование TCP-порта 3389.
3. **Сбросьте учетные данные пользователей**. Это действие позволяет сбросить вводимый вами пароль учетной записи локального администратора, если вы не уверены, что учетные данные правильны, или забыли их.
   
    Сначала укажите имя пользователя и новый пароль, назначив переменной `$cred` учетные данные:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Теперь обновите учетные данные на виртуальной машине. В следующем примере обновляются учетные данные на виртуальной машине `myVM` в расположении `WestUS` и в группе ресурсов `myResourceGroup`:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Перезапустите виртуальную машину**. Это действие может исправить внутренние неполадки, возникшие в самой виртуальной машине.
   
    В следующем примере перезапускается виртуальная машина `myVM` в группе ресурсов `myResourceGroup`:
   
    ```powershell
    Restart-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Заново разверните виртуальную машину**. Это действие выполняет повторное развертывание виртуальной машины на другом узле Azure для исправления каких-либо внутренних неполадок в работе платформы или сети.
   
    В следующем примере повторно развертывается виртуальная машина `myVM` в расположении `WestUS` и в группе ресурсов `myResourceGroup`:
   
    ```powershell
    Set-AzVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

6. **Проверьте маршрутизацию**. Убедитесь, что маршрут не препятствуют маршрутизации трафика на виртуальную машину или с нее, воспользовавшись возможностью [Следующий прыжок](../../network-watcher/network-watcher-check-next-hop-portal.md) в службе "Наблюдатель за сетями". Кроме того, вы можете просмотреть фактические маршруты для сетевого интерфейса. Дополнительные сведения см. в статье об [использовании фактических маршрутов для устранения проблем с потоком трафика на виртуальной машине](../../virtual-network/diagnose-network-routing-problem.md).

7. Убедитесь, что все локальные брандмауэры или брандмауэр на вашем компьютере разрешают исходящий трафик TCP 3389 в Azure.

Если у вас по-прежнему возникают проблемы с протоколом RDP, то вы можете [отправить запрос в службу поддержки](https://azure.microsoft.com/support/options/) или прочитать [более подробные сведения об основных понятиях и этапах устранения неполадок RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Устранение неполадок на виртуальных машинах, созданных с помощью классической модели развертывания
После выполнения каждого шага устранения неполадок попробуйте подключиться к виртуальной машине еще раз.

1. **Сбросьте подключение к удаленному рабочему столу**. Это действие позволяет сбросить конфигурацию удаленного рабочего стола, например, если удаленные соединения отключены или правила брандмауэра Windows блокируют подключения по RDP.
   
    Выберите свою виртуальную машину на портале Azure. Нажмите кнопку **... Дополнительные**, а затем щелкните **Сброс удаленного доступа**:
   
    ![Сброс конфигурации удаленного рабочего стола на портале Azure](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Проверьте конечные точки облачных служб**. Это действие проверяет наличие в облачных службах конечных точек, разрешающих трафик RDP. По умолчанию в качестве порта RDP используется TCP-порт 3389. Правило, разрешающее трафик RDP, может не создаваться автоматически при создании виртуальной машины.
   
   Выберите свою виртуальную машину на портале Azure. Нажмите кнопку **Конечные точки**, чтобы просмотреть конечные точки, настроенные для виртуальной машины. Убедитесь, что существуют конечные точки, разрешающие трафик RDP через TCP-порт 3389.
   
   В следующем примере показаны допустимые конечные точки, которые разрешают трафик RDP:
   
   ![Проверка конечных точек облачных служб на портале Azure](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   Если у вас нет конечной точки, разрешающей трафик RDP, [создайте конечную точку облачных служб](../windows/classic/setup-endpoints.md). Разрешите TCP использовать частный порт 3389.
3. **Просмотрите данные диагностики загрузки виртуальной машины**. Это действие позволяет проверить журналы консоли виртуальной машины и определить, не сообщает ли виртуальная машина о неполадках. Не на всех виртуальных машинах включена диагностика загрузки, поэтому это действие может быть необязательным.
   
    Определенные действия по устранению неполадок выходят за рамки данной статьи, но они могут указать на более широкую проблему, влияющую на подключение к удаленному рабочему столу. Дополнительные сведения о проверке журналов консоли и снимок экрана виртуальной машины см. в статье [Boot Diagnostics for VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) (Диагностика загрузки для виртуальных машин).
4. **Проверьте работоспособность ресурсов виртуальной машины**. Это действие проверяет, чтобы в работе платформы Azure не было известных неполадок, которые бы могли повлиять на подключение к виртуальной машине.
   
    Выберите свою виртуальную машину на портале Azure. Прокрутите область параметров вниз до раздела **Поддержка и устранение неполадок** в нижней части списка. Нажмите кнопку **Работоспособность ресурсов**. Если виртуальная машина работоспособна, то отобразится состояние **Доступно**:
   
    ![Проверка работоспособности ресурсов виртуальной машины на портале Azure](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Сбросьте учетные данные пользователей**. Это действие позволяет сбросить вводимый вами пароль учетной записи локального администратора, если вы не уверены или забыли учетные данные.  После входа на виртуальную машину сбросьте пароль для этого пользователя.
   
    Выберите свою виртуальную машину на портале Azure. Прокрутите область параметров вниз до раздела **Поддержка и устранение неполадок** в нижней части списка. Нажмите кнопку **Сбросить пароль**. Введите свое имя пользователя и новый пароль. В конце нажмите кнопку **Сохранить**:
   
    ![Сброс учетных данных пользователя на портале Azure](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Перезапустите виртуальную машину**. Это действие может исправить внутренние неполадки, возникшие в самой виртуальной машине.
   
    Выберите свою виртуальную машину на портале Azure и щелкните вкладку **Обзор**. Нажмите кнопку **Перезапустить**:
   
    ![Перезапуск виртуальной машины на портале Azure](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

7. Убедитесь, что все локальные брандмауэры или брандмауэр на вашем компьютере разрешают исходящий трафик TCP 3389 в Azure.

Если у вас по-прежнему возникают проблемы с протоколом RDP, то вы можете [отправить запрос в службу поддержки](https://azure.microsoft.com/support/options/) или прочитать [более подробные сведения об основных понятиях и этапах устранения неполадок RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Устранение определенных ошибок RDP
При попытке подключиться к виртуальной машине по протоколу RDP может появиться сообщение об определенной ошибке. Ниже перечислены наиболее распространенные сообщения об ошибках.

* [Удаленный сеанс отключен, поскольку отсутствуют доступные серверы лицензирования удаленных рабочих столов, которые могли бы провести лицензирование](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Службе удаленного рабочего стола не удается найти имя компьютера](troubleshoot-specific-rdp-errors.md#rdpname).
* [Произошла ошибка проверки подлинности. Не удается связаться с локальным администратором безопасности»](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Ошибка системы безопасности Windows: недействительные учетные данные](troubleshoot-specific-rdp-errors.md#wincred).
* [Не удается подключиться к удаленному компьютеру](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Дополнительные ресурсы
Если ни одна из указанных ошибок не возникла, но подключиться к виртуальной машине с помощью протокола удаленного рабочего стола не удается, прочтите [подробное руководство по устранению неполадок с удаленным рабочим столом](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Сведения об устранении неполадок с доступом к приложениям, работающим на виртуальной машине, см. в статье [Устранение проблем с подключением к приложениям на виртуальных машинах Linux в Azure](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Если вы подключаетесь к виртуальной машине Linux в Azure по протоколу SSH и у вас возникают проблемы, см. статью [Устранение неполадок с SSH-подключением к виртуальной машине Azure Linux: сбой, ошибка или отклонение](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


