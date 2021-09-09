### kafka





공식문서

https://kafka.apache.org/documentation/





Topic



프로듀서는 Topic에 Message를 정의하고

컨슈머는 Topic의 Message를 읽어온다



컨슈머는 오프셋 기준으로 메시지를 읽음

읽어 가도 메세지가 남는다?





프로듀서는 라운드 로빈이나 Key를 통해서 Topic을 선택한다

컨슈머는 컨슈머 그룹에 속하고 한개의 파티션은 그룹의 한개의 컨슈머에만 연결 가능

컨슈머 그룹 기준으로 각 파티션

