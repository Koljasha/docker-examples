# Используем официальный образ Python как базовый
FROM python:slim

# Устанавливаем пакеты
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        cron \
        tzdata \
        procps && \
    ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime && \
    echo "Europe/Moscow" > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Создаем Cron
COPY cronfile /etc/cron.d/

# Устанавливаем права и проверяем валидность cron-задания
# RUN chmod +x /app/script.sh && \
    # chmod 0644 /etc/cron.d/cronfile && \
    # crontab -u root /etc/cron.d/cronfile

# Копируем entrypoint
COPY entrypoint.sh /entrypoint.sh

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

# Устанавливаем entrypoint
ENTRYPOINT ["/entrypoint.sh"]

# Команда запуска сервера Django
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

