# Killstreak System - README

## 📋 Описание
Система достижений для убийств в стиле Counter-Strike 1.6. Отслеживает киллстрики игроков и показывает впечатляющие уведомления со звуками, визуальными эффектами и наградами.

---

## 🎮 Возможности

### **Уведомления**
- 🎵 Озвучка каждого достижения (загружается с GitHub)
- 🖼️ Анимированные картинки для каждого уровня
- ✨ Визуальные эффекты для никнейма (pulse, RGB, shake)
- ⏱️ Система очереди - уведомления не накладываются друг на друга
- 🎨 Плавное появление/исчезновение (fade in/out)

### **Эффекты для игроков**
- ❤️ **HEAL** - восстановление здоровья
- 🛡️ **GODMODE** - золотой материал + 10000 HP на время
- 💥 **Партиклы** - 4 типа эффектов:
  - `PARTICLES_EXPLOSION_RED` - медленный красный взрыв
  - `PARTICLES_EXPLOSION_RGB` - быстрый радужный взрыв
  - `PARTICLES_DRIP` - цветные частицы сверху вниз
  - `PARTICLES_DRIP_UP` - цветные частицы снизу вверх

### **Система уровней**
| Убийств | Уровень | Звук | Эффект никнейма | Награда |
|---------|---------|------|-----------------|---------|
| 3 | Triple Kill | pulse | 🟡 Жёлтый пульс | +50 HP |
| 4 | Ultra Kill | rgb | 🔴 RGB эффект | +75 HP |
| 5 | Rampage | rgb | 🟣 RGB эффект | +100 HP + Godmode 3s + 💥 Red Burst |
| 6 | Killing Spree | rgb_fast | 🔴 Быстрый RGB | Godmode 5s + 🌈 RGB Burst |
| 7 | Monster Kill | rgb_fast | 🟣 Быстрый RGB | Godmode 7s + 💧 Drip Down |
| 8 | Unstoppable | shake | 🔴 Тряска | Godmode 10s + 💥 Red Burst |
| 9 | Mega Kill | shake | 🟣 Тряска | Godmode 12s + 💧 Drip Up |
| 10 | Godlike | shake_rgb | 🟡 RGB тряска | Godmode 15s + 🌈 RGB Burst |
| 12 | Ludicrous Kill | shake_rgb | 🟡 RGB тряска | Godmode 20s + 🌈 RGB + 💧 Drip |
| 15 | Holy Shit | shake_rgb | ⚪ RGB тряска | Godmode 30s + ВСЕ ЭФФЕКТЫ |

### **Специальные убийства**
- 🎯 **Headshot** - убийство в голову (белый пульс)
- 🔪 **Knife Kill** - убийство ножом (+50 HP)
- 💣 **Grenade Kill** - убийство гранатой (оранжевая тряска)

---

## ⚙️ Команды

### Для всех игроков:
```
!kstoggle     - Включить/выключить систему (звук + визуал)
!killstreak   - То же самое
```

### Только для владельца чипа:
```
!ksdebug      - Показать случайное достижение для теста
```

---

## 🔧 Настройки (CONFIG)

```lua
debugMode = false              -- true = только владельцу видно
notificationDuration = 4       -- Секунды показа уведомления
nicknameTopMargin = 150        -- Отступ сверху экрана
messageMargin = 50             -- Отступ между nickname и message
imageMargin = 90               -- Отступ между message и image
soundVolume = 1.0              -- Громкость звуков (0.0 - 1.0)
sound2DAfterStreak = 3         -- Стрики >3 используют 2D звук
particleLimit = 500            -- Лимит партиклов от чипа
imageWidth = 400               -- Ширина картинки
imageHeight = 400              -- Высота картинки
```

---

## 📁 Структура файлов

### GitHub репозиторий:
```
https://github.com/TheLovelyMask/starfallthngs/
├── chips_onclassicbox/
│   ├── killstreak_sounds/     # .wav файлы
│   │   ├── triplekill1.wav
│   │   ├── ultrakill1.wav
│   │   ├── rampage1.wav
│   │   └── ...
│   └── killstreak_pngs/       # .png картинки
│       ├── triplekill1.png
│       ├── ultrakill1.png
│       └── ...
```

---

## 🎨 Компоновка на экране

```
┌─────────────────────────────┐
│                             │
│      [NICKNAME]             │  ← nicknameTopMargin (150px)
│                             │
│    [MESSAGE TEXT]           │  ← +messageMargin (50px)
│                             │
│    [    IMAGE    ]          │  ← +imageMargin (90px)
│    [             ]          │
│                             │
└─────────────────────────────┘
```

---

## 🛠️ Как добавить новый уровень

```lua
{
    id = "newlevel",                          -- Уникальный ID
    trigger = {type = "streak", count = 20},  -- Триггер
    sound = "newsound.wav",                   -- Файл звука
    nickname_effect = "rgb_fast",             -- Эффект ника
    nickname_color = {r = 255, g = 0, b = 0}, -- Цвет ника
    message = "NEW LEVEL!",                   -- Текст сообщения
    effects = {                               -- Эффекты
        {type = "HEAL", target = "attacker", args = {100}},
        {type = "GODMODE", target = "attacker", args = {5}},
        {type = "PARTICLES_EXPLOSION_RGB", target = "attacker"}
    }
}
```

### Типы триггеров:
- `{type = "streak", count = N}` - N убийств подряд
- `{type = "weapon", class = "weapon_name"}` - убийство оружием
- `{type = "method", hitgroup = 1}` - хедшот (hitgroup 1)

### Эффекты никнейма:
- `"none"` - без эффекта
- `"pulse"` - пульсация яркости
- `"rgb"` - плавная смена цвета
- `"rgb_fast"` - быстрая смена цвета
- `"shake"` - тряска
- `"shake_rgb"` - тряска + RGB

---

## 🔊 Звуки

### 2D vs 3D звук:
- **Стрики ≤ 3** → 3D позиционный звук (слышно откуда)
- **Стрики > 3** → 2D звук (всем одинаково громко)

Настраивается в: `sound2DAfterStreak = 3`

---

## 🎯 Особенности

### ✅ Система очереди
Если несколько уведомлений срабатывают одновременно - они показываются по очереди, не перекрывая друг друга.

### ✅ Лимит партиклов
Максимум 500 частиц от чипа. При превышении новые не создаются. Автоочистка по таймеру.

### ✅ Индивидуальные настройки
Каждый игрок может отключить систему командой `!kstoggle`. Настройки сохраняются пока игрок на сервере.

### ✅ Плавная анимация
- Fade in: 1 секунда
- Показ: `notificationDuration` (4 сек по умолчанию)
- Fade out: 1 секунда

---

## 🐛 Отладка

### Режим отладки:
```lua
CONFIG.debugMode = true  -- Только владельцу чипа видно
```

### Тест уведомлений:
```
!ksdebug  -- Показать случайное достижение
```

### Проверка звуков/картинок:
Убедись что файлы доступны по URL:
```
https://github.com/TheLovelyMask/starfallthngs/raw/refs/heads/main/chips_onclassicbox/killstreak_sounds/triplekill1.wav
https://github.com/TheLovelyMask/starfallthngs/raw/refs/heads/main/chips_onclassicbox/killstreak_pngs/triplekill1.png
```

---

## 📝 Требования

- **StarfallEx** чип
- **@superuser** права
- **@shared** режим
- Доступ к интернету для загрузки ассетов

---

## 💡 Советы

1. **Настройка визуала** - меняй `messageMargin` и `imageMargin` для компактности
2. **Громкость звука** - `soundVolume` от 0.0 до 1.0
3. **Отключение партиклов** - убери строчки с `PARTICLES_*` из эффектов
4. **Добавление уровней** - копируй существующий и меняй параметры

---

## 🎉 Примеры использования

### Убийства подряд:
```
3 килла → Triple Kill (+50 HP)
5 киллов → Rampage (+100 HP + Godmode 3s)
10 киллов → Godlike (Godmode 15s + RGB эффект)
```

### Специальные:
```
Хедшот → Headshot (белый пульс)
Нож → Knife Kill (+50 HP)
Граната → Grenade Kill (оранжевая тряска)
```

---

**Создано TheLovelyMask для ClassicBox** 🎮