.. _tidy:

Test data clean-up tool (Tidy)
==============================

.. contents::
   :depth: 1
   :local:

..
   Tidy is Robot Framework's built-in a tool for cleaning up and changing
   the format of Robot Framework test data files.

Tidy는 로봇 프레임워크에서 clean-up해주고, 로봇 프레임워크 테스트
데이타 파일의 포맷을 변경해주는 도구를 빌트 인하는 것이다.

..
   The output is written into the standard output stream by default, but
   an optional output file can be given starting from Robot Framework 2.7.5.
   Files can also be modified in-place using :option:`--inplace` or
   :option:`--recursive` options.

아웃풋은 기본으로 지정된 표준 아웃풋 스트림으로 작성된다. 하지만
추가적인 아웃풋 파일은 로봇 프레임워크 2.7.5부터 주어진다. 파일은
:option:`--inplace` 나 :option:`--recursive` 옵션을 이용하여 현재
위치에서 수정할 수 있다.

General usage
-------------

Synopsis
~~~~~~~~

::

    python -m robot.tidy [options] inputfile
    python -m robot.tidy [options] inputfile [outputfile]
    python -m robot.tidy --inplace [options] inputfile [more input files]
    python -m robot.tidy --recursive [options] directory

Options
~~~~~~~

 ..
    -i, --inplace    Tidy given file(s) so that original file(s) are overwritten
		     (or removed, if the format is changed). When this option is
		     used, it is possible to give multiple input files. Examples

 -i, --inplace    Tidy는 원본 파일은 덮어쓰기가 된다(혹은 포맷이 변경된 경우 삭제).
                  이 옵션이 사용될 때, 여러개의 입력 파일을 제공할 수 있다. 예를들어::

                      python -m robot.tidy --inplace tests.html
                      python -m robot.tidy --inplace --format txt *.html

 ..
    -r, --recursive  Process given directory recursively. Files in the directory
		     are processed in place similarly as when :option:`--inplace`
		     option is used.

 -r, --recursive  디렉토리가 회귀적으로 주어진 프로세스. 디렉토리
                  내부의 파일은 :option:`--inplace` 옵션이 사용될 때와
                  비슷하게 그자리에서 처리된다.

 ..
    -f, --format <robot|txt|html|tsv>
		     Output file format. If the output file is given explicitly,
		     the default value is got from its extension. Otherwise
		     the format is not changed.

 -f, --format <robot|txt|html|tsv>
                  출력 파일 형식. 만약 출력 파일이 명백히 주어졌다면, 기본 값은 파일의 확장자에서 얻을 수 있다.
                  그렇지 않으면 형식은 변하지 않는다.

 ..
    -p, --use-pipes  Use a pipe character (|) as a cell separator in the txt format.

 -p, --use-pipes  파이프 문자(|)를 txt 형식에서 셀 구분자로 사용한다.

 ..
    -s, --spacecount <number>
		     The number of spaces between cells in the txt format.
		     New in Robot Framework 2.7.3.

 -s, --spacecount <number>
                  txt 형식에서 셀간 공백의 수. 로봇 프레임워크 2.7.3부터 도입 되었다.

 ..
    -l, --lineseparator <native|windows|unix>
		     Line separator to use in outputs. The default is 'native'.

		     - *native*: use operating system's native line separators
		     - *windows*: use Windows line separators (CRLF)
		     - *unix*: use Unix line separators (LF)

		     New in Robot Framework 2.7.6.

 -l, --lineseparator <native|windows|unix>
                  출력에서 사용하기 위한 줄 구분자. 기본 값은 'native'.

                  - *native*: 운영체제의 네이티브 줄 구분자로 사용
                  - *windows*: 윈도우즈에서 줄 구분자로 사용 (CRLF)
                  - *unix*: Unix에서 줄 구분자로 사용 (LF)

                  로봇 프레임워크 2.7.6부터 도입 되었다.

 ..
    -h, --help       Show this help.

 -h, --help       이 도움말을 확인하세요.

Alternative execution
~~~~~~~~~~~~~~~~~~~~~

..
   Although Tidy is used only with Python in the synopsis above, it works
   also with Jython and IronPython. In the synopsis Tidy is executed as
   an installed module (`python -m robot.tidy`), but it can be run also as
   a script

Tidy가 위의 시놉시스에서 Python을 이용한 것만 다루었지만, Jython이나
IronPython에서도 동작한다. 시놉시스 Tidy는 설치된 모듈(`python -m
robot.tidy`) 처럼 수행된다. 하지만 스크립트로도 수행된다::

    python path/robot/tidy.py [options] arguments

..
   Executing as a script can be useful if you have done `manual installation`_
   or otherwise just have the :file:`robot` directory with the source code
   somewhere in your system.

스크립트를 수행하는것은 `manual installation`_ 가 완료되거나 시스템
내에 소스코드를 저장해둔 :file:`robot` 디렉토리를 가진다면 유용할 수
있다.

Output encoding
~~~~~~~~~~~~~~~

..
   All output files are written using UTF-8 encoding. Outputs written to the
   console use the current console encoding.

모든 아웃풋 파일은 UTF-8 인코딩을 이용하여 작성된다. 현재의 콘솔
인코딩을 이용해서 아웃풋이 콘솔에 입력된다.

Cleaning up test data
---------------------

..
   Test case files created with HTML editors or written by hand can be normalized
   using Tidy. Tidy always writes consistent headers, consistent order for
   settings, and consistent amount of whitespace between cells and tables.

HTML 에디터를 이용해서 생성된 혹은 직접 작성한 테스트 케이스 파일은
Tidy를 이용해서 정규화 한다. Tidy는 항상 한결같은 헤더, 한결같은 설정
순서, 한결같은 셀과 테이블 사이의 공백의 수를 갖는다.

Examples::

    python -m robot.tidy messed_up_tests.html cleaned_tests.html
    python -m robot.tidy --inplace tests.txt

Changing test data format
-------------------------

..
   Robot Framework supports test data in HTML, TSV and TXT formats and Tidy
   makes changing between the formats trivial. Input format is always determined
   based on the extension of the input file. Output format can be set using
   the :option:`--format` option, and the default value is got from the extension
   of the possible output file.

로봇 프레임워크는 HTML, TSV, TXT 형식의 테스트 데이터를 지원한다.
Tidy는 포맷간 변경을 쉽게 할 수 있다. 입력 포맷은 항상 입력 파일의
확장자에 따라 결정된다. 출력 포맷은 :option:`--format` 옵션을 이용해서
설정할 수 있고, 디폴트 값은 출력 파일의 확장자에서 얻는다.

Examples::

    python -m robot.tidy tests.html tests.txt
    python -m robot.tidy --format txt --inplace tests.html
    python -m robot.tidy --format tsv --recursive mytests
