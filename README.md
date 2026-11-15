# Phone IP Camera

PWA для передачи камеры Android в локальную сеть видеонаблюдения через компьютерный мост Node.js + FFmpeg + MediaMTX + ONVIF Proxy.

## Быстрый старт

- Веб-приложение: [`ip-camera/`](./ip-camera/)
- Инструкция по серверу Windows/Docker: [`ip-camera-server/README.md`](./ip-camera-server/README.md)
- Архитектура: Android PWA → HTTPS/WSS → Node.js/FFmpeg → MediaMTX RTSP → ONVIF Proxy → NVR.

Это экспериментальная интеграционная версия. Совместимость ONVIF необходимо проверить с конкретной моделью регистратора. Не публикуйте локальные порты сервера в интернет.

## GitHub Pages

После успешного запуска workflow приложение будет доступно по адресу:

https://1979207-lab.github.io/Phone-IP-Camera/ip-camera/
