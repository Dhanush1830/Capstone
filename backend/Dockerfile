# -------- Builder stage --------
FROM python:3.12-slim AS builder

WORKDIR /app

COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# -------- Runtime stage --------
FROM python:3.12-slim

WORKDIR /app

# Create non-root user
RUN useradd -m appuser

USER appuser

# Copy dependencies from builder
COPY --from=builder /root/.local /home/appuser/.local

ENV PATH=/home/appuser/.local/bin:$PATH

# Copy application code
COPY app app
COPY run.py .

EXPOSE 5000

CMD ["gunicorn", "-b", "0.0.0.0:5000", "run:app"]
