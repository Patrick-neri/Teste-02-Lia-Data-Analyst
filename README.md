# Teste-02-Lia-Data-Analyst
Segundo teste para vaga de Data Analyst

	
	--Consulta para retornar para cada departamento: média salarial, maior salário, menor salário e quantidade de empregados por departamento.
	
	with salarios as 
		(select sum(v.valor) as salario, ev.matr as num_matr
		from vencimento v 
		join emp_venc ev 
		on ev.cod_venc = v.cod_venc
		group by ev.matr
		order by ev.matr			
	),
	
		calculo_salarios as 
		(select salario, num_matr, e.matr as matricula_emp, e.lotacao as depto_empregado
		from salarios
		join empregado e 
		on num_matr=e.matr
		group by matricula_emp, salario, num_matr, e.lotacao
		order by num_matr
	)
	
	select round(AVG(salario),2) as media_salarial, round(MAX(salario),2) as maior_salario, round(MIN(salario),2) as menor_salario, count(depto_empregado) as qtde_empregados, d.nome as nome_departamento
	from calculo_salarios
	join departamento d 
	on depto_empregado=d.cod_dep
	group by depto_empregado , d.nome;
	
	
	
	
	
	
	
	
	
