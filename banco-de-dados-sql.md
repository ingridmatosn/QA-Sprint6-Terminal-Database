# Banco de dados — consultas SQL

Consultas em PostgreSQL sobre a base **Chicago Taxi Trips**, com as tabelas `cabs`, `trips` e `weather_records`.

## Tarefa 1 — Total de táxis cadastrados

```sql
SELECT
    COUNT(*) AS count
FROM
    cabs;
```

Resultado: **5.529 táxis**.

## Tarefa 2 — Empresas com menos de 100 carros

```sql
SELECT
    COUNT(*) AS cnt,
    company_name
FROM
    cabs
GROUP BY
    company_name
HAVING
    COUNT(*) < 100
ORDER BY
    cnt DESC;
```

Resultado: **51 empresas**. As dez primeiras:

| Carros | Empresa |
|---|---|
| 97 | Nova Taxi Affiliation Llc |
| 89 | Patriot Taxi Dba Peace Taxi Associat |
| 85 | Blue Diamond |
| 81 | Checker Taxi Affiliation |
| 80 | Chicago Medallion Management |
| 69 | Chicago Independents |
| 67 | 24 Seven Taxi |
| 60 | Checker Taxi |
| 55 | American United |
| 53 | Chicago Medallion Leasing INC |

A cauda da lista é formada por empresas de um único carro, o que mostra um mercado bastante pulverizado.

## Tarefa 3 — Classificar o clima por hora em Good/Bad

```sql
SELECT
    ts,
    CASE
        WHEN description LIKE '%rain%'
            OR description LIKE '%storm%'
        THEN 'Bad'
        ELSE 'Good'
    END AS weather_conditions
FROM
    weather_records
WHERE
    ts >= '2017-11-05 00:00:00'
    AND ts <= '2017-11-05 23:59:59';
```

Resultado: **24 registros**, um por hora do dia 05/11/2017. Amostra:

```
         ts          | weather_conditions
---------------------+--------------------
 2017-11-05 00:00:00 | Good
 2017-11-05 01:00:00 | Bad
 2017-11-05 02:00:00 | Good
 2017-11-05 03:00:00 | Good
 2017-11-05 04:00:00 | Bad
 2017-11-05 05:00:00 | Bad
```

## Tarefa 4 — Corridas por empresa em um período

```sql
SELECT
    cabs.company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM
    cabs
INNER JOIN trips
    ON cabs.cab_id = trips.cab_id
WHERE
    trips.start_ts >= '2017-11-15 00:00:00'
    AND trips.start_ts <= '2017-11-16 23:59:59'
GROUP BY
    cabs.company_name
ORDER BY
    trips_amount DESC;
```

Resultado: **64 empresas**. As dez primeiras:

| Empresa | Corridas |
|---|---|
| Flash Cab | 19.558 |
| Taxi Affiliation Services | 11.422 |
| Medallion Leasin | 10.367 |
| Yellow Cab | 9.888 |
| Taxi Affiliation Service Yellow | 9.299 |
| Chicago Carriage Cab Corp | 9.181 |
| City Service | 8.448 |
| Sun Taxi | 7.701 |
| Star North Management LLC | 7.455 |
| Blue Ribbon Taxi Association Inc. | 5.953 |

## Técnicas SQL aplicadas

| Técnica | Onde foi usada |
|---|---|
| `COUNT(*)` | Total de táxis cadastrados |
| `GROUP BY` + `HAVING` | Filtrar empresas por quantidade de carros |
| `CASE WHEN` + `LIKE` | Classificar o clima em Good/Bad a partir da descrição |
| `INNER JOIN` | Cruzar `cabs` com `trips` para contar corridas por empresa |
| `WHERE` com intervalo de datas | Recortar o período analisado |
| `ORDER BY` | Ordenar do maior para o menor volume |

