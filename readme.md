# Introduction

Сбор данных из 2chargers

# How to - reengeniring
Нужно, если вдруг изменилось что-то или новый функционал добавили
1. android (лучше старый)
2. установить apk 2chargers
3. установить tPacketCapture
4. собирать по нужным фичам пакеты трафика в формате pcap
5. разбирать их в wireshark 

## API по состоянию на 12_2022

- описание коннекторов, компаний и протокола
https://2chargers.net/api/filters

- еще какой-то запросик, про мощности зарядок
https://2chargers.net/api/filters/occupy

- все машины с коннекторами и всякое такое
https://2chargers.net/api/cars

- кампании с описанием, телефонами, приложениями
https://2chargers.net/api/companies

- запрос станций
https://2chargers.net/api/addresses

с жсоном (там всякие фильтры по коннекторам, по компаниям + география), кажется, выдача ограничена 500 адресами
{"userCoordinates":[[2.183847973273408,2.433936707675457],[79.08407537418269,76.8307976424694]],"connectorIds":[],"availableIds":[0,1,2,3],"companyIds":[45,44,42,41,40,39,38,37,36,35,34,32,31,30,29,28,27,26,25,24,23,22,21,19,18,17,16,13,12,11,10,8,7,6,5,4,3,2,1],"connectors":[{"id":2,"type_id":0},{"id":37,"type_id":0},{"id":38,"type_id":0},{"id":3,"type_id":0},{"id":39,"type_id":0},{"id":41,"type_id":0},{"id":40,"type_id":0},{"id":1,"type_id":0},{"id":50,"type_id":0},{"id":7,"type_id":0},{"id":4,"type_id":0},{"id":17,"type_id":0},{"id":42,"type_id":0},{"id":5,"type_id":0},{"id":13,"type_id":0},{"id":12,"type_id":0},{"id":9,"type_id":0},{"id":23,"type_id":0},{"id":24,"type_id":0},{"id":25,"type_id":0},{"id":26,"type_id":0},{"id":14,"type_id":0},{"id":11,"type_id":0},{"id":10,"type_id":0},{"id":49,"type_id":0},{"id":30,"type_id":0},{"id":44,"type_id":0},{"id":8,"type_id":0},{"id":19,"type_id":0},{"id":29,"type_id":0},{"id":6,"type_id":0},{"id":33,"type_id":0},{"id":34,"type_id":0},{"id":31,"type_id":0},{"id":32,"type_id":0},{"id":35,"type_id":0},{"id":15,"type_id":0},{"id":43,"type_id":0},{"id":16,"type_id":0},{"id":18,"type_id":0},{"id":36,"type_id":0},{"id":28,"type_id":0},{"id":27,"type_id":0}],"stationTypeIds":[],"zoomLevel":3.0,"onlyFast":false,"car_id":null,"kilowattRange":{"min":0.0,"max":0.0},"showStationsWithoutKwt":false,"withoutCompany":false,"hideDeleted":false}

пример станции

- по конкретной станции собирается так (post/get)
https://2chargers.net/api/addresses/8761/stations

- запрос трафика по дню недели (get), в конце цифра - номер для недели
https://2chargers.net/api/addresses/8761/traffic/1

- не знаю что это (get)
https://2chargers.net/api/addresses/8761/active-marks

- общая инфа по адресу, без мощностей (get)
https://2chargers.net/api/addresses/8761

- запрос поиска станций, get
https://2chargers.net/api/addresses/search-with-geo
{"query":"Москва","search_in_user_marks":false,"search_in_address_description":false,"order_by_distance":true}

## Что есть
- pcap файлы - траффик из приложения, открывать wireshark
- 20_11_2022 - собранные данные
- всякие csv - всякое собранное по разным методам
- parse.ipynb - этим все собирал

## Как работает

1. - prep segments 
    - потом надо joblib.dump(new_segments, 'segments_17_07_2023')

2. - scan locations
    - на выходе сохраняет csv ru_station_locations.csv - там локации в рф

3. - scan stations on locations
    - по каждой локации смотрит станции, на выходе сохранят файл all_stations.csv

4. - prep formatted output
    - тут возможно реформаттинг под требования + фильтрация лишнего, на выходе stations_formatted.csv