# eks-semi-project

### 수행 방법

AWS 작업 환경에서 진행합니다.

- Step 0
    - “Terraform을 사용하여 EKS 클러스터 구성해 봅니다.
    
- Step 1
    - ‘Stateful 애플리케이션 배포하기’ [튜토리얼](https://kubernetes.io/ko/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)을 진행합니다.
    - k8s 클러스터를 생성하고 볼륨 컨트롤러([참고 문서1](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/ebs-csi.html), [참고 문서2](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/efs-csi.html) 택1)를 설정한 후 위의 튜토리얼을 진행합니다.
    
- Step 2-1
    - Step 1에서 생성한 Wordpress App에 아래 조건들을 만족할 수 있도록 기능을 추가합니다.
    - Wordpress App 구성 조건
        - Deployment로 배포
        - resources, livenessProbe를 정의
        - HPA를 설정하여 Autoscailing이 가능하도록 정의 (Metric Server 설치)
        - Service와 Ingress(ALB)를 생성하여 wordpress app을 클러스터 외부로 노출 ([참고 문서](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/aws-load-balancer-controller.html))
        
- Step 2-2
    - stateless 애플리케이션 배포하기 (https://kubernetes.io/ko/docs/tutorials/stateless-application/)
        - 디플로이먼트로 배포
            - scale up, down 해보기
            - 특정 노드에 고정하여 배포하기(NodeAffinity)
        - NodePort, Port-forward로 노출하여 접근하기
        - Recreate, RollingUpdate의 동작 이해하고 BlueGreen 배포 구현해보기
        - (권장) Load Balancer 생성하기

- Step 3
    - Step 1과 별도의 MySQL App을 StatefulSet으로 배포해봅니다.
    - MySQL App 구성 조건
        - StatefulSet으로 배포
        - replicas는 2 이상으로 정의
        - resources, livenessProbe를 정의
        - Secret을 생성하여 root 패스워드 설정
        - PVC를 이용하여 스토리지와 연결
        - Headless Service를 생성하여 mysql app과 연결
