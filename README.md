# apr/03/2020 20:02:37 by RouterOS 6.46.4
# software id = Q1ER-B172
#
# model = RouterBOARD 750G r3
# serial number = 6F3806B37839
/interface ethernet
set [ find default-name=ether1 ] speed=100Mbps
set [ find default-name=ether2 ] speed=100Mbps
set [ find default-name=ether3 ] speed=100Mbps
set [ find default-name=ether4 ] speed=100Mbps
set [ find default-name=ether5 ] speed=100Mbps
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip firewall layer7-protocol
add name=torrent regexp="^.+(Torrent|torrent)"
/ip hotspot profile
set [ find default=yes ] html-directory=flash/hotspot
add dns-name=cpr.net hotspot-address=192.168.5.1 html-directory=flash/hotspot \
    name=hsprof1
/ip pool
add name=dhcp_pool0 ranges=192.168.2.2-192.168.2.254
add name=dhcp_pool1 ranges=192.168.5.2-192.168.5.254
/ip dhcp-server
add address-pool=dhcp_pool0 authoritative=after-2sec-delay disabled=no \
    interface=ether3 name=dhcp1
add address-pool=dhcp_pool1 authoritative=after-2sec-delay disabled=no \
    interface=ether4 name=dhcp2
/ip hotspot
add address-pool=dhcp_pool1 disabled=no interface=ether4 name=hotspot1 \
    profile=hsprof1
/queue tree
add max-limit=100M name=INCOMING parent=global queue=default
add max-limit=100M name=A.1.PAKET-TRAFIK parent=INCOMING queue=default
add max-limit=20M name=A.1.1.DNS packet-mark=vip-down parent=A.1.PAKET-TRAFIK \
    priority=1 queue=default
add max-limit=10M name=A.1.3.GAMES-ONLINE packet-mark=download-game parent=\
    A.1.PAKET-TRAFIK priority=1 queue=default
add max-limit=20M name=A.1.5.NORMAL parent=A.1.PAKET-TRAFIK queue=default
add max-limit=20M name=A.1.4.1.BROWSING packet-mark=browsing-down parent=\
    A.1.5.NORMAL priority=4 queue=pcq-download-default
add max-limit=20M name=A.1.4.2.LOW packet-mark=low-down parent=A.1.5.NORMAL \
    priority=5 queue=pcq-download-default
add max-limit=20M name=A.1.4.3.MIDLE packet-mark=midle-down parent=\
    A.1.5.NORMAL priority=6 queue=pcq-download-default
add max-limit=20M name=A.1.4.4.HIGH packet-mark=high-down parent=A.1.5.NORMAL \
    priority=7 queue=pcq-download-default
add max-limit=20M name=A.1.4.5.UNKNWON packet-mark=unknown-down parent=\
    A.1.5.NORMAL priority=7 queue=pcq-download-default
add max-limit=15M name=A.2.GGC-TELKOM packet-mark=ggc-telkom-down parent=\
    INCOMING queue=pcq-download-default
add max-limit=20M name=OUTGOING parent=global queue=default
add max-limit=20M name=B.1.PAKET-TRAFIK parent=OUTGOING queue=default
add max-limit=2M name=B.1.1.VIP packet-mark=vip-up parent=B.1.PAKET-TRAFIK \
    priority=1 queue=default
add max-limit=2M name=B.1.2.GAMES-ONLINE packet-mark=upload-game parent=\
    B.1.PAKET-TRAFIK priority=2 queue=default
add max-limit=20M name=B.1.4.NORMAL parent=B.1.PAKET-TRAFIK queue=default
add max-limit=20M name=B.1.4.1.BROWSING packet-mark=browsing-up parent=\
    B.1.4.NORMAL priority=4 queue=pcq-upload-default
add max-limit=20M name=B.1.4.2.LOW packet-mark=low-up parent=B.1.4.NORMAL \
    priority=5 queue=pcq-upload-default
add max-limit=20M name=B.1.4.3.MIDLE packet-mark=midle-up parent=B.1.4.NORMAL \
    priority=6 queue=pcq-upload-default
add max-limit=20M name=B.1.4.4.HIGH packet-mark=high-up parent=B.1.4.NORMAL \
    priority=7 queue=pcq-upload-default
add max-limit=20M name=B.1.4.5.UNKNWON packet-mark=unknown-up parent=\
    B.1.4.NORMAL priority=7 queue=pcq-upload-default
add max-limit=20M name=B.2.GGC-TELKOM packet-mark=ggc-telkom-up parent=\
    OUTGOING queue=pcq-upload-default
add max-limit=20M name="A.1.2. PING+LATENCY" packet-mark=ICMP-PACKET parent=\
    A.1.PAKET-TRAFIK priority=1 queue=default
add max-limit=10M name=A.1.4.GAMES-ONLINE2 packet-mark=download-game2 parent=\
    A.1.PAKET-TRAFIK priority=1 queue=default
add max-limit=2M name=B.1.3.GAMES-ONLINE2 packet-mark=upload-game2 parent=\
    B.1.PAKET-TRAFIK priority=2 queue=default
/ip address
add address=192.168.2.1/24 interface=ether3 network=192.168.2.0
add address=192.168.5.1/24 interface=ether4 network=192.168.5.0
/ip dhcp-client
add disabled=no interface=ether1
add add-default-route=no disabled=no interface=ether2
/ip dhcp-server network
add address=192.168.2.0/24 gateway=192.168.2.1
add address=192.168.5.0/24 gateway=192.168.5.1
/ip dns
set allow-remote-requests=yes servers=1.1.1.1,8.8.8.8
/ip firewall address-list
add address=192.168.1.0/24 list=private-lokal
add address=192.168.2.0/24 list=private-lokal
add address=192.168.3.0/24 list=private-lokal
add address=192.168.4.0/24 list=private-lokal
add address=192.168.5.0/24 list=private-lokal
add address=192.168.6.0/24 list=private-lokal
add address=192.168.7.0/24 list=private-lokal
add address=192.168.8.0/24 list=private-lokal
add address=192.168.9.0/24 list=private-lokal
add address=127.0.0.0/8 list=private-lokal
add address=169.254.0.0/16 list=private-lokal
add address=172.16.0.0/12 list=private-lokal
add address=192.0.0.0/24 list=private-lokal
add address=192.0.2.0/24 list=private-lokal
add address=192.168.0.0/16 list=private-lokal
add address=198.18.0.0/15 list=private-lokal
add address=198.51.100.0/24 list=private-lokal
add address=203.0.113.0/24 list=private-lokal
add address=224.0.0.0/3 list=private-lokal
add address=10.5.50.0/24 list=private-lokal
add address=74.125.200.188 list=games
add address=74.125.24.188 list=games
add address=157.240.218.61 list=games
add address=157.240.2.54 list=games
add address=34.193.38.112 list=games
add address=74.125.68.188 list=games
add address=173.194.66.188 list=games
/ip firewall filter
add action=passthrough chain=unused-hs-chain comment=\
    "place hotspot rules here" disabled=yes
/ip firewall mangle
add action=mark-connection chain=prerouting dst-address-list=games \
    new-connection-mark=GAME_TRAFFIC passthrough=yes src-address-list=\
    private-lokal
add action=mark-packet chain=forward in-interface=ether2 new-packet-mark=\
    download-game packet-size=0-512 passthrough=no
add action=mark-packet chain=forward in-interface=ether2 new-packet-mark=\
    download-game2 packet-size=513-2048 passthrough=no
add action=mark-packet chain=forward connection-bytes=1-256000 \
    new-packet-mark=upload-game out-interface=ether2 passthrough=no
add action=mark-packet chain=forward connection-bytes=256000-5000000 \
    new-packet-mark=upload-game2 out-interface=ether2 passthrough=no
add action=mark-routing chain=prerouting connection-mark=GAME_TRAFFIC \
    in-interface=!ether1 new-routing-mark=isp-game passthrough=yes
add action=mark-connection chain=prerouting comment=private-lokal \
    dst-address-list=private-lokal new-connection-mark=private-lokal \
    passthrough=yes src-address-list=private-lokal
add action=accept chain=prerouting comment=private-lokal connection-mark=\
    private-lokal dst-address-list=private-lokal src-address-list=\
    private-lokal
add action=mark-connection chain=prerouting new-connection-mark="ICMP LOKAL" \
    passthrough=yes protocol=icmp
add action=mark-packet chain=prerouting connection-mark="ICMP LOKAL" \
    new-packet-mark=ICMP-PACKET passthrough=yes
add action=mark-connection chain=prerouting comment=vip dst-address-list=\
    !private-lokal new-connection-mark=vip passthrough=yes protocol=icmp \
    src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=dns dst-address-list=\
    !private-lokal dst-port=53,5353,123,1194 new-connection-mark=vip \
    passthrough=yes protocol=tcp src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=dns dst-address-list=\
    !private-lokal dst-port=53,5353,123,1194 new-connection-mark=vip \
    passthrough=yes protocol=udp src-address-list=private-lokal
add action=accept chain=prerouting comment=vip connection-mark=vip
add action=mark-connection chain=prerouting comment=jump1 dst-address-list=\
    !private-lokal dst-port=\
    !21,22,23,80,81,88,5050,843,443,182,282,8777,1935,8000-8081 \
    layer7-protocol=!torrent new-connection-mark=jump1 passthrough=yes \
    protocol=tcp src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=jump1 dst-address-list=\
    !private-lokal dst-port=\
    !21,22,23,80,81,88,5050,843,443,182,282,8777,1935,8000-8081 \
    layer7-protocol=!torrent new-connection-mark=jump1 passthrough=yes \
    protocol=udp src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=jump11 connection-mark=\
    jump1 dst-address-list=!private-lokal dst-port=\
    !8030-8039,2222,5900-5911,1701-1723,8123,1194,8012,8123,8728 \
    layer7-protocol=!torrent new-connection-mark=jump11 passthrough=yes \
    protocol=tcp src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=jump11 connection-mark=\
    jump1 dst-address-list=!private-lokal dst-port=\
    !8030-8039,2222,5900-5911,1701-1723,8123,1194,8012,8123,8728 \
    layer7-protocol=!torrent new-connection-mark=jump11 passthrough=yes \
    protocol=udp src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=games connection-mark=\
    jump11 dst-address-list=!private-lokal dst-port=\
    !53,5353,5938,8291,12671-12675 layer7-protocol=!torrent \
    new-connection-mark=games passthrough=yes protocol=tcp src-address-list=\
    private-lokal
add action=mark-connection chain=prerouting comment=games connection-mark=\
    jump11 dst-address-list=!private-lokal dst-port=\
    !53,5353,5938,8291,12671-12675 layer7-protocol=!torrent \
    new-connection-mark=games passthrough=yes protocol=udp src-address-list=\
    private-lokal
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port=\
    339,365,396,678,779,900,983,2001-2010,2080-2099,5000-5508 protocol=tcp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port=\
    5551-5558,5601-5608,5651-5658,6000-6125,6170-6180,6695-6699,7020-7030 \
    protocol=tcp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port=\
    7090-7100,7320-7350,7770-7790,7800,9000-9999,9122,9330-9340,9930-9940 \
    protocol=tcp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port="9933,10000-10\
    010,10001-10094,10005-10020,10009,10012,10500-10515,11000-11150,11031" \
    protocol=tcp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port="12000-12010,1\
    4000-14050,14300-14440,17500,18900-18910,22100,27000-28998,27780" \
    protocol=tcp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port="27920-27940,2\
    8001-28010,29000,30097-30147,39190-39200,44590-44610,49001-49190,61000,620\
    00" protocol=tcp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port=\
    4360-4390,4950-4955,5100,5355,7008,7020-7050,7086-7995,7800-7850,8001 \
    protocol=udp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port=\
    8200-8220,9000-9020,9000-9999,9330-9340,10000-10007,10010,10013,10039 \
    protocol=udp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port="10080-10110,1\
    0096,10101-10201,10491,10612,11100-11125,11440-11460,11455,12060-12070" \
    protocol=udp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port="12070-12460,1\
    2235,13748,13894,13972,14000-14050,15000-15500,16300-16350,17000-18000" \
    protocol=udp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port="20000,20001,2\
    0002,24000-24050,26001-26010,27000-28998,30000,30000-30030,30100-30110" \
    protocol=udp
add action=add-dst-to-address-list address-list=games address-list-timeout=\
    none-static chain=prerouting connection-mark=games dst-port=\
    40000-40010,41182-41192,50000-50100,56849-57729,60275-64632,60970-60980 \
    protocol=udp
add action=accept chain=prerouting comment=games connection-mark=games
add action=mark-connection chain=prerouting comment=ggc-redirector content=\
    googlevideo.com dst-address-list=ggc-telkom new-connection-mark=\
    ggc-redirector passthrough=yes src-address-list=private-lokal
add action=add-dst-to-address-list address-list=ggc-telkom \
    address-list-timeout=none-dynamic chain=prerouting content=\
    googlevideo.com src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=ggc-redirector content=\
    youtube.com dst-address-list=ggc-telkom new-connection-mark=\
    ggc-redirector passthrough=yes src-address-list=private-lokal
add action=add-dst-to-address-list address-list=ggc-telkom \
    address-list-timeout=none-dynamic chain=prerouting content=youtube.com \
    src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=ggc-redirector content=\
    gvt1.com dst-address-list=ggc-telkom new-connection-mark=ggc-redirector \
    passthrough=yes src-address-list=private-lokal
add action=add-dst-to-address-list address-list=ggc-telkom \
    address-list-timeout=none-dynamic chain=prerouting content=gvt1.com \
    src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=ggc-redirector content=\
    windows dst-address-list=ggc-telkom new-connection-mark=ggc-redirector \
    passthrough=yes src-address-list=private-lokal
add action=mark-connection chain=prerouting comment=ggc-redirector content=\
    cdn dst-address-list=ggc-telkom new-connection-mark=ggc-redirector \
    passthrough=yes src-address-list=private-lokal
add action=accept chain=prerouting comment=ggc-redirector connection-mark=\
    ggc-redirector
add action=mark-connection chain=prerouting comment=all-trafik \
    dst-address-list=!private-lokal new-connection-mark=all-trafik \
    passthrough=yes src-address-list=private-lokal
add action=accept chain=prerouting comment=all-trafik connection-mark=\
    all-trafik
add action=jump chain=forward in-interface=ether1 jump-target=qos-down
add action=mark-packet chain=qos-down comment=vip-down connection-mark=vip \
    new-packet-mark=vip-down passthrough=no
add action=mark-packet chain=qos-down comment=ggc-telkom-down \
    connection-mark=ggc-redirector new-packet-mark=ggc-telkom-down \
    passthrough=no
add action=mark-packet chain=qos-down comment=browsing-down connection-bytes=\
    0-1000000 connection-mark=all-trafik new-packet-mark=browsing-down \
    passthrough=no
add action=mark-packet chain=qos-down comment=low-down connection-bytes=\
    1000001-10000000 connection-mark=all-trafik new-packet-mark=low-down \
    passthrough=no
add action=mark-packet chain=qos-down comment=midle-down connection-bytes=\
    10000001-50000000 connection-mark=all-trafik new-packet-mark=midle-down \
    passthrough=no
add action=mark-packet chain=qos-down comment=high-down connection-bytes=\
    50000001-0 connection-mark=all-trafik new-packet-mark=high-down \
    passthrough=no
add action=mark-packet chain=qos-down comment=unknown-down connection-mark=\
    all-trafik new-packet-mark=unknown-down passthrough=no
add action=mark-packet chain=qos-down comment=unknown-down new-packet-mark=\
    unknown-down passthrough=no
add action=return chain=qos-down
add action=jump chain=forward jump-target=qos-up out-interface=ether1
add action=mark-packet chain=qos-up comment=vip-up connection-mark=vip \
    new-packet-mark=vip-up passthrough=no
add action=mark-packet chain=qos-up comment=ggc-telkom-up connection-mark=\
    ggc-redirector new-packet-mark=ggc-telkom-up passthrough=no
add action=mark-packet chain=qos-up comment=browsing-up connection-bytes=\
    0-1000000 connection-mark=all-trafik new-packet-mark=browsing-up \
    passthrough=no
add action=mark-packet chain=qos-up comment=low-up connection-bytes=\
    1000001-10000000 connection-mark=all-trafik new-packet-mark=low-up \
    passthrough=no
add action=mark-packet chain=qos-up comment=midle-up connection-bytes=\
    10000001-50000000 connection-mark=all-trafik new-packet-mark=midle-up \
    passthrough=no
add action=mark-packet chain=qos-up comment=high-up connection-bytes=\
    50000001-0 connection-mark=all-trafik new-packet-mark=high-up \
    passthrough=no
add action=mark-packet chain=qos-up comment=unknown-up connection-mark=\
    all-trafik new-packet-mark=unknown-up passthrough=no
add action=mark-packet chain=qos-up comment=unknown-up new-packet-mark=\
    unknown-up passthrough=no
add action=return chain=qos-up
/ip firewall nat
add action=passthrough chain=unused-hs-chain comment=\
    "place hotspot rules here" disabled=yes
add action=masquerade chain=srcnat out-interface=ether1
add action=masquerade chain=srcnat out-interface=ether2
add action=masquerade chain=srcnat comment="masquerade hotspot network" \
    src-address=192.168.5.0/24
/ip hotspot user
add name=admin123 password=admin123
/ip route
add check-gateway=ping distance=1 gateway=192.168.100.1 routing-mark=isp-game
add check-gateway=ping distance=2 gateway=192.168.100.1
/system clock
set time-zone-name=Asia/Jakarta
/system ntp client
set enabled=yes primary-ntp=103.31.225.225 secondary-ntp=202.65.114.202
/system resource irq rps
set ether1 disabled=no
set ether2 disabled=no
set ether3 disabled=no
set ether4 disabled=no
set ether5 disabled=no
