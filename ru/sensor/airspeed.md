# Датчики воздушной скорости

Airspeed sensors are *highly recommended* for fixed-wing and VTOL frames. They are so important because the autopilot does not have other means to detect stall. For fixed-wing flight it is the airspeed that guarantees lift not ground speed!

![Цифровой датчик воздушной скорости](../../assets/hardware/sensors/airspeed/digital_airspeed_sensor.jpg)

## Hardware Options

Рекомендуемые цифровые датчики скорости относятся:

- Based on [Pitot tube](https://en.wikipedia.org/wiki/Pitot_tube)
  - MEAS Spec series (e.g. [MS4525DO](https://www.te.com/usa-en/product-CAT-BLPS0002.html), [MS5525](https://www.te.com/usa-en/product-CAT-BLPS0003.html))
    - [mRo I2C Airspeed Sensor JST-GH MS4525DO](https://store.mrobotics.io/mRo-I2C-Airspeed-Sensor-JST-GH-p/m10030a.htm) (mRo store)
    - [Digital Differential Airspeed Sensor Kit](https://store-drotek.com/793-digital-differential-airspeed-sensor-kit-.html) (Drotek).
  - [EagleTree Airspeed MicroSensor V3](http://www.eagletreesystems.com/index.php?route=product/product&product_id=63) (eagletreesystems)
  - [Sensirion SDP3X Differential Pressure Sensor]([https://www.sensirion.com/en/flow-sensors/differential-pressure-sensors/worlds-smallest-differential-pressure-sensor/](https://store-drotek.com/848-sdp3x-airspeed-sensor-kit-sdp33.html))
  - [Holybro Digital Air Speed Sensor](https://shop.holybro.com/digital-air-speed-sensor_p1029.html)
- Based on [Venturi effect](https://en.wikipedia.org/wiki/Venturi_effect)
  - [TFSLOT](./airspeed_tfslot.md) Venturi effect airspeed sensor.

All the above sensors are connected via the I2C bus/port.

:::note
Additionally, the [Avionics Anonymous Air Data Computer](https://www.tindie.com/products/avionicsanonymous/uavcan-air-data-computer-airspeed-sensor/) can be connected to the CAN bus to determine not only high-accuracy airspeed, but also true static pressure and air temperature via onboard barometer and an OAT probe.
:::

## Настройка

### Enable Airspeed Sensors

Airspeed sensor drivers are not started automatically. Enable each type using its [corresponding parameter](../advanced_config/parameters.md):

- **Sensirion SDP3X:** [SENS_EN_SDP3X](../advanced_config/parameter_reference.md#SENS_EN_SDP3X)
- **TE MS4525:** [SENS_EN_MS4525DO](../advanced_config/parameter_reference.md#SENS_EN_MS4525DO)
- **TE MS5525:** [SENS_EN_MS5525DS](../advanced_config/parameter_reference.md#SENS_EN_MS5525DS)
- **Eagle Tree airspeed sensor:** [SENS_EN_ETSASPD](../advanced_config/parameter_reference.md#SENS_EN_ETSASPD)

You should also check [ASPD_PRIMARY](../advanced_config/parameter_reference.md#ASPD_PRIMARY) is `1` (see next section - this is the default).

### Multiple Airspeed Sensors

If you have multiple airspeed sensors then you can select which sensor is _preferred_ as the primary source using [ASPD_PRIMARY](../advanced_config/parameter_reference.md#ASPD_PRIMARY), where `1`, `2` and `3` reflect the order in which the airspeed sensors were started:
- `-1`: Disabled (no airspeed information used).
- `0`: Synthetic airspeed estimation (groundspeed minus windspeed)
- `1`: First airspeed sensor started (default)
- `2`: Second airspeed sensor started
- `3`: Third airspeed sensor started

The airspeed selector validates the indicated sensor _first_ and only falls back to other sensors if the indicated sensor fails the checks. The selected sensor is then used to supply data to the estimator (EKF2).

### Sensor-specific Configuration

Other than enabling the sensor, sensor-specific configuration is often not required. If it is needed, it should be covered in the appropriate sensor page (for example [TFSLOT > Configuration](./airspeed_tfslot.md\#configuration)).

For sensors that require specific configuration and that do not have a page in this guide you may need to set the following parameters:

- **Sensirion SDP3X:** [CAL_AIR_CMODEL](../advanced_config/parameter_reference.md#CAL_AIR_CMODEL) (provides overview of required settings), [CAL_AIR_TUBED_MM](../advanced_config/parameter_reference.md#CAL_AIR_TUBED_MM), [CAL_AIR_TUBELEN](../advanced_config/parameter_reference.md#CAL_AIR_TUBELEN).

## Calibration

Airspeed sensors should be calibrated by following the instructions: [Basic Configuration > Airspeed](../config/airspeed.md).

## Информация для разработчиков

- [Airspeed drivers](https://github.com/PX4/PX4-Autopilot/tree/main/src/drivers/differential_pressure) (source code)
