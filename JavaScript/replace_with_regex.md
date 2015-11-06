#정규식을 이용한 통변환


##Capturing group사용

```javascript
str.replace(/(.*value="\w+)(\d+)(\w+".*)/, "$1!NEW_ID!$3")
```

###array의 name값 변환
```javascript
str.replace(/(name=["'][^"'[]*\[)([0-9]*)(\][^"'\[\]]*["'])/gi, '$1'+count_temp+'$3');
```

