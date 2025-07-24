# Script de Criação de Estrutura de Usuários, Diretórios e Permissões

Este documento descreve o processo para criar um script Bash que automatiza a criação de diretórios, grupos, usuários e a configuração de permissões conforme definido nas imagens fornecidas.

## Pré-requisitos

*   Sistema Linux com acesso ao terminal.
*   Conhecimento básico de comandos Linux (usuários, grupos, permissões, etc.).
*   Acesso de `root` ou permissões `sudo`.
*   Editor de texto para criar o script Bash (ex: `nano`, `vim`, `gedit`).
*   Cliente Git instalado e configurado (para subir o script no GitHub).

## Passo a Passo

1.  **Criação do Arquivo Bash:**

    *   Abra o terminal.
    *   Crie um novo arquivo chamado `provisionamento.sh` (ou qualquer nome de sua preferência) com o editor de texto de sua escolha:
        ```bash
        nano provisionamento.sh
        ```
        ou
          ```bash
        vi provisionamento.sh
        ```

2.  **Estrutura do Script:**

    *   Adicione o shebang no início do arquivo para indicar que é um script Bash:
        ```bash
        #!/bin/bash
        ```
    *   Adicione comentários para organizar e documentar o script.

3.  **Exclusão de Recursos Existentes (Opcional, mas recomendado para idempotência):**

    *   Adicione comandos para remover diretórios, grupos e usuários existentes para garantir que o script possa ser executado múltiplas vezes sem conflitos. Isso torna o script idempotente.
        ```bash
        # Excluir diretórios, grupos e usuários existentes (se existirem)
        groupdel GRP_ADM 2>/dev/null
        groupdel GRP_VEN 2>/dev/null
        groupdel GRP_SEC 2>/dev/null

        userdel -r carlos 2>/dev/null
        userdel -r maria 2>/dev/null
        userdel -r joao 2>/dev/null
        userdel -r debora 2>/dev/null
        userdel -r sebastiana 2>/dev/null
        userdel -r roberto 2>/dev/null
        userdel -r josefina 2>/dev/null
        userdel -r amanda 2>/dev/null
        userdel -r rogerio 2>/dev/null

        rm -rf /publico /adm /ven /sec 2>/dev/null
        ```
        *   O `2>/dev/null` redireciona erros para o "buraco negro", evitando mensagens de erro caso o recurso não exista.
        *   `-r` no `userdel` remove o diretório home do usuário.
        *   `-rf` no `rm` remove diretórios e seus conteúdos de forma recursiva e forçada.

4.  **Criação de Grupos:**

    *   Adicione comandos para criar os grupos `GRP_ADM`, `GRP_VEN` e `GRP_SEC`:
        ```bash
        # Criar grupos
        groupadd GRP_ADM
        groupadd GRP_VEN
        groupadd GRP_SEC
        ```

5.  **Criação de Usuários e Adição aos Grupos:**

    *   Adicione comandos para criar os usuários e adicioná-los aos seus respectivos grupos:
        ```bash
        # Criar usuários e adicionar aos grupos
        useradd -m -g GRP_ADM carlos
        useradd -m -g GRP_ADM maria
        useradd -m -g GRP_ADM joao

        useradd -m -g GRP_VEN debora
        useradd -m -g GRP_VEN sebastiana
        useradd -m -g GRP_VEN roberto

        useradd -m -g GRP_SEC josefina
        useradd -m -g GRP_SEC amanda
        useradd -m -g GRP_SEC rogerio
        ```
        *   `-m` cria o diretório home do usuário.
        *   `-g` especifica o grupo primário do usuário.

6.  **Criação de Diretórios:**

    *   Adicione comandos para criar os diretórios `/publico`, `/adm`, `/ven` e `/sec`:
        ```bash
        # Criar diretórios
        mkdir /publico
        mkdir /adm
        mkdir /ven
        mkdir /sec
        ```

7.  **Definindo o Usuário Root como Dono dos Diretórios:**

    *   Altere o proprietário dos diretórios para o usuário root:
        ```bash
        # Definir o dono dos diretórios como root
        chown root:root /publico
        chown root:root /adm
        chown root:root /ven
        chown root:root /sec
        ```

8.  **Configuração de Permissões:**

    *   Configurar permissões conforme as definições:
        *   Todos os usuários tem permissão total no diretório `/publico`.
        *   Usuários de cada grupo tem permissão total no diretório correspondente.
        *   Usuários não devem ter permissão de leitura, escrita ou execução em diretórios de outros departamentos.

        ```bash
        # Configurar permissões

        # Permissão total para todos no diretório /publico (777)
        chmod 777 /publico

        # Permissão total para o grupo GRP_ADM no diretório /adm
        chmod 770 /adm
        chgrp GRP_ADM /adm

        # Permissão total para o grupo GRP_VEN no diretório /ven
        chmod 770 /ven
        chgrp GRP_VEN /ven

        # Permissão total para o grupo GRP_SEC no diretório /sec
        chmod 770 /sec
        chgrp GRP_SEC /sec
        ```

9.  **Script Completo de Exemplo:**

    ```bash
    #!/bin/bash

    # Excluir diretórios, grupos e usuários existentes (se existirem)
    groupdel GRP_ADM 2>/dev/null
    groupdel GRP_VEN 2>/dev/null
    groupdel GRP_SEC 2>/dev/null

    userdel -r carlos 2>/dev/null
    userdel -r maria 2>/dev/null
    userdel -r joao 2>/dev/null
    userdel -r debora 2>/dev/null
    userdel -r sebastiana 2>/dev/null
    userdel -r roberto 2>/dev/null
    userdel -r josefina 2>/dev/null
    userdel -r amanda 2>/dev/null
    userdel -r rogerio 2>/dev/null

    rm -rf /publico /adm /ven /sec 2>/dev/null

    # Criar grupos
    groupadd GRP_ADM
    groupadd GRP_VEN
    groupadd GRP_SEC

    # Criar usuários e adicionar aos grupos
    useradd -m -g GRP_ADM carlos
    useradd -m -g GRP_ADM maria
    useradd -m -g GRP_ADM joao

    useradd -m -g GRP_VEN debora
    useradd -m -g GRP_VEN sebastiana
    useradd -m -g GRP_VEN roberto

    useradd -m -g GRP_SEC josefina
    useradd -m -g GRP_SEC amanda
    useradd -m -g GRP_SEC rogerio

    # Criar diretórios
    mkdir /publico
    mkdir /adm
    mkdir /ven
    mkdir /sec

    # Definir o dono dos diretórios como root
    chown root:root /publico
    chown root:root /adm
    chown root:root /ven
    chown root:root /sec

    # Configurar permissões
    # Permissão total para todos no diretório /publico (777)
    chmod 777 /publico

    # Permissão total para o grupo GRP_ADM no diretório /adm
    chmod 770 /adm
    chgrp GRP_ADM /adm

    # Permissão total para o grupo GRP_VEN no diretório /ven
    chmod 770 /ven
    chgrp GRP_VEN /ven

    # Permissão total para o grupo GRP_SEC no diretório /sec
    chmod 770 /sec
    chgrp GRP_SEC /sec

    echo "Provisionamento completo!"
    ```

10. **Tornar o Script Executável:**

    *   Dê permissão de execução ao script:
        ```bash
        chmod +x provisionamento.sh
        ```

11. **Executar o Script:**

    *   Execute o script como `root` (ou com `sudo`):
        ```bash
        sudo ./provisionamento.sh
        ```

12. **Verificação:**

    *   Após a execução, verifique se os diretórios, usuários e grupos foram criados corretamente e se as permissões estão configuradas conforme o esperado.

13. **Adicionar o Script ao Git e Subir para o GitHub:**

    *   Inicialize um repositório Git (se ainda não tiver um):
        ```bash
        git init
        ```
    *   Adicione o script ao repositório:
        ```bash
        git add provisionamento.sh
        ```
    *   Faça um commit:
        ```bash
        git commit -m "Adicionado script de provisionamento"
        ```
    *   Crie um repositório no GitHub.
    *   Adicione o remote ao seu repositório local:
        ```bash
        git remote add origin <URL_DO_SEU_REPOSITORIO_GITHUB>
        ```
    *   Suba o script para o GitHub:
        ```bash
        git push -u origin main
        ```

## Observações

*   Este script assume que você está executando-o em um ambiente onde os comandos `groupadd`, `useradd`, `mkdir`, `chown`, `chmod` e `chgrp` estão disponíveis.
*   Adapte o script conforme necessário para atender às suas necessidades específicas.
*   Considere adicionar tratamento de erros e logs para facilitar a depuração e o monitoramento.
*   Ao subir o script para o GitHub, considere torná-lo parte de um repositório maior com outros scripts de IaC e documentação.

Este README fornece um guia detalhado para criar e executar o script de provisionamento.

