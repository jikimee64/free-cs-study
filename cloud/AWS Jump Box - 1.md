# AWS Jump Box 만들기 - 1
	1부
    1.VPC
	2.Subnet
	3.Internet G/W (VPC에 붙이기,
		라우팅 테이블에서 퍼블릭/ 프라이빗 끼리 묶어서 사용하며 퍼블릭 라우팅 테이블에 IGW를 가져다가 붙였다.)
	4.RouteTable 생성 => public + IGW => private
	2부
    5.EC2 => Public subnet, sg 설정
	6.EIP (퍼블릭 아이피인 고정 IP 생성 및 EC2에 연결)
    7.JUMP BOX 설정
    
![](https://images.velog.io/images/hkjs96/post/e07ecd26-0948-4db7-baf9-187e8482798a/image.png)
## VPC 설정
### 1. VPC 생성 클릭
![](https://images.velog.io/images/hkjs96/post/cc0a0f14-81b9-433d-a06f-1951e17e89a3/image.png)
### 2. VPC 설정
- **VPC 설정 후 생성 버튼 클릭**
![](https://images.velog.io/images/hkjs96/post/5d30a87f-2a6d-4674-a93d-657d5e0598b6/image.png)
#### 이름 설정
- 리소스 명명 규칙을 이렇게 정의 했다고 하면 
  - {서비스} - {국가코드 2자리}{AZ 마지막 2자리} - {리소스명} - {단계} - {서버명}
  - VPC는 이렇게 정의 하고 **{서비스} - {국가코드 2자리} - vpc - {단계}**
    - **serviceName-kr-vpc-dev** 이런식으로 명명한다.

#### IPv4 CIDR 설정
- CIDR
  - Classless Inter-Domain Routing : 클래스 없는 도메인간 라우팅 기법
  - Class 체계보다 더 유연하게 IP주소를 여러 네트워크 영역 나눈다고 합니다.
  - 여튼 NetMask 가 16이기 때문에 앞에 10.10.x.x 고정이고, 10.10.0.0 ~ 10.10.255.255 까지 쓸 수 있습니다.
  ![](https://images.velog.io/images/hkjs96/post/3a006610-99c0-4b7b-851b-cae211784b9f/image.png)
  
## Subnet 설정
### 1. 서브넷 생성 클릭 
![](https://images.velog.io/images/hkjs96/post/a31db17a-5e81-45db-84fd-184792f24886/image.png)
### 2. 서브넷 설정
- **이름 설정** 후 **서브넷을 추가**하여 등록한 뒤 **서브넷 생성** 버튼을 누른다.
![](https://images.velog.io/images/hkjs96/post/0503182b-fda8-42cd-9f60-42d313cd5d83/image.png)
#### 이름 설정
- 리소스 명명에 규칙에 Subnet 명명은 **{서비스} - {국가코드 2자리}{AZ 마지막 2자리} - subnet - {단계} - {public/private/db/..}{숫자 인덱스 01,02,…}**
  - **serviceName-kr1a-subnet-dev-public01, serviceName-kr1a-subnet-dev-private01, serviceName-kr1a-subnet-dev-db01**
  
#### 서브넷 설정
- 가용 영역 설정 및 IPv4 CIDR 블록 설정
- CIDR 블록은 VPC에서 **10.10.0.0/16** 으로 설정 했기에 **10.10.x.x** 대역 안에서 정하며 서브넷 또한 구분 하기 위해서 **10.10.x.x/24** 로 서브넷당 **255**개의 주소를 할당할 수 있다.
- public과 private 서브넷을 두개씩 만들었다.
  - public01 : 10.10.1.0/24
  - private01 : 10.10.10.0/24
  - public02 : 10.10.2.0/24
  - private01 : 10.10.20.0/24
![](https://images.velog.io/images/hkjs96/post/d7e4f272-5d91-40fa-853a-0e5af7897a05/image.png)

  
## Internet G/W 설정
	1. VPC 에 붙이기
	2. 라우팅 테이블에서 퍼블릭/ 프라이빗 끼리 묶어서 사용하며 퍼블릭 라우팅 테이블에 IGW를 가져다가 붙였다.
### 1. IGW 생성 클릭
![](https://images.velog.io/images/hkjs96/post/6ba40975-f2e0-4594-ac53-200d860540e7/image.png)
### 2. IGW 설정
![](https://images.velog.io/images/hkjs96/post/9503766a-4fd1-4d5a-bf2a-8bfeeeafabfa/image.png)
#### 이름 설정
- 리소스 명명에 규칙에 IGW 명명은 **{서비스} - {국가코드 2자리} - igw - {단계}{숫자 인덱스 01,02,…}** 라하면
  - **serviceName-kr-igw-dev01**

### 3. VPC 에 붙이기
- **생성된 IGW** 를 우클릭 해보면 **VPC에 연결** 이 나오는데 이것을 클릭한다.
![](https://images.velog.io/images/hkjs96/post/30d94358-40c4-495f-8109-c44eb8335686/image.png)
- **VPC 를 선택하고 IGW를 연결한다.**
![](https://images.velog.io/images/hkjs96/post/7d6fa664-73b5-4979-9da9-a9a38615d432/image.png)

## Route Table 설정
### 1. 라우팅 테이블 생성 클릭
![](https://images.velog.io/images/hkjs96/post/8ac36315-5737-4f78-bb68-faaf306370d2/image.png)
### 2. 라우팅 테이블 설정
- 이름 명명 후 **라우팅테이블에 대해 사용할 VPC 설정**을 해준다.
- private 와 public에 관한 테이블 두개 설정한다.
![](https://images.velog.io/images/hkjs96/post/86335683-0854-4cf0-bc63-ce78d676d6f9/image.png)
#### 이름 설정
- 리소스 명명에 규칙에 RouteTable 명명은 **{서비스} - {국가코드 2자리} - rtb - {단계} - {public/private/db/..}{숫자 인덱스 01,02,…}**
  - serviceName-kr-rtb-dev-public01
### 3. 라우팅 테이블과 서브넷 연결
- **서브넷 연결 편집 클릭**
![](https://images.velog.io/images/hkjs96/post/8cbdf4a7-c3a4-49fa-b57c-f50c32938fc6/image.png)

- **public 라우팅테이블과 IGW 연결**
![](https://images.velog.io/images/hkjs96/post/446f9a64-c8ee-47f3-9b03-d021b3c56e4d/image.png)

- **연결할 서브넷 설정 및 저장**, public에 관한 라우팅 테이블이기 때문에 public 만 묶음 마찬가지로 private도 priave 서브넷 끼리 묶는다.
![](https://images.velog.io/images/hkjs96/post/6fa4e4f5-5c0a-48e4-be83-1b3ecd65214b/image.png)




# 참고 
[AWS의 점프 박스를 통한 SSH](https://medium.com/@yagizcemberci/ssh-through-jump-box-on-aws-7839c28fb8c6)

[VPC의CIDR블록의 사용이유와 설정방법](https://dev.classmethod.jp/articles/vpc-3/)

[\[네트워크\] CIDR이란?(사이더 란?)](https://kim-dragon.tistory.com/9)

