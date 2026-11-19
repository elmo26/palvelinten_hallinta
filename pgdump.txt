--
-- PostgreSQL database dump
--

\restrict TahyS0oMPjcN9cPvsNH23CeTBcQEZdpdm3e93VMQAZVJcwgQKYurAL8jeUNG1jD

-- Dumped from database version 17.12 (Debian 17.12-0+deb13u1)
-- Dumped by pg_dump version 17.12 (Debian 17.12-0+deb13u1)

SET statement_timeout = 0;
SET lock_timeout = 0;
SET idle_in_transaction_session_timeout = 0;
SET transaction_timeout = 0;
SET client_encoding = 'UTF8';
SET standard_conforming_strings = on;
SELECT pg_catalog.set_config('search_path', '', false);
SET check_function_bodies = false;
SET xmloption = content;
SET client_min_messages = warning;
SET row_security = off;

SET default_tablespace = '';

SET default_table_access_method = heap;

--
-- Name: sitsit; Type: TABLE; Schema: public; Owner: tauski
--

CREATE TABLE public.sitsit (
    id integer NOT NULL,
    name character varying(200)
);


ALTER TABLE public.sitsit OWNER TO tauski;

--
-- Name: sitsit_id_seq; Type: SEQUENCE; Schema: public; Owner: tauski
--

CREATE SEQUENCE public.sitsit_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER SEQUENCE public.sitsit_id_seq OWNER TO tauski;

--
-- Name: sitsit_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: tauski
--

ALTER SEQUENCE public.sitsit_id_seq OWNED BY public.sitsit.id;


--
-- Name: sitsit id; Type: DEFAULT; Schema: public; Owner: tauski
--

ALTER TABLE ONLY public.sitsit ALTER COLUMN id SET DEFAULT nextval('public.sitsit_id_seq'::regclass);


--
-- Data for Name: sitsit; Type: TABLE DATA; Schema: public; Owner: tauski
--

COPY public.sitsit (id, name) FROM stdin;
1	Antti
\.


--
-- Name: sitsit_id_seq; Type: SEQUENCE SET; Schema: public; Owner: tauski
--

SELECT pg_catalog.setval('public.sitsit_id_seq', 1, true);


--
-- Name: sitsit sitsit_pkey; Type: CONSTRAINT; Schema: public; Owner: tauski
--

ALTER TABLE ONLY public.sitsit
    ADD CONSTRAINT sitsit_pkey PRIMARY KEY (id);


--
-- PostgreSQL database dump complete
--

\unrestrict TahyS0oMPjcN9cPvsNH23CeTBcQEZdpdm3e93VMQAZVJcwgQKYurAL8jeUNG1jD

