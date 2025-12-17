## 🗂️ Estrutura de Pastas

backend/
├── docker-compose.yml
├── Dockerfile
├── .env
├── manage.py
│
├── config/                  # Configurações globais do Django
│   ├── __init__.py
│   ├── urls.py
│   ├── asgi.py
│   ├── wsgi.py
│   └── settings/
│       ├── __init__.py
│       ├── base.py          # Configurações comuns
│       ├── local.py         # Ambiente de desenvolvimento
│       └── production.py    # Ambiente de produção
│
├── apps/                    # Apps do projeto (regras de negócio)
│   ├── __init__.py
│   ├── users/
│   ├── eventos/
│   └── tickets/
│
└── requirements.txt
```

---

## 🧠 Conceito da Estrutura

### `config/`
A pasta `config` concentra **apenas** o que é necessário para inicializar o Django:

- settings
- urls globais
- asgi / wsgi

Não coloco regras de negócio, models ou lógica de aplicação aqui. Isso ajuda a manter o projeto organizado e evita acoplamento desnecessário.

---

### `apps/`
Todos os apps do projeto ficam dentro da pasta `apps`, organizados por domínio.

Exemplos:
- usuários
- imobiliaria
- Fiança
- Incêndio

Cada app é responsável apenas pelo seu contexto.

---

## ⚙️ Settings por Ambiente

settings por ambiente porque isso facilita muito o uso com Docker, CI/CD e produção.

### `base.py`
Contém tudo que é comum a qualquer ambiente:

- INSTALLED_APPS
- MIDDLEWARE
- TEMPLATES
- Configurações base de banco, internacionalização, etc.

### `local.py`
Usado durante o desenvolvimento:

```python
from .base import *

DEBUG = True
ALLOWED_HOSTS = ["*"]
```

### `production.py`
Usado em produção:

```python
from .base import *

DEBUG = False
ALLOWED_HOSTS = [
    "seudominio.com",
    "www.seudominio.com",
]
```

---

## 🚀 Criação do Projeto

O projeto foi iniciado com o comando abaixo, já definindo `config` como módulo principal:

```bash
django-admin startproject config .
```

Depois disso, o `settings.py` padrão foi convertido para uma estrutura em pasta:

```bash
mkdir config/settings
mv config/settings.py config/settings/base.py
touch config/settings/__init__.py
```

---

## 🧱 Criando Apps da Forma Correta

Sempre crio os apps manualmente dentro da pasta `apps` para manter o padrão.

### 1️⃣ Criar a pasta `apps`

```bash
mkdir -p apps
touch apps/__init__.py
```

---

### 2️⃣ Criar a pasta do app

Exemplo: `users`

```bash
mkdir -p apps/users
```

---

### 3️⃣ Criar o app com `startapp`

```bash
docker compose run --rm web python manage.py startapp users apps/users
```

Isso gera a estrutura padrão do Django dentro da pasta correta.

## 🐳 Uso com Docker

Subir o projeto:

```bash
docker compose up --build
```

Rodar comandos Django:

```bash
docker compose run --rm web python manage.py makemigrations
docker compose run --rm web python manage.py migrate
docker compose run --rm web python manage.py createsuperuser
```

---

## ❌ Erros Comuns

Alguns erros que esse padrão ajuda a evitar:
- misturar configuração com regra de negócio
- não separar settings por ambiente



