Rule1 ON Power1#state=0 DO Websend [192.168.1.34] Power 1 ENDON

Rule1 1

-------------------------
Modified Rule (27/4/2014)

Rule1 ON Power1#state=0 DO Power2 1 ENDON

Rule1 1

### To open Switch 2 when Swith 1 is changing states (i.e when toggle) (to test)
Rule1 ON Switch1#state=2 DO Power2 1 ENDON
