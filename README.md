<div align="center">

<img src="./assets/readme/hero.svg" alt="Tachyon" width="100%">

# Tachyon — AmneziaWG 3.1 Fix Fork

**Форк Tachyon для корректной работы AmneziaWG 3.1 через sing-box-lx на OpenWrt**

[![Original Project](https://img.shields.io/badge/Upstream-Dushnilin%2Ftachyon-181717?style=for-the-badge&logo=github)](https://github.com/Dushnilin/tachyon)
[![OpenWrt](https://img.shields.io/badge/OpenWrt-25.x-00B5E2?style=for-the-badge&logo=openwrt&logoColor=white)](https://openwrt.org/)
[![AmneziaWG](https://img.shields.io/badge/AmneziaWG-3.1-6C5CE7?style=for-the-badge)](https://github.com/amnezia-vpn/amneziawg-go)
[![sing-box-lx](https://img.shields.io/badge/sing--box--lx-1.14.x-00A86B?style=for-the-badge)](https://github.com/Leadaxe/sing-box-lx)

</div>

<img src="./assets/readme/divider_stream.svg" width="100%">

## ⚡ Что это

Это **не отдельный проект и не переписывание Tachyon**.

Это мой форк оригинального [**Dushnilin/tachyon**](https://github.com/Dushnilin/tachyon), в котором исправлена конкретная проблема с **AmneziaWG 3.1 + sing-box-lx**.

Вся основная функциональность, архитектура, LuCI, маршрутизация, DNS, Telegram-бот, DPI-инструменты и прочие возможности принадлежат оригинальному Tachyon.  
Полное описание проекта и документацию смотрите в [**оригинальном репозитории**](https://github.com/Dushnilin/tachyon).

Цель этого форка одна:

> **чтобы рабочий AmneziaWG 3.1 `.conf` корректно импортировался в Tachyon и реально поднимался через sing-box-lx.**

<img src="./assets/readme/divider_stream.svg" width="100%">

## 🛠️ Что исправлено

В оригинальной версии Tachyon конфиг **AmneziaWG 3.1** импортировался не полностью.

Рабочий `.conf`, который без проблем подключался через официальный клиент AmneziaWG, после импорта в Tachyon давал:

```text
MAIN: Not responding
```

Причина — при генерации конфигурации sing-box-lx терялись параметры AWG 3.1.

### Исправления в этом форке

| Исправление | Статус |
|---|:---:|
| Импорт `RandomTrailers` из `.conf` | ✅ |
| Импорт `DisableCookies` из `.conf` | ✅ |
| Генерация `random_trailers` для sing-box-lx | ✅ |
| Генерация `disable_cookies` для sing-box-lx | ✅ |
| `PersistentKeepalive` в формате диапазона, например `25-35` | ✅ |
| Корректное определение варианта `sing-box-lx` | ✅ |
| Настройки AWG 3.1 в LuCI | ✅ |
| Явный HTTP client для remote rule-sets в sing-box 1.14+ | ✅ |

После исправления Tachyon генерирует полноценный AWG 3.1 endpoint, и туннель работает нормально.

<img src="./assets/readme/divider_stream.svg" width="100%">

## ✅ Проверено

Исправления тестировались на следующей связке:

```text
Router:        GL.iNet Flint 2 / GL-MT6000
OpenWrt:       25.12.5
Tachyon:       1.3.22
sing-box-lx:   1.14.0-lx.35
Protocol:      AmneziaWG 3.1
```

Проверка проводилась на реальном рабочем AWG 3.1 конфиге:

- официальный AmneziaWG клиент — работает;
- оригинальная генерация Tachyon — `MAIN: Not responding`;
- после исправлений этого форка — туннель подключается и трафик маршрутизируется корректно.

<img src="./assets/readme/divider_stream.svg" width="100%">

## 🚀 Установка

> Для AmneziaWG 3.1 при установке sing-box выбирайте **Leadaxe / sing-box-lx**.

### OpenWrt

```sh
wget -O /tmp/tachyon-setup.sh https://raw.githubusercontent.com/krodach/tachyon/main/install.sh
```

```sh
sh /tmp/tachyon-setup.sh
```

Во время установки выберите вариант:

```text
sing-box Leadaxe (lx)
```

Именно `sing-box-lx` содержит поддержку AmneziaWG 3.x, которая используется этим форком.

<img src="./assets/readme/divider_stream.svg" width="100%">

## 🧩 Импорт AmneziaWG 3.1

В LuCI:

```text
Tachyon
→ Sections
→ нужная секция
→ Action: AmneziaWG
→ Load .conf
```

После импорта выберите:

```text
AmneziaWG Version: 3.1
```

Для корректного AWG 3.1 конфигурация может содержать, в частности:

```text
RandomTrailers
DisableCookies
HeaderProtectionKey
ContentPaddingAddition
RekeyAfterTime
RekeyTimeout
RejectAfterTime
KeepaliveTimeout
MaxHandshakeAttempts
```

Этот форк не выбрасывает `RandomTrailers` и `DisableCookies` при импорте и передаёт их в sing-box-lx.

<img src="./assets/readme/divider_stream.svg" width="100%">

## 🔄 Отличия от оригинального Tachyon

Форк намеренно остаётся максимально близким к upstream.

Я не пытаюсь вести отдельную документацию по всем возможностям Tachyon и не дублирую огромный README оригинального проекта.

Здесь поддерживаются только изменения, необходимые для моей конфигурации:

```text
OpenWrt 25.x
+
Tachyon
+
sing-box-lx
+
AmneziaWG 3.1
```

Если нужны сведения о:

- VLESS / Reality;
- Hysteria2;
- Zapret / ByeDPI;
- DNS;
- Telegram-боте;
- AI Doctor;
- маршрутизации;
- подписках;
- остальных функциях Tachyon;

смотрите [**оригинальную документацию Dushnilin/tachyon**](https://github.com/Dushnilin/tachyon).

<img src="./assets/readme/divider_stream.svg" width="100%">

## 🤝 Upstream и используемые проекты

- [**Tachyon — Dushnilin/tachyon**](https://github.com/Dushnilin/tachyon) — оригинальный проект.
- [**Forkop — ushan0v/forkop**](https://github.com/ushan0v/forkop) — проект, от которого происходит Tachyon.
- [**Podkop — itdoginfo/podkop**](https://github.com/itdoginfo/podkop) — исходная архитектурная база.
- [**sing-box-lx — Leadaxe/sing-box-lx**](https://github.com/Leadaxe/sing-box-lx) — используемое ядро с поддержкой AmneziaWG 3.x.
- [**AmneziaWG**](https://github.com/amnezia-vpn/amneziawg-go) — протокол AmneziaWG.

<img src="./assets/readme/divider_stream.svg" width="100%">

<div align="center">

### Tachyon AWG 3.1 Fix Fork

**Минимальные изменения upstream-кода ради нормально работающего AmneziaWG 3.1.**

[Оригинальный Tachyon](https://github.com/Dushnilin/tachyon) ·
[sing-box-lx](https://github.com/Leadaxe/sing-box-lx) ·
[AmneziaWG](https://github.com/amnezia-vpn/amneziawg-go)

</div>
