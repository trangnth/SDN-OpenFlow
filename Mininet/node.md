## Ghi chú

Tạo topo dạng cây, chiều sâu là 2, mỗi cha sẽ có ba con =>9 host, hiện trên con 229
  
    mn --topo tree,depth=2,fanout=3 --controller=remote,ip=192.168.169.229,port=6633 --switch ovs,protocol=OpenFlow13

https://hpn.hpwsportal.com/catalog.html#/Category/%7B%22categoryId%22%3A10384%7D/Show

    sudo mn --controller=remote,ip=IPADDRESS --custom custom_topos.py --topo fattree_5sw
