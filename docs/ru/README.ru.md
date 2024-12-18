<div align="center">

# Kerio updates mirror
### Локальное зеркало обновлений Kerio Control и Kerio Connect.

[English](../../README.md) · **Русский**
</div>

## Disclaimer
Тестирование работоспособности проводилось на следующих версиях ПО:
- **Ubuntu**: 24.04.1
- **Kerio Connect**: 10.0.6.8504
- **Kerio Control**: 9.4.5.8526
- **mkc**: 1.4.3

## Установка и использование

### Установка Docker и Docker Compose
Для работы требуется Docker и Docker Compose. Установите их, следуя инструкциям:
1. Установите дополнительные пакеты:
    ```bash
    sudo apt install curl software-properties-common ca-certificates apt-transport-https -y
    ```
2. Импортируйте GPG-ключ Docker:
    ```bash
    wget -O- https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor | sudo tee /etc/apt/keyrings/docker.gpg > /dev/null
    ```
3. Добавьте репозиторий Docker:
    ```bash
    echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu noble stable"| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
4. Обновите индекс пакетов:
    ```bash
    sudo apt update
    ```
5. Установите Docker:
    ```bash
    sudo apt install docker-ce -y
    ```
6. Установите Docker Compose:
    ```bash
    sudo apt install docker-compose -y
    ```

### Запуск зеркала из готовых Docker-образов
1. [Скачайте Docker-образы.](https://t.me/my_store_files_bot?start=kerio_connect_mirror)
2. Загрузите образы из архивов:
    ```bash
    sudo docker load -i kerio_connect_mirror.tar
    ```
3. Скачайте репозиторий и запустите ```docker-compose```:
    ```bash
    sudo docker-compose up -d
    ```

### Запуск зеркала из исходного кода
1. Скачайте репозиторий.
2. Скопируйте актуальную версию ```mkc``` в папку ```_mirrorKC/install/mirror```.
3. Дайте файлу ```mkc``` права на выполнение: ```chmod u+x```
4. Запустите ```docker-compose```:
   ```bash
    sudo docker-compose up -d
    ```

### Настройка
- В настройках почтового сервера Kerio Connect установите промежуточный HTTP-сервер:
  ```
  - Адрес: 172.222.0.5
  - Порт: 8118
  ```
  После этого будут обновляться антивирус и работать anti-spam.
- Мосты для соединения с TOR, находятся в файле: ```_tor/config/bridges.config```.
  - Если в файле docker-compose.yml установлен параметр ```USE_CHECK_TOR=true```, то будет выполняться фоновая проверка доступности сети Internet через TOR, и в случае недоступности TOR-мосты будут обновляться автоматически. 
  - Получить новые мосты можно на [официальном сайте](https://bridges.torproject.org) или через [Telegram-бота](https://t.me/GetBridgesBot).
- Посмотреть логи и установить дополнительные настройки: ```http://IP_СЕРВЕРА_KERIO_CONNECT:9980/logsmkc.php```

### Обновление Kerio Control при помощи зеркала обновлений
[Документация по настройке](./kerio_control.md)
- Антивирус будет обновляться автоматически.
- Скачивание новых BlockList's (Предотвращение вторжения) происходит автоматически (примерно каждые 8 часов).