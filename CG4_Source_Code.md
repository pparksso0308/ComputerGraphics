

## InitShader.h

### fopen_s()
1. 용도 :
2. _s : safety
3. 입력서식 :  FILE * fp = fopen_s(&fp, const char * filename, const char * mode); // &fp : 파일 포인터의 주소

### fseek()
1. 용도 : stream과 연관된 현재 파일 위치를 파일 내 새 위치로 변경
2. 입력서식 : int fseek(FILE *stream, long int offset, int origin);
3. origine에 올 수 있는 상수 : SEEK_SET(파일의 시작), SEEK_CUR(파일 포인터의 현재 위치), SEEK_END(파일의 끝)
4. 리턴값 : 성공(return 0), 실패(return error)

### ftell()
1. 용도 : stream과 연관된 파일의 현재 위치를 찾음
2. 입력서식 : long int ftell(FILE *stream);
3. 리턴값 : 성공(return 현재 파일의 위치), 실패(return error)

### fread()
1. 용도: 입력 stream에서 size 길이의 count항목까지 읽고, 지정된 buffer에 저장, 파일의 위치는 읽은 바이트의 수만큼 증가한다.
2. 입력서식 : size_t fread(void *buffer, size_t size, size_t count, FILE *stream);
3. 리턴값 : 성공(읽기에 성공한 전체 항목의 수), 실패(count전에 파일이 끝나면, count 보다 적은 값이 리턴, size 또는 count가 0이면 return 0)

### 소스코드
````
#include <iostream>

// Create a NULL-terminated string by reading the provided file
static char* readShaderSource(const char* shaderFile)
{
	FILE* fp;
	fopen_s(&fp, shaderFile, "rb");

	if (fp == NULL) { return NULL; }	// 파일이 잘 열렸는지 확인

	fseek(fp, 0L, SEEK_END);	// 파일 포인터를 맨 뒤로 이동
	long size = ftell(fp);		// size : 현재 파일 fp의 위치

	fseek(fp, 0L, SEEK_SET);	// 파일 포인터를 맨 앞으로 이동
	char* buf = new char[size + 1];	// buffer 할당
	fread(buf, 1, size, fp);	// buf에 파일 저장

	buf[size] = '\0';	// 문자열로 저장
	fclose(fp);

	return buf;	// buf 리턴
}


// Create a GLSL program object from vertex and fragment shader files
GLuint InitShader(const char* vShaderFile, const char* fShaderFile)
{
	struct Shader {
		const char*  filename;
		GLenum       type;
		GLchar*      source;
	}  shaders[2] = {
	{ vShaderFile, GL_VERTEX_SHADER, NULL },
	{ fShaderFile, GL_FRAGMENT_SHADER, NULL }
	};

	GLuint program = glCreateProgram();

	for (int i = 0; i < 2; ++i) {
		Shader& s = shaders[i];
		s.source = readShaderSource(s.filename);
		if (shaders[i].source == NULL) {
			std::cerr << "Failed to read " << s.filename << std::endl;
			exit(EXIT_FAILURE);
		}

		GLuint shader = glCreateShader(s.type);
		glShaderSource(shader, 1, (const GLchar**)&s.source, NULL);
		glCompileShader(shader);

		GLint  compiled;
		glGetShaderiv(shader, GL_COMPILE_STATUS, &compiled);
		if (!compiled) {
			std::cerr << s.filename << " failed to compile:" << std::endl;
			GLint  logSize;
			glGetShaderiv(shader, GL_INFO_LOG_LENGTH, &logSize);
			char* logMsg = new char[logSize];
			glGetShaderInfoLog(shader, logSize, NULL, logMsg);
			std::cerr << logMsg << std::endl;
			delete[] logMsg;

			exit(EXIT_FAILURE);
		}

		delete[] s.source;

		glAttachShader(program, shader);
	}

	/* link  and error check */
	glLinkProgram(program);

	GLint  linked;
	glGetProgramiv(program, GL_LINK_STATUS, &linked);
	if (!linked) {
		std::cerr << "Shader program failed to link" << std::endl;
		GLint  logSize;
		glGetProgramiv(program, GL_INFO_LOG_LENGTH, &logSize);
		char* logMsg = new char[logSize];
		glGetProgramInfoLog(program, logSize, NULL, logMsg);
		std::cerr << logMsg << std::endl;
		delete[] logMsg;

		exit(EXIT_FAILURE);
	}

	/* use program object */
	glUseProgram(program);

	return program;
}
````
## What happend
* 대부분의 OpenGL 기능이 사용되지 않음
* 더 이상 존재하지 않는 상태 변수 기본값을 무겁게 사용
  - 보기
  - 색상
  - 창 매개변수
* 다음 버전에서는 기본값을 보다 명확하게 나타낼것
* 그러나 processing 루프는 동일하다.
