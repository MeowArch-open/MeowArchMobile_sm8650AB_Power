# MeowArchMobile SM8650 Power

Linux power-state support for Qualcomm SM8650 devices in MeowArchMobile,
currently validated on Xiaomi zorn (Redmi K80).

The repository supplies an in-tree `qcom_battmgr` override through the standard
MeowArchMobile subsystem layout:

```text
source/kernel/drivers/power/supply/qcom_battmgr.c
```

The driver keeps charging policy in Qualcomm's `charger_pd` firmware and only
exposes standard Linux `power_supply` state. The SM8650 path provides:

- the zorn battery-property layout and units;
- Linux-standard charging current polarity;
- guarded state-of-charge reporting during cable-event firmware transients;
- battery, USB and wireless power-supply devices;
- normalized standard USB types, including conservative handling of Qualcomm
  private HVDCP and floating-charger values;
- USB connector temperature through `POWER_SUPPLY_PROP_TEMP`; and
- strict response matching so unsolicited Xiaomi status uploads cannot satisfy
  an unrelated battery or USB request.

It intentionally contains no Xiaomi authentication, UVDM challenge-response,
fast-charge classification, PPS/charge-pump policy, generic property writer, or
other active charging control. Firmware-default charging remains in effect.

## Integration

`MeowArchMobile_Builder` copies this repository's `source/kernel` overlay after
checking out the locked `MeowArchMobile_Kernel` revision. Enable
`CONFIG_BATTERY_QCOM_BATTMGR=y` in the Builder kernel fragment.

## License

The kernel driver is licensed under GPL-2.0-only. See `LICENSE`.
