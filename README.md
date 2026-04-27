# 🛡️ BruteForce-Medusa-Lab: Simulação de Ataque de Força Bruta

Este projeto é a implementação prática do desafio de segurança ofensiva da **DIO (Digital Innovation One)**. O foco é a utilização do **Kali Linux** e das ferramentas **Medusa/Hydra** para simular ataques de força bruta em um ambiente controlado (Metasploitable 2), documentando o processo e propondo medidas de mitigação eficazes.

O objetivo é exercitar a mentalidade de Blue Team (Defesa) após o teste ofensivo (Red Team).

---

## 💻 Ambiente de Laboratório

O laboratório foi configurado no **VirtualBox** com uma rede **Host-Only** para isolamento total do tráfego.

| Componente | Função | Endereço IP (Exemplo - **Ajuste para o Seu**!) |
| :--- | :--- | :--- |
| **Kali Linux** | Máquina Atacante (Medusa, Nmap, Hydra) | `192.168.56.101` |
| **Metasploitable 2** | Máquina Alvo Vulnerável (FTP, DVWA, SMB) | `192.168.56.100` |

### **Ferramentas Utilizadas:**

* **Kali Linux:** Plataforma de testes de penetração.
* **Medusa:** Ferramenta de força bruta para serviços de rede (FTP, SMB).
* **Hydra:** Ferramenta de força bruta otimizada para requisições HTTP (Web Forms).
* **Nmap:** Usado para enumeração e descoberta de serviços.

---

## 🔬 Metodologia de Ataque e Resultados

Abaixo estão os detalhes, comandos e resultados de cada cenário simulado.

### 1. Ataque de Força Bruta em Serviço FTP (Porta 21)

O ataque visou o serviço FTP vulnerável (VSFTPD 2.3.4) do Metasploitable 2.

#### **Etapas:**

1.  **Enumeração (Nmap):** Confirmação de que o serviço FTP estava ativo.
    ```bash
    nmap -sV -p 21 192.168.56.100
    ```
    *(Evidência em: `/images/1-ftp-nmap.png`)*
2.  **Execução do Ataque (Medusa):** Utilização de wordlists simples (`users.txt` e `senhas.txt`) disponíveis na pasta `/wordlists/`.

    ```bash
    medusa -h 192.168.56.100 -U wordlists/users.txt -P wordlists/senhas.txt -M ftp
    ```
    *(Evidência em: `/images/2-ftp-medusa.png`)*

#### **Resultado:**

* **Credencial Encontrada:** `msfadmin:msfadmin`
* **Validação:** O login foi confirmado via cliente `ftp` do terminal.
    *(Evidência em: `/images/3-ftp-login-success.png`)*

---

### 2. Ataque em Formulário Web (DVWA)

Simulação de quebra de login no formulário **Brute Force** do DVWA. A ferramenta **Hydra** foi utilizada, sendo mais adequada para a manipulação de requisições HTTP POST.

#### **Comando (Exemplo com Hydra):**

```bash
hydra -l admin -P wordlists/senhas.txt 192.168.56.100 http-post-form "/dvwa/vulnerabilities/brute/?username=^USER^&password=^PASS^&Login=Login:H=Location: /dvwa/vulnerabilities/brute/" -V -f
```
💡 Análise de Vulnerabilidades e Mitigação
A documentação do ataque é apenas o primeiro passo. A lição mais importante é saber como prevenir que isso ocorra em ambientes reais.

Serviço Vulnerável	Vulnerabilidade Principal	Recomendação de Mitigação
FTP e SMB	Uso de Credenciais Padrão/Fracas	Política de Senhas Fortes: Exigir comprimento mínimo (ex: 12 caracteres), complexidade (maiúsculas, minúsculas, números, símbolos).
FTP e SMB	Ausência de limite de tentativas	Rate Limiting e Bloqueio de Conta: Configurar o serviço para bloquear o IP ou a conta após 3 a 5 tentativas falhas em um curto período.
Formulário Web (DVWA)	Falha na proteção de Login	CAPTCHA ou Google reCAPTCHA: Implementar um mecanismo que dificulte a automação por bots. Tokens Anti-CSRF: Garantir que o formulário não seja facilmente replicável.
Geral	Visibilidade dos Serviços	Segurança por Profundidade: Limitar o acesso a serviços internos (FTP/SMB) apenas a IPs de confiança (ACLs ou Firewalls).

🔗 Próximos Passos e Reflexões
Este laboratório reforçou a importância de configurações de segurança default. O ataque de força bruta só foi possível devido a: 1) Senhas previsíveis (msfadmin:msfadmin), e 2) A ausência de limitação de tentativas de login.

Para aprimorar o aprendizado, os próximos passos seriam:

Ajustar o Nível de Segurança do DVWA para Medium e High e tentar automatizar os ataques, explorando a necessidade de manipular tokens e cookies de sessão.

**Utilizar o Nmap Scripting Engine (NSE) para identificar credenciais padrão em serviços.

Explorar o módulo telnet do Medusa no Metasploitable 2.

A documentação clara e a proposta de mitigação são tão vitais quanto a execução técnica do ataque.


| Plataforma | Link |
| :--- | :--- |
| **GitHub** (Todos Repositórios) | [https://github.com/FabioWlademir?tab=repositories](https://github.com/FabioWlademir?tab=repositories) |
| **LinkedIn** (Perfil Profissional) | [https://www.linkedin.com/in/fabiowlademir](https://www.linkedin.com/in/fabiowlademir) |
| **Website/Blog** (Artigos e Tutoriais) | [f2suporte.blogspot.com](https://f2suporte.blogspot.com) |
| **Página Pessoal** (GitHub Pages) | [https://fabiowlademir.github.io/](https://fabiowlademir.github.io/) |
| **E-mail** | `f2suporte@gmail.com` |


/
├── README.md
├── wordlists/
│   ├── senhas.txt
│   └── users.txt
└── images/
    ├── 1-ftp-nmap.png
    ├── 2-ftp-medusa.png
    ├── 3-ftp-login-success.png
    └── 4-mitigacao.png

    
📸 Evidências dos Testes de Força Bruta
https://github.com/FabioWlademir/projeto-medusa-dio/blob/main/images/README.md
