from grovepi import *
from grove_rgb_lcd import *
from time import sleep
from math import isnan
import MySQLdb
conexion = MySQLdb.connect(host='localhost',user='pruebaht_user',passwd='1234',db='pruebaht')
curs = conexion.cursor()
dht_sensor_port = 7 # connect the DHt sensor to port 7
dht_sensor_type = 0 # use 0 for the blue-colored sensor and 1 for the white-colored sensor
# set green as backlight color
# we need to do it just once
# setting the backlight color once reduces the amount of data transfer over the I2C line
setRGB(0,255,0)
while True:
    try:
        [temp,hum] = dht(dht_sensor_port,dht_sensor_type)
        print("temp =", temp, "C\thumidity =", hum,"%")
        if isnan(temp) is True or isnan(hum) is True:
            raise TypeError('nan error')
        t = temp
        h = hum
        setText_norefresh("Temp:" + str(t) + "C\n" + "Humidity :" + str(h) + "%")
        curs.execute("insert into datos (temperatura, humedad) values (%s, %s);", (t,h))
        

    except (IOError, TypeError) as e:
        print(str(e))
        setText("")
    except KeyboardInterrupt as e:
        print(str(e))
        setText("")
        break

    sleep(0.05)
    conexion.commit()
