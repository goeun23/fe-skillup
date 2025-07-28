# Incremental Static Regeneration

SG의 단점(빌드 후 데이터 고정)을 보완하기 위해 도입된 Next.js 만의 정적 재생성 방식

## 특징

- 기본 개념 : 빌드시 정적 HTML 생성, 이후 백그라운드에서 갱신
- 장점 : SG처럼 빠르면서, SSR 처럼 최신데이터 유지 가능
- 트리거 : 사용자 요청 발생 시 + 설정된 시간(revalidate)이 지났을때
- 설정방법 : fetch()에 next : {revalidate:N} 지정

```
const res = await getch('.../', {
    next : {revalidate:60} // 초 단위
})
```

## 동작 흐름

- 첫 요청 : 정적 HTML 응답
- 이후 요청 : 캐시된 HTML 응답
- revalidate 시간 경과 후 : 백그라운드에서 새 HTML 생성 + 캐시 교체
- 다음 요청부터 새 HTML 사용
  -> SG처럼 빠르면서도, SSR 처럼 최신성을 확보할 수 있는 전략
  -> 정적 페이지인데도, 조용히 다시 만들어서 교체해주는 방식
