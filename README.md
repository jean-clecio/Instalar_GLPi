# Documentação de Infraestrutura — GLPI no Ubuntu Server

> Guia técnico para implantação do sistema GLPI em ambiente Linux Ubuntu Server 24.04,
> cobrindo desde a configuração base do sistema operacional até o acesso externo ao banco de dados.

---

## Visão Geral

Este repositório documenta todo o processo de configuração e implantação do **GLPI 11** —
um sistema open source de gerenciamento de chamados (Help Desk) e inventário de TI —
sobre uma stack composta por **Ubuntu Server 24.04 + Nginx + PHP 8.3 + MariaDB**.

A implantação segue boas práticas de segurança, separando diretórios sensíveis da raiz
pública e configurando corretamente permissões de sistema de arquivos.

---

## Pré-requisitos

| Requisito | Detalhe |
|---|---|
| Sistema Operacional | Ubuntu Server 24.04 LTS |
| Acesso | Usuário com privilégios `sudo` ou `root` |
| Rede | IP fixo configurado na interface de rede |
| Conectividade | Acesso à internet para download de pacotes |

---

## Estrutura da Documentação

```
📄 1-Comandos_essenciais_do_ubuntu_server.md
    Configuração inicial do sistema operacional: atualização, hostname,
    rede, firewall, SSH e usuário administrativo.

📄 2-instalar_glpi.md
    Script comentado para instalação automatizada do GLPI 11,
    incluindo dependências, banco de dados, Nginx e permissões.

📄 3-MariaDB_e_Dbeaver.md
    Configuração do MariaDB para aceitar conexões remotas e
    integração com o cliente DBeaver para administração visual do banco.
```

---

## Fluxo de Implantação

```
[1] Configurar Ubuntu Server
        │
        ▼
[2] Instalar GLPI (script automatizado)
        │
        ▼
[3] Liberar acesso remoto ao banco (opcional)
        │
        ▼
[✓] GLPI disponível em http://<IP-do-servidor>
```

---

## Acesso ao Sistema

Após a instalação, o GLPI estará acessível via navegador no endereço:

```
http://192.168.1.74
```

> ⚠️ **Atenção:** As credenciais padrão do banco de dados presentes nos scripts
> (`senha`) são de exemplo. Em produção, utilize senhas seguras e únicas.

---

## Stack Tecnológica

| Componente | Versão | Função |
|---|---|---|
| Ubuntu Server | 24.04 LTS | Sistema operacional base |
| Nginx | Padrão do repositório | Servidor web / proxy reverso |
| PHP-FPM | 8.3 | Interpretador da aplicação GLPI |
| MariaDB | Padrão do repositório | Banco de dados relacional |
| GLPI | 11.0.0 | Sistema de Help Desk / ITSM |
| DBeaver | Qualquer versão recente | Cliente GUI para administração do banco |

---

## Considerações de Segurança

- O diretório público do Nginx aponta apenas para `/var/www/glpi/public`, impedindo acesso direto a arquivos internos da aplicação.
- Arquivos de configuração (`/etc/glpi/`), dados (`/var/lib/glpi/`) e logs (`/var/log/glpi/`) ficam **fora** da raiz web.
- O firewall UFW está habilitado com regras explícitas apenas para as portas necessárias (22, 80, 443 e opcionalmente 3306).
- A abertura da porta **3306 para acesso remoto** (DBeaver) deve ser restrita ao ambiente de desenvolvimento ou protegida por VPN em produção.

---

## Autor

Documentação elaborada para uso interno de referência técnica.
