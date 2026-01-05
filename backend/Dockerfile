# -------- Builder stage --------
FROM python:3.12-slim AS builder

WORKDIR /app

COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# -------- Runtime stage --------
FROM python:3.12-slim

WORKDIR /app

RUN useradd -m appuser

# Copy dependencies from builder stage
COPY --from=builder /root/.local /home/appuser/.local
ENV PATH=/home/appuser/.local/bin:$PATH

# Copy source code and tests
COPY app app
COPY tests tests
COPY run.py .

# Ensure Python can locate the app package
ENV PYTHONPATH=/app

# Fix permissions for non-root user
RUN chown -R appuser:appuser /app

USER appuser

EXPOSE 5000

CMD ["gunicorn", "-b", "0.0.0.0:5000", "run:app"]
