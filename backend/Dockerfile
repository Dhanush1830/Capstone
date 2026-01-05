# -------- Runtime stage --------
FROM python:3.12-slim

WORKDIR /app

# Create non-root user
RUN useradd -m appuser

# Copy dependencies
COPY --from=builder /root/.local /home/appuser/.local
ENV PATH=/home/appuser/.local/bin:$PATH

# Copy application code AND tests
COPY app app
COPY tests tests
COPY run.py .

# Fix ownership for non-root user
RUN chown -R appuser:appuser /app

USER appuser

EXPOSE 5000

CMD ["gunicorn", "-b", "0.0.0.0:5000", "run:app"]
