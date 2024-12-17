The network operates on iBGP, but is ready for OSPF or IS-IS protocols.
Схема модифицировна для исключения Split Brain: RR только на Spine коммутаторах, для максимальной надежности добавлена перемычка между обоими Spine.
Files:
shema-seti_max-reliability.png - Схема сети.
ip-plan_BGP_(+OSPF+ISIS)-max-reliability.txt - общий IP план для BGP и протколов OSPF и IS-IS.
config_all_max-reliability.txt - конфигурации всех коммутаторов.
show_max-reliability.txt - просмотр состояний BGP, ip маршрутов, в т.ч. при отказе линков.


<img width="547" alt="shema-seti_max-reliability" src="https://github.com/user-attachments/assets/073e8b3c-00bd-461a-91ce-ded09b5c5c28" />




