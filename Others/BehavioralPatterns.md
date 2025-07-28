# 행동 패턴

## Command

명령을 표준화된 객체로 만듦

- 마우스 이벤트, 단축키 이벤트든 상관없이 동일한 명령 수행 가능
- 비즈니스 로직은 receiver로 분리해도 되고, 안해도 된다.
- GrimpanMenu가 invoker(커맨드 실행) 역할, GrimpanHistory가 receiver(비즈니스 로직 수행) 역할
