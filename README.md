# 🎮 GEOMETRY DASH — Інструкція збірки APK

## Що потрібно
- Комп'ютер з Linux або WSL на Windows
- Python 3.8+
- Інтернет (~2 ГБ для завантаження Android SDK)

---

## Крок 1 — Встанови залежності (Linux / WSL)

```bash
sudo apt update
sudo apt install -y python3-pip git zip unzip openjdk-17-jdk \
    autoconf libtool pkg-config zlib1g-dev libncurses5-dev \
    libncursesw5-dev libtinfo5 cmake libffi-dev libssl-dev \
    build-essential ccache

pip3 install buildozer cython
```

---

## Крок 2 — Розмісти файли

Переконайся що в папці є:
```
geodash/
  ├── main.py          ← головний файл гри
  └── buildozer.spec   ← конфіг збірки
```

---

## Крок 3 — Збери APK

```bash
cd geodash
buildozer android debug
```

⏳ Перша збірка займає **20–40 хвилин** (завантажує Android SDK, NDK, Cython).
Наступні збірки — **2–5 хвилин**.

---

## Крок 4 — Знайди APK

Після успішної збірки файл буде тут:
```
geodash/bin/geodash-1.0-debug.apk
```

---

## Крок 5 — Встанови на планшет

**Варіант A — USB:**
```bash
adb install bin/geodash-1.0-debug.apk
```

**Варіант B — вручну:**
1. Скопіюй `.apk` на планшет (USB / Telegram / Google Drive)
2. На планшеті: Налаштування → Безпека → **Дозволити встановлення з невідомих джерел**
3. Відкрий файловий менеджер → знайди APK → встанови

---

## ❗ Якщо немає Linux

Використай **GitHub Actions** (безкоштовно):

1. Створи репозиторій на github.com
2. Завантаж `main.py` та `buildozer.spec`
3. Створи файл `.github/workflows/build.yml`:

```yaml
name: Build APK
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: |
          sudo apt-get update
          sudo apt-get install -y python3-pip git zip unzip \
            openjdk-17-jdk autoconf libtool pkg-config \
            zlib1g-dev libncurses5-dev cmake libffi-dev libssl-dev
          pip3 install buildozer cython
      - name: Build APK
        run: buildozer android debug
      - name: Upload APK
        uses: actions/upload-artifact@v3
        with:
          name: geodash-apk
          path: bin/*.apk
```

4. Після push → вкладка **Actions** → завантаж APK як артефакт

---

## 🎮 Управління в грі

| Дія | Кнопка |
|-----|--------|
| Стрибок | Торкнутись екрану |
| Меню | Кнопка назад Android |

---

## Швидкий варіант без збірки

Якщо не хочеш збирати APK — просто встанови **Pydroid 3** з Google Play
і запусти `main.py` напряму. Працює без збірки!


