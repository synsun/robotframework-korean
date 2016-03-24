Creating test suites
====================

..
   Robot Framework test cases are created in test case files, which can
   be organized into directories. These files and directories create a
   hierarchical test suite structure.

Robot Framework test cases는 디렉토리 안에 구성되는 테스트 파일 안에 생성된다.
이 파일과 디렉토리는 계층적인 test suite 구조를 생성한다.

.. contents::
   :depth: 2
   :local:

Test case files
---------------

..
   Robot Framework test cases `are created`__ using test case tables in
   test case files. Such a file automatically creates a test suite from
   all the test cases it contains. There is no upper limit for how many
   test cases there can be, but it is recommended to have less than ten,
   unless the `data-driven approach`_ is used, where one test case consists of
   only one high-level keyword.

Robot Framework test cases는 test case 파일에서 test case 표를
이용하여 `생성 된다`__. Test case 파일은 자동적으로 모든 테스트
케이스를 포함하는 test suite를 생성한다. 얼마나 많은 테스트를 만들 수
있는지에 대한 제한은 없지만, 하나의 상위 레벨 키워드로 하나의 테스트
케이스를 구성하여 사용하는 `data-driven approach`_ 를 사용하지 않는한
10개 이하로 만들것을 권장한다.

..
   The following settings in the Setting table can be used to customize the
   test suite:

아래와 같은 Setting 표에서의 설정은 test suite마다 개별 설정 가능하다:

`Documentation`:setting:
   Used for specifying a `test suite documentation`_
`Metadata`:setting:
   Used for setting `free test suite metadata`_ as name-value
   pairs.
`Suite Setup`:setting:, `Suite Teardown`:setting:
   Specify `suite setup and teardown`_.

..
   .. note:: All setting names can optionally include a colon at the end, for
	 example :setting:`Documentation:`. This can make reading the settings easier
	 especially when using the plain text format.

.. note:: 모든 설정이름은 선택적으로 끝에 콜론을 포함 할 수 있다. (예,
          :setting:`Documentation:`) 이것은 특히 일반 텍스트 형식을
          사용할 때 설정을 더 쉽게 읽도록 한다.
      
__ `Test case syntax`_

Test suite directories
----------------------

..
   Test case files can be organized into directories, and these
   directories create higher-level test suites. A test suite created from
   a directory cannot have any test cases directly, but it contains
   other test suites with test cases, instead. These directories can then be
   placed into other directories creating an even higher-level suite. There
   are no limits for the structure, so test cases can be organized
   as needed.

Test case 파일들은 디렉토리 안에 구성될 수 있다. 그리고 이런
디렉토리는 higher-level test suites을 생성한다. 디렉토리로부터 생성된
test suite는 테스트 케이스를 직접 가질 수는 없다. 하지만 대신에 test
cases 를 가진 다른 test suites를 포함할 수 있다. 그래서 이런
디렉토리는 higher-level suite를 생성하는 다른 디렉토리 안에 위치 할 수
있다. 구조에는 아무런 제한이 없으므로 test cases는 필요한대로 구성될
수 있다.

..
   When a test directory is executed, the files and directories it
   contains are processed recursively as follows:

디렉토리가 실행될 때, 포함된 파일과 디렉토리는 아래와 같이 재귀적으로 실행된다:

..
   - Files and directories with names starting with a dot (:file:`.`) or an
     underscore (:file:`_`) are ignored.
   - Directories with the name :file:`CVS` are ignored (case-sensitive).
   - Files not having one of the `recognized extensions`__ (:file:`.html`,
     :file:`.xhtml`, :file:`.htm`, :file:`.tsv`, :file:`.txt`, :file:`.rst`,
     or :file:`.rest`) are ignored (case-insensitive).
   - Other files and directories are processed.

- 점(:file:`.`) 혹은 언더바(:file:`_`)로 시작하는 파일과 디렉토리
  이름은 무시된다.
- 이름에 :file:`CVS` 를 갖는 디렉토리는 무시된다. (대소문자 구분함)
- `인식되는 확장자`__ (:file:`.html`, :file:`.xhtml`, :file:`.htm`,
  :file:`.tsv`, :file:`.txt`, :file:`.rst`, or :file:`.rest`) 중에
  하나도 갖지 않는 파일은 무시된다. (대소문자 구분함)
- 다른 파일과 디렉토리는 처리된다.

..
   If a file or directory that is processed does not contain any test
   cases, it is silently ignored (a message is written to the syslog_)
   and the processing continues.

만일 파일 혹은 디렉토리가 test cases를 갖지않은 채 실행된다면,
조용히 무시된다. (메시지는 syslog_ 에 남을 것)

__ `Supported file formats`_

Warning on invalid files
~~~~~~~~~~~~~~~~~~~~~~~~

..
   Normally files that do not have a valid test case table are silently ignored
   with a message written to the syslog_. It is possible to use a command line
   option :option:`--warnonskippedfiles`, which turns the message into a warning
   shown in `test execution errors`__.

일반적으로 유효한 test case 표를 가지지 않는 파일은 syslog_ 에
메시지를 남기고, 무시된다. 메시지를 `테스트 실행 에러`__ 에 경고
메시지를 출력하려면 명령행 옵션 :option:`--warnonskippedfiles` 을
사용한다.

__ `Errors and warnings during execution`_

Initialization files
~~~~~~~~~~~~~~~~~~~~

..
   A test suite created from a directory can have similar settings as a suite
   created from a test case file. Because a directory alone cannot have that
   kind of information, it must be placed into a special test suite initialization
   file. An initialization file name must always be of the format
   :file:`__init__.ext`, where the extension must be one of the `supported
   file formats`_ (for example, :file:`__init__.robot` or :file:`__init__.html`).
   The name format is borrowed from Python, where files named in this manner
   denote that a directory is a module.

디렉토리로부터 생성된 test suite는 테스트 케이스 파일로부터 생성된
suite와 비슷한 설정을 갖는다. 디렉토리 하나만으로는 그런 종류의 정보를
가질 수 없기 때문에, 특별한 test suite 초기화 파일을 갖게 된다. 초기화
파일 이름은 항상 `지원되는 파일 형식 <supported file formats_>`__ (예,
:file:`__init__.robot` or :file:`__init__.html`) 확장자를 가지는
:file:`__init__.ext` 형식을 가져야만 한다. 이름 형식은 파이썬에서
차용한 것이다. 이런 방식으로 이름붙여진 파일은 디렉토리가 모듈임을
나타내는 것이다.

..
   Initialization files have the same structure and syntax as test case files,
   except that they cannot have test case tables and not all settings are
   supported. Variables and keywords created or imported in initialization files
   *are not* available in the lower level test suites. If you need to share
   variables or keywords, you can put them into `resource files`_ that can be
   imported both by initialization and test case files.

초기화 파일은 test case 표를 갖지 못하는 것과, 모든 설정을 지원하지
않는 것을 빼고는 test case 파일과 같은 구조와 문법을 갖는다. 초기화
파일에서 생성되거나 임포트된 변수와 키워드는 lower level test
suites에서는 *사용 불가능하다*. 만일 변수와 키워드를 공유해야한다면,
초기화 파일과 테스트 케이스 파일을 이용하여 임포트 되는 `resource
files`_ 에 넣어두어야 한다.

..
   The main usage for initialization files is specifying test suite related
   settings similarly as in `test case files`_, but setting some `test case
   related settings`__ is also possible. How to use different settings in the
   initialization files is explained below.

초기화 파일의 주요 용도는 `test case files`_ 과 비슷하게 test suite과
관련된 설정을 지정하는 것이다. 하지만 `test case 관련 설정`__ 도 종종
가능하다. 어떻게 초기화 파일에서 다른 설정을 사용하는 방법은 아래에서
설명한다.

..
   `Documentation`:setting:, `Metadata`:setting:, `Suite Setup`:setting:, `Suite Teardown`:setting:
      These test suite specific settings work the same way as in test case files.
   `Force Tags`:setting:
      Specified tags are unconditionally set to all test cases in all test case files
      this directory contains directly or recursively.
   `Test Setup`:setting:, `Test Teardown`:setting:, `Test Timeout`:setting:
      Set the default value for test setup/teardown or test timeout to all test
      cases this directory contains. Can be overridden on lower level.
      Support for defining test timeout in initialization files was added in
      Robot Framework 2.7.
   `Default Tags`:setting:, `Test Template`:setting:
      Not supported in initialization files.

`Documentation`:setting:, `Metadata`:setting:, `Suite Setup`:setting:, `Suite Teardown`:setting:
   이 test suite 상세 설정은 test case 파일과 같은 방법으로
   동작한다.
   
`Force Tags`:setting:
   명시된 태그는 절대적으로 test case 파일의 모든 test cases가 
   디렉토리를 직접적으로 포함할지 재귀적으로 포함할지 설정한다.
   
`Test Setup`:setting:, `Test Teardown`:setting:, `Test Timeout`:setting:
   이 디렉토리가 포함하는 모든 test cases에 test setup과 teardown 혹은
   test timeout 기본 값을 설정한다. lower level에서 재정의 될 수 있다.
   초기화 파일에서 test timeout 정의 지원은 Robot Framework 2.7부터
   가능하다.
   
`Default Tags`:setting:, `Test Template`:setting:
   초기화 파일에서는 지원되지 않는다.

.. sourcecode:: robotframework

   *** Settings ***
   Documentation    Example suite
   Suite Setup      Do Something    ${MESSAGE}
   Force Tags       example
   Library          SomeLibrary

   *** Variables ***
   ${MESSAGE}       Hello, world!

   *** Keywords ***
   Do Something
       [Arguments]    ${args}
       Some Keyword    ${arg}
       Another Keyword

__ `Test case related settings in the Setting table`_

Test suite name and documentation
---------------------------------

..
   The test suite name is constructed from the file or directory name. The name
   is created so that the extension is ignored, possible underscores are
   replaced with spaces, and names fully in lower case are title cased. For
   example, :file:`some_tests.html` becomes :name:`Some Tests` and
   :file:`My_test_directory` becomes :name:`My test directory`.

test suite 이름은 파일이나 디렉토리 이름으로부터 구성된다. suite
이름은 확장자를 무시하고, 언더스코어(_) 는 공백으로 대체하고, 완전한
소문자 이름은 타이틀 제목(첫문자들을 대문자화)으로 바꾸어서 생성한다.
예를 들어, :file:`some_tests.html` 를 생성하면 이름은 :name:`Some
Tests` 이 되고, :file:`My_test_directory` (모두 소문자 아님)를
생성하면 이름은 :name:`My test directory` 이 된다.

..
   The file or directory name can contain a prefix to control the `execution
   order`_ of the suites. The prefix is separated from the base name by two
   underscores and, when constructing the actual test suite name, both
   the prefix and underscores are removed. For example files
   :file:`01__some_tests.txt` and :file:`02__more_tests.txt` create test
   suites :name:`Some Tests` and :name:`More Tests`, respectively, and
   the former is executed before the latter.

파일 혹은 디렉토리 이름은 suites의 `실행 순서 <execution order_>`__ 를
조절하기 위해서 접두어를 포함할 수 있다. 접두어는 두개의 언더스코어로
기본이름과 구분되며, 실제 test suite 이름을 구성할 때 접두어와
언더스코어 둘다 제거된다. 예를들어 :file:`01__some_tests.txt` 와
:file:`02__more_tests.txt` 는 각각 :name:`Some Tests` 와 :name:`More
Tests` test suites를 생성한다. 그리고 :file:`01__some_tests.txt` 가
:file:`02__more_tests.txt` 보다 먼저 실행된다.

..
   The documentation for a test suite is set using the :setting:`Documentation`
   setting in the Setting table. It can be used in test case files
   or, with higher-level suites, in test suite initialization files. Test
   suite documentation has exactly the same characteristics regarding to where
   it is shown and how it can be created as `test case
   documentation`_.

Test suite의 문서는 :setting:`Documentation` 을 이용하여 Setting
표에서 설정할 수 있다. 이것은 test case 파일안에서 혹은 higher-level
suites 가지는 test suite 초기호 파일안에서 사용할 수 있다. Test suite
문서는 `test case documentation`_ 생성 방법과 보여진 것과 정확히 같은
특징을 가진다.

.. sourcecode:: robotframework

   *** Settings ***
   Documentation    An example test suite documentation with *some* _formatting_.
   ...              See test documentation for more documentation examples.

..
   Both the name and documentation of the top-level test suite can be
   overridden in test execution. This can be done with the command line
   options :option:`--name` and :option:`--doc`, respectively, as
   explained in section `Setting metadata`_.

Top-level test suite의 이름과 문서는 테스트 실행 중 재정의 될 수 있다.
각각은 `Setting metadata`_ 섹션에서 설명된것 처럼, 명령행 옵션
:option:`--name` 와 :option:`--doc` 으로 할 수 있다.

Free test suite metadata
------------------------

..
   Test suites can also have other metadata than the documentation. This metadata
   is defined in the Setting table using the :setting:`Metadata` setting. Metadata
   set in this manner is shown in test reports and logs.

Test suites는 문서보다 다른 메타 데이터를 가질 수 있다. 이 메타
데이터는 :setting:`Metadata` 설정을 이용하여 Setting 표에서 정의할 수
있다. 이런 방법으로 메타 데이터를 설정하는 것은 테스트 리포트와
로그에서 확인할 수 있다.

..
   The name and value for the metadata are located in the columns following
   :setting:`Metadata`. The value is handled similarly as documentation, which means
   that it can be split `into several cells`__ (joined together with spaces)
   or `into several rows`__ (joined together with newlines),
   simple `HTML formatting`_ works and even variables_ can be used.

메타 데이터의 이름과 값은 :setting:`Metadata` 열에 위치한다. 값은
문서와 비슷하게 다루어진다. 즉 `여러 셀에 나뉨`__ (공백이 합쳐질 수
있음) 혹은 `여러 열로 나뉨`__ (개행문자와 합쳐질 수 있음), 간단한
`HTML formatting`_ 으로 작동한다. 심지어 variables_ 가 사용될 수도
있다.


__ `Dividing test data to several rows`_
__ `Newlines in test data`_

.. sourcecode:: robotframework

   *** Settings ***
   Metadata    Version        2.0
   Metadata    More Info      For more information about *Robot Framework* see http://robotframework.org
   Metadata    Executed At    ${HOST}

..
   For top-level test suites, it is possible to set metadata also with the
   :option:`--metadata` command line option. This is discussed in more
   detail in section `Setting metadata`_.

Top-level test suite에서, 메타 데이터 설정은 명령행
:option:`--metadata` 옵션으로 설정할 수 있다. 이것은 `Setting
metadata`_ 절에서더 자세히 설명한다.

Suite setup and teardown
------------------------

..
   Not only `test cases`__ but also test suites can have a setup and
   a teardown. A suite setup is executed before running any of the suite's
   test cases or child test suites, and a test teardown is executed after
   them. All test suites can have a setup and a teardown; with suites created
   from a directory they must be specified in a `test suite
   initialization file`_.

`Test cases`__ 뿐만 아니라 test suites는 setup과 teardown을 가질 수
있다. Suite setup은 suite의 test cases나 자식 test suite 이전에
실행된다. 그리고 teardown은 이후에 실행된다. 모든 test suites는
setup과 teardown을 가질 수 있다. 디렉토리로부터 생성된 suites는 반드시
`test suite 초기화 파일 <test suite initialization file_>`__ 에 명기해야
한다.

__ `Test setup and teardown`_

..
   Similarly as with test cases, a suite setup and teardown are keywords
   that may take arguments. They are defined in the Setting table with
   :setting:`Suite Setup` and :setting:`Suite Teardown` settings,
   respectively. Keyword names and possible arguments are located in
   the columns after the setting name.

Test cases와 비슷하게, suite setup과 teardown은 전달인자를 가질 수
있는 키워드이다. 그것들은 :setting:`Suite Setup` 와 :setting:`Suite
Teardown` 설정을 이용하여 각각 Setting 표에서 정의될 수 있다. 키워드
이름과 사용 가능한 전달인자는 설정 이름 뒤의 열에 위치한다.

..
   If a suite setup fails, all test cases in it and its child test suites
   are immediately assigned a fail status and they are not actually
   executed. This makes suite setups ideal for checking preconditions
   that must be met before running test cases is possible.

만일 suite setup이 실패한다면, 모든 test cases와 이것의 자식 test
suites는 즉시 실패 상태로 할당하고, 더이상 실행하지 않는다. 이것은
test cases가 구동 가능한 상태인지 작동 전에 사전 조건을 확인하여
suite setups를 이상적으로 만든다.

..
   A suite teardown is normally used for cleaning up after all the test
   cases have been executed. It is executed even if the setup of the same
   suite fails. If the suite teardown fails, all test cases in the
   suite are marked failed, regardless of their original execution status.
   Note that all the keywords in suite teardowns are executed even if one
   of them fails.

Suite teardown은 일반적으로 모든 test cases가 실행된 이후에
청소(cleaning up) 해주는데에 사용된다. 비록 같은 suite의 setup이
실패했을지라도 이것은 실행된다. 만일 suite teardown이 실패한다면, 원본
실행의 결과가 어떻든지간에 suite 내의 모든 test cases는 실패했다고
표기된다. Suite teardown의 모든 키워드들은 그중 하나가 실패했을지라도
실행된다는 것을 기억하라.

..
   The name of the keyword to be executed as a setup or a teardown can be
   a variable. This facilitates having different setups or teardowns
   in different environments by giving the keyword name as a variable
   from the command line.

Setup 혹은 teardown으로 실행될 키워드의 이름은 변수가 될 수 있다.
이것은 명령행에서 변수에 키워드 이름을 사용하여, 다른 환경에서는 다른
setups과 teardowns을 가지는 것을 용이하게 한다.
