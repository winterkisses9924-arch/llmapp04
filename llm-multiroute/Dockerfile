# ---- Base image ----
FROM python:3.12-slim

# ---- Security & env ----
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# ---- Create non-root user ----
RUN addgroup --system app && adduser --system --ingroup app app

# ---- Workdir ----
WORKDIR /app/llm-multiroute

# ---- System deps (minimal) ----
RUN apt-get update \
    && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/*

# ---- Python deps ----
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ---- App code ----
COPY app ./app
COPY .env.example ./.env.example

# ---- Ownership ----
RUN chown -R app:app /app

# ---- Switch user ----
USER app

# ---- Expose port ----
EXPOSE 8082

# ---- Run ----
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8082"]
