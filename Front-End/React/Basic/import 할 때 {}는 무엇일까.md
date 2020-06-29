# import 할 때 {}는 무엇일까

>React를 처음 접할 때 궁금했던 부분이다. 같은 파일에서 import 해오는데 어떤건 그냥 가져오고, 어떤건 {} 을 씌워서 꼭 ES6의 구조분해할당(destructuring assignment) 을 하는 것 처럼 가져온다. 
>
>이런 표기를 쓰는 이유과, 쓰고 안쓰고의 차이점을 알아보자



```javascript
import React from "react";
```

요렇게 그냥 import 하는 방식을 default import 라고 한다.

한 파일에서 쓴 함수나 클래스를 다른 파일에서 사용할 수 있게 export하는 경우가 있는데, 

이때 *export default* 로 export한 요소만 저렇게 *default import* 할 수 있다.

(참고로 모듈 하나에서 export default 는 하나만 존재할 수 있음)



default import 는 export한 이름 그대로 안써도 되고, 이름을 맘대로 바꿔서 import할 수 있다.



```javascript
import { useState } from "react";
```

이건  named import 라고 부르고 , default 표기없이 그냥 export된 것들을 import 할 때 쓴다.

default import 와는 다르게 이름 그대로 가져와야 한다.



아래는 다양한 Import 예시

```javascript
import defaultExport from "module-name";
import * as name from "module-name";
import { export } from "module-name";
import { export as alias } from "module-name";
import { export1 , export2 } from "module-name";
import { export1 , export2 as alias2 , [...] } from "module-name";
import defaultExport, { export [ , [...] ] } from "module-name";
import defaultExport, * as name from "module-name";
import "module-name";
```

자세한 사항이 궁금하다면 MDN문서를 찾아보자



참고자료

export에 대한 MDN문서: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/export

import에 대한 MDN문서:https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/import