# Phone IP Camera — локальный ONVIF/RTSP мост

Проект превращает Android-телефон в виртуальную IP-камеру для локального NVR.

## Схема
Android PWA → HTTPS/WSS → Node.js + FFmpeg → MediaMTX (RTSP publisher) → ONVIF Proxy → видеорегистратор.

PWA не может сама открыть RTSP-порт или отвечать на ONVIF WS-Discovery. Поэтому ONVIF и RTSP реализуются на постоянно работающем компьютере.

## Требования
- Windows 10/11 с Docker Desktop и WSL2.
- Android с Chrome.
- Телефон, компьютер и NVR в одной локальной сети.
- NVR с ручным добавлением ONVIF-устройства.

## Установка
1. Установите и запустите Docker Desktop.
2. Скопируйте `.env.example` в `.env` в этой папке.
3. В PowerShell выполните `ipconfig`, найдите IPv4 компьютера.
4. В `.env` задайте `HOST_IP`, уникальный `CAMERA_TOKEN` и `ONVIF_WEB_PASSWORD`.
5. В корне репозитория выполните:

```powershell
docker compose --env-file ip-camera-server/.env -f ip-camera-server/docker-compose.yml up -d --build
```

6. Получите локальный сертификат Caddy:

```powershell
docker cp ip-camera-caddy:/data/caddy/pki/authorities/local/root.crt .\\phonecam-root.crt
```

7. Установите `phonecam-root.crt` на Android как сертификат CA (пункты меню зависят от версии Android).
8. Откройте `https://HOST_IP:8443/`, введите CAMERA_TOKEN, запустите камеру.

## Добавление в NVR
- Протокол: ONVIF
- IP: адрес компьютера HOST_IP
- Порт: 8081
- Если требуется путь: `/onvif/device_service`
- ONVIF auth в этой конфигурации не включена; начните с пустых логина и пароля.

## Адреса
- PWA: `https://HOST_IP:8443/`
- ONVIF Proxy UI: `http://HOST_IP:8080/`
- ONVIF SOAP: TCP 8081
- RTSP: TCP 8554

Для Windows Docker Desktop multicast WS-Discovery может не проходить; используется ручное добавление ONVIF. Автообнаружение отключено.

## Ограничения
Это интеграционная сборка, не проверенная на конкретной модели NVR. Android может остановить захват при блокировке экрана, сворачивании браузера, энергосбережении или перегреве. Держите экран включённым и телефон на зарядке. Не открывайте порты в интернет. NETIP не реализован; используется стандартный ONVIF. Видео передаётся без звука.

Команды обслуживания:
```powershell
docker compose --env-file ip-camera-server/.env -f ip-camera-server/docker-compose.yml ps
docker compose --env-file ip-camera-server/.env -f ip-camera-server/docker-compose.yml logs -f
docker compose --env-file ip-camera-server/.env -f ip-camera-server/docker-compose.yml down
```
