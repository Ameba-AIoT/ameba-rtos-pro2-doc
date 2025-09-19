DC BUCK
============

.. contents::
  :local:
  :depth: 2

DC BUCK should be follow RTK recommended components as below plot, and PN.

Recommended DC BUCK

MT8102NSBR

MP1605(non P2P)

TPS62822(non P2P)

.. image:: ../_hw_static/03_DC_BUCK/03_DC_BUCK1.png

DC BUCK is highly correlated with system stability, RF performance, and power consumption. 
It requires extensive testing and verification. Replacing components is not recommended. 
Customers who insist on modifying must meet the following conditions and conduct their own testing and assume the consequences.


3.3V DC-DC Power Specification

 
Must meet:

(1) When Vin≧3.6V ; Iout=0A, then Vout=3.3V±3% and Vout ripple peak to peak < 100mV

(2) When Vin≧3.6V ; Iout=1A , then Vout=3.3V±3% and Vout ripple peak to peak < 100mV

(3) When Iout=0A→1A ; Electrical load switch frequency = 500Hz, Electrical load slew rate setting=2.5A/us, then Vout ripple peak to peak < 100mV

(4) When Iout=1A→0A ; Electrical load switch frequency = 500Hz, Electrical load slew rate setting=2.5A/us, then Vout ripple peak to peak < 100mV

(5) For battery application, when Vin=3.4V ~ 4.2V then Vout should be stable.

(6) Have Auto-Discharge function.

(7) Have Enable pin function to reset AmebaPro.

(8) Soft start time < 1.2ms

 

Recommend:

(1) Constant-on-time or DCS topology

(2) For battery application, Low IQ and good power efficiency.

(3) Duty ratio could be 100%

(4) Iout Max. rating > 1.5A
