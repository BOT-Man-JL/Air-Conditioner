# ͨ��Э��

> 2017 ������� F ��

## Json Spec

``` json
PACKET = JSON_REQ | JSON_RES \0
```

> ����� socket �ַ����� ���ַ� '\0' ����

## �ӿػ����� (Request)

``` json
JSON_REQ = {"request":ACTION, "param":PARAM}

ACTION = "auth" | "pulse"

1) ACTION = "auth"
  PARAM = {"room":ROOM_ID, "guest":GUEST_ID}
2) ACTION = "pulse"
  PARAM = {"room":ROOM_ID, "current":TEMP, "target":TEMP, "wind":WIND}

ROOM_ID = [string]
GUEST_ID = [string]
TEMP = [double]
WIND = 1 | 2 | 3
```

## ���ػ����� (Response)

``` json
JSON_RES = {"success":SUCCESS, "response":RESPONSE}

SUCCESS = true | false

1) SUCCESS = false
  RESPONSE = ERR_MSG
2) SUCCESS = true
  RESPONSE = {"hasWind":HAS_WIND, "energy":ENERGY, "cost":COST,
              "on":SERVER_ON, "mode":MODE}

ERR_MSG = [string]
HAS_WIND = true | false
ENERGY = [double]
COST = [double]
SERVER_ON = true | false
MODE = 0 | 1
```