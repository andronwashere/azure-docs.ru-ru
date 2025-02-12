---
title: Пользовательские политики Azure Active Directory B2C | Документация Майкрософт
description: Сведения о пользовательских политиках Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2938ae075bbd4c38b686ca6654bede678f876857
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66509795"
---
# <a name="custom-policies-in-azure-active-directory-b2c"></a>Пользовательские политики в Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Пользовательские политики — это файлы конфигурации, которые определяют поведение вашего клиента Azure Active Directory (Azure AD) B2C. Потоки пользователей предопределены на портале Azure AD B2C для наиболее распространенных задач идентификации. Разработчик удостоверений может полностью изменять пользовательские политики для выполнения большого количества задач.

## <a name="comparing-user-flows-and-custom-policies"></a>Сравнение потоков пользователей и пользовательских политик

| | Маршруты пользователей | Пользовательские политики |
|-|-------------------|-----------------|
| Целевые пользователи | Все разработчики приложений имеют или не имеют опыт работы с удостоверениями. | Специалисты по удостоверениям, системные интеграторы, консультанты и внутренние команды по работе с удостоверениями. Они знакомы с потоками OpenIDConnect, поставщиками удостоверений и аутентификацией на основе утверждений. |
| Метод настройки | Портал Azure с удобным пользовательским интерфейсом. | Непосредственное изменение XML-файлов и их отправка на портал Azure. |
| Настройка пользовательского интерфейса | Полная настройка пользовательского интерфейса, включая HTML, CSS и JavaScript.<br><br>Поддержка нескольких языков для пользовательских строк. | Аналогично |
| Настройка атрибутов | Стандартные и пользовательские атрибуты. | Аналогично |
| Управление токенами и сеансами | Пользовательский маркер и несколько параметров сеанса. | Аналогично |
| Поставщики удостоверений | Предопределенные локальных или социальных сетей поставщика и большинство поставщиков удостоверений OIDC, например федерацию с клиентами Azure Active Directory. | OIDC, SAML, OAuth в соответствии со стандартами.  Проверки подлинности можно также с помощью интеграции с помощью REST API. |
| Задачи, связанные с удостоверениями | Регистрация или вход в систему с помощью локальных учетных записей или учетных записей разных социальных сетей.<br><br>Самостоятельный сброс пароля.<br><br>Изменение профиля.<br><br>Многофакторная проверка подлинности.<br><br>Настройка маркеров и сеансов.<br><br>Потоки маркера доступа. | Выполняют те же задачи, что и потоки пользователей, с помощью пользовательских поставщиков удостоверений или настраиваемых областей.<br><br>Подготовка учетной записи пользователя в другой системе во время регистрации.<br><br>Отправка приветственного сообщения почты с помощью поставщика услуг электронной почты.<br><br>Использование пользовательского хранилища за пределами Azure AD B2C.<br><br>Проверка предоставленной пользователями информации с помощью доверенной системы с использованием API. |

## <a name="policy-files"></a>Файлы политики

Используются три вида файлов политик:

- **Базовый файл**. Содержит большинство определений. Рекомендуется вносить минимальное число изменений в этот файл, чтобы упростить устранение неполадок и долгосрочное обслуживание ваших политик.
- **Файл расширения**. Содержит уникальную конфигурацию вашего клиента.
- **Файл проверяющей стороны**. Однозадачный файл, который может вызвать непосредственно приложение или служба (т. е. проверяющая сторона). Каждой уникальной задаче требуется собственный файл проверяющей стороны. В зависимости от требований к фирменной символике их число может быть выражено как "общее число приложений х общее число вариантов использования".

Потоки пользователей в Azure AD B2C следуют описанному выше шаблону с тремя файлами, но разработчик видит только файл проверяющей стороны, в то время как портал Azure в фоновом режиме вносит изменения в файл расширения.

## <a name="custom-policy-core-concepts"></a>Ключевые понятия пользовательской политики

Служба управления удостоверениями и доступом клиентов (CIAM) в Azure включает в себя:

- Пользовательский каталог, который доступен через Microsoft Graph. В нем содержатся данные пользователя для локальных и федеративных учетных записей.
- Доступ к **Identity Experience Framework**, которая управляет доверием между пользователями и сущностями и обменивается утверждениями между ними для выполнения задач управления удостоверениями и доступом. 
- Служба маркеров безопасности (STS), которая выдает маркеры удостоверений, маркеры обновления и маркеры доступа (и эквивалентные утверждения SAML) и проверяет их для защиты ресурсов.

Azure AD B2C взаимодействует с поставщиками удостоверений, пользователями, другими системами и локальным каталогом в определенной последовательности, чтобы обеспечить задачу идентификации. Например, выполнение входа, регистрация нового пользователя или сброс пароля. Identity Experience Framework и политика (которая также называется путем взаимодействия пользователя или инфраструктурой доверия) устанавливают доверие между несколькими участниками и явно определяет субъекты, действия, протоколы и выполняемую последовательность шагов.

Identity Experience Framework — полностью настраиваемая управляемая политиками облачная платформа Azure, которая оркестрирует доверие между сущностями с использованием стандартных протоколов, таких как OpenIDConnect, OAuth, SAML, WSFed, и нескольких нестандартных, например, обмен утверждениями между системами на основе REST API. Эта платформа обеспечивает удобные возможности с поддержкой HTML и CSS.

Пользовательские политики представлены одним или несколькими XML-файлами, которые ссылаются друг на друга в иерархической цепочке. Элементы XML определяют схему утверждений, преобразования утверждений, определения содержимого, поставщиков утверждений, технические профили и шаги оркестрации пути взаимодействия пользователя, а также другие элементы. Пользовательская политика доступна в виде одного или нескольких XML-файлов, которые выполняются Identity Experience Framework при вызове проверяющей стороной. Разработчики, которые настраивают пользовательские политики, должны точно и подробно определить отношения доверия, чтобы добавить конечные точки метаданных, точные определения обмена утверждениями и настроить секретные данные, ключи и сертификаты, отличающиеся в зависимости от поставщика удостоверений.

### <a name="inheritance-model"></a>Модель наследования

Когда приложение вызывает файл политики проверяющей стороны, Identity Experience Framework в Azure AD B2C добавит все элементы из базового файла, затем из файла расширений и, наконец, из файла политики проверяющей стороны, чтобы создать действующую политику.  Элементы одного и того же типа и имени в файле проверяющей стороны будут переопределять те, что находятся в файле расширений, а последние переопределят значения в базовом файле.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Azure Active Directory B2C. Приступая к работе с настраиваемыми политиками](active-directory-b2c-get-started-custom.md)
