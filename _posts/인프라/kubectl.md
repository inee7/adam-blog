# kubectl



kubectl create -f x.yaml  

전체 컴포넌트를 시작한다.



kubectl replace -f x.yaml

x.yaml에 변경사항 있으면 반영하면서 재시작. 변경사항없으면 유지



kubectl replace --force -f x.yaml 

 x.yaml 변경사항과 상관없이 재시작



