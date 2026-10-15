# DIO-Planilhas-Controle-Investimento



# 📊 Simulador Inteligente de Investimento em FIIs (Fundos Imobiliários)



Este projeto consiste em uma ferramenta analítica e preditiva desenvolvida no Microsoft Excel para planejamento financeiro, alocação de carteira e tomada de decisão estratégica em Fundos de Investimento Imobiliário (FIIs).



---


## 1. As Perguntas que a Ferramenta Responde (e Onde Aparece Cada Resposta)



A ferramenta foi estruturada para solucionar tanto a abordagem direta (projeção de patrimônio a partir de aportes) quanto o planejamento reverso (cálculo de esforço necessário para atingir uma meta financeira).



### 🎯 Perguntas Centrais de Negócio


| Pergunta de Negócio | Onde aparece na ferramenta (Seção / Célula) | Como é calculada / Apresentada |
| :--- | :--- | :--- |
| **1. Quanto investir por mês?** | **Seção: INVESTIMENTO MENSAL**<br>Campo: *Quanto investir por mês?*<br>*(com apoio da seção CONFIGURAÇÕES)* | Valor de aporte periódico definido pelo investidor. Na seção CONFIGURAÇÕES, a planilha calcula automaticamente a **Sugestão de Investimento (30% do salário)** para orientar o orçamento. No **Simulador de Metas**, a planilha calcula o *Aporte Mensal Necessário* caso o usuário queira atingir um objetivo específico de renda. |
| **2. Por quantos anos?** | **Seção: INVESTIMENTO MENSAL**<br>Campo: *Por quantos anos?*<br>*(e na tabela de CENÁRIOS)* | Horizonte temporal do plano em anos (ex: 5 anos = 60 meses). Na tabela comparativa de CENÁRIOS, o investidor enxerga lado a lado o avanço em **2, 5, 10, 20 e 30 anos**. |
| **3. Qual a taxa de rendimento mensal?** | **Seção: INVESTIMENTO MENSAL**<br>Campo: *Taxa de Rendimento Mensal?*<br>*(e no SIMULADOR DE PERFIL)* | Rentabilidade média mensal dos proventos e valorização (ex: 1,079% a.m., ou sob os cenários de sensibilidade: Conservador 0,60%, Moderado 0,80% e Otimista 1,00% a.m.). |
| **4. Quanto de patrimônio vai acumular?** | **Seção: INVESTIMENTO MENSAL**<br>Campo: *Patrimônio Acumulado?*<br>*(e coluna Patrimônio Acumulado em CENÁRIOS)* | Montante acumulado ao término do prazo através da capitalização composta dos aportes mensais somados aos proventos reinvestidos. |
| **5. Quanto vai receber de dividendos por mês?** | **Seção: INVESTIMENTO MENSAL**<br>Campo: *Dividendos Mensais?*<br>*(e coluna Dividendo em CENÁRIOS)* | Renda passiva mensal gerada ao fim do período, obtida pela multiplicação direta do patrimônio acumulado pela taxa mensal da carteira. |


---

### 🔍 Perguntas Estratégicas Adicionais

* **Quanto do patrimônio final veio do meu esforço vs do "efeito bola de neve"?**
  * *Onde aparece:* No bloco **`SIMULADOR DE PERFIL`** (Resultados).
  * *Resposta:* Apresenta a abertura entre o **Total Aportado do Bolso** e o **Total Recebido em Proventos / Juros**.
* **FIIs rendem mais que a Renda Fixa tradicional após o imposto?**
  * *Onde aparece:* No bloco **`BENCHMARK (CDI LÍQUIDO)`**.
  * *Resposta:* Projeta o patrimônio gerado no CDI Líquido (descontados 15% de IR) e calcula a **Vantagem dos FIIs sobre o CDI** (em R\$).
* **Quanto preciso guardar para ter uma renda passiva de R\$ X no prazo Y?**
  * *Onde aparece:* No bloco **`SIMULADOR DE METAS`**.
  * *Resposta:* Exibe o **Patrimônio Alvo Necessário** e calcula o **Aporte Mensal Necessário para Bater a Meta**.

---

## 2. Como o VF e o PROCV Entram nos Cálculos

### 📈 A Função `VF` (Valor Futuro)

A função financeira `VF` projeta o crescimento do patrimônio sob o regime de juros compostos com aportes regulares.

* **Sintaxe no Excel:**
  ```excel
  =VF(taxa; nper; pgto; [vp]; [tipo])
  ```
* **Aplicação na Planilha:**
  ```excel
  =VF(Taxa_Mensal; Prazo_Anos * 12; -Aporte_Mensal)
  ```
  1. **`taxa`:** Taxa de rentabilidade mensal adotada (ex: 1,079% ou as taxas de sensibilidade 0,6%, 0,8% e 1,0%).
  2. **`nper` (`Prazo_Anos * 12`):** Converte o horizonte de anos em quantidade total de meses de capitalização.
  3. **`pgto` (`-Aporte_Mensal`):** O sinal negativo (`-`) indica a saída de caixa periódica do investidor, garantindo que o patrimônio acumulado retorne como saldo positivo.
* **Onde é utilizada:**
  * No bloco `INVESTIMENTO MENSAL` para o resultado principal;
  * Na matriz de `CENÁRIOS` para projetar 2, 5, 10, 20 e 30 anos;
  * No `SIMULADOR DE PERFIL` aplicando as taxas de cada perfil;
  * No `BENCHMARK (CDI LÍQUIDO)` utilizando a taxa mensal equivalente líquida de IR.

---

### 🔎 A Função `PROCV` com Chave Composta

A função `PROCV` automatiza a alocação de recursos buscando os percentuais de cada categoria de fundo de acordo com o perfil do investidor.

* **Sintaxe no Excel:**
  ```excel
  =PROCV(valor_procurado; matriz_tabela; num_indice_coluna; [procurar_intervalo])
  ```
* **Exemplo: Procurar a taxa dependendo do perfil selecionado numa lista suspensa:**
  ```excel
  =PROCV(perfil; cenarios_perfil; 2; FALSE)
  ```

---

## 3. O que Você Mudou em Relação à Ferramenta do Expert

A ferramenta original do Expert tratava-se de um modelo linear de projeção simples. A nova versão transformou a ferramenta em um **simulador analítico completo de suporte à tomada de decisão**:

```text
┌──────────────────────────────────────┐          ┌────────────────────────────────────────────────────────┐
│         FERRAMENTA DO EXPERT         │          │                  FERRAMENTA EVOLUÍDA                   │
├──────────────────────────────────────┤          ├────────────────────────────────────────────────────────┤
│ • Projeção estática com taxa única   │          │ • Análise de Sensibilidade Tripla (0,6%, 0,8%, 1,0%)   │
│ • Sentido único (Aporte -> Montante) │  ===>    │ • Engenharia Reversa de Metas (Funções PGTO e NPER)    │
│ • Sem comparativo de mercado         │          │ • Benchmark Inteligente: FIIs vs. CDI Líquido de IR    │
│ • Visão condensada do patrimônio     │          │ • Decomposição: Aportado do Bolso vs. Juros Recebidos  │
│ • Fórmulas com referências fixas     │          │ • Governança, Validação de Dados e Layout Executivo    │
└──────────────────────────────────────┘          └────────────────────────────────────────────────────────┘
```

| Funcionalidade / Aspecto | Ferramenta do Expert (Original) | Nossa Versão Evoluída (Nova) | Valor Agregado para a Decisão |
| :--- | :--- | :--- | :--- |
| **Cenários de Rentabilidade** | Usava apenas uma taxa estática (1,079% a.m.). | Implementou **3 cenários de sensibilidade**: Conservador (0,6%), Moderado (0,8%) e Otimista (1,0%). | Demonstra ao investidor o impacto da oscilação de proventos e ciclos de mercado. |
| **Decomposição do Resultado** | Mostrava apenas o patrimônio total acumulado. | Separa claramente o **Total Aportado do Bolso** do **Total Recebido em Proventos / Juros**. | Evidencia o efeito exponencial dos juros compostos ("dinheiro trabalhando pelo investidor"). |
| **Simulador de Metas (Cálculo Inverso)** | Não possuía (apenas calculava do aporte para o patrimônio). | Adicionou módulo reverso: usuário digita a **Renda Desejada** e a ferramenta calcula o **Aporte Mensal Necessário** via **`PGTO`**. | Alinha o modelo aos objetivos reais de vida e aposentadoria do usuário. |
| **Benchmark de Renda Fixa (CDI)** | Não comparava com nenhum indicador externo. | Integra comparativo com o **CDI Líquido**, aplicando alíquota regressiva de 15% de IR e calculando o ganho excedente dos FIIs. | Responde se o prêmio de risco dos FIIs supera a Renda Fixa tradicional. |
| **Arquitetura de Dados e Usabilidade** | Referências rígidas de células e interface básica. | Padronização com listas suspensas de validação, formatação condicional e blocos analíticos modulares. | Facilidade de navegação, prevenção contra erros de digitação e apresentação executiva. |

---

## 🛠️ Resumo das Fórmulas-Chave Utilizadas

* **Valor Futuro (Patrimônio):**
  \[VF = \text{VF}(\text{taxa}; \text{anos} \times 12; -\text{aporte})\]
* **Dividendo Mensal:**
  \[\text{Dividendo} = \text{Patrimônio Acumulado} \times \text{Taxa Mensal}\]
* **Taxa Mensal Equivalente do CDI Líquido:**
  \[\text{Taxa Mensal CDI} = \left(1 + \text{CDI Anual} \times (1 - \text{Alíquota IR})\right)^{1/12} - 1\]
* **Aporte Mensal para Meta (PGTO):**
  \[\text{Aporte Necessário} = \text{PGTO}\left(\text{taxa}; \text{anos} \times 12; 0; -\left(\frac{\text{Renda Desejada}}{\text{taxa}}\right)\right)\]


