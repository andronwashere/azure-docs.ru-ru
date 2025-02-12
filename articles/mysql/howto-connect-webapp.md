---
title: Подключение существующей службы приложений Azure к службе "База данных Azure для MySQL"
description: Инструкции по правильному подключению существующей службы приложений Azure к базе данных Azure для MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 5/21/2019
ms.openlocfilehash: 3fbffc805afb540499e38f1c0853260968228b22
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002008"
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Подключение существующей службы приложений Azure к базе данных Azure для сервера MySQL
В этом разделе объясняется, как подключить имеющуюся службу приложений Azure к базе данных Azure для сервера MySQL.

## <a name="before-you-begin"></a>Перед началом работы
Войдите на [портале Azure](https://portal.azure.com). Создайте базу данных Azure для сервера MySQL. Дополнительные сведения см. в статье [Создание базы данных Azure для сервера MySQL с помощью портала Azure](quickstart-create-mysql-server-database-using-azure-portal.md) или [Создание сервера базы данных Azure для MySQL с помощью Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md).

В настоящее время существует два решения предоставления доступа из службы приложений Azure к базе данных Azure для MySQL. Оба решения включают настройку правил брандмауэра уровня сервера.

## <a name="solution-1---allow-azure-services"></a>Решение 1: разрешение доступа к службам Azure
База данных Azure для MySQL обеспечивает безопасность доступа, используя брандмауэр для защиты данных. При настройке подключения из службы приложений Azure к базе данных Azure для сервера MySQL имейте в виду, что исходящие IP-адреса службы приложений являются динамическими по своей природе. Если выбрать параметр "Разрешить доступ к службам Azure", служба приложений сможет подключаться к серверу MySQL.

1. В колонке сервера MySQL в разделе "Параметры" щелкните **Безопасность подключения**, чтобы открыть колонку "Безопасность подключения" базы данных Azure для MySQL.

   ![Портал Azure: щелчок пункта "Безопасность подключения"](./media/howto-connect-webapp/1-connection-security.png)

2. Выберите значение **ВКЛ.** для параметра **Разрешить доступ к службам Azure**, а затем нажмите кнопку **Сохранить**.
   ![Портал Azure: параметр "Разрешить доступ к службам Azure"](./media/howto-connect-webapp/allow-azure.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>Решение 2. Создание правила брандмауэра, явно разрешающего исходящие IP-адреса
Вы можете явно добавить все исходящие IP-адреса службы приложений Azure.

1. В колонке "Свойства службы приложений" обратите внимание на поле **ИСХОДЯЩИЙ IP-АДРЕС**.

   ![Портал Azure — просмотр исходящих IP-адресов](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. В колонке "Безопасность подключения" для MySQL добавьте исходящие IP-адреса по одному.

   ![Портал Azure — добавление явных IP-адресов](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Не забывайте **сохранять** правила брандмауэра.

Хотя служба приложений Azure пытается поддерживать постоянные IP-адреса, бывают случаи, когда IP-адреса могут измениться. Например, это происходит, когда приложение выполняет перезапуск или возникает операция масштабирования, а также при добавлении новых компьютеров в региональные центры обработки данных Azure для повышения емкости. При изменении IP-адресов может наблюдаться простой приложения, если оно больше не может подключиться к серверу MySQL. Учтите это при выборе одного из предыдущих решений.

## <a name="ssl-configuration"></a>Настройка SSL
Протокол SSL по умолчанию включен для базы данных Azure для MySQL. Если приложение не использует SSL для подключения к базе данных, необходимо отключить SSL для сервера MySQL. Дополнительные сведения о настройке SSL см. в статье [Настройка SSL-подключений в приложении для безопасного подключения к базе данных Azure для MySQL](howto-configure-ssl.md).

### <a name="django-pymysql"></a>Django (PyMySQL)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'quickstartdb',
        'USER': 'myadmin@mydemoserver',
        'PASSWORD': 'yourpassword',
        'HOST': 'mydemoserver.mysql.database.azure.com',
        'PORT': '3306',
        'OPTIONS': {
            'ssl': {'ssl-ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}
        }
    }
}
```

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о строках подключения см. в [соответствующей статье](howto-connection-string.md).
