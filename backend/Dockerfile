# -------- Runtime stage --------
FROM python:3.12-slim

WORKDIR /app

RUN useradd -m appuser

# Copy dependencies
COPY --from=builder /root/.local /home/appuser/.local
ENV PATH=/home/appuser/.local/bin:$PATH

# Copy source code and tests
COPY app app
COPY tests tests
COPY run.py .

# Ensure Python can find the app package
ENV PYTHONPATH=/app

# Fix permissions
RUN chown -R appuser:appuser /app

USER appuser

EXPOSE 5000

CMD ["gunicorn", "-b", "0.0.0.0:5000", "run:app"]
