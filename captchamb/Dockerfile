# ===== Base image Python ổn định =====
FROM python:3.11-slim

# ===== Không hỏi linh tinh khi build =====
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# ===== Set working dir =====
WORKDIR /app

# ===== Cài dependency hệ thống cần cho numpy / onnx =====
RUN apt-get update && apt-get install -y \
    wget \
    ca-certificates \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    && rm -rf /var/lib/apt/lists/*

# ===== Copy requirements trước để cache layer =====
COPY requirements.txt .

# ===== Cài python packages =====
RUN pip install --upgrade pip \
    && pip install --no-cache-dir -r requirements.txt

# ===== Copy source code =====
COPY main.py .

# ===== Expose port (Render dùng biến $PORT) =====
EXPOSE 10000

# ===== Start app =====
CMD ["sh", "-c", "uvicorn main:app --host 0.0.0.0 --port ${PORT:-10000}"]
