Extending the Robot Framework Jar
=================================

..
   Adding additional test libraries or support code to the Robot Framework jar is
   quite straightforward using the ``jar`` command included in standard JDK
   installation. Python code must be placed in :file:`Lib` directory inside
   the jar and Java code can be placed directly to the root of the jar, according
   to package structure.

추가적인 테스트 라이브러리를 추가하거나 로봇 프레임워크 jar 코드를
지원하는 것은 표준 JDK 설치에 포함된 ``jar`` 명령어를 이용하여 꽤
간단하게 할 수 있다. 파이썬 코드는 jar 내부의 :file:`Lib` 디렉토리에
위치해야만 한다. 자바 코드는 패키지 구조에 따라 직접적으로 jar의
root로 입력될 것이다.

..
   For example, to add Python package `mytestlib` to the jar, first copy the
   :file:`mytestlib` directory under a directory called :file:`Lib`, then run
   following command in the directory containing :file:`Lib`

예를들어, 파이썬 패키지 `mytestlib`를 jar에 추가하기 위해서,
:file:`Lib` 라고 불리는 디렉토리 아래의 :file:`mytestlib` 디렉토리를
복사해야한다. 그리고 나서 아래의 :file:`Lib` 를 포함하는 디렉토리의
명령어를 수행해야 한다::

  jar uf /path/to/robotframework-2.7.1.jar Lib

..
   To add compiled java classes to the jar, you must have a directory structure
   corresponding to the Java package structure and add that recursively to the
   zip.

jar에 컴파일된 자바 클래스를 추가하기 위해서, 자바 패키지 구조에
해당하는 디렉토리 구조를 가져야만 한다. 그리고 회귀적으로 zip에
추가되어야 한다.

..
   For example, to add class `MyLib.class`, in package `org.test`,
   the file must be in :file:`org/test/MyLib.class` and you can execute

예를들어, `org.test` 패키지의 클래스 `MyLib.class` 를 추가하기 위해서,
파일은 :file:`org/test/MyLib.class` 안에 있어야만 하고 다음을 실행해야
한다::

  jar uf /path/to/robotframework-2.7.1.jar org
