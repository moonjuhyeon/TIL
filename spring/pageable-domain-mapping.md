> 기존 `Pageable` 클래스의 `Sort`는 `Entity.Column` 으로 정렬 조건을 받아옵니다.
그러나, 이러한 방식은 `Entity`의 노출이 불가피합니다.
따라서 `Entity`의 노출을 방지하기 위해 `SortStrategy`를 구축하여 `Column`으로 정렬 조건을 Mapping되도록 합니다.

> [https://cla9.tistory.com/96?category=869583](https://cla9.tistory.com/96?category=869583)
위 포스트를 기반으로, 제가 조금 개선하여 구현하였습니다.

---

---

## 1. 준비단계

### 1-1. `Pair` 클래스 구현

```java
@Getter
@Builder(access = AccessLevel.PRIVATE)
@ToString
public class Pair<T, U> {
    private T first;
    private U second;

    public static <T, U> Pair of(T first, U second) {
        return Pair.builder()
                .first(first)
                .second(second)
                .build();
    }
}
```

- 컬럼명과 정렬 상태를 담을 `Pair` 클래스를 생성

### 1-2. 정렬 상태를 표현하는 `Enum` 구현

```java
public enum SortOption {
    ASC,
    DESC
}
```

### 1-3. `PageableDto` 구현

```java
@Getter
@Builder
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class PageableDTO {
    private Integer page;
    private Integer size;
    private List<Pair<String, SortOption>> sorts;
}
```

- `Page` 관련 정보과 `Sort` 정보를 담을 `Dto`를 선언

## 2. 도메인 컬럼 매핑 Layer 구현

> Notice 도메인은 예제로 구축하였습니다.

- 도메인의 컬럼명을 받아오는 `getColumn` 메소드 구현

```java
public class StringUtil {
	// 중략

	public static String getColumn(Path<?> path){
		return path.getMetadata().getName();
	}
}
```

- 실제 DB의 Column과 Entity의 Column을 매핑하기 위해 `Enum` 구현
- `Entity`의 Column을 받아오기 위해, QueryDsl `QClass` 사용

```java
public enum NoticeMetaType {
  ID(StringUtil.getColumn(QNotice.notice.id)),
  TITLE(StringUtil.getColumn(QNotice.notice.title));

  private String title;
  NoticeMetaType(String column) {
    this.title = column;
  }

  public String getTitle(){
    return title;
  }
}
```

## 3. `DomainSpec` 클래스 구현

### 3-1 `SortStrategy` 클래스

```java
public interface SortStrategy<T extends Enum<T>> {
    Sort.Order getSortOrder(T domainKey, SortOption order);
}
```

```java
public class NoticeSortStrategy implements SortStrategy<NoticeMetaType> {
  @Override
  public Order getSortOrder(NoticeMetaType domainKey, SortOption order) {
    Order sortOrder = null;
    switch (domainKey){
      case ID:
      case TITLE:
          final String column = domainKey.getTitle();
          sortOrder = (order == SortOption.ASC) ? Order.asc(column) : Order.desc(column);
    }
    return sortOrder;
  }
}
```

- 각 `Entity` 별로 정렬을 구현할 전략을 설정할 수 있는 객체를 생성
- 위 `switch` 구문에서 `ID`를 삭제하면 Client로 넘어오는 id는 해당되지 않으므로 Null을 반환
- 따라서 `Strategy`에서 지정한 컬럼에 따라 정렬 대상을 한정할 수 있음

### 3-2 `DomainSpec` 클래스 생성

```java
public class DomainSpec<T extends Enum<T>, U> {
  private final Class<T> clazz;
  @Getter @Setter
  private int defaultPage = 0;
  @Getter @Setter
  private int defaultSize = Integer.MAX_VALUE;
  private SortStrategy sortStrategy;

  public DomainSpec(Class<T> clazz, SortStrategy<T> sortStrategy){
    this.clazz = clazz;
    this.sortStrategy = sortStrategy;
  }

  public DomainSpec(Class<T> clazz, SortStrategy<T> sortStrategy, int defaultPage, int defaultSize){
    this.clazz = clazz;
    this.sortStrategy = sortStrategy;
    this.defaultPage = defaultPage;
    this.defaultSize = defaultSize;
  }

  public void changSortStrategy(SortStrategy<T> sortStrategy){
    this.sortStrategy = sortStrategy;
  }

  public Pageable getPageable(PageableDto pageableDto){
    Integer page = pageableDto.getPage();
    Integer size = pageableDto.getSize();
    Pageable pageable;

    if(pageableDto.getSortList().size()<1){
      pageable = PageRequest.of(page == null ? defaultPage : page, size == null ? defaultSize : size);
    }else{
      List<Sort.Order> sortList = getSortOrders(pageableDto.getSortList());
      pageable = PageRequest.of(page == null ? defaultPage : page, size == null ? defaultSize : size, Sort.by(sortList));
    }

    return pageable;
  }

  private List<Sort.Order> getSortOrders(List<Pair<String, SortOption>> sortList) {
    Assert.notNull(this.sortStrategy,"There is no sort strategy");

    List<Sort.Order> orderList = new ArrayList<>();
    for (Pair<String, SortOption> sort : sortList) {
      T column;
      try {
        column = Enum.valueOf(this.clazz, sort.getFirst().toUpperCase());
      } catch (IllegalArgumentException e) {
        throw new IllegalArgumentException("Sort option error");
      }

      final Sort.Order order = sortStrategy.getSortOrder(column, sort.getSecond());
      Assert.notNull(order, "sort option error");

      orderList.add(order);
    }
    return orderList;
  }

}
```

- 정렬을 포함한 Pageable 객체를 얻어오기 위한 클래스
- SortStrategy를 통해 전달받은 구현체에 정렬 방법을 위임함

## 4. `PageableUtil` 구현

```java
public class PageableUtil {
  public static PageableDto setPageableDto(Pageable pageable){
    List<Pair<String, SortOption>> sortList = new ArrayList();
    List<String> list = pageable.getSort().stream().map(order -> order.toString().toLowerCase()).collect(
      Collectors.toList());
    for(String sort : list){
      String[] keywordArr = sort.trim().split(":");
      if(keywordArr[0].equals("asc")||keywordArr[0].equals("desc")){
        continue;
      }
      sortList.add(Pair.of(keywordArr[0], (keywordArr.length < 2 || keywordArr[1].equals("asc"))? SortOption.ASC : SortOption.DESC));
    }
    PageableDto pageableDto = new PageableDto();
    return pageableDto.builder()
      .page(pageable.getPageNumber())
      .size(pageable.getPageSize())
      .sortList(sortList)
      .build();
  }
}
```

- 정렬조건 띄어쓰기, 대소문자 구분 및 Pair 클래스의 정렬 조건을 List로 받아옴
- Pageable 객체를 PageableDto로 매핑하여 리턴

## 5. `Controller` 구현

```java
@GetMapping(value = "/notices")
  @ResponseBody
  public ResponseEntity<?> getAll(
      @RequestParam("noticeType") List<NoticeType> noticeTypeList,
      @RequestHeader(value = "user-agent", required = false) String userAgent,
      @PageableDefault(size = Integer.MAX_VALUE) Pageable pageable) {
    log.debug("request {} ", noticeTypeList);
    log.debug("sort {}", pageable.getSort());
    return service.getAllByNoticeTypeList(noticeTypeList
													, pageable
													, userAgent);
  }
```

## 6. `Service` 구현

```java
@Service
public class NoticeService {
    private final NoticeRepository repository;
		private final DomainSpec<NoticeMetaType, Notice> domainSpec = new DomainSpec<>(NoticeMetaType.class, new NoticeSortStrategy());
		
		public ResponseEntity<?> getAllByNoticeTypeList(List<NoticeType> noticeTypeList, Pageable pageable, String userAgent) {        
			
			// 중략
			Page<Notice> noticeList = null;
	    **DomainSpec<NoticeMetaType, Notice> domainSpec = new DomainSpec<>(NoticeMetaType.class, new NoticeSortStrategy());**
	    if (noticeTypeList != null && !noticeTypeList.isEmpty()) {
		      noticeList = noticeRepository
										.findAllByNoticeTypeIn(noticeTypeList
															,**domainSpec.getPageable(PageableUtil.setPageableDto(pageable))**);	    
				}
    }
}
```

- 정렬 조건을 포함한 `Pageable` 객체를 `NoticeSpec`에서 가져올 수 있도록 구성