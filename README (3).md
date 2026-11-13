# ⚡ Citizen Assist AI — Módulo de Energia e Sustentabilidade

Contribuição da disciplina **Soluções em Energias Renováveis e Sustentáveis** ao projeto **Citizen Assist AI**.
**Global Solution 2026 · Ciência da Computação (1CCPJ)**

## 👥 Integrantes

| Nome | RM |
|---|---|
| Conrado Gracie | 569157 |
| Renato Sandreschi | 569156 |

---

## 📌 Sobre o projeto

O notebook `GS_SERS__1_.ipynb` ajuda famílias a **entender onde a energia da casa é gasta** e **o que compensa fazer primeiro** para reduzir a conta de luz. Ele define o público, estima o consumo por aparelho, compara alternativas (eficiência × solar fotovoltaica), integra os resultados a um assistente de perguntas e respostas e gera automaticamente os arquivos do repositório.

### Público-alvo
Famílias de baixa renda em moradias urbanas na Grande São Paulo (ex.: Santo André/SP), com 4 pessoas e ligação monofásica, para quem a conta de energia é um dos maiores gastos fixos.

### Alternativas comparadas

| Cenário | Descrição |
|---|---|
| **A) Eficiência** | Banho 25% mais curto (8 → 6 min), troca de 6 lâmpadas incandescentes por LED de 9 W e régua com chave contra *stand-by* |
| **B) Solar** | Sistema fotovoltaico dimensionado sobre o consumo atual |
| **C) A + Solar** | Eficiência primeiro e, depois, um sistema solar menor |

Período de referência: **1 mês (30 dias)** para consumo/economia e **25 anos** para retorno financeiro.

---

## 🗂️ Estrutura do notebook

1. **Definição do contexto** — público, necessidade energética, papel do assistente e alternativas.
2. **Premissas e dados** — tarifa, solar, emissões e medidas de eficiência, cada um classificado e com origem registrada; perfil de consumo por equipamento (`E = P × t`) e gráfico de "para onde vai a energia".
3. **Cálculos das alternativas** — consumo, economia, investimento, *payback*, saldo em 25 anos e CO₂, mais **análise de sensibilidade** (tarifa × custo por kWp).
4. **Justificativa das recomendações** — custos, viabilidade, impacto ambiental e limitações.
5. **Integração ao assistente** — função `responder(pergunta)` que usa os resultados calculados.
6. **Testes com usuários** — registro de compreensão, utilidade, comentários e ajustes.
7. **Geração dos arquivos do repositório** — CSVs, JSON, `REFERENCIAS.md`, README e `entrega.txt`.

---

## 🏷️ Classificação dos dados

Todo dado do notebook recebe uma etiqueta:

| Etiqueta | Significado |
|---|---|
| `PÚBLICO` | Valor de fonte pública verificável |
| `CALCULADO` | Derivado de dados públicos por conta explícita |
| `ESTIMADO` | Estimativa justificada, a ser substituída por dado real quando houver |
| `HIPÓTESE` | Suposição do perfil de família adotado |

> ⚠️ **Todos os consumos da residência são estimados** (perfil típico). Substitua pelos dados reais da conta de luz sempre que possível.

### Principais premissas

| Parâmetro | Valor | Classificação |
|---|---|---|
| Tarifa adotada (TUSD+TE, com tributos, sem bandeira) | R$ 1,03/kWh | Calculado |
| Custo de disponibilidade (monofásico) | 30 kWh/mês | Público |
| Horas de sol pleno (HSP) | 4,3 kWh/m²·dia | Estimado |
| Taxa de desempenho (PR) | 0,78 | Estimado |
| Custo do sistema FV pequeno | R$ 4.500/kWp | Estimado |
| Fio B sobre energia injetada (Lei 14.300) | 60% em 2026 | Público |
| Fator de compensação | 0,90 | Calculado |
| Degradação dos módulos | 0,5% ao ano | Estimado |
| Reajuste anual da tarifa | 4% ao ano | Estimado |
| Fator de emissão do SIN (MCTI, 2023) | 0,0385 kgCO₂/kWh | Público |
| Emissão de ciclo de vida da FV | 0,041 kgCO₂e/kWh | Estimado |

A tabela completa (com unidade, período e origem) é exportada em `dados/premissas_origem.csv`.

---

## 📊 Resultados (estimados)

**Perfil de consumo:** ~**252 kWh/mês** (~R$ 260/mês). Maiores consumidores:

| Equipamento | kWh/mês | % do total |
|---|---|---|
| Chuveiro elétrico | 88 | 35% |
| Lâmpadas incandescentes | 54 | 21% |
| Geladeira antiga | 54 | 21% |

**Comparação das alternativas:**

| Cenário | kWp | Economia (R$/mês) | Investimento (R$) | Payback | Saldo em 25 anos (R$) | CO₂ evitado líquido (kg/ano) |
|---|---|---|---|---|---|---|
| A) Eficiência | — | 75,13 | 88 | ~1 mês | 37.457 | 33,7 |
| B) Solar | 2,0 | 185,51 | 9.000 | 3,85 anos | 76.398 | −6,0 |
| C) A + Solar | 1,5 | 212,87 | 6.838 | 2,60 anos | 91.573 | 29,2 |

**Sensibilidade do payback da solar (B), em anos:**

| | R$ 2.500/kWp | R$ 4.500/kWp | R$ 6.500/kWp |
|---|---|---|---|
| Tarifa R$ 0,85 | 2,64 | 4,60 | 6,43 |
| Tarifa R$ 1,03 | 2,20 | 3,85 | 5,40 |
| Tarifa R$ 1,20 | 1,90 | 3,33 | 4,70 |

### Conclusões

- **Comece pela eficiência:** custo baixíssimo, retorno em poucas semanas e não depende de telhado nem de autorização. O maior alvo é o chuveiro.
- **Dimensione a solar depois da eficiência (C):** o sistema fica menor (1,5 kWp contra 2,0 kWp) e o conjunto tem o melhor saldo em 25 anos.
- **O ganho climático da solar é praticamente nulo:** a rede brasileira já é muito limpa, e a fabricação dos painéis emite algo parecido. O benefício principal é **financeiro e de resiliência**.
- **Solar exige telhado adequado, aprovação da distribuidora e, para inquilinos, autorização do proprietário.**

---

## 🤖 Assistente (`responder()`)

Chatbot por palavras-chave que responde em linguagem simples usando os números calculados, sempre com um aviso de que os valores são estimativas. Temas cobertos:

consumo e conta · chuveiro · lâmpadas LED · *stand-by* · energia solar · retorno financeiro (*payback*) · impacto ambiental · economia/dicas · limitações · premissas · referências

Exemplo:

```text
👤 O chuveiro gasta muito?
🤖 🚿 O chuveiro elétrico é o maior gasto: ~88 kWh/mês (~R$ 91). Reduzir o banho em 25%
   (ex.: 8→6 min) economiza ~22 kWh (~R$ 23/mês), de graça.
```

Para conversar no Colab, descomente o bloco do **modo interativo** (digite `sair` para encerrar).

---

## 🧪 Testes com usuários

Três participantes testaram o assistente (notas de 1 a 5):

| Participante | Situação testada | Compreensão | Utilidade | Ajuste feito |
|---|---|---|---|---|
| 1 | Gasto do chuveiro e como economizar | 5 | 5 | Nenhum |
| 2 | Painel solar sendo inquilino | 4 | 5 | Deixado mais claro que o proprietário precisa autorizar |
| 3 | LED × incandescente | 5 | 5 | Nenhum |

---

## ▶️ Como executar

1. Abra `GS_SERS__1_.ipynb` no **Google Colab**.
2. Vá em **Ambiente de execução → Executar tudo**.
3. Dependências: `numpy`, `pandas`, `matplotlib` (já disponíveis no Colab). O README gerado automaticamente usa `DataFrame.to_markdown()`, que requer `tabulate`.

### Arquivos gerados (pasta `citizen_assist_energia/`)

```text
citizen_assist_energia/
├── README.md
├── REFERENCIAS.md
├── entrega.txt
├── dados/
│   ├── consumo_estimado.csv
│   ├── premissas_origem.csv
│   ├── premissas.json
│   ├── comparacao_alternativas.csv
│   └── sensibilidade_payback_solar.csv
└── testes/
    └── registro_testes_usuarios.csv
```

No Colab, a pasta também é baixada como `.zip` ao final da execução.

---

## 📚 Referências

Lista completa em `REFERENCIAS.md` (consulta em 12/11/2026). Principais fontes:

- ANEEL — REN 1.000/2021 e reajuste tarifário Enel SP 2026
- Faturas públicas da Enel SP (monofásicas) — Câmara dos Deputados e Senado
- Lei nº 14.300/2022 — micro e minigeração distribuída (Fio B)
- CRESESB/CEPEL — SunData; INPE — Atlas Brasileiro de Energia Solar
- Solfácil, Greener e Monitor do Mercado — preços de sistemas FV
- MCTI/SIRENE — fator de emissão do SIN
- Procel/INMETRO — potências típicas de aparelhos
- IPCC AR5 WGIII, Anexo III — emissões de ciclo de vida

---

## ⚠️ Limitações

Consumo, tarifa, bandeira tarifária, custo do sistema, irradiação, hábitos de uso e regras da Lei 14.300/2022 variam de caso a caso. Os valores são **estimativas, não garantias** de economia ou geração. Confira sua conta de luz e peça orçamentos antes de decidir.
