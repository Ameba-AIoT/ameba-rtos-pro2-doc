Image sensor
=================================

.. contents::
  :local:
  :depth: 2

For Mipi sensor application, please refer below.

GPIOE_0 --> sensor_RSTB

SEN_PDN --> sensor_PDN

GPIOA_5 --> sensor_PWR_EN

SEN_SCL --> I2C_SCL

SEN_SDA --> I2C_SDA

SEN_SYSCLK --> sensor clock

Pull high I2C by 4.7k ohms

.. image:: ../_hw_static/06_Image_sensor/06_Image_sensor1.png


Image sensor level shifter

For 1.8V sensor IO level shifter, please refer below.

Sen_RSTB --> use 4.7k, 5.6K ohms

Sen_PDN --> use 4.7k, 5.6K ohms

SEN_SCL --> I2C_SCL  NTJD4401N/SM1600DSCS, PH 4.7k ohms

SEN_SDA --> I2C_SDA NTJD4401N/SM1600DSCS, PH 4.7k ohms

SEN_SYSCLK --> use 470, 560 ohms

.. image:: ../_hw_static/06_Image_sensor/06_Image_sensor2.png

.. image:: ../_hw_static/06_Image_sensor/06_Image_sensor3.png

