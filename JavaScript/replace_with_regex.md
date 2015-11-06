#정규식을 이용한 통변환


##Capturing group사용

"'
str.replace(/(.*value="\w+)(\d+)(\w+".*)/, "$1!NEW_ID!$3")
"'

