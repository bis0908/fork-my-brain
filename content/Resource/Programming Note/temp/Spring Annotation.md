  

2022년 4월 4일 월요일

오후 1:34

1. 어노테이션 정리
    1. @Controller
    2. @RestController
    3. @RequestMapping
    4. @ResponseBody
    5. @RequestBody
    6. @GetMapping
    7. @PostMapping
    8. @PatchMapping
    9. @PutMapping
    10. @PathVariable
    11. @getter
    12. @setter
        1. @Setter 어노테이션이 멤버변수 위에 있으면 롬복은 자동으로 public void set{변수명}( {데이터타입} {변수명}) 형태의 자바 빈즈 setter 메소드를 만들어낸다
    13. @ToString
        1. @ToString 롬복 어노테이션은 자동으로 public String toString() 메소드를 만들어 낸다
    14. @Data
        1. @Data 어노테이션은 @Getter, @Setter, @ToString(), @EqualsAndHashCode 4개의 어노테이션이 합쳐진 어노테이션이다
        2. @EqualsAndHashCode 어노테이션은 equals와 hashcode 메소드를 만들어 준다
        3. hashCode는 객체의 고유 식별자를 반환하는 메소드.
        4. equals는 한 객체가 다른 객체가 같은지 비교하는 메소드.
        5. canEquals는 객체를 다른 객체와 비교 가능한지 여부를 반환.
2. Ref
    1. @RequestBody / @ResponseBody 차이
        1. [https://cheershennah.tistory.com/179](https://cheershennah.tistory.com/179)