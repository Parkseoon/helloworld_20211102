# helloworld_20211102

**1,2)getopts

1)getopt() 함수는 콘솔 기반의 프로그램(어플리케이션)을 제작하는데 있어 옵션 값(인자, 파라미터)을 받는데 유용한 함수다.

2)사실 '-a' 나 '--version'과 같은 옵션을 argc, argv로 처리할 수도 있겠지만 이를 직접 처리하는 것은 귀찮은 작업이다. 이를 위해 getopt 함수가 제공된다.

3)getopt() 함수는 stdio.h 와 unistd.h 모두에 정의되어있다. 두 헤더 파일에 정의된 함수의 차이는 딱히 없는 듯 하다.

|사용|보기|내용|
|---|-------|------------------------|
|헤더|#include<unistd.h>|x|
|인수|int argc|인수의 개수|
|반환|-1|더이상 옵션이 없을 때|
|반환|옵션문자|지정되지 않은 옵션일 때
|반환|그외|지정된 옵션일 경우 문자열 반환|

+추가설명

_옵션만 사용하고 싶을때

함수 사용법 : while( (c=getopt(argc, argc, "a")) != -1)

명령어 실행 : #./getoptTest -a

 

_옵션에 인자를 집어 넣고 싶을때

함수 사용법 : while( (c=getopt(argc, argc, "a:")) != -1)

명령어 실행 : #./getoptTest -a abc

 

_위의 두가지를 다 쓰고 싶을 때

함수 사용법 : while( (c=getopt(argc, argc, "a")) != -1)

명령어 실행 : #./getoptTest -a

명령어 실행 : #./getoptTest -a abc

 

"a" 이부분에서 맨앞에 : 이것을 빼게되면 a: 이것만 남게 되는데 이때는 인자 값이 없다면 오류를 발생하게 된다.

"a"와 같이 쓰면 인자를 넣어도 되고 안 넣어두 된다. 하지만 인자값이 없다면 case 'a'를 실행 하는 것이 아니라 case ':'를 실행하므로 처리는 따로 해주어야 한다.

없는 옵션을 실행하게 되면 case '?'가 실행된다.



[더 자세히] (https://mousepotato.tistory.com/61 [it's])

4)getopt를 사용하는 이유

-다양한 입력 값이 존재할 경우 사용자와 개발자의 편의를 보장한다.

-스크립트를 보다 체계적으로 관리 할 수 있다. 

```unix
#include "stdio.h"

int main(int argc, char *argv[]) {
	int num;
	extern char *optarg;
	extern int optind;

	num = getopt(argc, argv, "au:h");

	switch(num) {
		case 'a':
			printf("Welcome to Unix System Programming World!\n");
			break;
		case 'u':
			printf("Nice to meet %s\n", optarg);
			break;
		case 'h':
		default:
			printf("Option : \n");
			printf("\t-a : print \"Welcome to Unix System Programming World!\"\n");
			printf("\t-u [String] : print \"Nice to meet [String]\"\n");
			printf("\t-h : print Help page\n");
			break;
	}

	return 0;
}

```

5)getopt는 int 형의 반환형을 가지며, 세번째 파라미터(인자)에 포함된 문자를 받는다면 해당 문자의 값을 반환한다.

6)-u 옵션 처럼 뒤에 추가 인자를 받고 싶다면 getopt 함수를 사용할 때 세번째 파라미터에서 문자+콜론(:)으로 추가해줘야 한다.

_option이란 '-'로 시작하는 문자열을 의미한다._

_optstring은 option character를 나타내는 문자열이다.

_optarg은 0ptarg를 참조하는 것으로 option argument를 취할 수 있다.

_optind은 getopt가 다음에 처리할 argv배열의 index로 초기값이 1이다. 그래서 최초의 호출은 argv[1]부터해석을 시작한다.

_opterr는 error log의 출력을 제어한다. 0이면 출력,아니면 출력하지 않는다.

_optopt는 에러가 발생한 option character를 보유한다.


예시
```
#!/bin/bash

## 도움말 출력하는 함수
help() {
        echo "splt [OPTIONS] FILE"
                echo "    -h         도움말 출력."
                echo "    -a ARG     인자를 받는 opt."
                echo "    -b ARG     인자를 받는 opt2."
                exit 0
}
while getopts "a:b:h" opt
do
case $opt in
a) arg_a=$OPTARG
echo "Arg A: $arg_a"
;;
b) arg_b=$OPTARG
echo "Arg B: $arg_b"
echo "$arg_b"
;;
h) help ;;
?) help ;;
esac
done

```

**3)SED명령어(STREAMLINED EDITOR)

1)수정 치환 삭제 글추가 등을 할수 있는 편집에 특화된 명령어이다.

2)SED명령어는 명령행에서 파일을 인자로 받아 명령어를 통해 작업한 후 결과를 화면으로 확인하는 방법이다.

3)_SED명령어의 특징은 원본을 손상하지 않는 다는 점이다.

SED명령어는 동직시 내부적으로 두개의 워크스페이스를 사용하는데 패턴 스페이스와 홀드 스페이스이다.
![ZOQ](https://user-images.githubusercontent.com/94745591/142759010-6caf3ac5-dc0a-45ff-a0f0-c4d5102f3f40.PNG)

_패턴 버퍼는 SED가 파일을 라인단위로 읽을 떄 그 읽힌 라인이 저장되는 임시 공간이다. SED명령어로 출력하라 하면 여기있는 버퍼 내용을 출력하는 거고, 뭔가 조작을 하면 여기 저장되어있는 내용을 조작하는 것이다. 다시 말하지만 원본을 건들이지 않는다. 죽, 이 버퍼는 현재 내가 담고 있는 정보를 갖고 있는 것이다.

_호드 스페이스는 패턴버퍼와 다르게 짤븡ㄴ 순간 임시 버퍼가 아니라 좀 더 길게 가지고 있는 저장소이다. 2라인을 작업중이더라도 1라인을 기억하고 있을 수 있는 것이다. 즉, 어떤 내용은 홀드  스페이스에 저장하면, SED가 다음행을 읽더라도 나중에 내가 원할떄 불러와서 재사용할 수 있는 버퍼가 홀드 버퍼가 된다.

**sed명령어 대표적 사용법

|서브명령어|약자|내용|
|----|--------|---------------------------|
|p   |print | 출력|
|,|x|주소범위 지정|
|d|delete|삭제|
|^|x|행의 처음|
|$|x|행의 끝|
|별|x|앞의 문자를 0개이상 찾기|
|i플래그|x|변경 대상 단어를 찾을 시 대소문자 무시|

**sed subcommand 명령어 종류와 의미

|subcommand|의미|
|----|---------------------------------------|
|a\|현재 행에 하나 이상의 새로운 행을 추가|
|c\|현재 행의 내용을 새로운 내용으로 교체|
|i\|현재 행의 위에 텍스트 삽입|
|h|패턴 스페이스 내용을 홀드 스페이스에 복사|
|H|패턴 스페이스 내용을 홀드 스페이스에 추가|
|g|홀드 스페이스의 내용을 패턴스페이스에 복사(덮어쓰기)|
|H|홀드 스페이스 내용을 패턴 스페이스에 복사(내용뒤에 추가)|
|ㅣ|출력되지 않는 특수문자를 명확하게 출력한다.|
|P|행을 출력한다.|
|n|다음 입력 행을 첫번쨰 명령어가 아닌 다음 명령어에서 처리하게 한다.|
|q|sed를 종료한다.|
|r|파일로부터 행을 읽어온다.|
|!|선택된 행을 제외한 나머지 전체 행에 명령어를 적용한다.|
|s|문자열을 치환한다.|

**sed s와 같이 쓰는 치환 플래그

|s와 쓰이는 플래그| 의미|
|-------------|-------------------------------------------|
|g|치환이 행 전체에 대해 이뤄진다.|
|p|행을 출력|
|w|파일에 쓴다|
|x|홀드 버퍼와 패턴 스페이스의 내용을 서로 맞바꾼다.|
|y|한 문자를 다른 문자로 변환한다.(y에 정규표현식 메타문자를 사용할 수 없다.|

**4.awk명령어

awk명령어는 대부분의 리눅스 명령들이 그 명령의 이름만으로 대략적인 기능이 예상되는 것과 다르게,awk명령은 이름에 그 기능을 의미하는 단어나 약어가 포함되어있지 않다. awk는 최초에 awk기능을 디자인한 사람들의 이니셜을조합하여 만든 이름이기 때문이다.

-awk는 파일로부터 레코드를 선택하고 선택된 레코드에 포함된 값을 조작하거나 데이터화하는 것을 목적으로 하는 프로그램이다. 즉, awk명령의 입력으로 지정된 파일로부터 데이터를 분류한 다음, 분류된 텍스트 데이터를 바탕으로 패턴 매칭 여부를 검사하거나 데이터 조작 및 연산 등의 액션을 수행하고, 그결과를 출력하는 기능을 수행한다.

**_awk명령으로 할 수 있는 일

_텍스트 파일의 전체 내용 출력

_파일의 특정 필드만 출력

_특정 필드에 문자열을 추가해서 출력

_패턴이 포한된 레코드 출력

_특정 필드에 연산 수행 결과 출력

_필드 값 비교에 따라 레코드 출력

![캡캡](https://user-images.githubusercontent.com/94745591/142759895-2027bc1f-fa41-4265-b2da-cfed9f490f9d.PNG)

AWK에서는 레코드가 $0, 그리고 $1,...,$N은 필드를 나타내는 열을 나타낸다.
awk의 기본 사용법은 패턴과 액션을 정의하여 입력으로 주어진 파일의 데이터를 가공하여 출력한다. 

awk는 "awk programming language"라는 프로그래밍 언어로 작성된 프로그램을 실행한다. 리눅스에서 쉘 스크립트(Shell Script)로 작성된 파일이 리눅스 쉘(Shell)에 의해 실행되는 것을 떠올리면, awk가 "awk programming language" 문법으로 작성된 코드를 실행한다는 것의 의미가 쉽게 이해될 수 있다.



awk는 기본적으로 입력 데이터를 라인(line) 단위의 레코드(Record)로 인식한다. 그리고 각 레코드에 들어 있는 텍스트는 공백 문자(space, tab)로 구분된 필드(Field)들로 분류된다.. 이렇게 식별된 레코드 및 필드의 값들은 awk 프로그램에 의해 패턴 매칭 및 다양한 액션의 파라미터로 사용된다. (참고로, 레코드 구분 문자(newline)와 필드 구분 문자(space, tab)는 awk 프로그램 옵션으로 변경할 수 있다.)
![캡캡캡](https://user-images.githubusercontent.com/94745591/142759942-bd7ec4b6-8d14-479d-a3df-8131b8e88fb5.PNG)

|옵션| 내용|
|---|--------|
|-F|필드 구분문자 지정|
|-f|awk program파일 경로 지정|
|-v|awk program에서 사용될 특정 variavle값 지정|

_awk program

_-f옵션이 사용되지 않은 경우, awk가 실행할 awk program 코드 지정

_ARGUMENT

_입력파일 지정 또는 variable값 지정


pattern과 action에 작성되는 awk program 코드에는 다양한 표현식, 변수, 함수 등이 사용됩니다. 이 중 가장 중요한 변수는 레코드와 필드를 나타내는 변수인데, 하나의 레코드는 $0, 레코드에 포함된 각 필드는 그 순서대로 $1, $2, ..., $n 으로 지정된다.
![ㅇ](https://user-images.githubusercontent.com/94745591/142760069-4da90058-a160-470f-91a3-4eda9aa11efd.PNG)




