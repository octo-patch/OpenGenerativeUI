FROM python:3.12-slim

WORKDIR /app

# Install uv package manager (pinned version)
RUN pip install uv==0.7.12

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

# Run as non-root user
RUN addgroup --system --gid 1001 appuser && \
    adduser --system --uid 1001 --ingroup appuser appuser && \
    chown -R appuser:appuser /app
USER appuser

ENV PORT=8123

CMD uv run langgraph dev --host 0.0.0.0 --port ${PORT} --no-browser
