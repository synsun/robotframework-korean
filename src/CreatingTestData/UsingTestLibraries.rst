Using test libraries
====================

..
   Test libraries contain those lowest-level keywords, often called
   *library keywords*, which actually interact with the system under
   test. All test cases always use keywords from some library, often
   through higher-level `user keywords`_. This section explains how to
   take test libraries into use and how to use the keywords they
   provide. `Creating test libraries`_ is described in a separate
   section.

테스트 라이브러리는 종종 *library keywords* 라고 불리며, 테스트 중인
시스템(SUT)과 실제로 상호작용하는 lowest-level 키워드를 포함한다. 모든
test cases는 종종 higher-level `user keywords`_ 를 통하여 항상 몇몇
라이브러리의 키워드를 사용한다. 이번 섹션에서는 테스트 라이브러리를
사용하는 방법과 라이브러리가 제공하는 키워드를 사용하는 방법을
설명한다. `테스트 라이브러리 생성 <Creating test libraries_>`__ 은
다른 섹션에서 다룰 것이다.

.. contents::
   :depth: 2
   :local:

Importing libraries
-------------------

..
   Test libraries are typically imported using the :setting:`Library` setting,
   but it is also possible to use the :name:`Import Library` keyword.

일반적으로 테스트 라이브러리는 :setting:`Library` 설정을 이용하여
임포트 된다. 또한 :name:`Import Library` 키워드를 사용할 수도 있다.

Using `Library` setting
~~~~~~~~~~~~~~~~~~~~~~~

..
   Test libraries are normally imported using the :setting:`Library`
   setting in the Setting table and having the library name in the
   subsequent column. The library name is case-sensitive (it is the name
   of the module or class implementing the library and must be exactly
   correct), but any spaces in it are ignored. With Python libraries in
   modules or Java libraries in packages, the full name including the
   module or package name must be used.

테스트 라이브러리는 일반적으로 Setting 표에서 :setting:`Library`
설정을 이용하여 임포트 하고, 라이브러리 이름을 그다음 열에 입력한다.
라이브러리 이름은 대소문자 구분을 한다. (모듈의 이름이나 라이브러리를
구현하는 클래스의 이름이고 오류가 없어야만 한다.) 이름 안의 공백은
무시된다. 모듈내 파이썬 라이브러리 혹은 패키지내 자바 라이브러리는
모듈 혹은 패키지 이름을 포함한 전체 이름이 사용되어야 한다.

..
   In those cases where the library needs arguments, they are listed in
   the columns after the library name. It is possible to use default
   values, variable number of arguments, and named arguments in test
   library imports similarly as with `arguments to keywords`__.  Both the
   library name and arguments can be set using variables.

전달인자를 필요로하는 라이브러리를 사용하는 경우, 인자들은 라이브러리
이름 뒤에 나열한다. `키워드 전달인자`__ 와 유사하게 테스트 라이브러리
임포트에서 기본 값, 가변 인자, 명명된 전달인자를 사용할 수 있다.
라이브러리 이름과 전달인자는 변수를 이용하여 설정할 수 있다.

__ `Using arguments`_

.. sourcecode:: robotframework

   *** Settings ***
   Library    OperatingSystem 
   Library    com.company.TestLib
   Library    MyLibrary    arg1    arg2
   Library    ${LIBRARY}
   
..
   It is possible to import test libraries in `test case files`_,
   `resource files`_ and `test suite initialization files`_. In all these
   cases, all the keywords in the imported library are available in that
   file. With resource files, those keywords are also available in other
   files using them.

`Test case files`_, `resource files`_, `test suite initialization
files`_ 에서 테스트 라이브러리를 임포트 할 수 있다. 모든 케이스에서,
임포트된 라이브러리의 모든 키워드는 그 파일 안에서 사용가능하다.
Resource 파일에 있는 키워드들은 이 파일을 이용해서 다른 파일에서도
사용할 수 있다.

Using `Import Library` keyword
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Another possibility to take a test library into use is using the
   keyword :name:`Import Library` from the BuiltIn_ library. This keyword
   takes the library name and possible arguments similarly as the
   :setting:`Library` setting. Keywords from the imported library are
   available in the test suite where the :name:`Import Library` keyword was
   used. This approach is useful in cases where the library is not
   available when the test execution starts and only some other keywords
   make it available.

테스트 라이브러리를 사용할 수 있게 하는 다른 방법은 BuiltIn_
라이브러리에서 :name:`Import Library` 를 사용하는 것이다. 이 키워드는
:setting:`Library` 설정과 비슷하게, 라이브러리의 이름과 사용가능한
전달인자를 받는다. 임포트된 라이브러리의 키워드는 :name:`Import
Library` 키워드가 사용되었던 테스트 스위트에서 사용할 수 있다.
라이브러리는 사용할 수 없고, 테스트 실행이 시작되고 몇몇 다른
키워드들만이 사용 가능한 경우, 이런 접근은 유용하다.

.. sourcecode:: robotframework

   *** Test Cases ***
   Example
       Do Something 
       Import Library    MyLibrary    arg1    arg2
       KW From MyLibrary

Specifying library to import
----------------------------

..
   Libraries to import can be specified either by using the library name
   or the path to the library. These approaches work the same way regardless
   is the library imported using the :setting:`Library` setting or the
   :name:`Import Library` keyword.

임포트 하기 위한 라이브러리는 라이브러리 이름이나 라이브러리 경로로
지정할 수 있다. 이러한 접근 방식은 :setting:`Library` 를 설정하거나
:name:`Import Library` 키워드를 사용것과 관계없이 동일한 방법으로
임포트된 라이브러리와 동작한다.

Using library name
~~~~~~~~~~~~~~~~~~

..
   The most common way to specify a test library to import is using its
   name, like it has been done in all the examples in this section. In
   these cases Robot Framework tries to find the class or module
   implementing the library from the `module search path`_. Libraries that
   are installed somehow ought to be in the module search path automatically,
   but with other libraries the search path may need to be configured separately.

테스트 라이브러리를 임포트 하는 가장 보편적인 방법은 이 섹션의 모두
예제에서 수행한 것 처럼 그 이름을 사용하는 것이다. 이런 경우 Robot
Framework는 `모듈 검색 경로 <module search path_>`__ 로부터
라이브러리를 구현한 클래스나 모듈을 찾으려고 시도한다. 설치된
라이브러리는 어떻게든 자동으로 모듈 검색 경로에 있어야 하지만
다른 라이브러리의 검색경로는 별도로 구성할 필요가 있다.


..
   The biggest benefit of this approach is that when the module search
   path has been configured, often using a custom `start-up script`_,
   normal users do not need to think where libraries actually are
   installed. The drawback is that getting your own, possible
   very simple, libraries into the search path may require some
   additional configuration.

이러한 접근의 가장 큰 장점은 사용자 정의 `start-up script`_ 를
사용해서 모듈 탐색 경로가 설정되었을 때, 일반 사용자는 실제로
라이브러리가 어디에 설치되는 것인지 생각할 필요가 없다. 단점은 검색
경로에 가능한 아주 간단한 자신만의 라이브러리가 몇몇 추가적인 구성을
요구한다는 것이다.

Using physical path to library
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Another mechanism for specifying the library to import is using a
   path to it in the file system. This path is considered relative to the
   directory where current test data file is situated similarly as paths
   to `resource and variable files`_. The main benefit of this approach
   is that there is no need to configure the module search path.

임포트할 할 라이브러리를 지정하는 다른 메카니즘은 파일시스템에서
그것에 대한 경로를 사용하는 것이다. 이 경로는 현재 테스트 데이터
파일이 위치한 곳을 기준으로 `resource and variable files`_ 의 경로와
유사하게 상대경로가 이용된다. 이러한 접근의 주요 장점은 모듈 탐색
경로를 설정할 필요가 없다는 것이다.

..
   If the library is a file, the path to it must contain extension. For
   Python libraries the extension is naturally :file:`.py` and for Java
   libraries it can either be :file:`.class` or :file:`.java`, but the
   class file must always be available. If Python library is implemented
   as a directory, the path to it must have a trailing forward slash (`/`).
   Following examples demonstrate these different usages.

만일 라이브러리가 파일이라면, 경로는 반드시 확장자를 포함해야한다.
파이썬 라이브러리에서 확장자는 당연히 :file:`.py` 이고, 자바
라리브러리는 :file:`.class` 혹은 :file:`.java` 이지만 클래스 파일이
항상 사용가능하다. 만약 파이썬 라이브러리가 디렉토리로서 구현된다면,
경로는 뒤에 반드시 슬래시 (`/`)를 포함해야 한다. 다음 예제에서 이 다른
사용예를 보인다.

.. sourcecode:: robotframework

   *** Settings ***
   Library    PythonLib.py
   Library    /absolute/path/JavaLib.java
   Library    relative/path/PythonDirLib/    possible    arguments
   Library    ${RESOURCES}/Example.class

..
   A limitation of this approach is that libraries implemented as Python classes `must
   be in a module with the same name as the class`__. Additionally, importing
   libraries distributed in JAR or ZIP packages is not possible with this mechanism.

이러한 접근의 한계는 파이썬 클래스로 구현된 라이브러리는 `반드시
클래스와 동일한 이름을 가지는 모듈어야 한다`__ 는 것이다. 추가적으로,
JAR 혹은 ZIP 패키지로 분산된 라이브러리를 임포팅 하는것은 이
매카니즘으로는 불가능하다.

__ `Test library names`_

Setting custom name to test library
-----------------------------------

..
   The library name is shown in test logs before keyword names, and if
   multiple keywords have the same name, they must be used so that the
   `keyword name is prefixed with the library name`__. The library name
   is got normally from the module or class name implementing it, but
   there are some situations where changing it is desirable:

키워드 이름 앞에 있는 라이브러리 이름은 테스트 로그에서 확인할 수
있다. 복수의 키워드가 같은 이름을 가지고 있다면, `키워드 이름은
라이브러리 이름 접두어를 가진다`__ 를 사용해야 한다. 이 라이브러리
이름은 일반적으로 구현된 모듈이나 클래스 이름에서 가져오지만 그것을
변경하는 것이 바람직한 경우도 있다:

__ `Handling keywords with same names`_

..
   - There is a need to import the same library several times with
     different arguments. This is not possible otherwise.

   - The library name is inconveniently long. This can happen, for
     example, if a Java library has a long package name.

   - You want to use variables to import different libraries in
     different environments, but refer to them with the same name.

   - The library name is misleading or otherwise poor. In this case,
     changing the actual name is, of course, a better solution.

- 같은 라이브러리에 다른 전달인자를 사용할 경우, 여러번 임포트 해야할
  필요가 있다. 이것은 그렇게 하지 않으면 불가능하다.

- 라이브러리 이름은 불편하게 길다. 예를들어, 자바 라이브러리가 긴
  패키지 이름을 가졌을 경우 발생할 수 있는 일이다.

- 다른 환경에서 다른 라이브러리를 임포트 하기 위하여 변수를 사용할
  수 있다. 하지만 그것들은 같은 이름으로 불린다.

- 라이브러리 이름이 오해의 소지가 있거나 별로일 수 있다. 이런 경우,
  실제 이름을 바꾸는 것이 나은 방법이다.

..
   The basic syntax for specifying the new name is having the text
   `WITH NAME` (case-insensitive) after the library name and then
   having the new name in the next cell. The specified name is shown in
   logs and must be used in the test data when using keywords' full name
   (:name:`LibraryName.Keyword Name`).

새로운 이름을 짓는 기본 문법은 라이브러리 이름 뒤에 `WITH NAME`
(대소문자 구분) 문자를 갖도록 하고 다음 셀에 새로운 이름을 입력하도록
한다. 지정된 이름은 로그에 나타나고, 키워드의 완전한 이름
(:name:`LibraryName.Keyword Name`)을 사용할 때 테스트 데이터에서
사용되어야만 한다.

.. sourcecode:: robotframework

   *** Settings ***
   Library    com.company.TestLib    WITH NAME    TestLib
   Library    ${LIBRARY}             WITH NAME    MyName

..
   Possible arguments to the library are placed into cells between the
   original library name and the `WITH NAME` text. The following example
   illustrates how the same library can be imported several times with
   different arguments:

라이브러리에 사용가능한 전달인자는 원본 라이브러리 이름과 `WITH NAME`
문자 사이 셀에 위치한다. 다음 예제는 어떻게 같은 라이브버리가 여러번
다른 전달인자를 가지고 임포팅될 수 있는지 보여준다.

.. sourcecode:: robotframework

   *** Settings ***
   Library    SomeLibrary    localhost        1234    WITH NAME    LocalLib
   Library    SomeLibrary    server.domain    8080    WITH NAME    RemoteLib

   *** Test Cases ***
   My Test
       LocalLib.Some Keyword     some arg       second arg
       RemoteLib.Some Keyword    another arg    whatever
       LocalLib.Another Keyword

..
   Setting a custom name to a test library works both when importing a
   library in the Setting table and when using the :name:`Import Library` keyword.

테스트 라이브러리에 사용자 정의 이름을 설정하는 것은 라이브러리를 설정
표에서 임포팅할 때와 :name:`Import Library` 키워드를 사용할 때 둘다
동작한다.

Standard libraries
------------------

..
   Some test libraries are distributed with Robot Framework and these
   libraries are called *standard libraries*. The BuiltIn_ library is special,
   because it is taken into use automatically and thus its keywords are always
   available. Other standard libraries need to be imported in the same way
   as any other libraries, but there is no need to install them.

몇몇 테스트 라이브러리는 Robot Framework를 통해 배포되고 이
라이브러리는 *standard libraries* 로 불리운다. BuiltIn_ 라이브러리는
특별하다. 왜냐하면 자동으로 사용 가능하고, 키워드도 항상 사용가능하다.
다른 표준 라이브러리는 다른 라이브러리들과 같은 방법으로 임포트되어야
하지만 별도로 설치할 필요는 없다.

Normal standard libraries
~~~~~~~~~~~~~~~~~~~~~~~~~

..
   The available normal standard libraries are listed below with links to their
   documentations:

사용가능한 일반 표준 라이브러리는 아래에 하이퍼링크를 이용하여
각각의 설명으로 이동 가능하도록 나열하였다:

  - BuiltIn_
  - Collections_
  - DateTime_
  - Dialogs_
  - OperatingSystem_
  - Process_
  - Screenshot_
  - String_
  - Telnet_
  - XML_

.. _BuiltIn: ../libraries/BuiltIn.html
.. _Collections: ../libraries/Collections.html
.. _DateTime: ../libraries/DateTime.html
.. _Dialogs: ../libraries/Dialogs.html
.. _OperatingSystem: ../libraries/OperatingSystem.html
.. _Process: ../libraries/Process.html
.. _String: ../libraries/String.html
.. _Screenshot: ../libraries/Screenshot.html
.. _Telnet: ../libraries/Telnet.html
.. _XML: ../libraries/XML.html

Remote library
~~~~~~~~~~~~~~

..
   In addition to the normal standard libraries listed above, there is
   also :name:`Remote` library that is totally different than the other standard
   libraries. It does not have any keywords of its own but it works as a
   proxy between Robot Framework and actual test library implementations.
   These libraries can be running on other machines than the core
   framework and can even be implemented using languages not supported by
   Robot Framework natively.

위에서 살펴본 표준 라이브러리 뿐만 아니라 다른 표준 라이브러리와는
전체적으로 다른 :name:`Remote` 라이브러리도 있다. 이것은 자신 소유의
키워드를 가지고있지 않지만, Robot Framework와 실제 테스트 라이브러리
구현 사이에서 프록시처럼 작동할 수 있다. 이 라이브러리는 코어
프레임워크과는 다른 머신에서 수행할 수 있고 기존 Robot Framework에서
지원하지 않는 언어까지도 사용하여 구현 할 수 있다.

..
   See separate `Remote library interface`_ section for more information
   about this concept.

이 개념에 대한 더 많은 정보를 확인하려면 `Remote library interface`_
섹션을 참조하라.

External libraries
------------------

..
   Any test library that is not one of the standard libraries is, by
   definition, *an external library*. The Robot Framework open source community
   has implemented several generic libraries, such as Selenium2Library_ and
   SwingLibrary_, which are not packaged with the core framework. A list of
   publicly available libraries can be found from http://robotframework.org.

표준 라이브러리에 속하지 않는 테스트 라이브러리는 *an external
library* 정의한다. Robot Framework 오픈 소스 커뮤니티는 코어
프레임워크의 패키지가 아닌 Selenium2Library_ 와 SwingLibrary_ 같은
몇몇 일반적인 라이브러리를 구현했다. 공개적으로 사용가능한 라이브러리
목록은 http://robotframework.org 에서 확인할 수 있다.

..
   Generic and custom libraries can obviously also be implemented by teams using
   Robot Framework. See `Creating test libraries`_ section for more information
   about that topic.

일반 및 사용자 정의 라이브러리는 명백하게 Robot Framework를 사용하는
팀에 의해 구현될 수 있다. 더 많은 정보는 `테스트 라이브러리 만들기
<Creating test libraries_>`__ 섹션에서 확인할 수 있다.

..
   Different external libraries can have a totally different mechanism
   for installing them and taking them into use. Sometimes they may also require
   some other dependencies to be installed separately. All libraries
   should have clear installation and usage documentation and they should
   preferably automate the installation process.

다른 외부 라이브러리는 설치와 사용하기 위해 취득하는 부분에서
전체적으로 다른 메카니즘을 가진다. 때때로 설치되기 위해 각각 다른
의존성을 가진다. 모든 라이브러리는 명백한 설치와 사용 문서를 가져야
하고, 가급적이면 설치 과정이 자동화 되어야 한다.
