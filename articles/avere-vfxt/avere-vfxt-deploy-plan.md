---
title: Планирование системы Avere vFXT — Azure
description: Здесь описывается планирование, которое нужно выполнить перед развертыванием системы Avere vFXT для Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 46978d19a0789bb43e861ca89661aa5b78eb4ec7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60409893"
---
# <a name="plan-your-avere-vfxt-system"></a>Планирование системы Avere vFXT

В этой статье описывается планирование новых vFXT Avere для Azure кластер, расположение и размеры изменяются соответствующим образом для ваших потребностей. 

Прежде чем перейти в Azure Marketplace или к созданию виртуальных машин, подумайте, как кластер будет взаимодействовать с другими элементами в Azure. Спланируйте, где в частной сети и подсети будут находиться ресурсы кластера, а также решите, где будет ваше серверное хранилище. Создаваемые узлы кластера должны обладать достаточной производительностью для поддержки рабочих процессов. 

Ознакомьтесь с дополнительными сведениями, чтобы узнать больше.

## <a name="resource-group-and-network-infrastructure"></a>Группа ресурсов и инфраструктура сети

Определите, где будут находиться элементы развертывания Avere vFXT для Azure. На схеме ниже показано возможное упорядочение компонентов Avere vFXT для Azure:

![На схема отображается контроллер кластера и виртуальные машины кластера в одной подсети. Вокруг периметра подсети находится периметр виртуальной сети. Внутри виртуальной сети есть шестиугольник, представляющий конечную точку службы хранилища. Он связан с хранилищем BLOB-объектов за пределами виртуальной сети с помощью пунктирной стрелки.](media/avere-vfxt-components-option.png)

При планировании сетевой инфраструктуры системы Avere vFXT нужно следовать таким рекомендациям:

* Управление всеми элементами должно осуществляться с помощью новой подписки, созданной для развертывания Avere vFXT. Основные преимущества: 
  * Упрощенное отслеживание расходов — просмотр и аудит всех расходов на ресурсы, инфраструктуру и вычислительные циклы в одной подписке.
  * Более простая очистка — возможность удалить всю подписку по завершении работы с проектом.
  * Удобный секционирование ресурса квоты - защитить другие важные рабочие нагрузки от возможных регулирования, изолируя клиентов vFXT Avere и кластера в одной подписке ресурсов. Это позволит избежать конфликта при открытии большое количество клиентов для высокопроизводительных вычислений рабочего процесса.

* Разместите клиентские системы вычисления близко к кластеру vFXT. Серверное хранилище может находиться на более дальнем расстоянии.  

* Кластер vFXT и контроллера кластера виртуальной Машины должен находиться в той же виртуальной сети (vnet), в той же группе ресурсов и использовать одну и ту же учетную запись хранения. Шаблон создания автоматических кластера обрабатывает это для большинства ситуаций.

* Кластер должен быть расположен в собственной подсети, чтобы избежать конфликтов IP-адресов с клиентскими или вычислительными ресурсами. 

* Шаблон создания кластера можно создать большую часть ресурсов необходимая инфраструктура для кластера, включая группы ресурсов, виртуальные сети, подсети и учетные записи хранения. Если вы хотите использовать ресурсы, которые уже существуют, убедитесь, что они отвечают требованиям в этой таблице. 

  | Ресурс | Использовать существующую? | Требования |
  |----------|-----------|----------|
  | Группа ресурсов | Да, если не указано | Должен быть пустым| 
  | Учетная запись хранения | Да, если подключение существующий контейнер больших двоичных объектов после создания кластера <br/>  Не в том случае, если создается новый контейнер больших двоичных объектов во время создания кластера | Контейнер больших двоичных объектов должен быть пустым <br/> &nbsp; |
  | Виртуальная сеть | Да | Необходимо включить в конечной точке службы хранилища при создании нового контейнера BLOB-объектов Azure | 
  | Подсеть | Да |   |

## <a name="ip-address-requirements"></a>Требования к IP-адресам 

Убедитесь, что у кластера подсети есть достаточно большой диапазон IP-адресов для поддержки кластера. 

Кластер Avere vFXT использует следующие IP-адреса:

* IP-адрес для управления одним кластером. Этот адрес можно перемещать между узлами в кластере, но он всегда доступен, что позволяет подключаться к средству настройки на панели управления Avere.
* Для каждого узла кластера требуется:
  * По крайней мере один IP-адрес, ориентированный на клиента. (Все адреса, ориентированные на клиента, управляются кластером *vserver*, который при необходимости может перемещать их между узлами.)
  * Один IP-адрес для взаимодействия с кластером.
  * Один IP-адрес экземпляра (назначенный виртуальной машине).

Если вы используете хранилище BLOB-объектов Azure, также могут потребоваться IP-адреса из виртуальной сети кластера.  

* Учетной записи хранилища BLOB-объектов Azure нужно по крайней мере пять IP-адресов. Помните об этом требовании, если хранилище BLOB-объектов находится в той же виртуальной сети, что и кластер.
* Если для кластера вы используете хранилище BLOB-объектов Azure, которое находится за пределами виртуальной сети, необходимо создать конечную точку службы хранилища внутри виртуальной сети. Эта конечная точка не использует IP-адрес.

У вас есть возможность найти сетевые ресурсы и хранилище BLOB-объектов (если используется) в разных группах ресурсов кластера.

## <a name="vfxt-node-size"></a>размер узла vFXT

Виртуальные машины, выступающие в качестве узлов кластера, определяют пропускную способность запросов и емкость хранилища кэша. <!-- The instance type offered has been chosen for its memory, processor, and local storage characteristics. You can choose from two instance types, with different memory, processor, and local storage characteristics. -->

Каждый узел vFXT будет идентичен. То есть если вы создаете кластер с тремя узлами, вы получите три виртуальные машины одинакового типа и размера. 

| Тип экземпляра | Число виртуальных ЦП | Память  | Локальное хранилище на SSD-накопителях  | Максимальное число дисков данных | Пропускная способность диска без кэширования | Сетевая карта (количество) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_E32s_v3 | 32  | 256 Гиб | 512 ГБ  | 32 | 51 200 операций ввода-вывода в секунду <br/> 768 Мбит/с | 16 000 Мбит/с (8)  |

Кэш диска на каждый узел настраивается и может быть размером от 1000 до 8000 ГБ. 4 ТБ на каждый узел — это рекомендуемый размер кэша для Standard_E32s_v3 узлов.

Дополнительные сведения об этих виртуальных машин см. в документации по Microsoft Azure:

* [Размеры виртуальных машин, оптимизированных для операций в памяти](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Квота учетной записи

Убедитесь, что в вашей подписке достаточно ресурсов для запуска кластера Avere vFXT, а также используемых вычислительных или клиентских систем. Дополнительные сведения см. в разделе [Quota for the vFXT cluster](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) (Квота для кластера vFXT).

## <a name="back-end-data-storage"></a>Серверное хранилище данных

Где кластер Avere vFXT должен хранить ваши данные, если их нет в кэше? Определите, где будет храниться ваш рабочий набор долгосрочно: в новом контейнере больших двоичных объектов или в имеющейся облачной или аппаратной системе хранения. 

Если вы хотите использовать хранилище BLOB-объектов Azure для серверной части, в процессе разработки кластера vFXT следует создать контейнер. Этот сценарий создает и настраивает новый контейнер так, что его можно использовать сразу после создания. 

Дополнительные сведения см. в разделе о [создании Avere vFXT для Azure](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure).

> [!NOTE]
> Только пустые контейнеры хранилища BLOB-объектов можно использовать в качестве основных файловых хранилищ для системы Avere vFXT. В vFXT должна быть возможность управления хранилищем объектов без необходимости сохранения существующих данных. 
>
> Чтобы узнать, как эффективно копировать данные в новый контейнер кластера с использованием клиентских компьютеров и кэша vFXT Avere, обратитесь к статье [Moving data to the vFXT cluster - Parallel data ingest](avere-vfxt-data-ingest.md) (Перемещение данных в кластер vFXT. Прием параллельных данных).

Если вы хотите использовать существующую локальную систему хранения, ее необходимо добавить в кластер vFXT после его создания. Подробные инструкции о том, как добавить существующую систему хранения в кластер vFXT Avere, см. в разделе [Configure storage](avere-vfxt-add-storage.md) (Настройка контейнера).

## <a name="cluster-access"></a>Доступ к кластеру 

Кластер Avere vFXT для Azure расположен в частной подсети и не имеет общедоступного IP-адреса. У вас должен быть какой-либо метод доступа к частной подсети для администрирования кластера и клиентских подключений. 

Варианты доступа включают в себя:

* Узел перехода — назначьте общедоступный IP-адрес отдельной виртуальной машине в частной сети и используйте его для создания туннеля SSL к узлам кластера. 

  > [!TIP]
  > Если вы настроите общедоступный IP-адрес на контроллере кластера, контроллер можно использовать в качестве узла перехода. Дополнительные сведения см. в разделе [Контроллер кластера как узел перехода](#cluster-controller-as-jump-host).

* Виртуальная частная сеть (VPN) — настройте VPN типа "точка — сеть" или "сеть — сеть" для частной сети.

* Azure ExpressRoute — настройте частное подключение через партнер ExpressRoute. 

Дополнительные сведения об этих параметрах см. в разделе [Обмен данными через Интернет](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet).

### <a name="cluster-controller-as-jump-host"></a>Контроллер кластера как узел перехода

Контроллер кластера с настроенным общедоступным IP-адресом можно использовать в качестве узла перехода для подключения к кластеру Avere vFXT за пределами частной подсети. Однако, так как контроллер имеет права доступа для изменения узлов кластера, это создает небольшую угрозу безопасности.  

Для повышения безопасности для контроллера с помощью общедоступного IP-адреса скрипт развертывания автоматически создает группу безопасности сети, который ограничивает доступ входящего трафика только через порт 22. Вы можете дополнительно защитить систему, ограничив доступ по выбранному диапазону исходных IP-адресов, то есть разрешая подключения только от тех компьютеров, с которых вам нужен доступ к этому кластеру.

При создании кластера можно выбрать, следует ли создавать общедоступный IP-адрес на контроллере кластера. 

* Если вы создадите виртуальную сеть или подсеть, контроллеру кластера будет назначен общедоступный IP-адрес.
* При выборе существующей виртуальной сети и подсети контроллер кластера будет иметь только частные IP-адреса. 

## <a name="vm-access-roles"></a>Роли виртуальных Машин доступ 

Azure использует [управление доступом на основе ролей](../role-based-access-control/index.yml) (RBAC) для авторизации кластера виртуальных машин для выполнения определенных задач. Например контроллер кластера должна авторизации для создания и настройки виртуальных машин узла кластера. Узлы кластера необходимо иметь возможность назначить или переназначить IP-адреса узлов кластера.

Для виртуальных машин vFXT Avere используются две встроенные роли Azure: 

* Контроллер кластера использует встроенную роль [Avere участник](../role-based-access-control/built-in-roles.md#avere-contributor). 
* Узлы кластера используют встроенную роль [Avere оператор](../role-based-access-control/built-in-roles.md#avere-operator)

Если вам нужно настроить доступ роли для Avere vFXT компонентов, необходимо определить собственную роль и назначьте его виртуальным машинам во время их создания. Нельзя использовать шаблон развертывания в Azure Marketplace. Обратитесь в службу поддержки Майкрософт и поддержки, открыв запрос в службу на портале Azure, как описано в разделе [помощь с вашей системой](avere-vfxt-open-ticket.md). 

## <a name="next-step-understand-the-deployment-process"></a>Следующий шаг: ознакомление с процессом развертывания

В статье с [общими сведениями о развертывании](avere-vfxt-deploy-overview.md) дается общее представление всех шагов, необходимых для создания системы Avere vFXT для Azure и ее подготовки для обработки данных.  