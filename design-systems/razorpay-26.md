# Design System: Razorpay 26

Референс — [Razorpay Sprint'26](https://razorpay.com/sprint/26).  
Применён в проекте: визитка Марга АСТ (`vizitka.html`).

---

## Суть стиля

Электрический синий как главный фон. Чёрные карточки на синем. Белый текст. Максимальный контраст. Острые края, 0 border-radius. Высокая энергия. Никакой мягкости — только сила и точность.

---

## Цвета

```css
:root {
  --bg:    #0039FF;              /* главный фон — электрический синий */
  --card:  #000000;              /* карточки, кнопки-иконки */
  --white: #FFFFFF;              /* основной текст */
  --muted: #c6c8d7;              /* приглушённый текст, eyebrow, tagline */
  --ink:   #000000;              /* текст на светлых кнопках */
}
```

**Акцент:** `#767bd6` — лавандовый, только для цифр/метрик и выделенных фраз.  
**Иконка TG в hero:** `#a2a6ed` — светлее основного акцента.  
**CTA-кнопка фон:** `#c7c8da` — холодный серо-лавандовый.  
**Мета/ghost-текст:** `rgba(255,255,255,.35)` — почти невидимый.

---

## Типографика

**Шрифт:** Roboto Condensed (Google Fonts)  
**Подключение:** `Roboto+Condensed:wght@300;400;500;600;700;800;900`

Все display-элементы — `text-transform: uppercase`. Тело — mixed case.

| Роль | Size | Weight | Letter-spacing | Case |
|---|---|---|---|---|
| Hero-имя | `clamp(5rem, 22vw, 9rem)` | 900 | `.02em` | upper |
| Секционный H2 | `clamp(2.8rem, 12vw, 5rem)` | 800 | `.02em` | upper |
| CTA-заголовок | `clamp(2.2rem, 12vw, 4.2rem)` | 800 | `.02em` | upper |
| Метрика кейса | `clamp(3rem, 14vw, 5.5rem)` | 700 | `.01em` | — |
| Номер benefit (01–04) | `clamp(2rem, 8vw, 3rem)` | 700 | — | — |
| Название карточки | `clamp(1rem, 4vw, 1.3rem)` | 600 | `.05em` | upper |
| Tagline строка 2 | `31px` | 600 | `.04em` | mixed |
| Tagline строка 1 | `31px` | 300 | `.06em` | upper |
| Eyebrow / label | `27px` | 300 | `.06em` | upper |
| Описание / body | `1.35rem` | 300 | `.03em` | mixed |
| Ghost-мета | `1.2rem` | 300 | `.03em` | upper |

**Line-height:** `.88–.9` для заголовков (очень плотно). `1.35–1.6` для body.

---

## Карточки

```css
background: #000000;
border-radius: 0;     /* острые углы — принципиально */
border: none;
padding: 1.3rem 1.8rem;  /* benefits */
padding: 1.8rem 2rem 2.8rem;  /* cases */
gap: 2px;  /* между стопкой карточек */
```

Карточки стакаются в колонку с зазором `2px` — создаёт ощущение единого блока с тонкими разрывами.

---

## Компоновка

```css
--col: min(520px, 92vw);  /* ширина контентной колонки */
```

- Одна центрированная колонка
- Секции: `padding: clamp(3.5rem, 9vw, 6rem) 1.5rem`
- Фоновой линии между секциями нет — синий идёт непрерывно
- Breakpoint: `480px`

---

## Фон

Сплошной `#0039FF` + SVG grain-текстура через `body::before`:

```css
body::before {
  content: '';
  position: fixed; inset: 0; z-index: 0;
  pointer-events: none;
  background-image: url("data:image/svg+xml,..."); /* feTurbulence, baseFreq 0.65, 3 octaves */
  opacity: 0.05;
}
```

Grain едва заметен — добавляет тактильность, не перекрывает синий.

---

## Анимации

### Вход элементов

```css
@keyframes fade-up {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: none; }
}
```

Easing: `cubic-bezier(.34, 1.56, .64, 1)` — пружина с небольшим overshoot.  
Duration: `.5s–.65s`. Delays: stagger +0.09s (benefits), +0.12s (cases).

### Scroll trigger

```js
const io = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold: 0.08 });
```

### Hover

**Только transform — никаких цветовых переходов:**
- Cards: `translateX(5px)` (benefits), `translateY(-4px)` (cases, CTA-кнопка)
- Transition: `.3s–.45s cubic-bezier(.34,1.56,.64,1)`

### Аватар

```css
@keyframes pop-in {
  from { opacity: 0; transform: scale(.25) rotate(-18deg); }
  to   { opacity: 1; transform: none; }
}
```

---

## Кнопки

### Icon-кнопка (hero)
```css
width: 47px; height: 47px;
background: #000;
color: #a2a6ed;  /* цвет иконки через currentColor */
border-radius: 0;
```

### CTA-кнопка (нижний блок)
```css
background: #c7c8da;
color: #000;
padding: .45rem 1rem .45rem .55rem;
font-weight: 700; font-size: .95rem;
letter-spacing: .03em; text-transform: uppercase;
border-radius: 0;
```

---

## Модальное окно

```css
/* Оверлей */
background: rgba(0,0,0,.85);
backdrop-filter: blur(8px);

/* Карточка */
width: min(500px, 90vw);
background: #000;
border-radius: 0;
/* Вход: translateY(24px) scale(.96) → none, spring easing */
```

---

## Декоративные элементы

- **Разделительная черта CTA:** `width: 2.4rem; height: 4px; background: #c7c8da`
- **Номера карточек и метрики:** цвет `#767bd6` (лавандовый акцент)
- **Стрелка в benefits:** `›` — анимируется `translateX(4px)` при hover

---

## Ощущение

> Электрический. Уверенный. Без украшений — только структура и энергия.  
> Синий давит, чёрные карточки удерживают взгляд. Типографика кричит, но не истерит.  
> Подходит для: личного бренда, B2B-продукта, tech-стартапа, агентства.  
> Не подходит для: детей, wellness, luxury.
