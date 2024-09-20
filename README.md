
# **리눅스 PAM을 사용한 비밀번호 규제 및 네트워크 설정**

## 1. **네트워크 설정 - NAT 및 고정 IP 설정**

### **포트 포워딩 규칙 설정**

![NAT 설정](https://github.com/user-attachments/assets/9cc55e37-6bf5-4255-a95a-473eeb6884fc)

![포트포워딩](https://github.com/user-attachments/assets/e6e705fa-8e5d-4f9f-95dc-953dffcfaffb)

```bash
Host myserver01
HostName 127.0.0.1
User ubuntu
Port 22

Host myserver02
HostName 127.0.0.1
User ubuntu
Port 8022

Host myserver03
HostName 127.0.0.1
User ubuntu
Port 22
```

### **고정 IP 설정 방법**

고정 IP 설정을 통해 네트워크에서 시스템의 IP 주소가 고정되도록 설정

1. **Netplan 설정 파일 확인 및 편집**:
   
   `netplan`을 사용해 네트워크 설정을 구성. 설정 파일을 편집하여 고정 IP를 설정

   ```bash
   sudo vi /etc/netplan/00-installer-config.yaml
   ```

2. **설정 파일 내용**:


   ```yaml
   network:
     version: 2
     renderer: networkd
     ethernets:
       enp0s3:
         addresses:
           - 10.0.2.21/24  # 변경된 고정 IP 주소
         routes:
           - to: default
             via: 10.0.2.1  # 게이트웨이
         nameservers:
           addresses:
             - 8.8.8.8
         dhcp4: false
   ```

3. **설정 적용**:

   변경된 설정을 적용하려면 다음 명령어를 사용

   ```bash
   sudo netplan apply
   ```

4. **네트워크 상태 확인**:

   `ip addr` 명령어를 사용하여 네트워크 설정이 제대로 적용되었는지 확인

   ```bash
   ip addr
   ```

---

## 2. **네트워크 설정 - NAT 네트워크로 변경**

![3](https://github.com/user-attachments/assets/a6f72c59-6a5a-4440-9bd2-c3fcfbb0bba1)

```bash
Host myserver01
HostName 127.0.0.1
User ubuntu
Port 22

Host myserver02
HostName 127.0.0.1
User ubuntu
Port 8022

Host myserver03
HostName 127.0.0.1
User ubuntu
Port 8033
```

---


## 3. **PAM을 사용하여 비밀번호 규제 설정**

리눅스 시스템에서 PAM(Pluggable Authentication Modules)을 사용하여 **비밀번호 최소 길이를 8자리 이상**으로 규제하는 방법입니다.

### **설정 방법**:

1. **PAM 패스워드 정책 설정 파일 수정**:
   
   `/etc/pam.d/common-password` 파일을 열어 비밀번호 정책을 설정

   ```bash
   sudo vi /etc/pam.d/common-password
   ```

2. **비밀번호 최소 길이 설정**:
   
   `pam_pwquality.so` 모듈을 사용하여 비밀번호 최소 길이를 설정. 8자리 이상을 요구하는 설정을 추가하거나 수정

   ```bash
   password requisite pam_pwquality.so retry=3 minlen=8
   ```

3. **설정 적용 후 비밀번호 변경**:
   
   설정을 저장한 후, 사용자의 비밀번호를 변경하여 규제가 잘 적용되었는지 확인합니다.

   ```bash
   passwd your_username
   ```
![4](https://github.com/user-attachments/assets/60f90885-a9ca-434b-8674-c38f6564a92c)

---

PAM을 사용한 보안 정책 설정과 네트워크 환경 설정에 대한 방법을 설명<br>
이를 통해 리눅스 시스템에서 비밀번호 규제와 네트워크 관리 기술 습득
