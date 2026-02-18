# Crypto Data Pipeline – Data Analytics & GCP

Este projeto foi desenvolvido como parte de um desafio técnico com foco em **Data Analytics**, utilizando ferramentas gratuitas do ecossistema Google.

O objetivo é demonstrar a construção de um **pipeline de dados ponta a ponta**, desde a extração de dados em uma API pública até a visualização final em um dashboard interativo, seguindo boas práticas de organização, modelagem e consumo analítico.

---

## Links do projeto

- Notebook (Google Colab):  
  https://colab.research.google.com/drive/1VITq2MzmyiObGOpM0650vL5qZEh7Li6X

- Dashboard (Looker Studio):  
  https://lookerstudio.google.com/reporting/2c72146d-874e-4346-9b3c-3bf3200ee483

---

## Arquitetura da solução

API CoinGecko → Python (Google Colab) → BigQuery → Looker Studio

---

## Stack utilizada

- Python 3.x (Google Colab)
- API CoinGecko
- Google BigQuery (Sandbox)
- Looker Studio

---

## Ingestão de dados

Os dados são extraídos da API CoinGecko utilizando Python no Google Colab.  
O pipeline conta com tratamento de rate limit e retentativas automáticas, garantindo maior resiliência durante a coleta.

A carga é realizada de forma incremental no BigQuery, utilizando campos de controle como `snapshot_date` e `load_ts`, permitindo manter o histórico das execuções sem sobrescrita indevida.

---

## Modelagem no BigQuery

Os dados são armazenados e organizados no BigQuery seguindo uma arquitetura em camadas, separando claramente ingestão e consumo analítico.

### Camada RAW (`crypto_raw`)
- Armazena os dados brutos extraídos da API
- Mantém histórico completo das coletas
- Permite reexecução segura do pipeline
- Contém snapshots completos e cargas pontuais de correção histórica

### Camada ANALYTICS (`crypto_analytics`)
- Contém apenas views analíticas
- Regras de negócio aplicadas diretamente no BigQuery
- Dados prontos para consumo no Looker Studio
- Evita duplicação de dados e reduz a complexidade no BI

Principais views criadas:
- `vw_crypto_master_30d`
- `vw_kpis_latest`
- `vw_top10_mcap_latest`
- `vw_crypto_history_10d`
- `vw_top3_history_fixed`
- `vw_market_kpi_final`

O consumo das views no Looker Studio valida a correta modelagem e funcionamento do BigQuery, não sendo necessário o compartilhamento direto do projeto GCP.

---

## Dashboard

O dashboard foi estruturado em duas páginas, cada uma com um objetivo analítico distinto.

### Página 1 – Visão geral do mercado
Esta página tem como foco uma visão macro do mercado de criptomoedas, baseada no snapshot mais recente disponível.

Os principais elementos são:
- Capitalização total do mercado de criptomoedas (market cap global no momento da coleta)
- Ranking das 10 maiores criptomoedas por capitalização de mercado
- Variação percentual de preço nas últimas 24 horas das Top 10 moedas
- Análise histórica de preço das 3 principais criptomoedas (Bitcoin, Ethereum e Tether), por serem as mais representativas do mercado

O objetivo dessa página é oferecer um panorama rápido do estado atual do mercado e do comportamento das moedas mais relevantes.

### Página 2 – Análise detalhada por moeda
A segunda página é voltada para uma análise mais aprofundada, permitindo explorar individualmente cada uma das Top 10 criptomoedas por capitalização de mercado.

Nesta página é possível:
- Selecionar uma moeda específica entre as Top 10
- Analisar métricas individuais do ativo
- Visualizar o histórico de preços e variações
- Comparar o comportamento da moeda escolhida dentro do contexto do mercado

O foco desta página é permitir uma análise direcionada, saindo da visão macro e entrando no detalhe de cada ativo de forma isolada.

---

## Validações realizadas

Antes da entrega, foram realizadas validações para garantir a consistência do pipeline e dos dados:

- Conferência da ingestão incremental no BigQuery
- Validação de snapshots e histórico na camada RAW
- Verificação do retorno das views analíticas
- Testes de consistência e navegação no dashboard

---

## Possíveis evoluções

- Automatização do pipeline (Cloud Scheduler / Cloud Functions)
- Inclusão de métricas de volatilidade e indicadores adicionais
- Monitoramento de qualidade dos dados
- Ampliação do período histórico analisado

---

## Autor

Vinícius Correa  
Analista de Dados | Analytics | BI
