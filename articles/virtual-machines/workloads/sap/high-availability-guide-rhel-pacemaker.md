---
title: Настройка кластера Pacemaker в Red Hat Enterprise Linux в Azure | Документы Майкрософт
description: Настройка кластера Pacemaker в Red Hat Enterprise Linux в Azure
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/17/2018
ms.author: sedusch
ms.openlocfilehash: cd56df8e88a84f52933e5ee43fecd598252ba6c0
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479762"
---
# <a name="setting-up-pacemaker-on-red-hat-enterprise-linux-in-azure"></a>Настройка кластера Pacemaker в Red Hat Enterprise Linux в Azure

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2002167]: https://launchpad.support.sap.com/#/notes/2002167
[2009879]: https://launchpad.support.sap.com/#/notes/2009879
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1999351]: https://launchpad.support.sap.com/#/notes/1999351

[virtual-machines-linux-maintenance]:../../linux/maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot

> [!TIP]
> Pacemaker в Red Hat Enterprise Linux использует агент Azure Fence для ограждения узла кластера, если это необходимо. Доступна новая версия агента ограждения Azure, и отработка отказа больше не занимает длительное время, если происходит сбой ресурса или узлы кластера не могут обмениваться данными друг с другом. Дополнительные сведения см. в статье [Ограждение виртуальной машины Azure, входящей в кластер высокой доступности RHEL, занимает много времени, завершается сбоем или таймаутом до завершения работы виртуальной машины](https://access.redhat.com/solutions/3408711).

Прежде всего прочитайте следующие примечания и документы SAP:

* примечание к SAP [1928533], которое содержит:
  * список размеров виртуальных машин Azure, поддерживаемых для развертывания ПО SAP;
  * важные сведения о доступных ресурсах для каждого размера виртуальной машины Azure;
  * сведения о поддерживаемом программном обеспечении SAP и сочетаниях операционных систем и баз данных;
  * сведения о требуемой версии ядра SAP для Windows и Linux в Microsoft Azure.
* примечание к SAP [2015553], в котором описываются предварительные требования к SAP при развертывании программного обеспечения SAP в Azure;
* примечание к SAP [2002167], содержащее рекомендуемые параметры ОС для Red Hat Enterprise Linux;
* примечание к SAP [2009879], содержащее рекомендации по SAP HANA для Red Hat Enterprise Linux;
* примечание к SAP [2178632], содержащее подробные сведения обо всех доступных метриках мониторинга для SAP в Azure;
* примечание к SAP [2191498], содержащее сведения о необходимой версии агента SAP Host Agent для Linux в Azure;
* примечание к SAP [2243692], содержащее сведения о лицензировании SAP в Linux в Azure;
* примечание к SAP [1999351], содержащее дополнительные сведения об устранении неполадок, связанных с расширением для расширенного мониторинга Azure для SAP;
* [вики-сайт сообщества SAP](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes), содержащий все необходимые примечания к SAP для Linux;
* [Планирование и реализация виртуальных машин Azure для SAP в Linux][planning-guide]
* [Развертывание виртуальных машин Azure для SAP в Linux (Эта статья)][deployment-guide]
* [Развертывание СУБД на виртуальных машинах Azure для SAP в Linux][dbms-guide]
* [Репликация системы SAP HANA в кластере pacemaker](https://access.redhat.com/articles/3004101)
* Общая документация по RHEL
  * [Общие сведения о надстройке для обеспечения высокой доступности](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Администрирование надстройки для обеспечения высокой доступности](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Справочник по надстройке для обеспечения высокой доступности](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Документация по RHEL, относящаяся к Azure:
  * [Политики поддержки для кластеров высокой доступности RHEL — виртуальные машины Microsoft Azure как члены кластера](https://access.redhat.com/articles/3131341)
  * [Установка и настройка кластера высокой доступности Red Hat Enterprise Linux 7.4 (и более поздних версий) в Microsoft Azure](https://access.redhat.com/articles/3252491)
  * [Настройка SAP S/4HANA ASCS/ERS с автономным сервером очереди 2 (ENSA2) в Pacemaker на RHEL 7,6](https://access.redhat.com/articles/3974941)

## <a name="cluster-installation"></a>Установка кластера

![Общие сведения о Pacemaker в RHEL](./media/high-availability-guide-rhel-pacemaker/pacemaker-rhel.png)

Ниже приведены элементы с префиксами: **[A]**  — применяется ко всем узлам, **[1**] — применяется только к узлу 1, **[2]**  — применяется только к узлу 2.

1. **[A]** Регистрация

   Зарегистрируйте виртуальные машины и подключите их к пулу, содержащему репозитории для RHEL 7.

   <pre><code>sudo subscription-manager register
   # List the available pools
   sudo subscription-manager list --available --matches '*SAP*'
   sudo subscription-manager attach --pool=&lt;pool id&gt;
   </code></pre>

   Обратите внимание, что, присоединив пул к образу PAYG RHEL для Azure Marketplace, вы будете эффективно выставлять счет за использование RHEL: один раз для образа PAYG и один раз для назначения RHEL в присоединенном пуле. Чтобы устранить эту проблемы, Azure теперь предоставляет образы BYOS RHEL. Дополнительные сведения можно найти [здесь](https://aka.ms/rhel-byos).

1. **[A]** Включение RHEL для репозиториев SAP

   Чтобы установить необходимые пакеты, включите приведенные ниже репозитории.

   <pre><code>sudo subscription-manager repos --disable "*"
   sudo subscription-manager repos --enable=rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-sap-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-eus-rpms
   </code></pre>

1. **[A]** Установка надстройки для обеспечения высокой доступности RHEL

   <pre><code>sudo yum install -y pcs pacemaker fence-agents-azure-arm nmap-ncat
   </code></pre>

   > [!IMPORTANT]
   > Мы рекомендуем использовать следующие версии агента ограждения Azure (или более поздней версии), чтобы клиенты могли использовать преимущества более быстрого перехода на другой ресурс, если происходит сбой ресурса или узлы кластера не могут обмениваться данными друг с другом.  
   > RHEL 7,6: фенце-ажентс-4.2.1 -11. el7 _ 6.8  
   > RHEL 7,5: фенце-ажентс-4.0.11 -86. el7 _ 5.8  
   > RHEL 7,4: фенце-ажентс-4.0.11 -66. el7 _ 4.12  
   > Дополнительные сведения см. в разделе [Виртуальная машина Azure, работающая в качестве члена кластера высокой доступности RHEL, может занять очень много времени, или если не удается закрыть виртуальную машину](https://access.redhat.com/solutions/3408711) .

   Проверьте версию агента ограждения Azure. При необходимости обновите его до версии, равной или более поздней, чем указано выше.
   <pre><code># Check the version of the Azure Fence Agent
    sudo yum info fence-agents-azure-arm
   </code></pre>

1. **[A]** Установите разрешения имен.

   Можно использовать DNS-сервер или внести изменения в файл /etc/hosts на всех узлах. В этом примере показано, как использовать файл /etc/hosts.
   Замените IP-адрес и имя узла в следующих командах. Преимущество использования /etc/hosts в том, что кластер становится независимым от службы DNS, которая также может являться единой точкой отказа.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Вставьте следующие строки в /etc/hosts. Измените IP-адрес и имя узла в соответствии со своей средой.

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[A]** Измените пароль hacluster на тот же пароль.

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]** Добавление правил брандмауэра для pacemaker

   Добавьте приведенные ниже правила брандмауэра для всех взаимодействий между узлами кластера.

   <pre><code>sudo firewall-cmd --add-service=high-availability --permanent
   sudo firewall-cmd --add-service=high-availability
   </code></pre>

1. **[A]** Включение основных служб кластера

   Чтобы включить службу Pacemaker и запустить ее, выполните приведенные ниже команды.

   <pre><code>sudo systemctl start pcsd.service
   sudo systemctl enable pcsd.service
   </code></pre>

1. **[1]** Создание кластера Pacemaker

   Чтобы выполнить проверку подлинности узлов и создать кластер, выполните приведенные ниже команды. Установите токен в значение 30 000, чтобы разрешить обслуживание с сохранением памяти. Дополнительные сведения см. в [этой статье для Linux][virtual-machines-linux-maintenance].

   <pre><code>sudo pcs cluster auth <b>prod-cl1-0</b> <b>prod-cl1-1</b> -u hacluster
   sudo pcs cluster setup --name <b>nw1-azr</b> <b>prod-cl1-0</b> <b>prod-cl1-1</b> --token 30000
   sudo pcs cluster start --all

   # Run the following command until the status of both nodes is online
   sudo pcs status

   # Cluster name: nw1-azr
   # WARNING: no stonith devices and stonith-enabled is not false
   # Stack: corosync
   # Current DC: <b>prod-cl1-1</b> (version 1.1.18-11.el7_5.3-2b07d5c5a9) - partition with quorum
   # Last updated: Fri Aug 17 09:18:24 2018
   # Last change: Fri Aug 17 09:17:46 2018 by hacluster via crmd on <b>prod-cl1-1</b>
   #
   # 2 nodes configured
   # 0 resources configured
   #
   # Online: [ <b>prod-cl1-0</b> <b>prod-cl1-1</b> ]
   #
   # No resources
   #
   #
   # Daemon Status:
   #   corosync: active/disabled
   #   pacemaker: active/disabled
   #   pcsd: active/enabled
   </code></pre>

1. **[A]** Задание требуемых голосов

   <pre><code>sudo pcs quorum expected-votes 2
   </code></pre>

## <a name="create-stonith-device"></a>Создание устройства STONITH

Устройство STONITH использует субъект-службу для авторизации в Microsoft Azure. Чтобы создать субъект-службу, выполните следующее.

1. Перейдите на сайт <https://portal.azure.com>.
1. Откройте колонку "Azure Active Directory".  
   Перейдите в колонку "Свойства" и запишите идентификатор каталога. Это **идентификатор клиента**.
1. Щелкните "Регистрация приложений".
1. Нажмите кнопку "создать регистрацию"
1. Введите имя, выберите "учетные записи только в каталоге организации" 
2. Выберите тип приложения "Интернет", введите URL-адрес входа (например, http:/ЛОКАЛХОСТ\/) и нажмите кнопку "Добавить".  
   URL-адрес входа не используется и может быть любым допустимым URL-адресом.
1. Выберите Сертификаты и секреты, а затем — новый секрет клиента.
1. Введите описание нового ключа, выберите "никогда не истекает" и нажмите кнопку "Добавить".
1. Запишите его значение. Он используется в качестве **пароля** субъекта-службы.
1. Выберите Обзор. Запишите идентификатор приложения. Он используется в качестве имени пользователя (**идентификатора для входа** в следующих шагах) субъекта-службы.

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]** Создайте пользовательскую роль для агента ограждения.

У субъекта-службы по умолчанию нет разрешений на доступ к ресурсам Azure. Необходимо предоставить субъекту-службе разрешения на запуск и завершение (выключение) всех виртуальных машин кластера. Если вы еще не создали эту пользовательскую роль, ее можно создать с помощью [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell) или [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).

Используйте следующее содержимое для входного файла. Необходимо адаптировать содержимое для ваших подписок, поэтому замените c276fc76-9cd4-44c9-99a7-4fd71546436e и e91d47c4-76f3-4271-a796-21b4ecfe3624 идентификаторами своих подписок. Если у вас имеется только одна подписка, удалите вторую запись в AssignableScopes.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to power-off and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/powerOff/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="a-assign-the-custom-role-to-the-service-principal"></a>**[A]** Назначьте пользовательскую роль субъекту-службе.

Назначьте субъекту-службе пользовательскую роль Linux Fence Agent Role, созданную в последней главе. Больше не используйте роль владельца!

1. Перейдите на сайт https://portal.azure.com.
1. Откройте колонку "Все ресурсы".
1. Выберите виртуальную машину первого узла кластера.
1. Выберите "Управление доступом (IAM)".
1. Выберите "Добавить назначение ролей".
1. Выберите роль Linux Fence Agent Role.
1. Введите имя созданного ранее приложения.
1. Щелкните Сохранить

Повторите предыдущие шаги для второго узла кластера.

### <a name="1-create-the-stonith-devices"></a>**[1]** Создайте устройства STONITH.

После изменения разрешений для виртуальных машин можно настроить устройства STONITH в кластере.

<pre><code>
sudo pcs property set stonith-timeout=900
</code></pre>

Чтобы настроить устройство ограждения, выполните приведенную ниже команду.

> [!NOTE]
> Параметр pcmk_host_map требуется только в том случае, если имена узлов RHEL и Azure не совпадают. См. раздел команды, выделенный полужирным шрифтом.

<pre><code>sudo pcs stonith create rsc_st_azure fence_azure_arm login="<b>login ID</b>" passwd="<b>password</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" subscriptionId="<b>subscription id</b>" <b>pcmk_host_map="prod-cl1-0:10.0.0.6;prod-cl1-1:10.0.0.7"</b> power_timeout=240 pcmk_reboot_timeout=900</code></pre>

### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]** Разрешите использование устройства STONITH.

<pre><code>sudo pcs property set stonith-enabled=true
</code></pre>

## <a name="next-steps"></a>Следующие шаги

* [Планирование и реализация виртуальных машин Azure для SAP][planning-guide]
* [Развертывание виртуальных машин Azure для SAP][deployment-guide]
* [Развертывание СУБД виртуальных машин Azure для SAP][dbms-guide]
* Сведения о том, как установить высокий уровень доступности и спланировать аварийное восстановление SAP HANA на виртуальных машинах Azure, см. в статье [высокий уровень доступности SAP HANA на виртуальные машины Azure][sap-hana-ha] .
