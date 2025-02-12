---
title: Устранение неполадок с развертыванием OpenShift в Azure | Документация Майкрософт
description: Устранение неполадок с развертыванием OpenShift в Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: af6746e7246b8783e5bdbef34cf1b57427aa7ebb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60771283"
---
# <a name="troubleshoot-openshift-deployment-in-azure"></a>Устранение неполадок с развертыванием OpenShift в Azure

Если кластер OpenShift не удается развернуть, на портале Azure отображается сообщение об ошибке. Разобраться в сообщении и понять причину проблемы может быть непросто. Просмотрите сообщение на наличие кодов завершения 3, 4 или 5. Ниже приводятся сведения об этих трех кодах завершения.

- Код завершения 3: Имя Red Hat подписки пользователя / пароль или идентификатор организации / ключ активации неверны
- Код завершения 4: Неправильный ваш идентификатор пула Red Hat или отсутствуют права
- Код завершения 5: Не удалось подготовить тонкий пул тома Docker

Если указаны другие коды завершения, подключитесь к узлу (узлам) по протоколу SSH, чтобы просмотреть файлы журналов.

**Платформа контейнеров OpenShift**

По протоколу SSH подключитесь к узлу со сборником схем Ansible. Для шаблона или предложения из Marketplace используйте узел-бастион. Из узла-бастиона вы можете подключится по протоколу SSH ко всем другим узлам в кластере (главному узлу, узлу инфраструктуры, узлу CNS и вычислительному узлу). Вам понадобятся права привилегированного пользователя, чтобы просмотреть файлы журналов. Доступ с правами привилегированного пользователя отключен по умолчанию для SSH-подключений, поэтому не используйте его при подключении к другим узлам.

**OKD**

По протоколу SSH подключитесь к узлу со сборником схем Ansible. Для шаблона OKD (версии 3.9 и более ранних) используйте главный узел 0. Для шаблона OKD (версии 3.10 и более поздних) используйте узел-бастион. Из узла со сборником схем Ansible вы можете подключится по протоколу SSH ко всем другим узлам в кластере (главному узлу, узлу инфраструктуры, узлу CNS и вычислительному узлу). Вам понадобятся права привилегированного пользователя (sudo su -), чтобы просмотреть файлы журналов. Доступ с правами привилегированного пользователя отключен по умолчанию для SSH-подключений, поэтому не используйте его при подключении к другим узлам.

## <a name="log-files"></a>Файлы журналов

Файлы журналов (stderr и stdout) для сценариев подготовки узла находятся в `/var/lib/waagent/custom-script/download/0` на всех узлах. Если произошла ошибка во время подготовки узла, просмотрите файлы журналов, чтобы определить ошибку.

Если скрипты подготовки выполнен успешно, то файлы журналов в `/var/lib/waagent/custom-script/download/1` каталога узла playbook ansible необходимо проверить. Если ошибка произошла во время фактической установки OpenShift, в файле STDOUT она будет указана. Используйте эту информацию при обращении в службу поддержки за помощью.

Пример выходных данных

```json
TASK [openshift_storage_glusterfs : Load heketi topology] **********************
fatal: [mycluster-master-0]: FAILED! => {"changed": true, "cmd": ["oc", "--config=/tmp/openshift-glusterfs-ansible-IbhnUM/admin.kubeconfig", "rsh", "--namespace=glusterfs", "deploy-heketi-storage-1-d9xl5", "heketi-cli", "-s", "http://localhost:8080", "--user", "admin", "--secret", "VuoJURT0/96E42Vv8+XHfsFpSS8R20rH1OiMs3OqARQ=", "topology", "load", "--json=/tmp/openshift-glusterfs-ansible-IbhnUM/topology.json", "2>&1"], "delta": "0:00:21.477831", "end": "2018-05-20 02:49:11.912899", "failed": true, "failed_when_result": true, "rc": 0, "start": "2018-05-20 02:48:50.435068", "stderr": "", "stderr_lines": [], "stdout": "Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2\n\tAllowing file volumes on cluster.\n\tAllowing block volumes on cluster.\n\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4\n\t\tAdding device /dev/sdd ... OK\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131\n\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdd ... OK\n\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\t\tAdding device /dev/sdd ... OK", "stdout_lines": ["Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2", "\tAllowing file volumes on cluster.", "\tAllowing block volumes on cluster.", "\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4", "\t\tAdding device /dev/sdd ... OK", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131", "\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdd ... OK", "\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\t\tAdding device /dev/sdd ... OK"]}

PLAY RECAP *********************************************************************
mycluster-cns-0       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-1       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-2       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-infra-0     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-1     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-2     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-master-0    : ok=502  changed=198  unreachable=0    failed=1   
mycluster-master-1    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-master-2    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-node-0      : ok=143  changed=55   unreachable=0    failed=0   
mycluster-node-1      : ok=143  changed=55   unreachable=0    failed=0   
localhost                  : ok=13   changed=0    unreachable=0    failed=0   

INSTALLER STATUS ***************************************************************
Initialization             : Complete (0:00:39)
Health Check               : Complete (0:00:24)
etcd Install               : Complete (0:01:24)
Master Install             : Complete (0:14:59)
Master Additional Install  : Complete (0:01:10)
Node Install               : Complete (0:10:58)
GlusterFS Install          : In Progress (0:03:33)
    This phase can be restarted by running: playbooks/openshift-glusterfs/config.yml

Failure summary:

  1. Hosts:    mycluster-master-0
     Play:     Configure GlusterFS
     Task:     Load heketi topology
     Message:  Failed without returning a message.
```

Вот список наиболее распространенных ошибок во время установки:

1. Закрытый ключ имеет парольную фразу.
2. Секрет хранилища ключей с закрытым ключом не был правильно создан.
3. Учетные данные субъекта-службы были введены неправильно.
4. Субъект-служба не имеет доступа участника к группе ресурсов.

### <a name="private-key-has-a-passphrase"></a>Закрытый ключ имеет парольную фразу

Вы увидите ошибку, которая запрещено разрешение для ssh. SSH к узлу playbook ansible для проверки парольную фразу для закрытого ключа.

### <a name="key-vault-secret-with-private-key-wasnt-created-correctly"></a>Секрет хранилища ключей с закрытым ключом не был правильно создан

Закрытый ключ копируется в узел playbook ansible — ~/.ssh/id_rsa. Убедитесь, что этот файл исправен. Проверьте его, установив SSH-подключение между узлом со сборником схем Ansible и одним из узлов кластера.

### <a name="service-principal-credentials-were-entered-incorrectly"></a>Учетные данные субъекта-службы были введены неправильно

При указании входных данных для шаблона или предложения из Marketplace была допущена ошибка. Убедитесь, что указан правильный идентификатор приложения (clientId) и пароль (clientSecret) для субъекта-службы. Выполните проверку с помощью такой команды в Azure CLI.

```bash
az login --service-principal -u <client id> -p <client secret> -t <tenant id>
```

### <a name="service-principal-doesnt-have-contributor-access-to-the-resource-group"></a>Субъект-служба не имеет доступа участника к группе ресурсов.

Если включен поставщик облачных служб Azure, используемый субъект-служба должен иметь доступ участника к группе ресурсов. Выполните проверку с помощью такой команды в Azure CLI.

```bash
az group update -g <openshift resource group> --set tags.sptest=test
```

## <a name="additional-tools"></a>Дополнительные средства

При некоторых ошибках можно также использовать следующие команды для получения дополнительных сведений:

1. состояние systemctl \<службы >
2. journalctl -xe
