# --- Fase 1: Builder ---
# Immagine base per installare le dipendenze.
FROM python:3.9-slim AS builder
WORKDIR /app
# Versione fissata: l'ultima Poetry che gira su Python 3.9.
RUN pip install "poetry==2.2.1"
# CORREZIONE FINALE: Forza Poetry a creare .venv nella directory del progetto
RUN poetry config virtualenvs.in-project true

# Il poetry.lock è nel repository: il build installa versioni fissate, senza risolverle di nuovo.
COPY pyproject.toml poetry.lock ./

RUN poetry install --no-root --only main

# --- Fase 2: Production ---
# Immagine finale e leggera.
FROM python:3.9-slim
WORKDIR /app
# Ora questo comando funzionerà perché .venv è stato creato in /app
COPY --from=builder /app/.venv ./.venv
COPY ./src ./src
ENV PATH="/app/.venv/bin:$PATH"
CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8004"]