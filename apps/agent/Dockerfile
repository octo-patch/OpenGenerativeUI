FROM python:3.12-slim

WORKDIR /app

# Install uv package manager
RUN pip install uv

# Copy dependency files first for layer caching
COPY pyproject.toml uv.lock ./

# Install dependencies
RUN uv sync --frozen

# Copy application source
COPY main.py ./
COPY src ./src
COPY skills ./skills
COPY langgraph.json ./

# Patch langgraph.json to use local env path (was ../../.env for monorepo)
RUN sed -i 's|"env": "../../.env"|"env": ".env"|' langgraph.json

# Create empty .env so langgraph doesn't error (secrets come from Render env vars)
RUN touch .env

ENV PORT=8123

CMD uv run langgraph dev --host 0.0.0.0 --port ${PORT} --no-browser
