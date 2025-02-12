---
title: Что делать, если прерывание работы службы Azure влияет на Azure Key Vault — Azure Key Vault | Документация Майкрософт
description: Узнайте, что делать, если прерывание работы службы Azure влияет на хранилище ключей Azure.
services: key-vault
author: barclayn
manager: barbkess
editor: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: barclayn
ms.openlocfilehash: dba1fe91a635f467f4a3aeeaa048897065822869
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66236645"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Доступность и избыточность хранилища ключей Azure

Хранилище ключей Azure имеет несколько уровней избыточности, благодаря которым ключи и секреты остаются доступными для приложения даже при сбое отдельных компонентов службы.

Содержимое хранилища ключей реплицируется в пределах региона, а также в дополнительный регион, расположенный на расстоянии не менее 240 км от основного, но в той же географической области. Это обеспечивает высокий уровень надежности ключей и секретов. В разделе [Непрерывность бизнес-процессов и аварийное восстановление в службах BizTalk: пары регионов Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) можно получить сведения об определенных парах регионов.

При сбое отдельных компонентов службы хранилища ключей ваши запросы будут обслуживаться дополнительными компонентами в текущем регионе, чтобы не допустить снижения функциональности. Для этого не нужно предпринимать какие-либо действия. Все выполняется автоматически и незаметно для вас.

В редких случаях, когда становится недоступным весь регион Azure, ваши запросы к хранилищу ключей Azure в этом регионе автоматически перенаправляются в дополнительный регион (это называется *отработкой отказа*). Когда основной регион снова становится доступным, запросы отправляются обратно в основной регион (это называется *восстановлением размещения*). Опять же, так как это происходит автоматически, то никаких дополнительных действий предпринимать не нужно.

Через эту схему высокого уровня доступности Azure Key Vault не простаивают для действий по обслуживанию.

Существует несколько моментов, на которые следует обратить внимание.

* В случае отработки отказа для региона на отработку отказа службы может потребоваться несколько минут. Запросы, отправленные в это время, могут завершаться ошибкой, пока не завершится отработка отказа.
* После завершения отработки отказа хранилище ключей находится в режиме только для чтения. В этом режиме поддерживаются следующие запросы:
  * получение списка хранилищ ключей;
  * получение свойств из хранилищ ключей;
  * получение списка секретов;
  * получение секретов;
  * получение списка ключей;
  * получение ключей и их свойств;
  * Шифрование
  * расшифровка;
  * оборачивание;
  * разворачивание;
  * Проверка
  * вход;
  * Azure Backup
* После восстановления размещения становятся доступными все типы запросов (в том числе чтение *и* запись).

