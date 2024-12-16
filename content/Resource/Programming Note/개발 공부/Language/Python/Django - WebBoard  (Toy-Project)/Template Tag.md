> [!info] [Django] 템플릿 언어  
> Django의 템플릿 언어(template language)는 강력함과 편리함 사이의 균형을 잡고자 설계되었다. 템플릿 언어를 사용하면 HTML 작업을 훨씬 수월하게 할 수 있다. {% extends "base_generic.html" %} {% block title %}{{ section.title }}{% endblock %} {% block content %} {{ section.title }} {% for story in story_list %} {{ story.headline|upper }} {{ story.tease|truncatewords:"100" }} {% endfor %} {% endblock %} 장고 공식 문서에 나와있는 예제를 보면서 각 기능들을 공부해보자.  
> [https://velog.io/@hidaehyunlee/Django-%ED%85%9C%ED%94%8C%EB%A6%BF-%EC%96%B8%EC%96%B4](https://velog.io/@hidaehyunlee/Django-%ED%85%9C%ED%94%8C%EB%A6%BF-%EC%96%B8%EC%96%B4)  

1. 템플릿 태그?
    
    1. HTML 작업에 파이썬 끼얹기
    2. 파이썬에서 처리된 데이터를 HTML에 뿌려줄 때 사용.
    
    ```
    {% if 조건문1 %}    <p>조건문1에 해당되는 경우</p>{% elif 조건문2 %}    <p>조건문2에 해당되는 경우</p>{% else %}    <p>조건문1, 2에 모두 해당되지 않는 경우</p>{% endif %}
    ```
    
    ```
    {% for item in list %}    <p>순서: {{ forloop.counter }} </p>    <p>{{ item }}</p>{% endfor %}
    ```
    
    ```
    {{ Object.items }}
    ```