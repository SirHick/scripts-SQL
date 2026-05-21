# scripts-SQL

(13/05)
Script do BD Estoque Comercial
CREATE DATABASE estoquecomercial;

use estoquecomercial;

CREATE TABLE ItensEstoque(
id_Item INT NOT NULL AUTO_INCREMENT,
descricaoItem VARCHAR(200),
setorItem VARCHAR(200),
precoVendaItem DOUBLE(9,2),
estoqueItem INT,
PRIMARY KEY (id_Item)
);

INSERT INTO ItensEstoque
(descricaoItem,setorItem,precoVendaItem,estoqueItem)VALUES
('Suco de Laranja','Bebidas','7.50',250),
('Macarrão 1kg','Alimentos','5.20',180),
('Sabão em pó','Limpeza','12.90',90),
('Café Torrado','Alimentos','15.80',120),
('Iogurte Natural','Laticínios','4.30',350),
('Biscoito Integral', NULL,'3.90', 210),
('Molho de Tomate','Alimentos','2.80',500); 

SELECT descricaoItem, setorItem, precoVendaItem from ItensEstoque
WHERE descricaoItem <> 'Iogurte Natural';

SELECT precoVendaItem, estoqueItem from ItensEstoque
WHERE descricaoItem = 'Biscoito Integral' AND setorItem is NULL;

SELECT descricaoItem, setorItem
FROM ItensEstoque
WHERE id_Item = 1 OR precoVendaItem > 30;

SELECT descricaoItem
FROM ItensEstoque
WHERE NOT precoVendaItem = 7.50;

SELECT * from ItensEstoque
WHERE descricaoItem BETWEEN 'A%' AND 'L%';

SELECT precoVendaItem 
FROM ItensEstoque
WHERE setorItem 
IN ('ALIMENTOS', 'LIMPEZA', 'PRAIA', 'BEBIDAS', 'LATICÍNIOS');

SELECT * FROM ItensEstoque
WHERE descricaoItem LIKE '%Tom%';

set @minha_idade = 22; -- Variáveis Temporárias
set @nome_produto = 'PC';
SELECT (precoVendaItem * @minha_idade) AS Total 
FROM ItensEstoque
WHERE precoVendaItem BETWEEN 1.00 AND @minha_idade;

SELECT * from ItensEstoque;

SELECT precoVendaItem, setorItem, estoqueItem 
FROM ItensEstoque
WHERE precoVendaItem > 4.00
ORDER BY precoVendaItem, setorItem, estoqueItem ASC; 

SELECT COUNT(descricaoItem)
FROM ItensEstoque
WHERE descricaoItem LIKE '%a%';

SELECT AVG(precoVendaItem) 
FROM ItensEstoque;

SELECT SUM(estoqueItem)
FROM ItensEstoque;

SELECT MIN(precoVendaItem)
FROM ItensEstoque;

SELECT MAX(precoVendaItem)
FROM ItensEstoque;

-- 1) Verificar quantidade de itens por setor;
-- 2) Verificar média valor produto por setor;
-- 3) Verificar valor min e max por setor;

-- 1
SELECT setorItem, COUNT(estoqueItem) AS totalPorSetor
FROM ItensEstoque
GROUP BY setorItem; 

-- 2
SELECT setorItem, AVG(precoVendaItem) as mediaPrecoSetor
FROM ItensEstoque
GROUP BY setorItem;

-- 3
SELECT setorItem, MIN(precoVendaItem) as MinValorSetor, 
    MAX(precoVendaItem) as MaxValorSetor
FROM ItensEstoque
GROUP BY setorItem;

********************************************************************

(20/05)
CREATE DATABASE EscolaDB;
USE EscolaDB;

CREATE TABLE Alunos (
    id_aluno INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100),
    cidade VARCHAR(100),
    idade INT
);

CREATE TABLE Cursos (
    id_curso INT PRIMARY KEY AUTO_INCREMENT,
    nome_curso VARCHAR(100),
    carga_horaria INT
);

CREATE TABLE Matriculas (
    id_matricula INT PRIMARY KEY AUTO_INCREMENT,
    id_aluno INT,
    id_curso INT,
    nota DECIMAL(4,2),
    faltas INT,
FOREIGN KEY (id_aluno) REFERENCES Alunos (id_aluno),
FOREIGN KEY (id_curso) REFERENCES Cursos (id_curso)
);

INSERT INTO Alunos (nome, cidade, idade)
VALUES
('Carlos','São Paulo',18),
('Mariana','Curitiba',22),
('João','Florianópolis',19),
('Fernanda','São Paulo',25),
('Lucas','Rio de Janeiro',20),
('Patricia','Curitiba',21),
('Ana','Porto Alegre',23),
('Bruno','São Paulo',24);

INSERT INTO Cursos (nome_curso, carga_horaria)
VALUES
('Python',40),
('Banco de Dados',60),
('Java',80),
('Data Science',100);

INSERT INTO Matriculas (id_aluno, id_curso, nota, faltas)
VALUES
(1,1,8.5,2),
(1,2,7.0,5),
(2,1,9.5,1),
(2,4,8.0,4),
(3,2,6.5,6),
(3,3,7.5,3),
(4,4,9.0,0),
(5,1,5.5,10),
(5,2,6.0,7),
(6,3,8.5,2),
(7,4,7.0,5),
(8,2,9.5,1);


-- Básicas --

SELECT * FROM Alunos;

SELECT nome FROM Alunos;

SELECT nome_curso FROM Cursos;

SELECT * FROM Alunos
WHERE cidade = 'São Paulo';

SELECT nome, idade FROM Alunos
WHERE idade > 20;

SELECT * FROM Cursos
WHERE carga_horaria > 50;

SELECT nome, idade FROM Alunos
WHERE idade BETWEEN 18 and 22;

SELECT nome, cidade FROM Alunos
WHERE cidade = 'Curitiba';

SELECT nome, idade FROM Alunos
WHERE idade < 21;

SELECT id_matricula FROM Matriculas;


-- Intermediárias --

SELECT nome, nota FROM Alunos
Join Matriculas
ON Alunos.id_aluno = Matriculas.id_aluno
WHERE nota > 8;

SELECT nome, faltas FROM Alunos
JOIN Matriculas
ON Alunos.id_aluno = Matriculas.id_aluno
WHERE faltas > 5;

SELECT nome_curso, carga_horaria FROM Cursos
Where carga_horaria = 80;

SELECT nome, cidade FROM Alunos
WHERE cidade <> 'São Paulo';

SELECT nome FROM Alunos
Where nome like 'A%';

SELECT nome FROM Alunos
Where nome like '%a';

SELECT nome_curso FROM Cursos
WHERE nome_curso like '%Dados%';

SELECT id_matricula, nota FROM Matriculas
WHERE nota BETWEEN 7 and 9;

SELECT nome, idade FROM Alunos
WHERE idade = 20;

SELECT nome_curso, carga_horaria FROM Cursos
WHERE carga_horaria <= 60;


-- Questões com GROUP BY --


SELECT cidade, COUNT(nome) as QtdAlunos 
FROM Alunos
GROUP BY cidade;

SELECT cidade, AVG(idade) as mediaIdade
FROM Alunos
GROUP BY cidade;

SELECT nome_curso, COUNT(Matriculas.id_curso) as QtdMatricula
FROM Cursos
Join Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso;

-- DROP DATABASE EscolaDB;

SELECT nome_curso, AVG(nota) as MediaNota
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso;

SELECT nome_curso, COUNT(faltas) as TotalFaltas
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso;

SELECT nome_curso, MAX(nota) as MaiorNota
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso;

SELECT nome_curso, Min(nota) as MenorNota
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso;

SELECT nome_curso, SUM(faltas) as SomaTotalFaltas
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso;

SELECT nome, AVG(nota) as MediaNotaAluno
FROM Alunos
JOIN Matriculas
ON Alunos.id_aluno = Matriculas.id_aluno    
GROUP BY nome;

SELECT nome, COUNT(idade) as FaixaEtaria
FROM Alunos   
GROUP BY nome;


-- Questões Avançadas — HAVING e ORDER BY --

-- Liste as cidades que possuem mais de 2 alunos.

SELECT cidade, COUNT(nome)
FROM Alunos
GROUP BY cidade
HAVING COUNT(nome) > 2;

-- Exiba os cursos cuja média de notas seja maior que 8.

SELECT nome_curso, AVG(nota) as media_nota
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso
HAVING AVG(nota) > 8;

-- Mostre os cursos que possuem mais de 2 matrículas.

SELECT nome_curso, COUNT(id_matricula)
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso
HAVING COUNT(id_matricula) > 2;

-- Liste os alunos cuja soma de faltas seja maior que 5.

SELECT nome_curso, SUM(faltas) as SomaFaltas
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso
HAVING SomaFaltas > 5;

-- Exiba os cursos cuja menor nota seja maior que 6.

SELECT nome_curso, MIN(nota)
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso
HAVING MIN(nota) > 6;

-- Mostre os cursos ordenados pela carga horária em ordem decrescente.

SELECT nome_curso, carga_horaria
FROM Cursos
ORDER BY carga_horaria DESC;

-- Liste os alunos ordenados por idade do maior para o menor.

SELECT nome, idade
FROM Alunos
ORDER BY idade DESC;

-- Exiba a média de notas por curso ordenada da maior para a menor.

SELECT nome_curso, AVG(nota)
FROM Cursos
JOIN Matriculas
ON Cursos.id_curso = Matriculas.id_curso
GROUP BY nome_curso
ORDER BY AVG(nota) DESC;

-- Mostre as cidades ordenadas pela quantidade de alunos.

SELECT cidade, COUNT(nome)
FROM Alunos
GROUP BY cidade
ORDER BY COUNT(nome) DESC;

-- Liste os alunos com média de notas maior que 7 ordenados pela média decrescente.

SELECT nome, AVG(nota)
FROM Alunos
JOIN Matriculas
ON Alunos.id_aluno = Matriculas.id_aluno
GROUP BY nome
HAVING AVG(nota) >  7
ORDER BY AVG(nota) DESC;
