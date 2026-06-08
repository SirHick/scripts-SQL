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


________________________________________________________________________________________________________

(27/05)

use HospitalDB;

SELECT * FROM `Medicos`;    

-- =========================================================
-- POPULAÇÃO DO BANCO HospitalDB
-- =========================================================

USE HospitalDB;

-- =========================================================
-- HOSPITAIS
-- =========================================================

INSERT INTO Hospitais
(nome, cidade, estado, endereco)
VALUES
('Hospital São Lucas', 'Joinville', 'SC', 'Rua das Flores, 120'),
('Hospital Vida Nova', 'Curitiba', 'PR', 'Av. Central, 450'),
('Hospital Santa Clara', 'Florianópolis', 'SC', 'Rua Beira Mar, 900'),
('Hospital Esperança', 'Blumenau', 'SC', 'Rua XV de Novembro, 88'),
('Hospital Saúde Total', 'Porto Alegre', 'RS', 'Av. Brasil, 300');

-- =========================================================
-- ESPECIALIDADES
-- =========================================================

INSERT INTO Especialidades
(nome)
VALUES
('Cardiologia'),
('Ortopedia'),
('Pediatria'),
('Neurologia'),
('Clínico Geral');

-- =========================================================
-- MÉDICOS
-- =========================================================

INSERT INTO Medicos
(nome, crm, telefone, email, salario,
 id_especialidade, id_hospital)
VALUES
('Carlos Mendes', 'CRM1001', '47999990001',
 'carlos@hospital.com', 18000.00, 1, 1),

('Fernanda Lima', 'CRM1002', '47999990002',
 'fernanda@hospital.com', 22000.00, 2, 1),

('Ricardo Alves', 'CRM1003', '41999990003',
 'ricardo@hospital.com', 19500.00, 3, 2),

('Patricia Souza', 'CRM1004', '48999990004',
 'patricia@hospital.com', 25000.00, 4, 3),

('Juliana Costa', 'CRM1005', '47999990005',
 'juliana@hospital.com', 17000.00, 5, 4);

-- =========================================================
-- PACIENTES
-- =========================================================

INSERT INTO Pacientes
(nome, cpf, data_nascimento,
 telefone, email, endereco,
 tipo_sanguineo, alergias)
VALUES
('João Pedro', '11111111111',
 '2000-05-10', '47988880001',
 'joao@email.com',
 'Rua Azul, 100',
 'O+', 'Dipirona'),

('Mariana Silva', '22222222222',
 '1995-08-15', '47988880002',
 'mariana@email.com',
 'Rua Verde, 220',
 'A+', 'Nenhuma'),

('Lucas Almeida', '33333333333',
 '1988-12-01', '47988880003',
 'lucas@email.com',
 'Rua Vermelha, 50',
 'B-', 'Amendoim'),

('Ana Costa', '44444444444',
 '2002-03-20', '47988880004',
 'ana@email.com',
 'Rua das Palmeiras, 77',
 'AB+', 'Penicilina'),

('Bruno Rocha', '55555555555',
 '1990-11-11', '47988880005',
 'bruno@email.com',
 'Av. Central, 500',
 'O-', 'Nenhuma');

-- =========================================================
-- CONVÊNIOS
-- =========================================================

INSERT INTO Convenios
(nome, telefone, cobertura)
VALUES
('Unimed', '0800111111',
 'Consultas e exames'),

('Bradesco Saúde', '0800222222',
 'Internações e cirurgias'),

('SulAmérica', '0800333333',
 'Cobertura nacional'),

('Amil', '0800444444',
 'Consultas, exames e internações'),

('Particular', '0800555555',
 'Sem cobertura');

-- =========================================================
-- PACIENTE CONVÊNIO
-- =========================================================

INSERT INTO PacienteConvenio
(id_paciente, id_convenio, numero_carteira)
VALUES
(1,1,'UNI123456'),
(2,2,'BRA987654'),
(3,3,'SUL456789'),
(4,4,'AMI654321'),
(5,5,'PAR111222');

-- =========================================================
-- CONSULTAS
-- =========================================================

INSERT INTO Consultas
(data_consulta, diagnostico,
 observacoes, valor,
 id_paciente, id_medico)
VALUES
('2025-08-01 10:00:00',
 'Hipertensão',
 'Paciente apresentou pressão elevada',
 250.00, 1, 1),

('2025-08-02 14:00:00',
 'Fratura no braço',
 'Necessário imobilização',
 450.00, 2, 2),

('2025-08-03 09:30:00',
 'Gripe',
 'Repouso recomendado',
 180.00, 3, 3),

('2025-08-04 16:00:00',
 'Enxaqueca',
 'Solicitado exame complementar',
 320.00, 4, 4),

('2025-08-05 11:15:00',
 'Check-up',
 'Paciente saudável',
 200.00, 5, 5);

-- =========================================================
-- MEDICAMENTOS
-- =========================================================

INSERT INTO Medicamentos
(nome, fabricante, estoque, preco)
VALUES
('Dipirona', 'EMS', 150, 12.50),
('Amoxicilina', 'Medley', 80, 35.90),
('Paracetamol', 'Neo Química', 200, 8.90),
('Ibuprofeno', 'Cimed', 120, 15.40),
('Omeprazol', 'Eurofarma', 90, 22.00);

-- =========================================================
-- RECEITAS
-- =========================================================

INSERT INTO Receitas
(id_consulta, data_receita, observacoes)
VALUES
(1, '2025-08-01', 'Tomar após refeições'),
(2, '2025-08-02', 'Uso por 7 dias'),
(3, '2025-08-03', 'Uso contínuo'),
(4, '2025-08-04', 'Evitar esforço físico'),
(5, '2025-08-05', 'Apenas se necessário');

-- =========================================================
-- RECEITA MEDICAMENTO
-- =========================================================

INSERT INTO ReceitaMedicamento
(id_receita, id_medicamento,
 dosagem, frequencia)
VALUES
(1,1,'500mg','8 em 8 horas'),
(2,2,'1 cápsula','12 em 12 horas'),
(3,3,'750mg','6 em 6 horas'),
(4,4,'400mg','8 em 8 horas'),
(5,5,'20mg','1 vez ao dia');

-- =========================================================
-- EXAMES
-- =========================================================

INSERT INTO Exames
(nome, resultado, data_exame,
 id_paciente, id_medico)
VALUES
('Hemograma',
 'Sem alterações',
 '2025-08-01', 1, 1),

('Raio-X',
 'Fratura confirmada',
 '2025-08-02', 2, 2),

('Teste Covid',
 'Negativo',
 '2025-08-03', 3, 3),

('Ressonância',
 'Sem anomalias',
 '2025-08-04', 4, 4),

('Check-up sanguíneo',
 'Normal',
 '2025-08-05', 5, 5);

-- =========================================================
-- QUARTOS
-- =========================================================

INSERT INTO Quartos
(numero, tipo, capacidade,
 status_quarto, id_hospital)
VALUES
('101', 'UTI', 1, 'Ocupado', 1),
('102', 'Enfermaria', 2, 'Livre', 1),
('201', 'Apartamento', 1, 'Livre', 2),
('202', 'UTI', 1, 'Manutenção', 3),
('301', 'Enfermaria', 3, 'Ocupado', 4);

-- =========================================================
-- INTERNAÇÕES
-- =========================================================

INSERT INTO Internacoes
(data_entrada, data_saida,
 motivo, id_paciente, id_quarto)
VALUES
('2025-08-01 08:00:00',
 '2025-08-05 10:00:00',
 'Cirurgia cardíaca', 1, 1),

('2025-08-02 09:00:00',
 '2025-08-06 14:00:00',
 'Fratura grave', 2, 2),

('2025-08-03 10:00:00',
 NULL,
 'Observação clínica', 3, 3),

('2025-08-04 12:00:00',
 '2025-08-07 09:00:00',
 'Crise de enxaqueca', 4, 4),

('2025-08-05 07:00:00',
 NULL,
 'Exames complementares', 5, 5);

-- =========================================================
-- SETORES
-- =========================================================

INSERT INTO Setores
(nome)
VALUES
('Recepção'),
('TI'),
('Financeiro'),
('Enfermagem'),
('Administração');

-- =========================================================
-- FUNCIONÁRIOS
-- =========================================================

INSERT INTO Funcionarios
(nome, cpf, cargo,
 salario, id_setor, id_hospital)
VALUES
('Roberto Silva', '66666666666',
 'Recepcionista', 3200.00, 1, 1),

('Camila Souza', '77777777777',
 'Analista de TI', 5500.00, 2, 1),

('Paulo Mendes', '88888888888',
 'Auxiliar Financeiro', 4200.00, 3, 2),

('Juliana Alves', '99999999999',
 'Enfermeira', 6200.00, 4, 3),

('Felipe Rocha', '10101010101',
 'Administrador', 7500.00, 5, 4);

-- =========================================================
-- PAGAMENTOS
-- =========================================================

INSERT INTO Pagamentos
(valor, data_pagamento,
 forma_pagamento, id_consulta)
VALUES
(250.00, '2025-08-01', 'Cartão', 1),
(450.00, '2025-08-02', 'PIX', 2),
(180.00, '2025-08-03', 'Dinheiro', 3),
(320.00, '2025-08-04', 'Cartão', 4),
(200.00, '2025-08-05', 'PIX', 5);

-- =========================================================
-- USUÁRIOS DO SISTEMA
-- =========================================================

INSERT INTO UsuariosSistema
(usuario, senha_hash, nivel_acesso)
VALUES
('admin',
'123hashadmin',
'Administrador'),

('medico01',
'123hashmedico',
'Médico'),

('recepcao01',
'123hashrecepcao',
'Recepção'),

('financeiro01',
'123hashfinanceiro',
'Financeiro'),

('ti01',
'123hashti',
'TI');

-- =========================================================
-- LOGS
-- =========================================================

INSERT INTO Logs
(acao, data_log, id_usuario)
VALUES
('Login no sistema',
 '2025-08-01 08:00:00', 1),

('Cadastro de paciente',
 '2025-08-01 08:10:00', 3),

('Atualização de consulta',
 '2025-08-02 14:30:00', 2),

('Registro de pagamento',
 '2025-08-03 16:00:00', 4),

('Backup do banco executado',
 '2025-08-04 23:00:00', 5);

SELECT * FROM Pacientes
where tipo_sanguineo =  'A+';

SELECT Medicos.nome,
        Medicos.crm,
        Especialidades.nome,
        Hospitais.nome,
        COUNT(Consultas.id_consulta)
FROM Medicos, Especialidades, Hospitais, `Consultas`
WHERE Medicos.id_especialidade = Especialidades.id_especialidade
and Medicos.id_hospital = 2
and Hospitais.id_hospital = Medicos.id_hospital
and Consultas.id_medico = Medicos.id_medico
GROUP BY Medicos.nome, Medicos.crm, Especialidades.nome, Hospitais.nome;

SELECT M.nome,
M.crm,
E.nome,
MAX(M.salario)
FROM `Medicos` AS M
JOIN `Especialidades` AS E
ON M.id_especialidade = E.id_especialidade
GROUP BY M.nome, M.crm, E.id_especialidade
ORDER BY MAX(M.salario);

SELECT M.nome,
M.salario,
E.nome,
M.telefone,
H.nome as hp
FROM `Medicos` AS M
JOIN `Especialidades` AS E ON M.id_especialidade = E.id_especialidade
JOIN `Hospitais` as H on M.id_medico = `H`.id_hospital;

SELECT M.nome, M.email, E.nome, COUNT(C.id_consulta), M.salario
FROM Medicos AS M 
JOIN Especialidades AS E 
ON M.id_especialidade = E.id_especialidade
JOIN Consultas AS C 
ON M.id_medico = C.id_medico
GROUP BY M.id_medico;

SELECT M.nome, M.email, M.telefone, E.nome as especialidade, H.nome as Hospital, COUNT(Exames.id_exame) as Exames_solicitados
FROM Medicos as M
JOIN Especialidades AS E ON M.id_especialidade = E.id_especialidade
JOIN Hospitais AS H ON M.id_hospital = H.id_hospital
JOIN Exames ON M.id_medico = Exames.id_medico
GROUP BY M.id_medico;

(08/06)
use EscolaDB;

-- ======== Questões Básicas — Subquery como filtro ========

-- Liste os alunos que possuem a maior idade cadastrada.
SELECT * FROM `Alunos`
WHERE idade = (SELECT MAX(idade) FROM `Alunos`);

-- Exiba os alunos que possuem idade menor que a média das idades.
SELECT nome, idade FROM `Alunos`
WHERE idade < (SELECT AVG(idade) FROM `Alunos`);

-- Mostre os cursos que possuem a maior carga horária.
SELECT * FROM `Cursos`
WHERE carga_horaria = (SELECT MAX(carga_horaria) FROM `Cursos`);

-- Liste os alunos que possuem nota igual à maior nota registrada nas matrículas.
SELECT a.*, m.nota
FROM `Alunos` a
JOIN `Matriculas` m ON a.id_aluno = m.id_aluno
WHERE m.nota = (SELECT MAX(nota) FROM `Matriculas`);

-- Exiba os alunos que possuem nota menor que a média geral das notas.

SELECT a.nome, m.nota
FROM `Alunos` a
JOIN `Matriculas` m ON a.id_aluno = m.id_aluno
WHERE m.nota < (SELECT AVG(nota) FROM `Matriculas`);

-- Mostre os cursos cuja carga horária seja maior que a média das cargas horárias.

SELECT `Cursos`.carga_horaria, nome_curso
FROM `Cursos`
WHERE carga_horaria > (SELECT AVG(carga_horaria) FROM `Cursos`);

-- Liste os alunos que possuem exatamente a menor idade cadastrada.

SELECT `Alunos`.nome, idade
FROM `Alunos`
WHERE idade = (SELECT MIN(idade) FROM `Alunos`);

-- Exiba as matrículas cuja quantidade de faltas seja maior que a média de faltas.

SELECT `Matriculas`.id_matricula, Alunos.nome,`Matriculas`.faltas
FROM `Matriculas`, `Alunos`
WHERE faltas > (SELECT AVG(faltas) FROM `Matriculas`);

-- Mostre os cursos que possuem carga horária diferente da maior carga horária.

SELECT * FROM `Cursos`
WHERE carga_horaria <> (SELECT MAX(carga_horaria) FROM `Cursos`);

-- Liste os alunos que possuem nota igual à menor nota registrada.

SELECT * FROM `Matriculas`
WHERE nota = (SELECT MIN(nota) FROM `Matriculas`);


-- ============= Questões Intermediárias — Subquery com IN ==================

-- Liste os nomes dos alunos que possuem matrícula cadastrada.
SELECT nome FROM `Alunos`
WHERE id_aluno IN (SELECT id_aluno FROM `Matriculas`);

-- Exiba os cursos que possuem alunos matriculados.
SELECT * FROM `Cursos`
WHERE id_curso IN (SELECT id_curso FROM `Matriculas`);

-- Mostre os alunos que estão matriculados no curso “Python”.
SELECT * FROM `Alunos`
WHERE id_aluno IN (
    SELECT id_aluno FROM `Matriculas` 
    WHERE id_curso IN (SELECT id_curso FROM `Cursos` WHERE nome_curso = 'Python')
);

-- Liste os alunos matriculados em cursos com carga horária maior que 60 horas.
SELECT * FROM `Alunos`
WHERE id_aluno IN (
    SELECT id_aluno FROM `Matriculas` 
    WHERE id_curso IN (SELECT id_curso FROM `Cursos` WHERE carga_horaria > 60)
);

-- Exiba os cursos nos quais existem alunos com nota maior que 8.
SELECT * FROM `Cursos`
WHERE id_curso IN (SELECT id_curso FROM `Matriculas` WHERE nota > 8);

-- Mostre os alunos que possuem mais de uma matrícula.
SELECT * FROM `Alunos`
WHERE id_aluno IN (
    SELECT id_aluno FROM `Matriculas` 
    GROUP BY id_aluno 
    HAVING COUNT(*) > 1
);

-- Liste os cursos que NÃO possuem matrículas cadastradas.
SELECT * FROM `Cursos`
WHERE id_curso NOT IN (SELECT id_curso FROM `Matriculas`);

-- Exiba os alunos que possuem faltas maiores que 5 em alguma matrícula.
SELECT * FROM `Alunos`
WHERE id_aluno IN (SELECT id_aluno FROM `Matriculas` WHERE faltas > 5);

-- Mostre os cursos frequentados por alunos da cidade de Curitiba.
SELECT * FROM `Cursos`
WHERE id_curso IN (
    SELECT id_curso FROM `Matriculas` 
    WHERE id_aluno IN (SELECT id_aluno FROM `Alunos` WHERE cidade = 'Curitiba')
);

-- Liste os alunos matriculados no curso com maior carga horária.
SELECT * FROM `Alunos`
WHERE id_aluno IN (
    SELECT id_aluno FROM `Matriculas` 
    WHERE id_curso IN (SELECT id_curso FROM `Cursos` WHERE carga_horaria = (SELECT MAX(carga_horaria) FROM `Cursos`))
);


-- ============= Questões Avançadas — Subquery com operadores de comparação ==================

-- Exiba os alunos cuja idade seja maior que a média de idade dos alunos de São Paulo.
SELECT * FROM `Alunos`
WHERE idade > (SELECT AVG(idade) FROM `Alunos` WHERE cidade = 'São Paulo');

-- Liste os cursos cuja média de notas seja maior que a média geral das notas.
SELECT c.* FROM `Cursos` c
WHERE (SELECT AVG(nota) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) > (SELECT AVG(nota) FROM `Matriculas`);

-- Mostre os alunos cuja soma de faltas seja maior que a média total de faltas registradas.
SELECT a.* FROM `Alunos` a
WHERE (SELECT SUM(faltas) FROM `Matriculas` m WHERE m.id_aluno = a.id_aluno) > (SELECT AVG(faltas) FROM `Matriculas`);

-- Exiba os cursos cuja maior nota registrada seja igual à maior nota do sistema.
SELECT c.* FROM `Cursos` c
WHERE (SELECT MAX(nota) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) = (SELECT MAX(nota) FROM `Matriculas`);

-- Liste os alunos cuja média de notas seja menor que a média geral dos alunos.
SELECT a.* FROM `Alunos` a
WHERE (SELECT AVG(nota) FROM `Matriculas` m WHERE m.id_aluno = a.id_aluno) < (SELECT AVG(nota) FROM `Matriculas`);

-- Mostre os cursos cuja quantidade de matrículas seja maior que a média de matrículas por curso.
SELECT c.* FROM `Cursos` c
WHERE (SELECT COUNT(*) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) > (SELECT COUNT(*) / COUNT(DISTINCT id_curso) FROM `Matriculas`);

-- Exiba os alunos que possuem nota maior que todas as notas do curso “Banco de Dados”.
SELECT a.* FROM `Alunos` a
JOIN `Matriculas` m ON a.id_aluno = m.id_aluno
WHERE m.nota > ALL (
    SELECT m2.nota FROM `Matriculas` m2 
    JOIN `Cursos` c ON m2.id_curso = c.id_curso 
    WHERE c.nome_curso = 'Banco de Dados'
);

-- Liste os cursos cuja menor nota seja maior que a média geral das menores notas dos cursos.
SELECT c.* FROM `Cursos` c
WHERE (SELECT MIN(nota) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) > (
    SELECT AVG(min_nota) FROM (SELECT MIN(nota) AS min_nota FROM `Matriculas` GROUP BY id_curso) AS subquery
);

-- Mostre os alunos cuja idade seja igual à idade média dos alunos.
SELECT * FROM `Alunos`
WHERE idade = (SELECT AVG(idade) FROM `Alunos`);

-- Exiba os cursos cuja carga horária seja menor que a maior carga horária cadastrada.
SELECT * FROM `Cursos`
WHERE carga_horaria < (SELECT MAX(carga_horaria) FROM `Cursos`);


-- ============= Questões — Subquery como nova coluna ==================

-- Liste os alunos e exiba ao lado a quantidade total de matrículas de cada aluno.
SELECT nome, (SELECT COUNT(*) FROM `Matriculas` m WHERE m.id_aluno = a.id_aluno) AS total_matriculas 
FROM `Alunos` a;

-- Exiba os cursos e mostre ao lado a média das notas de cada curso.
SELECT nome_curso, (SELECT AVG(nota) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) AS media_notas 
FROM `Cursos` c;

-- Liste os alunos e mostre a soma total de faltas de cada um.
SELECT nome, (SELECT SUM(faltas) FROM `Matriculas` m WHERE m.id_aluno = a.id_aluno) AS total_faltas 
FROM `Alunos` a;

-- Exiba os cursos e mostre quantos alunos estão matriculados em cada curso.
SELECT nome_curso, (SELECT COUNT(id_aluno) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) AS total_alunos 
FROM `Cursos` c;

-- Liste os alunos e apresente sua maior nota registrada.
SELECT nome, (SELECT MAX(nota) FROM `Matriculas` m WHERE m.id_aluno = a.id_aluno) AS maior_nota 
FROM `Alunos` a;

-- Exiba os cursos e mostre a menor nota registrada em cada curso.
SELECT nome_curso, (SELECT MIN(nota) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) AS menor_nota 
FROM `Cursos` c;

-- Liste os alunos e mostre a média de notas de cada um em uma nova coluna chamada Media_Aluno.
SELECT nome, (SELECT AVG(nota) FROM `Matriculas` m WHERE m.id_aluno = a.id_aluno) AS Media_Aluno 
FROM `Alunos` a;

-- Exiba os cursos e apresente o total de faltas registradas em cada curso.
SELECT nome_curso, (SELECT SUM(faltas) FROM `Matriculas` m WHERE m.id_curso = c.id_curso) AS total_faltas 
FROM `Cursos` c;

-- Liste os alunos e mostre a quantidade de cursos diferentes em que estão matriculados.
SELECT nome, (SELECT COUNT(DISTINCT id_curso) FROM `Matriculas` m WHERE m.id_aluno = a.id_aluno) AS total_cursos 
FROM `Alunos` a;

-- Exiba os cursos e mostre a quantidade de alunos aprovados (nota maior ou igual a 7).
SELECT nome_curso, (SELECT COUNT(*) FROM `Matriculas` m WHERE m.id_curso = c.id_curso AND m.nota >= 7) AS total_aprovados 
FROM `Cursos` c;


-- ============= Questões Desafio — Misturando GROUP BY + HAVING + SUBQUERY ==================

-- Liste as cidades cuja média de idade seja maior que a média geral de idade dos alunos.
SELECT cidade 
FROM `Alunos` 
GROUP BY cidade 
HAVING AVG(idade) > (SELECT AVG(idade) FROM `Alunos`);

-- Exiba os cursos cuja média de notas seja maior que a média das médias dos cursos.
SELECT c.nome_curso
FROM `Cursos` c
JOIN `Matriculas` m ON c.id_curso = m.id_curso
GROUP BY c.id_curso, c.nome_curso
HAVING AVG(m.nota) > (
    SELECT AVG(media_curso) FROM (SELECT AVG(nota) AS media_curso FROM `Matriculas` GROUP BY id_curso) AS subquery
);

-- Mostre os alunos cuja soma de faltas seja maior que a soma média de faltas dos alunos.
SELECT a.nome
FROM `Alunos` a
JOIN `Matriculas` m ON a.id_aluno = m.id_aluno
GROUP BY a.id_aluno, a.nome
HAVING SUM(m.faltas) > (
    SELECT AVG(soma_faltas) FROM (SELECT SUM(faltas) AS soma_faltas FROM `Matriculas` GROUP BY id_aluno) AS subquery
);

-- Liste os cursos que possuem quantidade de matrículas acima da média de matrículas por curso.
SELECT c.nome_curso 
FROM `Cursos` c
JOIN `Matriculas` m ON c.id_curso = m.id_curso
GROUP BY c.id_curso, c.nome_curso
HAVING COUNT(*) > (SELECT COUNT(*) / COUNT(DISTINCT id_curso) FROM `Matriculas`);

-- Exiba os alunos cuja média de notas seja maior que a média dos alunos da cidade de São Paulo.
SELECT a.nome
FROM `Alunos` a
JOIN `Matriculas` m ON a.id_aluno = m.id_aluno
GROUP BY a.id_aluno, a.nome
HAVING AVG(m.nota) > (
    SELECT AVG(m2.nota) FROM `Matriculas` m2 JOIN `Alunos` a2 ON m2.id_aluno = a2.id_aluno WHERE a2.cidade = 'São Paulo'
);

-- Mostre os cursos cuja carga horária seja maior que a média das cargas horárias dos cursos com matrícula.
SELECT nome_curso 
FROM `Cursos` 
GROUP BY id_curso, nome_curso, carga_horaria
HAVING carga_horaria > (
    SELECT AVG(carga_horaria) FROM `Cursos` WHERE id_curso IN (SELECT DISTINCT id_curso FROM `Matriculas`)
);


-- Liste os alunos que possuem mais matrículas que a média de matrículas dos alunos.
SELECT a.nome 
FROM `Alunos` a
JOIN `Matriculas` m ON a.id_aluno = m.id_aluno
GROUP BY a.id_aluno, a.nome
HAVING COUNT(*) > (SELECT COUNT(*) / COUNT(DISTINCT id_aluno) FROM `Matriculas`);

-- Exiba os cursos cuja maior nota seja inferior à maior nota geral do sistema.
SELECT c.nome_curso 
FROM `Cursos` c
JOIN `Matriculas` m ON c.id_curso = m.id_curso
GROUP BY c.id_curso, c.nome_curso
HAVING MAX(m.nota) < (SELECT MAX(nota) FROM `Matriculas`);

-- Mostre os alunos cuja média de faltas seja menor que a média geral de faltas.
SELECT a.nome
FROM `Alunos` a
JOIN `Matriculas` m ON a.id_aluno = m.id_aluno
GROUP BY a.id_aluno, a.nome
HAVING AVG(m.faltas) < (SELECT AVG(faltas) FROM `Matriculas`);

-- Liste os cursos cuja quantidade de alunos aprovados seja maior que a média de aprovados dos cursos.
SELECT c.nome_curso
FROM `Cursos` c
JOIN `Matriculas` m ON c.id_curso = m.id_curso
WHERE m.nota >= 7
GROUP BY c.id_curso, c.nome_curso
HAVING COUNT(*) > (
    SELECT AVG(qtd_aprovados) FROM (SELECT COUNT(*) AS qtd_aprovados FROM `Matriculas` WHERE nota >= 7 GROUP BY id_curso) AS subquery
);
