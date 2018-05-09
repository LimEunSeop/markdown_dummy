
```python
# 서울 지역의 미세먼지 농도를 구한다.
def getFinedust(location):

    url = "http://openapi.seoul.go.kr:8088/54775266536e6564393578574d4c41/json/TimeAverageAirQuality/1/25/"
    cur_datetime = datetime.today().strftime("%Y%m%d%H") + "00"

    while True: # 해당 시간의 정보가 없을경우 될때까지 루프돌리기
        uri = url + cur_datetime
        response_body = requests.get(uri).json()
        if response_body.has_key('TimeAverageAirQuality'): # True면 데이터가 존재하는것.
            break
        cur_datetime = str(int(cur_datetime) - 100) # 한시간 전의 데이터가 있는지 다시 확인

    # 해당 지역의 미세먼지 농도 읽기
    items = response_body['TimeAverageAirQuality']['row']
    for item in items:
        if location in item['MSRSTE_NM']:
            pm10 = int(item['PM10'])
            pm25 = int(item['PM25'])
            break
    else:
        return '지역없음 : ' + location # loop에 break가 없었다는 것은 지역이 없다는 뜻

    # 미세먼지 정보 헤더 메세지
    ret_msg = location + " 미세먼지 정보 (%s-%s-%s %s시 기준)\n" % (cur_datetime[0:4], cur_datetime[4:6], cur_datetim$
                         "PM10 : " + str(pm10)

    # 미세먼지(PM10) 농도 및 상태 세팅
    ret_msg += "PM10 : " + str(pm10)
    if pm10 > 150:
        status = " (매우나쁨)"
    elif pm10 > 80:
        status = " (나쁨)"
    elif pm10 > 30:
        status = " (보통)"
    else:
        status = " (좋음)"
    ret_msg = ret_msg + status

    # 초미세먼지(PM25) 농도 및 상태 세팅
    ret_msg = ret_msg + "\nPM25 : " + str(pm25)
    if pm25 > 75:
        status = " (매우나쁨)"
    elif pm25 > 35:
        status = " (나쁨)"
    elif pm25 > 15:
        status = " (보통)"
    else:
        status = " (좋음)"
    ret_msg = ret_msg + status

    return ret_msg
```
