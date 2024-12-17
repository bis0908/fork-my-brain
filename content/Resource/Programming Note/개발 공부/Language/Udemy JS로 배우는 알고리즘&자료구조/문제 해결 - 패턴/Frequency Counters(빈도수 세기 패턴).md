---
tags:
  - Javascript
  - Algorithm
---

- JS의 객체를 사용해 다양한 값과 빈도를 수집
- 빈도수 세기 패턴의 핵심은 객체를 만들어 값의 빈도를 수집하는 것

```js
function validAnagram(str1, str2) {  
	if (str1.length !== str2.length) {    
			return false;  
		}  
		
		let valid1 = {};  
		
		for (const str of str1) {    
			valid1[str] = (valid1[str] || 0) + 1;  
		}  
		
		// str2 문자열 길이만큼 반복  
		for (const str of str2) {    
			// if there is no key    
			if (!valid1[str]) {      
				return false;    
			} else {      
				valid1[str] -= 1;    
			}  
		}  
	return true;
}
```