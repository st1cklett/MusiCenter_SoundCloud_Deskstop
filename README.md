<p align="center">
  <img src="web/assets/icon.ico" width="80" />
</p>

<h1 align="center">MusiCenter</h1>

<p align="center">
  <b>Десктопный музыкальный плеер для SoundCloud с умной рекомендательной системой</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-Windows-blue" />
  <img src="https://img.shields.io/badge/python-3.10+-yellow" />
  <img src="https://img.shields.io/badge/license-MIT-green" />
</p>

---

## Что это?

MusiCenter — полноценный десктопный плеер, который работает с каталогом SoundCloud и превращает его в персональное радио. Внутри — рекомендательный движок **СаундВолна**, построенный на векторной базе данных Qdrant и спектральном анализе аудио в реальном времени.

Приложение не требует подписки, не показывает рекламу и работает полностью на вашем компьютере.

## Возможности

**Плеер**
- Стриминг треков из SoundCloud с кроссфейдом между композициями
- 10-полосный эквалайзер с пресетами
- Караоке-режим с синхронизированным текстом (LRCLIB + Musixmatch)
- Тайм-коды комментариев прямо на прогресс-баре
- Горячие клавиши, очередь воспроизведения, shuffle, repeat

**СаундВолна — бесконечное персональное радио**
- Анализирует ваши лайки и строит вкусовой профиль в 72-мерном векторном пространстве
- 9 режимов настроения и активности: Бодрое, Весёлое, Грустное, Спокойное, Тренировка, Работа, Сон, Пробуждение, Дорога
- Каждый режим калиброван на основе научных исследований (BPM-диапазоны, спектральные характеристики)
- Режимы «Любимое», «Незнакомое», «Популярное» для управления балансом между знакомым и новым
- Определение BPM из аудио через onset detection, когда SoundCloud не предоставляет метаданные
- MMR-алгоритм разнообразия и DJ-style энергетическая кривая для плавных переходов

**Импорт из Яндекс Музыки**
- Перенос библиотеки из Яндекс Музыки в SoundCloud через автоматический поиск и мэтчинг
- Импортированные треки используются для улучшения рекомендаций СаундВолны

**Интерфейс**
- Три встроенные темы: Бездна, Монохром, Кибер + редактор собственных тем
- Полноэкранный режим (F11)
- Фоновый режим в трее
- Анимированный баннер СаундВолны с gaussian blob canvas

## Стек

| Компонент | Технология |
|-----------|-----------|
| Backend | Python, Flask |
| Frontend | JavaScript (Vanilla), CSS |
| Desktop | pywebview (WebView2) |
| Рекомендации | Qdrant Cloud (72-dim vectors, HNSW, Cosine) |
| Аудио-анализ | Web Audio API (FFT 2048, 10 FPS) |
| Текст песен | LRCLIB + Musixmatch |

## Установка

### Из исходников

```bash
git clone https://github.com/YOUR_USERNAME/MusiCenter.git
cd MusiCenter
pip install -r requirements.txt
python main.py
```

### Сборка .exe

```bash
pip install pyinstaller
pyinstaller MusiCenter.spec
```

Готовый файл будет в папке `dist/`.

## Настройка Qdrant

СаундВолна использует Qdrant Cloud для хранения векторных эмбеддингов треков. Для работы рекомендаций:

1. Создайте бесплатный кластер на [cloud.qdrant.io](https://cloud.qdrant.io)
2. Скопируйте URL кластера и API-ключ
3. Вставьте их в `web/js/components/qdrant.js`:
```js
_QDRANT_URL: 'https://YOUR-CLUSTER.cloud.qdrant.io:6333',
_QDRANT_KEY: 'YOUR_API_KEY',
```
4. И в `server.py`:
```python
QDRANT_URL = 'https://YOUR-CLUSTER.cloud.qdrant.io:6333'
QDRANT_KEY = 'YOUR_API_KEY'
```

Без Qdrant приложение работает, но рекомендации будут использовать только legacy-алгоритм на основе related tracks.

## Горячие клавиши

| Клавиша | Действие |
|---------|---------|
| `Space` | Пауза / воспроизведение |
| `Ctrl + →` | Следующий трек |
| `Ctrl + ←` | Предыдущий трек |
| `Ctrl + ↑/↓` | Громкость |
| `Ctrl + M` | Без звука |
| `F11` | Полный экран |
| `Q` | Очередь |

## Лицензия

MIT

---

<p align="center">
  <sub>Сделано на основе <a href="https://github.com/zxcloli666/SoundCloud-Desktop">SoundCloud-Desktop</a></sub>
</p>
