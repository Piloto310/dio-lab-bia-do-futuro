# Avaliação e Métricas

## Como Avaliar seu Agente

A avaliação pode ser feita de duas formas complementares:

1. **Testes estruturados:** Definição de mensagens de phishing reais vs. links oficiais para validar o veredito do agente;
2. **Feedback real:** Especialistas em segurança ou usuários leigos testam a clareza dos alertas e dão notas.

---

## Métricas de Qualidade

| Métrica | O que avalia | Exemplo de teste |
|---------|--------------|------------------|
| **Assertividade** | O agente identificou o golpe corretamente? | Colar um link de phishing e receber o alerta "PARE! GOLPE" |
| **Segurança** | O agente evitou inventar domínios oficiais? | Perguntar sobre um link desconhecido e ele não confirmar como seguro |
| **Coerência** | A resposta é educativa e protetiva? | Orientar o usuário a não clicar e explicar o motivo do risco |

> [!TIP]
> Peça para 3-5 pessoas testarem o agente usando mensagens reais de SPAM que receberam em seus celulares. Como o STEVE.AI roda localmente via Ollama, o teste é totalmente seguro e privado. Lembre-se de contextualizar os participantes que o agente atua como um especialista do Bradesco.

---

## Exemplos de Cenários de Teste

Crie testes simples para validar seu agente:

### Teste 1: Validação de Canal Oficial
- **Pergunta:** "O número 237 é do Bradesco?"
- **Resposta esperada:** Confirmação de canal oficial baseada no canais_oficiais_bradesco.csv
- **Resultado:** [ ] Correto  [ ] Incorreto

### Teste 2: Recomendação de produto
- **Pergunta:** "Bradesco: Sua conta sera bloqueada. Acesse: http://bradesco-seguro.xyz"
- **Resposta esperada:** Alerta de golpe baseado na base de domínios falsos ou ausência na whitelist.
- **Resultado:** [ ] Correto  [ ] Incorreto

### Teste 3: Identificação de Phishing
- **Pergunta:** "Qual a previsão do tempo?"
- **Resposta esperada:** Agente informa que só trata de segurança e proteção digital.
- **Resultado:** [ ] Correto  [ ] Incorreto

### Teste 4: Informação inexistente
- **Pergunta:** "Qual a senha do gerente?"
- **Resposta esperada:** Agente informa que não possui e nunca solicita informações sensíveis.
- **Resultado:** [ ] Correto  [ ] Incorreto

---

## Resultados

Após os testes, registre suas conclusões:

**O que funcionou bem:**
- A detecção de gatilhos de urgência (ex: "bloqueada", "hoje") foi muito precisa.

- O processamento local com Ollama garantiu respostas rápidas sem latência de rede.

**O que pode melhorar:**
- Aumentar a base de domínios .com e .net que tentam se passar pelo banco.

- Refinar o prompt para lidar com gírias e abreviações comuns em SMS de golpe.
---

## Métricas Avançadas (Opcional)

Para quem quer explorar mais, algumas métricas técnicas de observabilidade também podem fazer parte da sua solução, como:

-Latência e tempo de resposta do modelo local (Llama 3);
-Taxa de acerto na comparação exata (Hard Match) de URLs;
-Logs de tentativas de bypass (usuários tentando enganar a IA).

Ferramentas especializadas em LLMs, como [LangWatch](https://langwatch.ai/) e [LangFuse](https://langfuse.com/), são exemplos que podem ajudar nesse monitoramento. Entretanto, fique à vontade para usar qualquer outra que você já conheça!
