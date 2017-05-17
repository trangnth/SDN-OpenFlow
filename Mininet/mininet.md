# Mininet

1. [Overview](#overview)



2. [Deployment](#deployment)



3. [Research](#research)

<a name="overview"></a>
## 1. Overview

Mininet giả lập toàn bộ mạng có các host, link, switch trên một máy duy nhất.Mininet sử dụng công nghệ ảo hóa (ở mức đơn giản) để tạo nên hệ thống mạng hoàn chỉnh, chạy chung trên cùng một kernel, hệ thống và user code.

Các host ảo, switch, liên kết và các controller trên mininet là các thực thể thực sự, được giả lập dưới dạng phần mềm thay vì phần cứng. Một host mininet có thể thực hiện ssh vào đó, chạy bất kì phần mềm nào đã cài trên hệ thống linux (môi trường mà mininet đang chạy). Các phần mềm này có thể gửi gói tin thông các ethernet interface của mininet với tốc độ liên kết và trễ đặt trước.

Mininet cho phép tạo topo mạng nhanh chóng, tùy chỉnh được topo mạng, chạy được các phần mềm thực sự như web servers, TCP monitoring, Wireshark; tùy chỉnh được việc chuyển tiếp gói tin. Mininet cũng dễ dàng sử dụng và không yêu cầu cấu hình đặc biệt gì về phần cứng để chạy: mininet có thể cài trên laptop, server, VM, cloud (linux).

<a name="deployment"></a>
## 2. Deployment
### Requirements
Ubuntu 14.04

### Intallation
Chạy các lệnh sau:


	sudo apt-get update
	sudo apt-get install git
	git clone git://github.com/mininet/mininet
	cd mininet
	git tag
	git checkout -b 2.2.1 2.2.1
	cd ..
	mininet/util/install.sh -a


<a name="research"></a>

## 3. Research


Các host được đặt tên từ h1-hn

Các switch từ s1-sn

Ethernet trên các host bắt đầu từ 0, trên các switch bắt đầu từ 1

Giao diện đầu tiên của 'h1' được gọi là 'h1-eth0' và giao diện thứ ba của 'h2' được gọi là 'h2-eth2'. Cổng đầu tiên của switch 's1' được đặt tên là 's1-eth1'.


### Mininet Topologies

Mininet có các dạng topo: minimal, single, reversed, linear và tree

#### Minimal

Dạng này rất đơn giản chỉ gồm 1 OpenFlow switch kết nối với 2 hosts

	mn --topo minimal

<img src = "\img\1.png">


	mininet> net
	h1 h1-eth0:s1-eth1
	h1 h2-eth0:s1-eth2
	s1 lo: s1-eth1:h1-eth0 s1-eth2:h2-eth0
	c0


#### Single

Topo gồm 1 OpenFlow switch kết nối với k port

	mn --topo single, 4

<img src = "img\2.png">


	mininet> net
	h1 h1-eth0:s1-eth1
	h2 h2-eth0:s1-eth2
	h3 h3-eth0:s1-eth3
	h4 h4-eth0:s1-eth4
	s1 lo:  s1-eth1:h1-eth0 s1-eth2:h2-eth0 s1-eth3:h3-eth0 s1-eth4:h4-eth0
	c0


#### Reversed

Giống Single, nhưng thứ tự các giao diện kết nối từ các host tới switch là ngược lại

	mn --topo reversed,4 

<img src = "img\3.png">


	mininet> net
	h1 h1-eth0:s1-eth4
	h2 h2-eth0:s1-eth3
	h3 h3-eth0:s1-eth2
	h4 h4-eth0:s1-eth1
	s1 lo:  s1-eth1:h4-eth0 s1-eth2:h3-eth0 s1-eth3:h2-eth0 s1-eth4:h1-eth0
	c0


#### Linear 

Topo này gồm có k switch và k host, mỗi switch sẽ được kết nối tới một host

	mn --topo linear,4

<img src = "img\4.png">


	mininet> net
	h1 h1-eth0:s1-eth1
	h2 h2-eth0:s2-eth1
	h3 h3-eth0:s3-eth1
	h4 h4-eth0:s4-eth1
	s1 lo:  s1-eth1:h1-eth0 s1-eth2:s2-eth2
	s2 lo:  s2-eth1:h2-eth0 s2-eth2:s1-eth2 s2-eth3:s3-eth2
	s3 lo:  s3-eth1:h3-eth0 s3-eth2:s2-eth3 s3-eth3:s4-eth2
	s4 lo:  s4-eth1:h4-eth0 s4-eth2:s3-eth3
	c0


#### Tree

Topo này là một cây gồm k mức
	
	mn --topo tree,3 

<img src = "img\5.png">


	mininet> net
	h1 h1-eth0:s3-eth1
	h2 h2-eth0:s3-eth2
	h3 h3-eth0:s4-eth1
	h4 h4-eth0:s4-eth2
	h5 h5-eth0:s6-eth1
	h6 h6-eth0:s6-eth2
	h7 h7-eth0:s7-eth1
	h8 h8-eth0:s7-eth2
	s1 lo:  s1-eth1:s2-eth3 s1-eth2:s5-eth3
	s2 lo:  s2-eth1:s3-eth3 s2-eth2:s4-eth3 s2-eth3:s1-eth1
	s3 lo:  s3-eth1:h1-eth0 s3-eth2:h2-eth0 s3-eth3:s2-eth1
	s4 lo:  s4-eth1:h3-eth0 s4-eth2:h4-eth0 s4-eth3:s2-eth2
	s5 lo:  s5-eth1:s6-eth3 s5-eth2:s7-eth3 s5-eth3:s1-eth2
	s6 lo:  s6-eth1:h5-eth0 s6-eth2:h6-eth0 s6-eth3:s5-eth1
	s7 lo:  s7-eth1:h7-eth0 s7-eth2:h8-eth0 s7-eth3:s5-eth2
	c0


### Custom Topologies

Ngoài ra Mininet còn hỗ trợ việc tạo một topo bất kỳ bằng việc tạo một file code python và chạy nó

Ví dụ: Tạo 1 topo gồm 2 Switch, mỗi Switch gồm 2 host, controller kết nối tới ODL với ip=192.168.169.229

(custom_topo.sh)[custom_topo.sh]

Sau đó chạy với lệnh `python custom_topo.sh` và xem topo trên ODL

<img src="img\9.png">
