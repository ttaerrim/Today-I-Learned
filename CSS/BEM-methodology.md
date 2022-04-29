# BEM 방법론

## ✔️ BEM이란?
**B**lock, **E**lement, **M**odifier로 구성된 이름을 짓는 방식 

### BLOCK
그 자체로 의미가 있는 구성 요소 (header, container, menu, checkbox, input... 등)
### ELEMENT
단독으로 쓰일 수 없고 상위의 요소와 함께 묶여서 쓰이는 요소 (`menu item`, `list item`, `checkbox caption`, `header title`...)
### MODIFIER
보이는 상태나 속성 따위를 변하게 하는 플래그 (`disabled`, `highlighted`, `checked`, `fixed`, `size big`, `color yellow`)

<img src="http://getbem.com/assets/github_captions.jpg"/>

그래서 이 block, element, modifier를 `block__element--modifier` 형태로 `__`와 `--`로 구분하여 작성한다.
또한 이름 내에서 띄어쓰기와 같은 구분이 필요한 경우 `-`를 사용한다. (`live-learning__item`)