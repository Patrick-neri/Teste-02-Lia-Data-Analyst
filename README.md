# 📊 Teste 02 - Lia Data Analyst  
**Segundo teste para vaga de Data Analyst**  

---

## 🔗 Gist  
Você pode acessar o código diretamente no Gist:  
[👉 Clique aqui para acessar](https://gist.github.com/Patrick-neri/8a9b28b7695ba509665a4c86e3ba77be)  

---

## 📝 Descrição  

Para cada departamento, realize uma consulta em PostgresSQL que mostre o nome do departamento, a quantidade de empregados, a média salarial, o maior e o menor salários. Ordene o resultado pela maior média salarial.

Dicas: você pode usar a função COALESCE(value , 0) para substituir um valor nulo por zero e pode usar a função ROUND(value, 2) para mostrar valores com duas casas decimais.


---

## 🛠️ Código SQL  

```sql
-- Consulta para retornar para cada departamento: 
-- média salarial, maior salário, menor salário e quantidade de empregados por departamento.

WITH salarios AS (
    SELECT 
        SUM(v.valor) AS salario, 
        ev.matr AS num_matr
    FROM vencimento v 
    JOIN emp_venc ev ON ev.cod_venc = v.cod_venc
    GROUP BY ev.matr
    ORDER BY ev.matr
),

calculo_salarios AS (
    SELECT 
        salario, 
        num_matr, 
        e.matr AS matricula_emp, 
        e.lotacao AS depto_empregado
    FROM salarios
    JOIN empregado e ON num_matr = e.matr
    GROUP BY matricula_emp, salario, num_matr, e.lotacao
    ORDER BY num_matr
)

SELECT 
    ROUND(AVG(salario), 2) AS media_salarial, 
    ROUND(MAX(salario), 2) AS maior_salario, 
    ROUND(MIN(salario), 2) AS menor_salario, 
    COUNT(depto_empregado) AS qtde_empregados, 
    d.nome AS nome_departamento
FROM calculo_salarios
JOIN departamento d ON depto_empregado = d.cod_dep
GROUP BY depto_empregado, d.nome;
