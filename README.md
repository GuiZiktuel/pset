# pset

CREATE TABLE public.funcionario (
                cpf_ CHAR(11) NOT NULL,
                data_nascimento DATE NOT NULL,
                primeiro_nome VARCHAR(20) NOT NULL,
                nome_meio CHAR(1),
                ulitmo_nome VARCHAR(20) NOT NULL,
                endereco VARCHAR(50),
                sexo CHAR(1),
                cpf_supervisor CHAR(11) NOT NULL,
                salario NUMERIC(10,2),
                numero_departamento INTEGER NOT NULL,
                CONSTRAINT pk_funcionario PRIMARY KEY (cpf_)
);


CREATE TABLE public.departamento (
                numero_departamento INTEGER NOT NULL,
                nome_departamento VARCHAR(20) NOT NULL,
                cpf_gerente CHAR(11) NOT NULL,
                data_inicio_gerente DATE,
                CONSTRAINT pk_departamento PRIMARY KEY (numero_departamento)
);


CREATE TABLE public.projeto (
                numero_projeto INTEGER NOT NULL,
                nome_projeto VARCHAR(20) NOT NULL,
                local_projeto VARCHAR(15),
                numero_departamento INTEGER NOT NULL,
                CONSTRAINT pk_projeto PRIMARY KEY (numero_projeto)
);


CREATE TABLE public.localizacoes_departamento (
                numero_departamento INTEGER NOT NULL,
                local VARCHAR(20) NOT NULL,
                CONSTRAINT pk_localizacoes_departamento PRIMARY KEY (numero_departamento, local)
);


CREATE TABLE public.trabalha_em (
                cpf_funcionario CHAR(11) NOT NULL,
                numero_projeto INTEGER NOT NULL,
                horas NUMERIC(3,1) NOT NULL,
                CONSTRAINT pk_trabalha_em PRIMARY KEY (cpf_funcionario, numero_projeto)
);


CREATE TABLE public.dependente (
                nome_dependente VARCHAR(20) NOT NULL,
                cpf_funcionario CHAR(11) NOT NULL,
                sexo CHAR(1) DEFAULT F,M,
                data_nascimento DATE,
                parentesco VARCHAR(15) NOT NULL,
                CONSTRAINT pk_dependente PRIMARY KEY (nome_dependente, cpf_funcionario)
);


ALTER TABLE public.dependente ADD CONSTRAINT funcionario_dependente_fk
FOREIGN KEY (cpf_funcionario)
REFERENCES public.funcionario (cpf_)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.trabalha_em ADD CONSTRAINT funcionario_trabalha_em_fk
FOREIGN KEY (cpf_funcionario)
REFERENCES public.funcionario (cpf_)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.departamento ADD CONSTRAINT funcionario_departamento_fk
FOREIGN KEY (cpf_gerente)
REFERENCES public.funcionario (cpf_)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.localizacoes_departamento ADD CONSTRAINT departamento_localizacoes_departamento_fk
FOREIGN KEY (numero_departamento)
REFERENCES public.departamento (numero_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.projeto ADD CONSTRAINT departamento_projeto_fk
FOREIGN KEY (numero_departamento)
REFERENCES public.departamento (numero_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.trabalha_em ADD CONSTRAINT projeto_trabalha_em_fk
FOREIGN KEY (numero_projeto)
REFERENCES public.projeto (numero_projeto)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;
