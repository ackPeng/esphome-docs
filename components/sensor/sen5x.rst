SEN5X and SEN6X Series Environmental sensor
=================================

.. seo::
    :description: Instructions for setting up SEN5X and SEN6X Series Environmental sensor for PM, RH/T, VOC, NOx, CO2 and HCHO measurements.
    :image: sen54.png
    :keywords: Sensirion, SEN50, SEN54, SEN55, SEN5X, SEN60, SEN63C, SEN65, SEN66, SEN68

The ``sen5x`` sensor platform allows you to use your Sensirion `SEN50 <https://sensirion.com/products/catalog/SEN50/>`__, `SEN54 <https://sensirion.com/products/catalog/SEN54/>`__ , `SEN55 <https://sensirion.com/products/catalog/SEN55/>`__ , `SEN60 <https://sensirion.com/products/catalog/SEN60/>`__  , `SEN63C <https://sensirion.com/products/catalog/SEN63C/>`__  , `SEN65 <https://sensirion.com/products/catalog/SEN65/>`__  , `SEN66 <https://sensirion.com/products/catalog/SEN66/>`__  and `SEN68 <https://sensirion.com/products/catalog/SEN68/>`__  Environmental sensors with ESPHome.
The :ref:`I²C Bus <i2c>` is required to be set up in your configuration for this sensor to work.
Only I²C communication is implemented in this component.

.. _Sensirion SEN5X Series: https://sensirion.com/products/catalog/SEK-SEN5x
.. _Sensirion SEN6X Series: https://sensirion.com/sen6x-air-quality-sensor-platform


.. figure:: images/sen54.png
    :align: center
    :width: 50.0%

.. figure:: images/sen66.png
    :align: center
    :width: 50.0%

.. figure:: images/sen54-web.png
    :align: center
    :width: 100.0%

.. code-block:: yaml

    # Example configuration entry
    sensor:
      - platform: sen5x
        id: sen54
        pm_1_0:
          name: " PM <1µm Weight concentration"
          id: pm_1_0
          accuracy_decimals: 1
        pm_2_5:
          name: " PM <2.5µm Weight concentration"
          id: pm_2_5
          accuracy_decimals: 1
        pm_4_0:
          name: " PM <4µm Weight concentration"
          id: pm_4_0
          accuracy_decimals: 1
        pm_10_0:
          name: " PM <10µm Weight concentration"
          id: pm_10_0
          accuracy_decimals: 1
        temperature:
          name: "Temperature"
          accuracy_decimals: 1
        humidity:
          name: "Humidity"
          accuracy_decimals: 0
        voc:
          name: "VOC"
          algorithm_tuning:
            index_offset: 100
            learning_time_offset_hours: 12
            learning_time_gain_hours: 12
            gating_max_duration_minutes: 180
            std_initial: 50
            gain_factor: 230
        temperature_compensation:
          offset: 0
          normalized_offset_slope: 0
          time_constant: 0
        acceleration_mode: low
        store_baseline: true
        address: 0x69
        update_interval: 10s

Configuration variables:
------------------------

- **pm_1_0** (*Optional*): The information for the **Weight Concentration** sensor for fine particles up to 1μm. Readings in µg/m³.

  - All options from :ref:`Sensor <config-sensor>`.

- **pm_2_5** (*Optional*): The information for the **Weight Concentration** sensor for fine particles up to 2.5μm. Readings in µg/m³.

  - All options from :ref:`Sensor <config-sensor>`.

- **pm_4_0** (*Optional*): The information for the **Weight Concentration** sensor for coarse particles up to 4μm. Readings in µg/m³.

  - All options from :ref:`Sensor <config-sensor>`.

- **pm_10_0** (*Optional*): The information for the **Weight Concentration** sensor for coarse particles up to 10μm. Readings in µg/m³.

  - All options from :ref:`Sensor <config-sensor>`.

- **auto_cleaning_interval** (*Optional*): Reads/Writes the interval in seconds of the periodic fan-cleaning.

- **temperature** (*Optional*): Temperature.Note only available with Sen54 or Sen55. The sensor will be ignored on unsupported models.

  - All options from :ref:`Sensor <config-sensor>`.

- **humidity** (*Optional*): Relative Humidity. Note only available with Sen54 or Sen55. The sensor will be ignored on unsupported models.

  - All options from :ref:`Sensor <config-sensor>`.

- **co2** (*Optional*): Carbon dioxide (CO₂). Note: Only available with SEN63C or SEN66. The sensor will be ignored on unsupported models.
  - **auto_self_calibration** (*Optional*): True enables automatic CO₂ self calibration. False disables automatic CO₂ calibration.
  - **altitude_compensation** (*Optional*): Enable compensating deviations due to current altitude (in meters). Notice: Set altitude_compensation or ambient_pressure_compensation_source but not both.
  - **ambient_pressure_compensation_source** (*Optional*): Set an external pressure sensor ID used for ambient pressure compensation. The pressure sensor must report pressure in hPa. The correction is applied before updating the state of the CO₂ sensor.

  - All options from :ref:`Sensor <config-sensor>`.

- **voc** (*Optional*): VOC Index. Note: Only available with SEN54, SEN55, SEN65, SEN66 or SEN68. The sensor will be ignored on unsupported models.

  - **algorithm_tuning** (*Optional*): The VOC algorithm can be customized by tuning 6 different parameters. For more details see `Engineering Guidelines for SEN5X <https://sensirion.com/media/documents/25AB572C/62B463AA/Sensirion_Engineering_Guidelines_SEN5x.pdf>`__

    - **index_offset** (*Optional*): VOC index representing typical (average) conditions. Allowed values are in range 1..250. The default value is 100.
    - **learning_time_offset_hours** (*Optional*): Time constant to estimate the VOC algorithm offset from the history in hours. Past events will be forgotten after about twice the  learning time. Allowed values are in range 1..1000. The default value is 12 hour
    - **learning_time_gain_hours** (*Optional*): Time constant to estimate the VOC algorithm gain from the history in hours. Past events will be forgotten after about twice the learning time. Allowed values are in range 1..1000. The default value is 12 hours.
    - **gating_max_duration_minutes** (*Optional*): Maximum duration of gating in minutes (freeze of estimator during high VOC index signal). Zero disables the gating. Allowed values are in range 0..3000. The default value is 180 minutes
    - **std_initial** (*Optional*): Initial estimate for standard deviation. Lower value boosts events during initial learning period, but may result in larger device-todevice variations. Allowed values are in range 10..5000. The default value is 50.
    - **gain_factor** (*Optional*): Gain factor to amplify or to attenuate the VOC index output. Allowed values are in range 1..1000. The default value is 230.

  - All other options from :ref:`Sensor <config-sensor>`.

- **nox** (*Optional*): NOx Index. Note: Only available with SEN55, SEN65, SEN66 or SEN68. The sensor will be ignored on unsupported models.
  - **algorithm_tuning** (*Optional*): The NOx algorithm can be customized by tuning 5 different parameters. For more details see `Engineering Guidelines for SEN5x <https://sensirion.com/media/documents/25AB572C/62B463AA/Sensirion_Engineering_Guidelines_SEN5x.pdf>`__

    - **index_offset** (*Optional*): NOx index representing typical (average) conditions. Allowed values are in range 1..250. The default value is 100.
    - **learning_time_offset_hours** (*Optional*): Time constant to estimate the NOx algorithm offset from the history in hours. Past events will be forgotten after about twice the  learning time. Allowed values are in range 1..1000. The default value is 12 hour
    - **learning_time_gain_hours** (*Optional*): Time constant to estimate the NOx algorithm gain from the history in hours. Past events will be forgotten after about twice the learning time. Allowed values are in range 1..1000. The default value is 12 hours.
    - **gating_max_duration_minutes** (*Optional*): Maximum duration of gating in minutes (freeze of estimator during high NOx index signal). Zero disables the gating. Allowed values are in range 0..3000. The default value is 180 minutes
    - **std_initial** (*Optional*): The initial estimate for standard deviation parameter has no impact for NOx. This parameter is still in place for consistency reasons with the VOC tuning parameters command. This parameter must always be set to 50.
    - **gain_factor** (*Optional*): Gain factor to amplify or to attenuate the VOC index output. Allowed values are in range 1..1000. The default value is 230.

  - All other options from :ref:`Sensor <config-sensor>`.

- **hcho** (*Optional*): Formaldehyde (HCHO) in ppb. Note: Only available with SEN68. The sensor will be ignored on unsupported models.

  - All other options from :ref:`Sensor <config-sensor>`.

- **store_baseline** (*Optional*, boolean): Stores and retrieves the baseline VOC and NOx information for quicker startups. Note only available with SEN54, SEN55, SEN65, SEN66 and SEN68. Defaults to ``true``.
- **temperature_compensation** (*Optional*): These parameters allow to compensate temperature effects of the design-in at customer side by applying a custom temperature offset to the ambient temperature. Note: Only available with SEN54 and SEN55
. 
  The compensated ambient temperature is calculated as follows:

      T_Ambient_Compensated = T_Ambient + (slope*T_Ambient) + offset

  Where slope and offset are the values set with this command, smoothed with the specified time constant. The time constant is how fast the slope and offset are applied. After the specified value in seconds, 63% of the new slope and offset are applied.
  More details about the tuning of these parameters are included in the application note `Temperature Acceleration and Compensation Instructions for SEN5x. <https://sensirion.com/media/documents/9B9DE2A7/61E957EB/Sensirion_Temperature_Acceleration_and_Compensation_Instructions_SEN.pdf>`__


  - **offset** (*Optional*): Temperature offset [°C]. Defaults to ``0``
  - **normalized_offset_slope** (*Optional*): Normalized temperature offset slope. Defaults to ``0``
  - **time_constant** (*Optional*): Time constant in seconds. Defaults to ``0``

- **acceleration_mode** (*Optional*): Allowed value are ``low``, ``medium`` and ``high``. (default is ``low``). Note only available with SEN54 and SEN55.

  By default, the RH/T acceleration algorithm is optimized for a sensor which is positioned in free air. If the sensor is integrated into another device, the ambient RH/T output values might not be optimal due to different thermal behavior.
  This parameter can be used to adapt the RH/T acceleration behavior for the actual use-case, leading in an improvement of the ambient RH/T output accuracy. There is a limited set of different modes available.
  Medium and high accelerations are particularly indicated for air quality monitors which are subjected to large temperature changes. Low acceleration is advised for stationary devices not subject to large variations in temperature

- **address** (*Optional*, int): Manually specify the I²C address of the sensor.
  Defaults to ``0x69`` for the SEN5X sensors or ``0x6b`` for the SEN6X sensors.

.. note::

    The sensor needs about a minute "warm-up". The VOC and NOx gas index algorithm needs a number of samples before the values stabilize.


Wiring:
-------

Both the SEN5X and SEN6X sensors have a JST GHR-06V-S 6 pin type connector, with a 1.25mm pitch. The cable needs this connector:

.. figure:: images/jst6pin.png
    :align: center
    :width: 50.0%

For the SEN5X sensors:
- 1 is connected to 5V
- 2 is connected to ground
- 3 is SDA
- 4 is SCL
- 5 is SEL, must be connected to ground in order to work with this component.
- 6 is no-connect

For the SEN6X sensors:
- 1 is connected to 3.3V
- 2 is connected to ground
- 3 is SDA
- 4 is SCL
- 5 is connected to ground
- 6 is connected to 3.3V

For SEN5X sensors you must connect pin no. 5 ground enabling the I²C interface. 
Since the SEN5X sensors have a dual interface (UART/I²C) you must connect pin-5 to ground enabling the only the I²C interface. The SEN6X sensors only support an I²C interface. Pin no.5 is still connected to ground (pin no.2). Pin 6 is not used on the SEN5X sensors. But on the SEN6X sensors it can be connected VDD or (pin no. 1).

Automatic Cleaning:
-------------------

The SEN5X sensors have an automatic fan-cleaning which will accelerate the built-in fan to maximum speed for 10 seconds in order to blow out the dust accumulated inside the fan. 
The default automatic-cleaning interval is 168 hours (1 week) of uninterrupted use. Switching off the sensor resets this time counter. 
When the module is in Measurement-Mode an automatic fan-cleaning procedure will be triggered periodically following a defined cleaning interval. This will accelerate the fan to maximum speed for 10 seconds to blow out the accumulated dust inside the fan.

- Measurement values are not updated while the fan-cleaning is running.
- The cleaning interval is set to 604,800 seconds (i.e., 168 hours or 1 week).
- The interval can be configured using the Set Automatic Cleaning Interval command.
- Set the interval to 0 to disable the automatic cleaning.
- A sensor reset, resets the cleaning interval to its default value
- If the sensor is switched off, the time counter is reset to 0. Make sure to trigger a cleaning cycle at least every week if the sensor is switched off and on periodically (e.g., once per day).
- The cleaning procedure can also be started manually with the ``start_autoclean_fan`` Action

The SEN6X sensor supports fan cleaning but not the automatic fan cleaning interval.

.. _start_fan_autoclean_action:

``sen5x.start_fan_autoclean`` Action
------------------------------------

This :ref:`action <config-action>` manually starts a fan-cleaning cycle .

.. code-block:: yaml
      on_...:
        then:
          - sen5x.start_fan_autoclean: my_sen54

CO₂ Calibration and Compensation:
-------------------
The CO₂ sensor by default has auto-calibration enabled. Auto-calibration will adjust the minimum measurement over the last week or so to the outdoor average of slightly more than 400 ppm. 
Auto-calibration assumes that you are opening the windows at least once a week. If you don't open the windows then over time the CO₂ level will tend downward. 
For example, over the last week the actual CO₂ minimum was 600 ppm. Auto-calibration will make that 400 ppm which is actually low by 200 ppm.

If you know your minimums are not going to be 400 ppm then you can disable auto-calibration, and occasionally take the sensor outside for 5 minutes and then force a manual CO₂ calibration.

Only the SEN63C and the SEN66 have a CO₂ sensor.

.. _perform_forced_co2_calibration:

``sen5x.perform_forced_co2_calibration`` Action
------------------------------------

This :ref:`action <config-action>` forces a manual calibration on the CO₂ sensor.

.. code-block:: yaml
  number:
    - platform: template
      id: co2_forced_cal_value
      name: "CO2 Calibration Value"
      device_class: carbon_dioxide
      entity_category: CONFIG
      optimistic: true
      max_value: 1200
      min_value: 400
      step: 1
      initial_value: 420
      set_action:
        - delay: 1s
  button:
    - platform: template
      name: "CO2 Calibrate"
      entity_category: CONFIG
      on_press:
        - sen5x.perform_forced_co2_calibration:
            value: !lambda |-
              float value = id(co2_forced_cal_value).state;
              return value;
            id: sen66_sensor

The CO₂ sensor also supports pressure compensation. You can either add ``ambient_pressure_compensation_source`` to your configuration or you can occasionally call the ``sen5x.set_ambient_pressure_compensation`` action.

.. _set_ambient_pressure_compensation:

``sen5x.set_ambient_pressure_compensation`` Action
------------------------------------

This :ref:`action <config-action>` updates the current pressure used in CO₂ pressure compensation. Must be hPa or mbar.

.. code-block:: yaml
  sensor:
    - platform: copy
      id: pressure_to_sen6x
      source_id: pressure
      unit_of_measurement: hPa
      filters:
        - lambda: |-
            // convert Pa to hPa (or mBar)
            return x / 100.0;
      on_value:
        then:
          - lambda: !lambda |-
              id(sen66_sensor)->set_ambient_pressure_compensation(x);


See Also
--------

- :ref:`sensor-filters`
- :doc:`absolute_humidity`
- :doc:`sds011`
- :doc:`pmsx003`
- :doc:`ccs811`
- :doc:`scd4x`
- :doc:`sps30`
- :doc:`sgp4x`
- :doc:`sht4x`
- :apiref:`sen5x/sen5x.h`
- :ghedit:`Edit`
