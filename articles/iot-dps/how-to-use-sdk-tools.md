---
title: Использование инструментов, предоставленных в пакетах SDK для службы подготовки устройств к добавлению в Центр Интернета вещей, для упрощения разработки
description: В этом документе рассматривается использование для разработки инструментов, предоставляемых в пакетах SDK для службы подготовки устройств к добавлению в Центр Интернета вещей.
author: yzhong94
ms.author: yizhon
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.openlocfilehash: dc8c29b1c7d4e5056cb6aeee6335e32687fd547f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60627327"
---
# <a name="how-to-use-tools-provided-in-the-sdks-to-simplify-development-for-provisioning"></a>Упрощение разработки для процесса подготовки с использованием инструментов, предоставляемых в пакетах SDK
Служба подготовки устройств к добавлению в Центр Интернета вещей упрощает процесс подготовки с помощью [автоматической JIT-подготовки](concepts-auto-provisioning.md), выполняемой безопасным и масштабируемым способом.  При этом требуется аттестация безопасности в виде сертификата X.509 или доверенного платформенного модуля (TPM).  Корпорация Майкрософт также сотрудничает с [другими партнерами-поставщиками оборудования безопасности](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/), чтобы повысить уровень безопасности развертывания решения Центра Интернета вещей. Требования к безопасности оборудования могут быть трудными для понимания. Чтобы разработчики могли использовать удобный уровень для написания клиентов, которые обращаются к службе подготовки, предоставляется набор пакетов SDK для службы подготовки устройств к добавлению в Центр Интернета вещей Azure. Пакеты SDK также содержат образцы общих сценариев и набор инструментов для упрощения аттестации безопасности при разработке.

## <a name="trusted-platform-module-tpm-simulator"></a>Симулятор доверенного платформенного модуля (TPM)
[TPM](https://docs.microsoft.com/azure/iot-dps/concepts-security) может ссылаться на стандарт безопасного хранения ключей, используемых для проверки подлинности платформы, или относиться к интерфейсу ввода-вывода, используемого для взаимодействия с модулями реализации стандарта. Модули TPM могут существовать как дискретные аппаратные средства, интегрированные аппаратные средства, микропрограммные или программные средства.  В рабочей среде TPM находится на устройстве в виде дискретного, интегрированного оборудования либо посредством встроенного ПО. На этапе тестирования разработчикам предоставляется программный симулятор TPM.  Пока этот симулятор доступен только для разработки на платформе Windows.

Ниже приведены шаги для использования симулятора TPM.
1. [Подготовьте среду разработки](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java) и клонируйте репозиторий GitHub:
   ```
   git clone https://github.com/Azure/azure-iot-sdk-java.git
   ```
2. Перейдите в папку симулятора TPM в ```azure-iot-sdk-java/provisioning/provisioning-tool/tpm-simulator/```.
3. Сначала запустите Simulator.exe, а затем любое клиентское приложение для устройства подготовки.
4. Симулятор нужно оставить работать в фоновом режиме на протяжении всего процесса подготовки, чтобы получить идентификатор регистрации и ключ подтверждения.  Оба значения действительны только для одного экземпляра запуска.

## <a name="x509-certificate-generator"></a>Генератор сертификатов X.509
[Сертификаты X.509](https://docs.microsoft.com/azure/iot-dps/concepts-security#x509-certificates) можно использовать в качестве механизма аттестации для масштабирования производства и упрощения подготовки устройств к работе.  Существует [несколько способов](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) получения сертификата X.509.
* Для рабочей среды рекомендуем приобрести сертификат ЦС X.509 в общедоступном корневом центре сертификации.
* Для среды тестирования вы можете создать корневой сертификат X.509 или цепочку сертификатов X.509 с помощью:
    * OpenSSL. Для создания сертификата можно использовать сценарии:
        * [Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools)
        * [PowerShell или Bash](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)
        
    * Эмулятор обработчика композиций для удостоверений устройств (DICE). DICE может использоваться для идентификации устройств криптографическими средствами и аттестации на основе протокола TLS и клиентских сертификатов X.509.  [Узнайте](https://www.microsoft.com/research/publication/device-identity-dice-riot-keys-certificates/) больше об идентификации устройств с использованием DICE.

### <a name="using-x509-certificate-generator-with-dice-emulator"></a>Использование генератора сертификатов X.509 с эмулятором DICE
В [пакете SDK для Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-tools/provisioning-x509-cert-generator) содержится генератор сертификатов X.509 с эмулятором DICE.  Этот генератор работает на нескольких платформах.  Созданный сертификат может использоваться для разработки на других языках.

В настоящее время эмулятор DICE выдает корневой, промежуточный, конечный сертификаты и связанный закрытый ключ.  Однако корневой и промежуточный сертификат нельзя использовать для подписи отдельного конечного сертификата.  Если вы намерены протестировать сценарий регистрации группы, в котором конечные сертификаты нескольких устройств подписываются с помощью одного сертификата для подписи, можете использовать OpenSSL для создания цепочки сертификатов.

Чтобы создать сертификат X.509 с помощью этого генератора, сделайте следующее:
1. [Подготовьте среду разработки](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java) и клонируйте репозиторий GitHub:
   ```
   git clone https://github.com/Azure/azure-iot-sdk-java.git
   ```
2. Измените значение корневого расположения на azure-iot-sdk-java.
3. Запустите ```mvn install -DskipTests=true```, чтобы загрузить все необходимые пакеты и скомпилировать пакет SDK.
4. Перейдите к корневому расположению для генератора сертификатов X.509 в ```azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator```.
5. Выполните сборку с помощью ```mvn clean install```.
6. Запустите средство с помощью таких команд:
   ```
   cd target
   java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
   ```
7. (Необязательно.) Введите _общее имя_ для сертификатов, когда появится соответствующий запрос.
8. Это средство локально создает **клиентский сертификат**, **закрытый ключ сертификата клиента**, **промежуточный сертификат** и **корневой сертификат**.

**Клиентский сертификат** — это корневой сертификат на устройстве.  **Клиентский сертификат** и связанный **закрытый ключ** необходимы в клиенте устройства. Механизм его размещения в клиентском приложении может отличаться в зависимости от выбора языка.  Дополнительные сведения см. в [кратком руководстве](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) по созданию имитированного устройства с использованием сертификата X.509.

Корневой или промежуточный сертификат можно использовать для создания группы регистраций или индивидуальной регистрации [программным образом](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments-sdks) или с помощью [портала](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

## <a name="next-steps"></a>Дальнейшие действия
* Разработка с использованием [пакета SDK для Azure IoT]( https://github.com/Azure/azure-iot-sdks) для Центра Интернета Azure и службы подготовки устройств к добавлению в Центр Интернета вещей
