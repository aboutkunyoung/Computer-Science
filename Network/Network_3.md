나는 처음 부임 후 가설 업무나 통신실 바닥 공사 등 물리적인 작업들이 너무 많아서 스위치를 다루는 데까지 시간이 좀 걸렸었다. 물리적인 시간도 없었을 뿐더러 실제로 사용하는 장비들을 PuTTY로 접속해서 만진다는 점이 상당히 무서웠다. 그래서 반장이 안 쓰는 스위치를 학습용으로 줘서 몇 번 만지면서 갖고 놀았던 게 도움이 됐다. 사회에서는 스위치 장비가 없으면 Packet Tracer라는 프로그램을 사용하여 스위치 내부 설정값들을 주로 만져볼 수 있다.

내가 생각했을 때, 스위치에서 가장 중요한 점은 비밀번호다. 내가 부임하고 스위치들을 좀 만져보려 했었는데, 100여 대의 장비 중 주요 장비를 제외한 모든 장비의 비밀번호가 막혀 있었다. 그래서 훈련 기간 동안 훈련을 빠지고 스위치 초기화만 며칠 동안 날 잡고 했던 적도 있다.

네트워크를 운용/유지하는 측면에서 스위치를 본다면 주로 쓰는 명령어가 그리 많지 않다.

기본 명령어

> show running-config

→ 현재 장비에서 활성화된 설정을 확인한다.

가장 많이 봐야 하는 명령어이다. 현재의 상태를 일정 주기별로 백업해 두어야 하고, 이 설정값을 기준으로 장비를 관리한다. (전임자가 만들어준 이 설정을 의심하면서 바라볼 필요도 있다.)

> show interfaces

→ 스위치의 모든 인터페이스에 대한 상태와 통계 정보를 표시한다.

스위치에서 각각의 포트는 다른 인터페이스이다. 이 특징으로 한 스위치를 포트별로 나누어서 여러 대역의 망으로 나누어 사용할 수 있다. (장비가 부족할 경우 이럴 수 있는데, 이랬다가 엄청 혼났다.)

> show vlan

→ 스위치의 VLAN 정보를 제공한다.

VLAN 정보를 얻을 수 있다. 일반 인트라넷에서 VLAN 태그 번호를 통해 VoIP 통신이 이루어진다.

> show ip route

→ 스위치의 라우팅 테이블 내용을 확인한다.

라우팅 테이블은 네트워크의 핵심이다. 망 안정화를 위해 꼭 봐야 하는 정보이다.

설정 값 변경 명령어

> configure terminal / conf t

→ 전역 구성 모드로 들어간다.

> interface <interface-id>

→ 특정 인터페이스를 선택한다.

> vlan <vlan-id>

→ VLAN 아이디를 생성하거나 기존 VLAN을 수정한다.

> ip address <ip-address> <subnet-mask>

→ 인터페이스에 IP 주소와 서브넷 마스크를 설정한다.

> shutdown / no shutdown

→ 인터페이스를 비활성화하거나 활성화한다.

> write memory

→ 메모리에 저장한다. (저장하지 않고 스위치를 재부팅하면 설정이 날아간다!)

---

Layer3 계층에서 포트별 다른 아이피 대역 설정 하는법

> configure terminal
> interface gigabitEthernet 1/0/1
> ip address 192.168.10.1 255.255.255.0
> no shutdown
> interface gigabitEthernet 1/0/2
> ip address 192.168.20.1 255.255.255.0
> no shutdown

여기서 gigabitEthernet 1/0/1 은 각각의 포트 번호이다. 이런식으로 포트별로 아이피를 지정해 줄 수 있고, 아니면 VLAN을 통째로 만들어서 포트별로 넣어주는 방식도 있다. 유사한 방식으로 ACL 정책을 넣어줄 수 있다.

> configure terminal
> interface vlan 10
> ip address 192.168.1.1 255.255.255.0
> no shutdown
> interface vlan 20
> ip address 192.168.2.1 255.255.255.0
> no shutdown

> configure terminal
> interface gigabitEthernet 1/0/1
> switchport mode access
> switchport access vlan 10
> interface gigabitEthernet 1/0/2
> switchport mode access
> switchport access vlan 20

---

일단 내가 썼었던 명령어는 이 정도이다. 여기에 추가하자면 ACL을 넣는 것 정도? 그런데 문제점은 사업마다 들어오는 스위치가 다 달랐기 때문에, 제조사마다 오묘하게 다른 명령어가 너무 어려웠다. 듣기로는 명령어 자체가 특허 등록되어 있는 자산이라서, 회사가 다른 회사의 명령어를 따라 할 수 없다고 한다. 대충 엣지코어는 시스코 명령어와 유사했고, 알카텔 스위치는 프랑스 회사라서 명령어가 심각하게 난해했다. 그런데 알카텔 스위치도 상당히 많았다.
