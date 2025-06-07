# Análise de Corridas de Táxi e Clima em Chicago — 2017

Este projeto combina técnicas de **web scraping com Python** e **consultas SQL em PostgreSQL** para analisar a relação entre o clima e as corridas de táxi na cidade de Chicago ao longo de 2017.

## 🔍 Objetivo

Investigar como o clima influencia nas corridas de táxi, identificando padrões com base em:
- Volume de corridas por empresa
- Condições climáticas (chuva, tempestade etc.)
- Dias da semana e horários
- Localizações de embarque e desembarque

## 🧰 Ferramentas e Tecnologias

- **Python (BeautifulSoup, pandas)**: Extração e tratamento de dados meteorológicos a partir de uma página HTML.
- **PostgreSQL**: Consultas avançadas para análise de dados de corridas.
- **SQL JOINs, CASE, ILIKE, DATE functions**: Utilizadas para filtrar e agrupar os dados por condições específicas.

## 📄 Destaques das Consultas SQL

- Total de corridas por empresa em datas específicas.
- Filtro por empresas contendo "Yellow" ou "Blue".
- Agrupamento por categorias usando `CASE`.
- Classificação do clima como "Bom" ou "Ruim" com base na descrição.
- Relação entre clima, localização e duração das corridas.

## 📈 Exemplo de Análise

```sql
SELECT
  t.start_ts,
  CASE
    WHEN w.description LIKE '%rain%' OR w.description LIKE '%storm%' THEN 'Bad'
    ELSE 'Good'
  END AS weather_conditions,
  t.duration_seconds
FROM trips t
JOIN weather_records w ON t.start_ts = w.ts
WHERE
  t.pickup_location_id = 50 AND
 t.dropoff_location_id = 63 AND
  EXTRACT(DOW FROM t.start_ts) = 6 AND
  w.description IS NOT NULL
ORDER BY t.trip_id;
```
## 📎 Dados Utilizados

- **Clima**: Extraído via scraping da página HTML pública [Visualizar dados de clima (Chicago 2017)](https://practicum-content.s3.us-west-1.amazonaws.com/data-analyst-eng/moved_chicago_weather_2017.html)
- **Corridas**: Tabelas PostgreSQL fictícias com estrutura realista (`cabs`, `trips`, `weather_records` etc.)


 
