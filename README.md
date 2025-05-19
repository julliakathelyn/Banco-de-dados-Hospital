# 🏥 O Hospital Fundamental. 

Um pequeno hospital local busca desenvolver um novo sistema que atenda melhor às suas necessidades. Atualmente, parte da operação ainda se apoia em planilhas e arquivos antigos, mas espera-se que esses dados sejam transferidos para o novo sistema assim que ele estiver funcional. Neste momento, é necessário analisar com cuidado as necessidades desse cliente e sugerir uma estrutura de banco de dados adequada por meio de um Diagrama Entidade-Relacionamento.

### Mãos a Obra - Parte 1 :

Analise a seguinte descrição e extraia dela os requisitos para o banco de dados:
O hospital necessita de um sistema para sua área clínica que ajude a controlar consultas realizadas. Os médicos podem ser generalistas, especialistas ou residentes e têm seus dados pessoais cadastrados em planilhas digitais. Cada médico pode ter uma ou mais especialidades, que podem ser pediatria, clínica geral, gastroenterologia e dermatologia. Alguns registros antigos ainda estão em formulário de papel, mas será necessário incluir esses dados no novo sistema.

Os pacientes também precisam de cadastro, contendo dados pessoais (nome, data de nascimento, endereço, telefone e e-mail), documentos (CPF e RG) e convênio. Para cada convênio, são registrados nome, CNPJ e tempo de carência.

As consultas também têm sido registradas em planilhas, com data e hora de realização, médico responsável, paciente, valor da consulta ou nome do convênio, com o número da carteira. Também é necessário indicar na consulta qual a especialidade buscada pelo paciente.

Deseja-se ainda informatizar a receita do médico, de maneira que, no encerramento da consulta, ele possa registrar os medicamentos receitados, a quantidade e as instruções de uso. A partir disso, espera-se que o sistema imprima um relatório da receita ao paciente ou permita sua visualização via internet.

![teste de cores e design](DiagramaER-HospitalFundamental.drawio.png)

# 🏥 Os Segredos do Hospital

Após a primeira versão do projeto de banco de dados para o sistema hospitalar, notou-se a necessidade de expansão das funcionalidades, incluindo alguns requisitos essenciais a essa versão do software. As funcionalidades em questão são para o controle na internação de pacientes. Será necessário expandir o Modelo ER desenvolvido e montar o banco de dados, criando as tabelas para o início dos testes.

Entender do assunto
Considere a seguinte descrição e o diagrama ER abaixo:

No hospital, as internações têm sido registradas por meio de formulários eletrônicos que gravam os dados em arquivos. 

Para cada internação, são anotadas a data de entrada, a data prevista de alta e a data efetiva de alta, além da descrição textual dos procedimentos a serem realizados. 

As internações precisam ser vinculadas a quartos, com a numeração e o tipo. 

Cada tipo de quarto tem sua descrição e o seu valor diário (a princípio, o hospital trabalha com apartamentos, quartos duplos e enfermaria).

Também é necessário controlar quais profissionais de enfermaria estarão responsáveis por acompanhar o paciente durante sua internação. Para cada enfermeiro(a), é necessário nome, CPF e registro no conselho de enfermagem (COREN).

A internação, obviamente, é vinculada a um paciente – que pode se internar mais de uma vez no hospital – e a um único médico responsável.

![teste de cores e design](img-rreadme-hospotal/internacao.png)

Mãos a obra?
Faça a ligação do diagrama acima ao diagrama desenvolvido na atividade antrior, construindo relacionamentos com entidades relacionadas. E eleve o seu diagrama para que já selecionando os tipos de dados que serão trabalhados e em quais situações. 

Por último, crie um script SQL para a geração do banco de dados e para instruções de montagem de cada uma das entidades/tabelas presentes no diagrama completo (considerando as entidades do diagrama da atividade anterior e as novas entidades propostas no diagrama acima). Também crie tabelas para relacionamentos quando necessário. Aplique colunas e chaves primárias e estrangeiras.

![teste de cores e design](DiagramaER-Os-Segredos-do-Hospital.drawio.png)

# SQL
```CREATE DATABASE hospital;

CREATE TABLE Tipo_Medico (
    ID_Tipo_Medico INT PRIMARY KEY,
    descricao VARCHAR(50)
);


CREATE TABLE Especialidade (
    ID_Especialidade INT PRIMARY KEY,
    nome VARCHAR(50)
);


CREATE TABLE Medico (
    ID_Medico INT PRIMARY KEY,
    FK_Tipo_Medico INT,
    FK_Especialidade INT,
    CPF VARCHAR(14),
    RG VARCHAR(20),
    CRM VARCHAR(20),
    Nome VARCHAR(100),
    Data_Nascimento DATE,
    Telefone VARCHAR(20),
    FOREIGN KEY (FK_Tipo_Medico) REFERENCES Tipo_Medico(ID_Tipo_Medico),
    FOREIGN KEY (FK_Especialidade) REFERENCES Especialidade(ID_Especialidade)
);


CREATE TABLE Convenio (
    ID_Convenio INT PRIMARY KEY,
    Nome VARCHAR(100),
    CNPJ_CNPJ VARCHAR(20),
    Tempo_Carencia INT
);


CREATE TABLE Paciente (
    ID_Paciente INT PRIMARY KEY,
    FK_Convenio INT,
    Nome VARCHAR(100),
    Data_Nascimento DATE,
    Endereco VARCHAR(200),
    Telefone VARCHAR(20),
    Email VARCHAR(100),
    CPF VARCHAR(14),
    RG VARCHAR(20),
    Alergia TEXT,
    FOREIGN KEY (FK_Convenio) REFERENCES Convenio(ID_Convenio)
);


CREATE TABLE Consulta (
    ID_Consulta INT PRIMARY KEY,
    FK_Paciente INT,
    FK_Medico INT,
    FK_Especialidade INT,
    Data_Consulta DATE,
    Hora_Realizacao TIME,
    Valor_Consulta DECIMAL(10,2),
    Nome_Convenio VARCHAR(100),
    Numero_Carteira VARCHAR(50),
    Sala_da_Consulta VARCHAR(20),
    FOREIGN KEY (FK_Paciente) REFERENCES Paciente(ID_Paciente),
    FOREIGN KEY (FK_Medico) REFERENCES Medico(ID_Medico),
    FOREIGN KEY (FK_Especialidade) REFERENCES Especialidade(ID_Especialidade)
);


CREATE TABLE Receita (
    ID_Receita INT PRIMARY KEY,
    FK_Consulta INT,
    Medicamentos_Receitados TEXT,
    Quantidade_de_Remedios VARCHAR(100),
    Instruções_De_Uso TEXT,
    FOREIGN KEY (FK_Consulta) REFERENCES Consulta(ID_Consulta)
);


CREATE TABLE Enfermeiro (
    ID_Enfermeiro INT PRIMARY KEY,
    Nome VARCHAR(100),
    CPF VARCHAR(14),
    CRE VARCHAR(20)
);


CREATE TABLE Tipo_Quarto (
    ID_Tipo_Quarto INT PRIMARY KEY,
    valor_diaria DECIMAL(10,2),
    descricao TEXT
);


CREATE TABLE Quarto (
    ID_Quarto INT PRIMARY KEY,
    FK_Tipo_Quarto INT,
    numero VARCHAR(10),
    tipo VARCHAR(50),
    FOREIGN KEY (FK_Tipo_Quarto) REFERENCES Tipo_Quarto(ID_Tipo_Quarto)
);


CREATE TABLE Internacao (
    ID_Internacao INT PRIMARY KEY,
    FK_Enfermeiro INT,
    Quarto INT,
    Data_Entrada DATE,
    Data_Prev_Alta DATE,
    Data_Alta DATE,
    Procedimento TEXT,
    FOREIGN KEY (FK_Enfermeiro) REFERENCES Enfermeiro(ID_Enfermeiro),
    FOREIGN KEY (Quarto) REFERENCES Quarto(ID_Quarto)
);```





https://github.com/julliakathelyn
