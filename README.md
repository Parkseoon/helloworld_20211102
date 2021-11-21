# helloworld_20211102

1)getopt() 함수는 콘솔 기반의 프로그램(어플리케이션)을 제작하는데 있어 옵션 값(인자, 파라미터)을 받는데 유용한 함수다.

2)사실 '-a' 나 '--version'과 같은 옵션을 argc, argv로 처리할 수도 있겠지만 이를 직접 처리하는 것은 귀찮은 작업이다. 이를 위해 getopt 함수가 제공된다.

3)getopt() 함수는 stdio.h 와 unistd.h 모두에 정의되어있다. 두 헤더 파일에 정의된 함수의 차이는 딱히 없는 듯 하다.


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

	return 0;ㅎㄷ새ㅔㅅ
}

```

4)getopt는 int 형의 반환형을 가지며, 세번째 파라미터(인자)에 포함된 문자를 받는다면 해당 문자의 값을 반환한다.

5)-u 옵션 처럼 뒤에 추가 인자를 받고 싶다면 getopt 함수를 사용할 때 세번째 파라미터에서 문자+콜론(:)으로 추가해줘야 한다.

| 명령어 |설명|
|optarg|0ptarg를 참조하는 것으로 option argument를 취할 수 있다.|
|optind|getopt가 다음에 처리할 argv배열의 index로 초기값이 1이다. 그래서 최초의 호출은 argv[1]부터해석을 시작한다.|
|opterr|error log의 출력을 제어한다. 0이면 출력,아니면 출력하지 않는다.|
|optopt|에러가 발생한 option character를 보유한다.|
