# 💳 PE Quick Payment Page

**Self-hosted сторінка швидкої оплати для ФОП за банківськими реквізитами.**

Проста платіжна сторінка без бази даних та стороннього backend-фреймворку.

Користувач може вказати суму та призначення платежу, отримати QR-код, відкрити платіж у банківському застосунку або скористатися готовим коротким посиланням.

<p align="center">

![PHP](https://img.shields.io/badge/PHP-7.4%2B-777BB4?logo=php&logoColor=white)
![HTML](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-F7DF1E?logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/license-not_specified-lightgrey)
![Database](https://img.shields.io/badge/database-none-success)

</p>

<p align="center">

**[Repository](https://github.com/SapozhnikUA/PE-QuikPaymentPage)**

</p>

---

## ✨ Features

- 💳 Оплата за реквізитами ФОП
- 🇺🇦 QR-код платежу у форматі НБУ
- 💰 Динамічна сума платежу
- 📝 Динамічне призначення платежу
- 📱 Адаптивний інтерфейс для смартфонів та ПК
- 🏦 Deep-link посилання для банківських застосунків
- 🔗 Постійні короткі посилання на платежі
- 📋 Копіювання реквізитів
- 📇 Експорт реквізитів у vCard
- 📄 Завантаження повних реквізитів у текстовому форматі
- ⚙️ Повне налаштування через `config.php`
- 🔐 Адміністративний режим
- 🔑 Авторизація за паролем
- 🛡️ Серверні токени сесій
- 💾 Автоматичне збереження коротких посилань
- ♻️ Резервне копіювання `links.json`
- 📧 Скидання адміністративного доступу через email
- 🚫 Без бази даних
- 🚀 Працює на звичайному PHP-хостингу

---

# 🖥️ How it works

```text
                         ┌─────────────────────┐
                         │       КЛІЄНТ        │
                         │                     │
                         │  сума + призначення │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     index.html      │
                         │                     │
                         │  QR / Bank Links   │
                         │  реквізити / URL    │
                         └──────────┬──────────┘
                                    │
                         GET config │
                                    ▼
                         ┌─────────────────────┐
                         │      save.php       │
                         │                     │
                         │   public config     │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     config.php      │
                         │                     │
                         │  реквізити ФОП      │
                         │  налаштування       │
                         └─────────────────────┘


       Адміністратор
              │
              │ password
              ▼
       ┌───────────────┐
       │   save.php    │
       │ authentication│
       └───────┬───────┘
               │
               │ token
               ▼
       ┌───────────────┐
       │   links.json  │
       │               │
       │ short URLs    │
       └───────────────┘
```

Основна сторінка працює на стороні браузера. PHP використовується для отримання публічної конфігурації, адміністративної авторизації та збереження коротких посилань.

---

# 🚀 Quick Start

## Requirements

- PHP **7.4+**
- Apache, Nginx або інший PHP-сумісний вебсервер
- HTTPS — рекомендовано
- PHP має мати права запису до:
  - `config.php`
  - `links.json`

Для функції скидання доступу через email також потрібна робоча PHP `mail()`.

Вимоги безпосередньо відповідають поточній реалізації `save.php`.

---

## 1. Clone

```bash
git clone https://github.com/SapozhnikUA/PE-QuikPaymentPage.git
cd PE-QuikPaymentPage
```

Або просто завантажте репозиторій на PHP-хостинг.

---

## 2. Create configuration

Створіть реальний конфіг із прикладу:

```bash
cp config.example.php config.php
```

**`config.php` не повинен потрапити в Git.**

У репозиторії вже передбачений `config.example.php`; перший PHP-рядок цього файлу є захисним і його не можна видаляти.

---

## 3. Configure

Відкрийте:

```text
config.php
```

та заповніть реквізити ФОП.

Мінімально необхідні:

```text
payee.name
payee.iban
payee.tax_id
```

Решта параметрів відповідає зовнішньому вигляду та поведінці сторінки.

---

## 4. Open the page

```text
https://example.com/pe/
```

Після завантаження сторінка отримує публічну частину конфігурації через:

```text
save.php?action=config
```

Пароль, його хеш та серверні сесії у frontend не передаються.

---

# ⚙️ Configuration

Приклад структури:

```php
<?php http_response_code(404); exit; ?>
{
    "password_hash": "",
    "admin_email": "admin@example.com",
    "mail_from": "noreply@example.com",
    "site_url": "https://example.com/pe/",
    "session_days": 30,

    "payee": {
        "name": "ФОП Ім'я Прізвище По батькові",
        "iban": "UA000000000000000000000000000",
        "tax_id": "1234567890",
        "bank": "Назва банку",
        "mfo": "000000",
        "bank_edrpou": "00000000",
        "phone": "+380000000000",
        "email": "admin@example.com"
    },

    "default_purpose": "Оплата за послуги",

    "profile": {
        "name": "Ім'я Прізвище",
        "subtitle": "ФОП · оплата за послуги",
        "badge": "Приймаю оплату",
        "footer": "ФОП І. П."
    },

    "page": {
        "title": "Оплата за послуги — ФОП І. П.",
        "description": "Оплата за реквізитами"
    }
}
```

Структура конфігурації відповідає поточному `config.example.php`.

### Configuration reference

| Parameter | Description |
|---|---|
| `password_hash` | Хеш адміністративного пароля |
| `admin_email` | Email для адміністративних операцій |
| `mail_from` | Адреса відправника системних листів |
| `site_url` | Базова URL-адреса сторінки |
| `session_days` | Тривалість адміністративної сесії |
| `payee.name` | ПІБ / назва отримувача |
| `payee.iban` | IBAN отримувача |
| `payee.tax_id` | РНОКПП / ЄДРПОУ |
| `payee.bank` | Назва банку |
| `payee.mfo` | МФО |
| `payee.bank_edrpou` | ЄДРПОУ банку |
| `payee.phone` | Телефон |
| `payee.email` | Email отримувача |
| `default_purpose` | Призначення платежу за замовчуванням |
| `profile.name` | Ім'я у профілі сторінки |
| `profile.subtitle` | Підзаголовок профілю |
| `profile.badge` | Текст бейджа |
| `profile.footer` | Підпис у нижній частині сторінки |
| `profile.logo_alt` | ALT для логотипа |
| `page.title` | `<title>` сторінки |
| `page.description` | Meta description |

---

# 💰 Payment URL

Платіж може бути сформований прямо через URL.

## Сума + призначення

```text
https://example.com/pe/?sum=500&purpose=Оплата%20за%20послуги
```

Наприклад:

```text
https://example.com/pe/?sum=1500&purpose=Оплата%20за%20інформаційні%20послуги
```

Після відкриття сторінка автоматично використовує передані параметри платежу.

---

# 🔗 Short URLs

Для часто використовуваних платежів можна створювати короткі URL.

Наприклад:

```text
https://example.com/pe/?abc123
```

або:

```text
https://example.com/pe/?c=abc123
```

Короткий код ідентифікує запис у `links.json`.

Приклад:

```json
{
    "links": {
        "abc123": {
            "sum": "500",
            "purpose": "Оплата за інформаційні послуги"
        }
    }
}
```

Код короткого посилання має довжину до 6 символів при серверному створенні; backend використовує спеціальний алфавіт без неоднозначних символів.

---

# 📱 Mobile banking

На мобільних пристроях сторінка може формувати посилання для відкриття підтримуваного банківського застосунку.

Використовуються:

- Android `intent://`
- iOS custom URL schemes

Формат посилання залежить від платформи та конкретного банку.

Якщо потрібний банк не підтримує відповідний deep-link, користувач може скористатися QR або скопіювати реквізити вручну.

---

# 🇺🇦 QR Payment

QR формується безпосередньо у браузері на основі поточних параметрів платежу.

Зміна:

```text
Сума
```

або:

```text
Призначення
```

призводить до оновлення платіжних даних та QR.

Це дозволяє використовувати одну сторінку для різних сум та призначень.

---

# 👨‍💼 Admin Mode

Адміністративний режим:

```text
https://example.com/pe/?admin
```

Після входу адміністративні операції виконуються через `save.php`.

Основні операції backend:

```text
GET  ?action=config
POST ?action=setup
POST ?action=login
POST ?action=whoami
POST ?action=logout
POST ?action=save
POST ?action=reset_request
GET  ?action=reset
POST ?action=reset
```

Ці endpoints реалізовані безпосередньо у `save.php`.

---

# 🔐 Authentication

Адміністративний доступ побудований на:

```text
Password
   ↓
password_hash()
   ↓
Server-side session token
   ↓
X-Key HTTP header
```

Пароль не передається назад frontend як конфіденційна конфігурація.

Токен:

- генерується криптографічно випадковим способом;
- зберігається на сервері у вигляді SHA-256;
- має термін дії;
- передається у `X-Key`;
- може бути відкликаний.

Поточні обмеження backend:

```text
Minimum password length : 8
Maximum password length : 256
Maximum active sessions : 20
Maximum login failures  : 5
Lock duration           : 15 min
Reset link TTL          : 1 hour
Reset email cooldown    : 5 min
Maximum stored links    : 5000
```


---

# 💾 Data Storage

Проєкт не використовує MySQL, SQLite або іншу БД.

Дані зберігаються у звичайних файлах:

```text
config.php
links.json
```

Репозиторій також містить:

```text
links.json.bak
```

для резервної копії даних коротких посилань.

### Advantages

- проста установка;
- немає міграцій;
- немає налаштування БД;
- легко переносити між серверами;
- легко створювати резервні копії.

---

# 🗂️ Project Structure

```text
PE-QuikPaymentPage/
│
├── index.html
│   └── Frontend платіжної сторінки
│
├── save.php
│   └── Backend / API / authentication
│
├── config.example.php
│   └── Шаблон конфігурації
│
├── links.json
│   └── Короткі платіжні посилання
│
├── links.json.bak
│   └── Backup links.json
│
└── .gitignore
```

Поточна структура репозиторію саме така.

---

# 🔒 Security

## `config.php`

**Ніколи не публікуйте реальний `config.php`.**

Він містить:

- хеш адміністративного пароля;
- адміністративні сесії;
- email;
- платіжні реквізити.

Захисний перший рядок:

```php
<?php http_response_code(404); exit; ?>
```

повинен залишатися на місці.

Backend використовує його як guard перед JSON-конфігурацією.

---

## HTTPS

Для production використовуйте:

```text
https://
```

Особливо якщо використовується адміністративний режим.

---

## File permissions

PHP повинен мати можливість записувати:

```text
config.php
links.json
```

але інші файли сайту не варто робити глобально доступними на запис.

---

# 🧩 Browser-side architecture

Frontend не потребує JavaScript-фреймворку.

Основні операції виконуються безпосередньо у браузері:

```text
User input
    ↓
Payment data
    ↓
QR generation
    ↓
Bank links
    ↓
Payment URL
```

Для роботи з backend використовується звичайний `fetch()`:

```javascript
fetch('save.php?action=config')
```

та JSON API.

---

# 📤 Export

Сторінка може сформувати повний текстовий набір реквізитів:

```text
РЕКВІЗИТИ

Отримувач: ФОП ...
IBAN: UA...
РНОКПП: ...
Призначення: ...
Сума: ...
Банк: ...
МФО: ...
Код банку ЄДРПОУ: ...
Тел.: ...
E-mail: ...

Посилання для оплати: ...
```

Також формується vCard із контактними даними та основними банківськими реквізитами.

---

# 🛠️ Deployment

Приклад для звичайного shared hosting:

```text
public_html/
└── pe/
    ├── index.html
    ├── save.php
    ├── config.php
    ├── links.json
    └── links.json.bak
```

Після завантаження:

```text
https://example.com/pe/
```

---

# 🧪 Testing checklist

Після встановлення перевірте:

- [ ] сторінка відкривається по HTTPS;
- [ ] реквізити відображаються правильно;
- [ ] IBAN валідний;
- [ ] РНОКПП / ЄДРПОУ правильний;
- [ ] змінюється сума;
- [ ] змінюється призначення;
- [ ] QR формується коректно;
- [ ] QR читається банківським застосунком;
- [ ] працює копіювання;
- [ ] працюють банківські deep-links на смартфоні;
- [ ] створюється коротке посилання;
- [ ] коротке посилання відкривається;
- [ ] `?admin` відкриває адміністративний режим;
- [ ] створення пароля працює;
- [ ] авторизація працює;
- [ ] коротке посилання зберігається у `links.json`;
- [ ] reset access працює, якщо налаштований `mail()`.

---

# 📌 Design goals

Проєкт свідомо побудований навколо декількох принципів:

### Simple

Без фреймворків та бази даних.

### Portable

Можна перенести на інший PHP-хостинг простим копіюванням файлів.

### Mobile-first

Основний сценарій — оплата зі смартфона.

### Self-hosted

Платіжна сторінка та її дані знаходяться на власному сервері.

### Minimal backend

PHP використовується тільки там, де він дійсно потрібен.

---

# 📜 License

License у репозиторії наразі явно не визначена.

Якщо проєкт планується як open-source, рекомендується додати окремий файл `LICENSE` та вибрати відповідну ліцензію.

---

# 👤 Author

**SapozhnikUA**

GitHub:

**https://github.com/SapozhnikUA/PE-QuikPaymentPage**

---

## ⭐ Contributing

Bug reports, suggestions and pull requests are welcome.

Перед створенням Pull Request бажано перевірити:

```text
✓ PHP 7.4+
✓ Mobile browsers
✓ Desktop browsers
✓ QR generation
✓ Payment URLs
✓ Admin authentication
✓ links.json persistence
```

---

<p align="center">

**PE Quick Payment Page**

_Проста self-hosted сторінка оплати для ФОП._

</p>