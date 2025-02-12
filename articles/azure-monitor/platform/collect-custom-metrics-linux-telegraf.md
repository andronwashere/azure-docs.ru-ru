---
title: Сбор пользовательских метрик для виртуальной машины Linux с помощью агента Telegraf InfluxData
description: Сбор пользовательских метрик для виртуальной машины Linux с помощью агента Telegraf InfluxData
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 14415b88cd6036642442ef9ae23e8dee301bb908
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60741611"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Сбор пользовательских метрик для виртуальной машины Linux с помощью агента Telegraf InfluxData

Azure Monitor позволяет собирать пользовательские метрики с помощью телеметрии приложения, агента, запущенного в ресурсах Azure, или даже систем внешнего мониторинга. Затем их можно отправлять непосредственно в Azure Monitor. В этой статье описано инструкции по развертыванию агента Telegraf [InfluxData](https://www.influxdata.com/) на виртуальной машине Linux в Azure и настройке агента для публикации метрик в Azure Monitor. 

## <a name="influxdata-telegraf-agent"></a>Агент Telegraf InfluxData 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) — это агент, управляемый подключаемым модулем, который позволяет собирать метрики из более чем 150 различных источников. В зависимости от рабочих нагрузок вашей виртуальной машины можно настроить использование специализированных входных подключаемых модулей для сбора метрик. К примеру MySQL, NGINX и Apache. Использование выходных подключаемых модулей позволяет агенту выполнять запись в места назначения на ваш выбор. Агент Telegraf интегрирован непосредственно с REST API пользовательских метрик Azure Monitor. Он также поддерживает выходной подключаемый модуль Azure Monitor. Использование этого модуля позволит агенту собирать метрики конкретной рабочей нагрузки на виртуальной машине Linux и отправлять их в качестве пользовательских метрик в Azure Monitor. 

 ![Обзор агента Telegraph](./media/collect-custom-metrics-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Отправка пользовательских метрик 

В этом руководстве мы развернем виртуальную машину Linux под управлением операционной системы Ubuntu 16.04 LTS. Агент Telegraf поддерживается для большинства операционных систем Linux. Пакеты Debian и RPM с распакованными двоичными файлами Linux доступны на [портале загрузки InfluxData](https://portal.influxdata.com/downloads). Дополнительные инструкции по установке и доступные варианты установки Telegraf см. в этом [руководстве по установке Telegraf](https://docs.influxdata.com/telegraf/v1.8/introduction/installation/). 

Войдите на [портале Azure](https://portal.azure.com).

Создайте новую виртуальную машину Linux. 

1. В области навигации слева выберите  **Создать ресурс** . 
1. Введите в поле поиска **Виртуальная машина**.  
1. Выберите **Ubuntu 16.04 LTS** и нажмите кнопку **Создать**. 
1. Укажите имя виртуальной машины, например  **MyTelegrafVM**.  
1. Сохраните тип диска **SSD**. Затем укажите **имя пользователя**, например  **azureuser**. 
1. В области   **Тип проверки подлинности** выберите   **Пароль**. Затем введите пароль, который будет использоваться позже для SSH в этой виртуальной машине. 
1. Выберите  **Создать группу ресурсов**. Укажите имя, например  **myResourceGroup**. Выберите  **Расположение**. Нажмите кнопку   **OK**. 

    ![Создание виртуальной машины Ubuntu.](./media/collect-custom-metrics-linux-telegraf/create-vm.png)

1. Выберите размер виртуальной машины. Фильтрацию можно совершить с помощью  **Тип вычисления**  или  **Тип диска**, например. 

    ![Обзор агента Telegraph. Размер виртуальной машины](./media/collect-custom-metrics-linux-telegraf/vm-size.png)

1. На странице  **Параметры**  щелкните  **Сеть** > **Группа безопасности сети** > **Выберите общедоступные входящие порты** и укажите  **HTTP**  и  **SSH (22)** . Сохраните остальные значения по умолчанию и нажмите кнопку  **ОК**. 

1. На странице сводной информации выберите  **Создать** , чтобы начать развертывание виртуальной машины. 

1. Новая виртуальная машина закрепляется на панели мониторинга портала Azure. После завершения развертывания автоматически отобразятся сводные сведения о виртуальной машине. 

1. На панели виртуальной машины перейдите на вкладку **Удостоверение**. Выберите значение **Включить** для назначаемого системой удостоверения в вашей виртуальной машине. 
 
    ![Предварительный просмотр удостоверения виртуальной машины Telegraf](./media/collect-custom-metrics-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>Подключение к виртуальной машине 

Создайте SSH-подключение к виртуальной машине. На странице обзора виртуальной машины нажмите кнопку  **Подключиться** . 

![Обзор страницы виртуальной машины Telegraf](./media/collect-custom-metrics-linux-telegraf/connect-VM-button2.png)

На странице  **Подключения к виртуальной машине**  сохраните значения по умолчанию, чтобы использовать подключение по DNS-имени через порт 22. В поле  **Вход с использованием локальной учетной записи виртуальной машины** представлена команда для подключения. Нажмите кнопку, чтобы скопировать эту команду. Ниже представлен пример команды подключения по SSH: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

Вставьте команду подключения по SSH в оболочку, например, Azure Cloud Shell или Bash на Ubuntu в Windows, или используйте клиент SSH, чтобы создать подключение. 

## <a name="install-and-configure-telegraf"></a>Установка и настройка Telegraf 

Чтобы установить пакет Telegraf Debian на виртуальной машине, выполните следующие команды в рамках сеанса SSH: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Файл конфигурации Telegraf определяет операции Telegraf. Пример файла конфигурации по умолчанию установлен по пути **/etc/telegraf/telegraf.conf**. В примере файла конфигурации перечислены все возможные входные и выходные подключаемые модули. Тем не менее мы собираемся создать пользовательский файл конфигурации и настроить агент для его использования, выполнив следующие команды. 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]  
> Приведенный выше код включает только два входных подключаемых модуля: **cpu** и **mem**. Вы также можете подсоединить другие модули в зависимости от рабочей нагрузки, выполняемой на компьютере. Например, Docker, MySQL и NGINX. Полный список входных подключаемых модулей см. в разделе **Дополнительная настройка**. 

Наконец для запуска агента с помощью новой конфигурации мы принудительно остановим и запустим агент, выполнив следующие команды. 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
Теперь агент будет собирать метрики из каждого из указанных входных подключаемых модулей и выдавать их в Azure Monitor. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Отображение метрик Telegraf на портале Azure 

1. Откройте [портал Azure](https://portal.azure.com). 

1. Перейдите к новой вкладке  **Монитор** . Выберите   **Метрики**.  

     ![Монитор. Метрики (предварительная версия)](./media/collect-custom-metrics-linux-telegraf/metrics.png)

1. Выберите свою виртуальную машину в селекторе ресурсов.

     ![Диаграмма метрик](./media/collect-custom-metrics-linux-telegraf/metric-chart.png)

1. Выберите пространство имен **Telegraf/CPU** (Telegraf/ЦП) и выберите метрику **usage_system**. Вы можете отфильтровать по измерениям в этой метрике или разделить их.  

     ![Выбор пространства имен и метрики](./media/collect-custom-metrics-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Дополнительная конфигурация 

В приведенном выше руководстве содержатся сведения о том, как настроить агент Telegraf для сбора метрик из нескольких основных входных подключаемых модулей. Агент Telegraf поддерживает более 150 входных подключаемых модулей, некоторые из них поддерживают дополнительные варианты конфигурации. Компания InfluxData опубликовала [список поддерживаемых подключаемых модулей](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) и инструкции по [их настройке](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/).  

Кроме того, в этом пошаговом руководстве агент Telegraf использовали для выдачи метрик виртуальной машине, на которой развернут агент. Агент Telegraf можно также использовать как сборщик и сервер пересылки метрик для других ресурсов. Сведения от том, как настроить агент для выдачи метрик для других ресурсов Azure, см. в статье [Azure Monitor Custom Metrics Output for Telegraf](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md) (Выходные данные пользовательских метрик Azure Monitor для Telegraf).  

## <a name="clean-up-resources"></a>Очистка ресурсов 

Если группа ресурсов, виртуальная машина и все связанные с ними ресурсы вам больше не требуются, их можно удалить. Для этого выберите группу ресурсов для виртуальной машины и нажмите  **Удалить**. Затем подтвердите имя удаляемой группы ресурсов. 

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения о настраиваемых метриках см. в [этой статье](metrics-custom-overview.md).



