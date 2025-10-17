# Используем официальный образ Python как базовый
FROM python:slim

# Устанавливаем пакеты
# RUN apt-get update && \
    # DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata && \
    # ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime && \
    # echo "Europe/Moscow" > /etc/timezone && \
    # dpkg-reconfigure -f noninteractive tzdata && \
    # apt-get clean && \
    # rm -rf /var/lib/apt/lists/*
RUN \
    ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime && \
    echo "Europe/Moscow" > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata

# Устанавливаем переменные окружения
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /app

# Копируем файл зависимостей и устанавливаем их
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Копируем проект
COPY app/ .

# Порт для Django
EXPOSE 8000

# Команда запуска сервера Django
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

