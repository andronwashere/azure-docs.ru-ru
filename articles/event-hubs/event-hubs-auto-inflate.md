---
title: Автоматическое масштабирование единиц пропускной способности — Центры событий Azure | Документация Майкрософт
description: Включение автоматического расширения пространства имен для автоматического масштабирования единиц пропускной способности.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 22c12d3233d85a02f6eef8d63e5a4494b4f0cdfa
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273702"
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Автоматическое масштабирование единиц пропускной способности Центров событий Azure
Центры событий Azure представляют собой платформу потоковой передачи платформы с высокой степенью масштабируемости. Таким образом, показатель использования службы "Центры событий" часто увеличивается после начала работы с ней. Для такого использования требуется увеличить число предопределенных [единиц пропускной способности](event-hubs-scalability.md#throughput-units), чтобы масштабировать Центры событий и обеспечить обработку возросших скоростей передачи данных. Функция **автоматического расширения** Центров событий автоматически масштабирует число единиц пропускной способности в соответствии с потребностями. Увеличение единиц пропускной способности предотвращает сценарии регулирования, в которых:

* скорости входящего трафика данных превышают установленные единицы пропускной способности;
* скорости запросов исходящего трафика данных превышают установленные единицы пропускной способности.

Служба "Центры событий" увеличивает пропускную способность, когда загрузка превышает минимальное пороговое значение. При это ни запросы не возвращают ошибки ServerBusy.

## <a name="how-auto-inflate-works"></a>Как работает автоматическое расширение

Трафик Центров событий контролируется с помощью [единиц пропускной способности](event-hubs-scalability.md#throughput-units). Одна единица пропускной способности обеспечивает передачу до 1 МБ/с входящего трафика и до 2 МБ/с исходящего трафика. В Центрах событий (цен. категория "Стандартный") можно настроить от 1 до 20 единиц пропускной способности. Автоматическое расширение позволяет начать с минимального числа единиц пропускной способности. Затем эта функция автоматически масштабируется до необходимого максимального количества единиц пропускной способности в рамках имеющихся ограничений по мере роста трафика. Функция автоматического расширения обеспечивает следующие преимущества:

- Эффективный механизм масштабирования, позволяющий начать с малого и расширяться по мере роста.
- Автоматическое масштабирование до указанного верхнего предела без проблем регулирования.
- Более высокая степень контроля масштабирования, так как вы управляете тем, когда и насколько следует масштабировать.

## <a name="enable-auto-inflate-on-a-namespace"></a>Включение автоматического расширения в пространстве имен

Вы можете включать или отключать автоматическое расширение в пространстве имен в службе "Центры событий", используя один из следующих способов:

- [Портал Azure](https://portal.azure.com).
- [Шаблон Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate).

### <a name="enable-auto-inflate-through-the-portal"></a>Включение автоматического расширения на портале


#### <a name="enable-at-the-time-of-creation"></a>Включение во время создания 
Вы можете включить функцию автоматического расширения **при создании пространства имен в службе "Центры событий"** :
 
![Включение автоматического расширения во время создания концентратора событий](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Если этот параметр включен, можно начать с малого числа единиц пропускной способности и повышать его по мере роста потребностей использования. Верхний лимит для расширения не оказывает немедленного воздействия на цены, которые зависят от числа единиц пропускной способности, используемых в час.

#### <a name="enable-auto-inflate-for-an-existing-event-hub"></a>Включение автоматического расширения для имеющегося концентратора событий
Кроме того, можно также включить функцию автоматического расширения и изменить ее параметры с помощью следующих инструкций: 
 
1. На странице **Пространство имен Центров событий** выберите **Отключено** в разделе **Auto-inflate throughput units** (Единицы пропускной способности автоматического расширения).  

    ![Выбор единиц пропускной способности на странице пространства имен службы "Центры событий"](./media/event-hubs-auto-inflate/select-throughput-units.png)
2. На странице **Параметры масштабирования** установите флажок **Включить** (если не была включена функция автоматического масштабирования).

    ![Выберите "Включить"](./media/event-hubs-auto-inflate/scale-settings.png)
3. Введите **максимальное** число единиц пропускной способности или используйте полосы прокрутки, чтобы задать значение. 
4. (Необязательно.) Обновите **минимальное** число единиц пропускной способности в верхней части этой страницы. 


> [!NOTE]
> Когда вы применяете конфигурацию автоматического расширения, чтобы увеличить число единиц пропускной способности, служба "Центры событий" создает журналы диагностики, которые предоставляют сведения о том, когда и почему пропускная способность увеличилась. Чтобы активировать журнал ведения диагностики для концентратора событий, на портале Azure на странице "Концентратор событий" в меню слева выберите **Параметры диагностики**. Дополнительные сведения см. в статье [Настройка журналов диагностики для центра событий Azure](event-hubs-diagnostic-logs.md). 

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Включение автоматического расширения с помощью шаблона Azure Resource Manager

Вы можете включить автоматическое расширение во время развертывания шаблона Azure Resource Manager. Например, задайте для свойства `isAutoInflateEnabled` значение **true** и установите для `maximumThroughputUnits` значение 10. Например:

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Полный шаблон см. в разделе [Создание пространства имен Центров событий и включение расширения](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) на сайте GitHub.


## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Центрах событий см. в следующих источниках:

* [Общие сведения о Центрах событий](event-hubs-what-is-event-hubs.md)

