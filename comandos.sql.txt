-- DROP SCHEMA public;

CREATE SCHEMA public AUTHORIZATION pg_database_owner;

-- DROP SEQUENCE public.colaborador_colaborador_cd_id_seq;

CREATE SEQUENCE public.colaborador_colaborador_cd_id_seq
	INCREMENT BY 1
	MINVALUE 1
	MAXVALUE 9223372036854775807
	START 1
	CACHE 1
	NO CYCLE;
-- DROP SEQUENCE public.imagem_img_cd_id_seq;

CREATE SEQUENCE public.imagem_img_cd_id_seq
	INCREMENT BY 1
	MINVALUE 1
	MAXVALUE 9223372036854775807
	START 1
	CACHE 1
	NO CYCLE;
-- DROP SEQUENCE public.roles_id_seq;

CREATE SEQUENCE public.roles_id_seq
	INCREMENT BY 1
	MINVALUE 1
	MAXVALUE 2147483647
	START 1
	CACHE 1
	NO CYCLE;
-- DROP SEQUENCE public.users_id_seq;

CREATE SEQUENCE public.users_id_seq
	INCREMENT BY 1
	MINVALUE 1
	MAXVALUE 9223372036854775807
	START 1
	CACHE 1
	NO CYCLE;
-- DROP SEQUENCE public.usuario_usuario_cd_id_seq;

CREATE SEQUENCE public.usuario_usuario_cd_id_seq
	INCREMENT BY 1
	MINVALUE 1
	MAXVALUE 9223372036854775807
	START 1
	CACHE 1
	NO CYCLE;-- public.imagem definition

-- Drop table

-- DROP TABLE imagem;

CREATE TABLE imagem (
	img_cd_id bigserial NOT NULL,
	img_tx_dados bytea NULL,
	img_tx_nome varchar(255) NULL,
	img_tx_tipo varchar(255) NULL,
	CONSTRAINT imagem_pkey PRIMARY KEY (img_cd_id)
);


-- public.roles definition

-- Drop table

-- DROP TABLE roles;

CREATE TABLE roles (
	id serial4 NOT NULL,
	"name" varchar(20) NULL,
	CONSTRAINT roles_name_check CHECK (((name)::text = ANY ((ARRAY['ROLE_USER'::character varying, 'ROLE_ADM'::character varying])::text[]))),
	CONSTRAINT roles_pkey PRIMARY KEY (id)
);


-- public.usuario definition

-- Drop table

-- DROP TABLE usuario;

CREATE TABLE usuario (
	usuario_cd_id bigserial NOT NULL,
	usu_tx_nome varchar(255) NULL,
	CONSTRAINT usuario_pkey PRIMARY KEY (usuario_cd_id)
);


-- public.colaborador definition

-- Drop table

-- DROP TABLE colaborador;

CREATE TABLE colaborador (
	colaborador_cd_id bigserial NOT NULL,
	col_dt_nascimento timestamp(6) NULL,
	col_tx_email varchar(255) NULL,
	col_tx_face varchar(255) NULL,
	col_tx_github varchar(255) NULL,
	col_tx_inst varchar(255) NULL,
	col_tx_link varchar(255) NULL,
	col_tx_nome varchar(255) NULL,
	col_tx_nome_social varchar(255) NULL,
	col_tx_telefone varchar(255) NULL,
	fk_imagem_id int8 NULL,
	usuario_cd_id int8 NULL,
	CONSTRAINT colaborador_pkey PRIMARY KEY (colaborador_cd_id),
	CONSTRAINT uk_251k4pxbymdksrpa5gybkx7pr UNIQUE (usuario_cd_id),
	CONSTRAINT uk_5u9yncekhsqskmqnil29ypabf UNIQUE (fk_imagem_id),
	CONSTRAINT uk_7814i94hw5q548sdn7mqm4gco UNIQUE (col_tx_email),
	CONSTRAINT fk32kq05kdleerd9f37joy0j0cm FOREIGN KEY (fk_imagem_id) REFERENCES imagem(img_cd_id),
	CONSTRAINT fktfbgt697tx1k4abovf13epcix FOREIGN KEY (usuario_cd_id) REFERENCES usuario(usuario_cd_id)
);


-- public.users definition

-- Drop table

-- DROP TABLE users;

CREATE TABLE users (
	id bigserial NOT NULL,
	email varchar(50) NULL,
	"password" varchar(120) NULL,
	fk_usuario_cd_id int8 NULL,
	CONSTRAINT uk6dotkott2kjsp8vw4d0m25fb7 UNIQUE (email),
	CONSTRAINT uk_on8lntyb7kqth9ynta2wvt004 UNIQUE (fk_usuario_cd_id),
	CONSTRAINT users_pkey PRIMARY KEY (id),
	CONSTRAINT fk7b464h5mjq5a4yt008wlxh03m FOREIGN KEY (fk_usuario_cd_id) REFERENCES usuario(usuario_cd_id)
);


-- public.user_roles definition

-- Drop table

-- DROP TABLE user_roles;

CREATE TABLE user_roles (
	user_id int8 NOT NULL,
	role_id int4 NOT NULL,
	CONSTRAINT user_roles_pkey PRIMARY KEY (user_id, role_id),
	CONSTRAINT fkh8ciramu9cc9q3qcqiv4ue8a6 FOREIGN KEY (role_id) REFERENCES roles(id),
	CONSTRAINT fkhfh9dx7w3ubf1co1vdev94g3f FOREIGN KEY (user_id) REFERENCES users(id)
);
