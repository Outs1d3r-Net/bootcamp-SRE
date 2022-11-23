# Procedimentos

- [ ]  Instalação Wordpress Manual
- [ ]  Instalação do ansible
- [ ]  Primeiros passos com ansible
- [ ]  Instalação Wordpress automatizado
- [ ]  Validação
- [ ]  Exercicíos

[DevDocs](https://devdocs.io/ansible/)

[Ansible Community Documentation](https://docs.ansible.com/ansible_community.html)

## Conceitos

---

### Roles

As ROLES permitem que você carregue automaticamente variáveis, arquivos, tarefas e outros artefatos do Ansible relacionados com base em uma estrutura de arquivos conhecida.

![Untitled](Procedimentos%20e9a82fd2e0e04ce5996a3395a7a0fef1/Untitled.png)

t**asks/main.yml** - O responsavel em executar as tarefas.

**handlers/main.yml** - Handlers é utilizado para manipular a função.

**library/module.py** - Pasta para carregar bibliotecas externas.

**defaults/main.yml** - Pasta de variaveis, mas o default tem prioridade baixa e  podem ser facilmente substituídas por qualquer outra variável.

**vars/main.yml** - Pasta de variaveis.

**files/main.yml** - Pasta de arquivo.

**templates/main.yml** - Pasta de templates de arquivos.

**meta/main.yml** - Onde inclui as dependências da role.