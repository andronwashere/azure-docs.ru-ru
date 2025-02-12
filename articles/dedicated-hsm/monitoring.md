---
title: Варианты мониторинга выделенных устройств HSM Azure | Документация Майкрософт
description: Общие сведения о вариантах мониторинга выделенных устройств HSM Azure и полномочиях мониторинга
services: dedicated-hsm
author: barclayn
manager: barbkess
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 2bcf1d1e007398cf80839f5e1d0bac78fd80db07
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60912235"
---
# <a name="azure-dedicated-hsm-monitoring"></a>Мониторинг выделенных устройств HSM Azure

Служба выделенных устройств HSM Azure предоставляет физическое устройство в исключительное пользование клиента со всеми возможностями административного контроля и под ответственность за управление устройством. Она предоставляет устройство [Gemalto SafeNet Luna 7 HSM модели A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/).  Корпорация Майкрософт не будет иметь административного доступа после подготовки устройства клиентом, за исключением подключения последовательного порта на физическом устройстве для мониторинга. Это означает, что клиенты сами отвечают за выполнение стандартных рабочих операций, включая расширенный мониторинг и анализ журналов.
Клиенты полностью отвечают за приложения, в которых используются устройства HSM, и за поддержкой или консультациями должны обращаться в компанию Gemalto. Так как клиент владеет средствами санации операций, корпорация Майкрософт не может гарантировать высокий уровень доступности для этой службы. Клиент должен обеспечить правильную настройку своих приложений для достижения высокого уровня доступности. Корпорация Майкрософт будет отслеживать и поддерживать работоспособность устройства и возможность подключения к сети.

## <a name="microsoft-monitoring"></a>Мониторинг со стороны Майкрософт

По умолчанию используемое устройство Gemalto SafeNet поддерживает варианты мониторинга по протоколу SNMP и через последовательный порт. Корпорация Майкрософт применила подключение по последовательному порту как метод физического подключения к устройству для получения основных данных телеметрии о работоспособности устройства. Отслеживаются такие элементы, как температура и состояние компонентов, например блоков питания и вентиляторов.
Для этой цели корпорация Майкрософт использует роль monitor без прав администратора, настроенную на устройстве Gemalto. Эта роль позволяет получать данные телеметрии, но не дает никакого доступа к устройству с точки зрения задач администрирования или просмотра криптографических сведений. Наши клиенты могут быть полностью уверены, что устройства остаются в их полном распоряжении с точки зрения управления, администрирования и использования для хранения конфиденциальных криптографических ключей. Если клиента не устраивает даже такой минимальный уровень доступа для базового контроля работоспособности, он может отключить учетную запись мониторинга. Очевидно, что в таком случае корпорация Майкрософт не будет получать эти сведения и не сможет предоставлять упреждающие уведомления о проблемах с работоспособностью устройств. В этой ситуации клиент принимает полную ответственность за работоспособность устройства.
Функция мониторинга настроена на опрос устройства для получения данных о работоспособности через каждые 10 минут. Так как передача данных через последовательный порт по своей природе подвержена высокому риску ошибок, оповещение создается лишь после появления нескольких индикаторов неработоспособности устройства в течение часа. По этому оповещению генерируется упреждающее уведомление для клиента о возможных проблемах.
Также будут выполнены меры по снижению влияния и исправлению ситуации в зависимости от характера проблем с минимально возможным риском. Например, при сбое питания выполняется процедура горячей замены, которая не создает событий незаконного изменения, а значит ее влияние на основные операции и связанный с ней риск можно считать минимальными. Также могут применяться такие процедуры, как полная очистка устройства и его вывод из эксплуатации, если это потребуется для снижения рисков безопасности для клиента. В такой ситуации клиент должен подготовить новое устройство и подключить его к сохранившемуся устройству из пары высокого уровня доступности, запустив при этом синхронизацию устройств. Нормальная работоспособность будет восстановлена за минимальное время, с минимальными перебоями и минимальным риском для безопасности.  

## <a name="customer-monitoring"></a>Мониторинг со стороны клиента

Ценность предложения выделенного устройства HSM заключается в том уровне контроля за устройством, который получает клиент, что особенно важно в контексте облачной службы. Естественным следствием этого контроля становится ответственность за мониторинг работоспособности устройства и управление им. Устройство Gemalto SafeNet комплектуется документацией по реализации SNMP и Syslog. Мы рекомендуем всем клиентам службы выделенных устройств HSM использовать эти возможности даже при сохранении учетной записи мониторинга Майкрософт. Если же учетная запись мониторинга Майкрософт отключена, это становится обязательным требованием.
Любая из предлагаемых технологий позволит клиенту своевременно выявлять проблемы и обращаться в службу поддержки Майкрософт для запуска работ по исправлению.

## <a name="next-steps"></a>Дальнейшие действия

Мы рекомендуем хорошо разобраться в основных понятиях службы, таких как высокий уровень доступности и безопасность, прежде чем переходить к подготовке устройств, разработке или развертыванию приложений. Дополнительные сведения об основных понятиях:

* [Высокая доступность](high-availability.md)
* [Физическая безопасность](physical-security.md)
* [Сеть](networking.md)
* [Возможности поддержки](supportability.md)
