# SQLi-02
## SQL Injection Vulnerability Allowing Login Bypass (Python)

### Overview

Este projeto demonstra uma vulnerabilidade de SQL Injection que permite a um atacante contornar o sistema de autenticação (login bypass).

A falha ocorre quando inputs de utilizador (username/password) são inseridos diretamente numa query SQL sem proteção adequada.

EXs:
01:
![SQLi 2](https://github.com/user-attachments/assets/e7957176-59a6-4f7e-8d8f-8359d447ea65)

02:
![SQLi 2 1](https://github.com/user-attachments/assets/e37ffc34-f7e8-4f3c-8afb-0c908df2ffe4)

### Vulnerable Login Example (Python)
import sqlite3

def login(username, password):
    conn = sqlite3.connect("database.db")
    cursor = conn.cursor()

    query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
    print("Executing:", query)

    cursor.execute(query)
    result = cursor.fetchone()

    conn.close()
    return result is not None
    
### Vulnerability Explanation:

A query depende diretamente do input do utilizador. Isso permite manipular a lógica do WHERE.

Exploitation Example
Acessei uma página, bypassando a autenticação.
Burp Suite:
Input Malicioso:
username: admininistrator '--
password: 12324
Query Resultante
SELECT * FROM users 
WHERE username = 'admininistrator'  AND password = 12324

Resultado:
O Sinal '-- é serviu para ignorar todo o resto que vier depois.
O atacante consegue autenticar-se sem password válida

### Impacto
Bypass completo de autenticação
Acesso não autorizado a contas
Possível acesso a contas administrativas
Comprometimento total do sistema

### Secure Version (Mitigation)
Uso de Queries Parametrizadas
import sqlite3:

def login(username, password):
    conn = sqlite3.connect("database.db")
    cursor = conn.cursor()

    query = "SELECT * FROM users WHERE username = ? AND password = ?"
    cursor.execute(query, (username, password))

    result = cursor.fetchone()
    conn.close()
    return result is not None

    
Additional Security Measures - Medidas adicionais de Segurança:
🔒 Hash de passwords (ex: bcrypt)
🔒 Nunca armazenar passwords em plain text
🔒 Rate limiting em tentativas de login
🔒 Mensagens de erro genéricas (evitar leaks)
🔒 Monitorização de atividade suspeita

### Risks - Riscos
Account takeover
Privilege escalation
Data exposure
System compromise

### Conclusion - Conclusão

O login bypass via SQL Injection é uma vulnerabilidade crítica. Em Python, evitar concatenação de strings e usar queries parametrizadas é essencial para proteger sistemas de autenticação.
