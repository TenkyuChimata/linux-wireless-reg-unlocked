# linux-wireless-reg-unlocked

Patched Intel **iwlwifi** and Linux **cfg80211/regulatory** components to relax wireless restrictions and expose additional capabilities.

This project provides external kernel modules for Arch Linux that modify both the Intel iwlwifi driver and the cfg80211 regulatory layer, allowing advanced users greater control over Wi-Fi behavior without rebuilding the full kernel.

---

## 🚀 Features

- Add `lar_disable` module parameter to iwlwifi
- Disable Intel iwlwifi LAR (Location Aware Regulatory)
- Expose channels normally filtered by NVM validity checks
- Remove multiple bandwidth and transmit restrictions
- Remove `NO_IR` limitations (allow AP / initiating transmissions)
- Force-enable 6 GHz VLP AP capability
- Reduce cfg80211 regulatory intersection enforcement
- Ignore driver and Country-IE regulatory hints
- Built as external kernel modules (no full kernel rebuild required)

---

## ⚠️ Disclaimer

> 🚨 IMPORTANT

This project intentionally bypasses regulatory enforcement mechanisms.

- May violate local wireless communication laws
- May cause interference with licensed spectrum users
- Not certified for production or commercial use

Use at your own risk. You are responsible for complying with local regulations.

---

## 📥 Installation

### Manual build

```bash
git clone https://github.com/TenkyuChimata/linux-wireless-reg-unlocked.git
cd linux-wireless-reg-unlocked
makepkg -si
```

---

## 🔧 Usage

Reboot after installation to load the modified modules.

This package installs a modprobe configuration enabling:

```bash
options iwlwifi lar_disable=Y
```

Verify module parameter:

```bash
modinfo iwlwifi | grep lar_disable
```

Check regulatory state:

```bash
iw reg get
iw phy
```

You may still manually set a regulatory domain:

```bash
sudo iw reg set JP
```

---

## 📡 What is modified

### iwlwifi layer

- Optional LAR disable via module parameter
- Skip selected invalid channel filtering
- Remove NO_IR-related transmit restrictions
- Relax bandwidth and regulatory capability checks
- Force-enable 6 GHz VLP AP capability

### cfg80211 layer

- Bypass regulatory domain intersection logic
- Ignore driver-provided regulatory hints
- Ignore Country IE regulatory updates

---

## 🧩 How it works

This project patches:

- `drivers/net/wireless/intel/iwlwifi/*`
- `drivers/net/wireless/intel/iwlwifi/mvm/*`
- `net/wireless/reg.c`

Key behavior changes include:

- Treat LAR as disabled when requested
- Expose channels normally filtered out
- Remove multiple regulatory-based restrictions
- Force-enable selected 6 GHz capabilities
- Bypass cfg80211 regulatory merging and overrides

---

## 🧪 Tested hardware

- Intel AX211

---

## ⚠️ Known limitations

- 5 GHz and 6 GHz AP operation may still be restricted by firmware
- Hardware/firmware may prevent actual transmission on some channels
- Displayed capabilities may exceed real hardware capability
- Behavior may vary across different Intel chipsets and firmware versions
- Future kernel updates may break patch compatibility
