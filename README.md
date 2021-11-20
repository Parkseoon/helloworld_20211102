# helloworld_20211102

1)getopt() 함수는 콘솔 기반의 프로그램(어플리케이션)을 제작하는데 있어 옵션 값(인자, 파라미터)을 받는데 유용한 함수다.
2)사실 '-a' 나 '--version'과 같은 옵션을 argc, argv로 처리할 수도 있겠지만 이를 직접 처리하는 것은 귀찮은 작업이다. 이를 위해 getopt 함수가 제공된다.
3)getopt() 함수는 stdio.h 와 unistd.h 모두에 정의되어있다. 두 헤더 파일에 정의된 함수의 차이는 딱히 없는 듯 하다.


'''c
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
'''
