# Визитка Марга АСТ — редизайн с нуля

**Дата:** 2026-05-29  
**Файл:** `vizitka.html`  
**Статус:** Approved

---

## Задача

Пересобрать `vizitka.html` с нуля по новой дизайн-системе. Старый файл — тёмный (чёрный фон, оранжевые акценты, canvas-анимация частиц). Новый — электрический синий, чёрные карточки, без canvas.

---

## Технические ограничения

- Один файл `vizitka.html` (HTML + `<style>` + `<script>` inline)
- Без внешних JS-библиотек — только ванильный JS и CSS
- Шрифт через Google Fonts (Noto Sans Condensed)
- Адаптив: mobile-first, breakpoint 480px
- Фото: `src="photo.jpg.jpg"` (двойное расширение, не переименовывать)

---

## Цветовая палитра

```css
:root {
  --bg:    #0039FF;              /* главный фон */
  --card:  #000000;              /* карточки */
  --white: #FFFFFF;              /* основной текст */
  --muted: rgba(255,255,255,.5); /* приглушённый */
  --ink:   #000000;              /* текст на синих/светлых блоках */
}
```

---

## Типографика

**Шрифт:** Noto Sans Condensed (Google Fonts)  
**Подключение:** `Noto+Sans+Condensed:wght@300;400;500;600;700;800;900`  
**Курсив:** не использовать  
**Все display-элементы:** `text-transform: uppercase`  
**Тело:** mixed case

| Роль | Weight | Размер | Case |
|---|---|---|---|
| Hero-имя (МАРГА АСТ) | 900 | `clamp(5rem, 22vw, 9rem)` | uppercase |
| Секционный H2 | 800 | `clamp(2.8rem, 12vw, 5rem)` | uppercase |
| Метрики кейсов | 700 | `clamp(3rem, 14vw, 5.5rem)` | — |
| Номера benefits (01–04) | 700 | `clamp(2rem, 8vw, 3rem)` | — |
| Названия карточек | 600 | `clamp(1rem, 4vw, 1.3rem)` | uppercase |
| Eyebrow / label | 600 | `0.7rem`, letter-spacing `.28em` | uppercase |
| Tagline / body | 400 | `clamp(.85rem, 2.5vw, 1rem)` | mixed |
| Приглушённый | 300 | `0.78rem` | mixed |

---

## Фон

Сплошной `#0039FF` + SVG grain-текстура через `::before` на `body`:
- `<feTurbulence>` filter, `baseFrequency ~0.65`, `numOctaves: 3`
- Opacity: `0.05` — едва заметный шум
- `pointer-events: none`, `z-index: -1`
- Статичный (не анимируется)

---

## Карточки

```css
background: #000000;
border-radius: 0;     /* острые углы */
border: none;
padding: 1.5rem 1.8rem;
```

---

## Компоновка

```css
--col: min(520px, 92vw);  /* ширина колонки */
```

- Секции: `padding: clamp(3.5rem, 9vw, 6rem) 1.5rem`
- Разделители: `height: 1px; background: rgba(255,255,255,.12)`

---

## Анимации

- Вход: `fade-up` — `opacity 0→1` + `translateY(18px)→0`, `cubic-bezier(.34,1.56,.64,1)`, duration `.5s`
- Scroll trigger: IntersectionObserver + класс `.visible`
- Stagger: +0.09s на каждую `.b-card`, +0.12s на каждую `.case-card`
- Hover: только `transform` — `translateY(-4px)` на кейсах, `translateX(5px)` на benefits

---

## Структура страницы (5 блоков)

### 1. Hero
- Аватар: 84×84px, `border-radius: 50%`, 2px белая обводка
- `src="photo.jpg.jpg"`
- Eyebrow: "Специалист по внедрению ИИ" (600, 0.7rem, uppercase)
- Имя: МАРГА АСТ (900, `clamp(5rem,22vw,9rem)`)
- Tagline: "Внедрение ИИ: от идеи до профита" (400)
- Кнопка: Telegram → `https://t.me/marga_ast`, чёрная кнопка, белый текст

### 2. Benefits — "ПОЧЕМУ ЭТО РАБОТАЕТ"
- Eyebrow: МЕТОДОЛОГИЯ
- 4 чёрные карточки в колонку, gap: 2px
- Каждая: номер (01–04, weight 700) + название (weight 600 uppercase) + стрелка `›`
- Клик → открывает модалку с текстом

Содержимое:
1. Не точечно — системно
2. Только то, что окупается
3. Результат через 2 недели
4. Эффект без зависимости

### 3. Cases — "ЧТО УЖЕ РАБОТАЕТ"
- Eyebrow: КЕЙСЫ
- 3 чёрные карточки, gap: 2px
- Структура карточки: крупная метрика (weight 700) + tag (300) + title (600) + desc (400) + meta (600, uppercase, muted)

| Метрика | Tag | Title | Meta |
|---|---|---|---|
| −60% | Интернет-магазин · ИИ-поддержка | Чатбот вместо колл-центра | обращений в поддержку |
| ×3 | Контент-агентство · Генерация | Контент в три раза быстрее | скорость производства контента |
| 40ч | Производство · Аналитика | Отчёты без Excel | в месяц сэкономлено |

### 4. CTA — "НАПИШИ МНЕ"
- Чёрный блок (`background: #000`)
- Белый текст
- Кнопка: белая, чёрный текст, Telegram, `https://t.me/marga_ast`

### 5. Modal
- Оверлей: `rgba(0,0,0,.85)` + `backdrop-filter: blur(8px)`
- Чёрная карточка `min(500px, 90vw)`, `border-radius: 0`
- Закрытие: кнопка ✕, клик по оверлею, Escape
- Контент: SVG-иконка + title (weight 800 uppercase) + text (400)

---

## JS — что переносим из старого файла

### BENEFITS (массив)
4 объекта `{title, text}` с полными текстами для модалок.

### MODAL_ICONS (массив)
4 inline SVG строки (сеть, рост, молния, щит).

### Логика модального окна (~35 строк)
- Клик по `.b-card` → заполнить modal + `overlay.classList.add('open')`
- Кнопка закрытия, клик по оверлею, `Escape`

### IntersectionObserver
- `.b-card` — threshold 0.08, stagger 0.09s
- `.case-card` — threshold 0.1, stagger 0.12s

---

## Адаптив (480px)

```css
@media (max-width: 480px) {
  .hero { padding: 4rem 1rem; }
  .s-wrap { padding: 3rem 1rem; }
  .cta-wrap { padding: 0 1rem 4rem; }
}
```
