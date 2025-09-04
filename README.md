# Projeto Ansible para Backup de Equipamentos de Rede

Este projeto Ansible foi configurado para automatizar o backup das configurações de diversos equipamentos de rede, incluindo:

*   Firewalls FortiGate
*   Switches Dell (OS6, OS9 e OS10)
*   Switches HPE/Aruba (AOS-CX, ProCurve/AOS-S e Comware/3Com)

## Pré-requisitos

Para utilizar este projeto, você precisará de uma máquina de controle com Ansible instalado. O ideal é utilizar um sistema baseado em Linux (como Ubuntu, CentOS) ou o WSL no Windows.

## 1. Instalação do Ansible

Se você ainda não tem o Ansible instalado, siga as instruções para o seu sistema operacional.

**Para Ubuntu/Debian:**

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

**Para CentOS/RHEL:**

```bash
sudo yum install epel-release
sudo yum install ansible
```

Para outras distribuições, consulte a [documentação oficial do Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

## 2. Instalação das Coleções Ansible

Este projeto depende de coleções Ansible específicas para cada fabricante. Para instalá-las, execute o seguinte comando:

```bash
ansible-galaxy collection install dellemc.os10 dellemc.os6 dellemc.os9 arubanetworks.aoscx hpe.procurve hpe.comware fortinet.fortios
```

## 3. Configuração do Inventário

Antes de executar o playbook, você precisa configurar o arquivo `inventory` com os endereços IP e as credenciais dos seus equipamentos.

1.  Abra o arquivo `inventory`.
2.  Edite os hosts de exemplo ou adicione novos hosts dentro dos grupos apropriados (ex: `[dell_os10]`, `[fortigates]`)
3.  Certifique-se de que a variável `ansible_host` contém o IP correto do equipamento.

**Importante:** Este playbook assume que você está usando autenticação por usuário e senha. Você precisará fornecer as credenciais ao executar o playbook. Para um ambiente de produção, é highly recomendável o uso de [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) para proteger suas credenciais.

## 4. Execução do Playbook

Para executar o playbook e realizar o backup de todos os equipamentos configurados no inventário, utilize o comando abaixo. Você será solicitado a fornecer o usuário e a senha para acessar os equipamentos.

```bash
ansible-playbook playbook.yml -u SEU_USUARIO --ask-pass
```

Substitua `SEU_USUARIO` pelo nome de usuário utilizado para acessar os equipamentos.

## 5. Verificação dos Backups

Após a execução bem-sucedida do playbook, os arquivos de backup serão salvos nas seguintes pastas:

*   `backup/switches/`
*   `backup/firewalls/`

Cada arquivo de backup será nomeado com o nome do host definido no inventário (ex: `dellos10-switch1.cfg`).
