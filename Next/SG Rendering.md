# Static Generation(SG)

Next.js의 pre-rendering 방식 중 하나로, 빌드 시 HTML을 미리 생성해서 정적 파일로 저장한다.

## 특징

- 시점 : 빌드 타임에 한 번만 실행
- 결과물 : HTML + JSON 생성되어 CDN에서 serving 가능
- 속도 : 가장 빠름(네트워크 I/O 없음)
- 대표함수 : getStaticProps, generateStaticProps(App router 기준)
- 예시 : 블로그, 상품 상세, 문서 페이지 등 자주 안바뀌는 페이지 등

## App router 기준 SG 조건

- 서버 컴포넌트에서
- fetch() 사용 시 cache:’force-cache’
- 동적 라우트는 generateStaticParams 사용

```markdown
export async function generateStaticParams(){
const products = await fetchProducts();
return products.map(p=> ({id:p.id});
}

export default async function ProductPage({params}){
const res = await fetch('.../');
const product = await res.json();
return <div>{product.name}</div>
}
```

- SG는 빌드 시점 데이터 고정 → 자주 바뀌는 데이터에는 부적합
- 데이터를 바꾸려면 다시 빌드해야함 → 이를 해결하는게 ISR
