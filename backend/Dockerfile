FROM dhi.io/python:3.12-debian12-dev AS build

WORKDIR /app

COPY --from=ghcr.io/astral-sh/uv:0.10.0 /uv /uvx /bin/

COPY pyproject.toml uv.lock README.md ./
RUN uv sync --locked --no-dev

COPY . .

FROM dhi.io/python:3.12-debian12

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app
COPY --from=build /app /app

EXPOSE 8000
CMD ["/app/.venv/bin/gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "3", "--timeout", "120", "--worker-tmp-dir", "/dev/shm", "--access-logfile", "-", "--error-logfile", "-", "--log-level", "info"]
