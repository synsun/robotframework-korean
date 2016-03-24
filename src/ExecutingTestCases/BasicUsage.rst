Basic usage
===========

..
   Robot Framework test cases are executed from the command line, and the
   end result is, by default, an `output file`_ in XML format and an HTML
   report_ and log_. After the execution, output files can be combined and
   otherwise `post-processed`__ with the ``rebot`` tool.

Robot Framework test cases는 명령행으로 실행된다. 최종 결과는
기본적으로 XML 형식의 `output file`_ 및 HTML 형식의 report_ 와 log_ 로
출력된다. 시험 수행 후, ``rebot`` 툴로 `후처리`__ 를 통하여 출력
파일들(output files)은 결합할 수 있다.

__ `Post-processing outputs`_

.. contents::
   :depth: 2
   :local:

.. _executing test cases:

Starting test execution
-----------------------

Synopsis
~~~~~~~~

::

    robot [options] data_sources
    python|jython|ipy -m robot.run [options] data_sources
    python|jython|ipy path/to/robot/run.py [options] data_sources
    java -jar robotframework.jar [options] data_sources

..
   Test execution is normally started using ``robot`` `runner script`_. This is
   script is new in Robot Framework 3.0 and executes the tests using the
   interpreter that was used for installing Robot Framework. There can also be
   ``pybot``, ``jybot`` or ``ipybot`` scripts, which are otherwise identical, but
   the first one executes tests using Python_, the second using Jython_, and the
   last one using IronPython_. Alternatively it is possible to use
   ``robot.run`` `entry point`_ either as a module or a script using
   any interpreter, or use the `standalone JAR distribution`_.

테스트 실행은 일반적으로 ``robot`` `runner script`_ 를 사용하여
시작된다. 이 스크립트는 Robot Framework 3.0에 새로 들어갔으며, Robot
Framework를 설치하는 데 사용했던 인터프리터(interpreter)를 사용하여
테스트를 수행한다. ``pybot``, ``jybot`` 또는 ``ipybot`` 스크립트도
동일하다. 하지만 처음으로 Python_ 을 사용하여 테스트를 수행하고,
두번째로 Jython_ 을 사용하고 마지막으로 IronPython_ 를 사용한다.
대안적으로 모듈 또는 임의의 인터프리터를 사용하는 스크립트를
``robot.run`` `진입점 <entry point_>`__ 으로 사용하거나 `standalone
JAR distribution`_ 을 사용할 수 있다.

..
   Regardless of execution approach, the path (or paths) to the test data to be
   executed is given as an argument after the command. Additionally, different
   command line options can be used to alter the test execution or generated
   outputs in some way.


수행방법과 상관없이, 수행될 테스트 데이터에 대한 경로(혹은 경로들)는
명령 후에 인자로 주어진다. 추가적으로 다른 명령 옵션은 테스트 실행을
변경하거나 원하는 방식으로 결과를 생성하는데 사용될 수 있다.


Specifying test data to be executed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Robot Framework test cases are created in files__ and directories__,
   and they are executed by giving the path to the file or directory in
   question to the selected runner script. The path can be absolute or,
   more commonly, relative to the directory where tests are executed
   from. The given file or directory creates the top-level test suite,
   which gets its name, unless overridden with the :option:`--name` option__,
   from the `file or directory name`__. Different execution possibilities
   are illustrated in the examples below. Note that in these examples, as
   well as in other examples in this section, only the ``robot`` script
   is used, but other execution approaches could be used similarly.

Robot Framework test cases는 파일__ 과 디렉토리__ 안에 생성되며,
선택된 실행 스크립트에 해당 파일이나 디렉토리에 대한 경로를
제공함으로써 실행된다. 이 경로는 테스트 케이스가 수행되는 디렉토리와
연관된, 절대적이거나 상대적인 경로이어야한다. 주어진 파일과 디렉토리는
최상위 test suite를 생성하고, :option:`--name` 옵션__ 으로 재정의하지
않는 한, `파일 또는 디렉토리 이름`__ 으로 부터 이름을 가져온다. 다른
수행 방법들은 아래 예에서 설명된다. 이런 예제뿐만 아니라 이 절에
나와있는 다른 예에서 단지 ``robot`` 스크립트가 이용되지만, 다른 수행
방법이 유사하게 사용될 수 있다는 것에 유의하라.


__ `Test case files`_
__ `Test suite directories`_
__ `Setting the name`_
__ `Test suite name and documentation`_

::

   robot test_cases.html
   robot path/to/my_tests/
   robot c:\robot\tests.txt

..
   It is also possible to give paths to several test case files or
   directories at once, separated with spaces. In this case, Robot
   Framework creates the top-level test suite automatically, and
   the specified files and directories become its child test suites. The name
   of the created test suite is got from child suite names by
   catenating them together with an ampersand (&) and spaces. For example,
   the name of the top-level suite in the first example below is
   :name:`My Tests & Your Tests`. These automatically created names are
   often quite long and complicated. In most cases, it is thus better to
   use the :option:`--name` option for overriding it, as in the second

공백으로 구분함으로써 한번에 여러 개의 테스트 케이스 파일이나
디렉토리를 주는 것 또한 가능하다. 이러한 경우에, Robot Framework는
최상위 test suite를 자동적으로 생성하고, 명시된 파일이나 디렉토리들은
하위 test suites가 된다. 생성된 test suite의 이름은 앰퍼센드(&)와
공백으로 하위 test suite를 연결하여 만든다. 예를 들어, 아래의 첫번째
예제의 최상위 test suite의 이름은 :name:`My Tests & Your Tests` 이다.
이처럼 자동적으로 생성된 이름들은 꽤 길거나 복잡하다. 대부분의 경우,
두번째 예제처럼 옵션을 사용하여 :option:`--name` option 형식을
사용하는 편이 낫다.

예제::

   robot my_tests.html your_tests.html
   robot --name Example path/to/tests/pattern_*.html


Using command line options
--------------------------

..
   Robot Framework provides a number of command line options that can be
   used to control how test cases are executed and what outputs are
   generated. This section explains the option syntax, and what
   options actually exist. How they can be used is discussed elsewhere
   in this chapter.

Robot Framework는 많은 명령행 옵션을 제공한다. 이 옵션들은 어떻게
테스트 케이스들을 수행하고 결과들을 어떻게 생성할 것인지 결정한다. 이
절은 옵션 문법에 대해 설명하고 어떤 옵션이 있는지 설명한다. 이 장의
다른 곳에서 이 옵션들이 어떻게 사용되는지 논의한다.


Using options
~~~~~~~~~~~~~

..
   When options are used, they must always be given between the runner
   script and the data sources.

옵션들이 사용될 때, 항상 실행 스크립트와 데이터 소스사이에 입력되어야
한다.

예제::

   robot -L debug my_tests.txt
   robot --include smoke --variable HOST:10.0.0.42 path/to/tests/

Short and long options
~~~~~~~~~~~~~~~~~~~~~~

..
   Options always have a long name, such as :option:`--name`, and the
   most frequently needed options also have a short name, such as
   :option:`-N`. In addition to that, long options can be shortened as
   long as they are unique. For example, `--logle DEBUG` works,
   while `--lo log.html` does not, because the former matches only
   :option:`--loglevel`, but the latter matches several options. Short
   and shortened options are practical when executing test cases
   manually, but long options are recommended in `start-up scripts`_,
   because they are easier to understand.

옵션은 항상 :option:`--name` 처럼 긴 이름을 가지고 있고 사용 빈도가
높은 옵션들은 :option:`-N` 처럼 짧은 이름도 가지고 있다. 그리고 긴
옵션은 고유하기만 하면 짧아 질수 있다. 예를 들어, `--logle DEBUG` 는
동작하는 반면, `--lo log.html` 는 동작하지 않는다. 왜냐하면, 전자는
오직 :option:`--loglevel` 에만 일치하지만, 후자는 여러 옵션에 일치되기
때문에 안된다. 짧거나 짧아진 옵션은 수동으로 테스트 케이스를 실행할때
유용하다. 하지만 긴 옵션은 이해하는데 쉽기 때문에 `시작 스크립트
<start-up scripts_>`__ 에 권장된다.

..
   The long option format is case-insensitive, which facilitates writing option
   names in an easy-to-read format. For example, :option:`--SuiteStatLevel`
   is equivalent to, but easier to read than :option:`--suitestatlevel`.

긴 옵션 형식은 대소문자를 구분하지 않아서 옵션 읽기 쉬운 형식으로
이름을 쓸 수 있다. 예를 들어, :option:`--SuiteStatLevel` 는
:option:`--suitestatlevel` 과 같지만 가독성이 높다.

Setting option values
~~~~~~~~~~~~~~~~~~~~~

..
   Most of the options require a value, which is given after the option
   name. Both short and long options accept the value separated
   from the option name with a space, as in `--include tag`
   or `-i tag`. With long options, the separator can also be the
   equals sign, for example `--include=tag`, and with short options the
   separator can be omitted, as in `-itag`.

대부분의 옵션은 값이 필요하고, 옵션 뒤에 붙는다. 옵션이 길던 짧던,
`--include tag` 나 `-i tag` 처럼 공백으로 옵션이름과 값을 구분하여
받는다. 이 구분자는 `--include=tag` 처럼 등호 일수도 있고 짧은 옵션의
경우 `-itag` 처럼 구분자가 생략될 수도 있다.

..
   Some options can be specified several times. For example,
   `--variable VAR1:value --variable VAR2:another` sets two
   variables. If the options that take only one value are used several
   times, the value given last is effective.

어떤 옵션은 여러번 사용할 수 있다. 예를 들어 `--variable
VAR1:value --variable VAR2:another` 은 두 개의 값을 설정한다. 만약
하나의 값만 받는 옵션이 여러번 쓰이면, 마지막 값이 유효하다.

Disabling options accepting no values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Options accepting no values can be disabled by using the same option again
   with `no` prefix added or dropped. The last option has precedence regardless
   of how many times options are used. For example, `--dryrun --dryrun --nodryrun
   --nostatusrc --statusrc` would not activate the dry-run mode and would return
   normal status rc.

no 값을 받는 옵션은 `no` 접두사를 더하거나 생략한 동일한 옵션을 다시
사용함으로써 비활성화 할 수 있다. 옵션을 사용한 횟수와 상관없이 마지막
옵션이 우선순위를 갖는다. 예를 들어, `--dryrun --dryrun --nodryrun
--nostatusrc --statusrc` 의 경우 dry-run 모드를 활성화 하지 않고,
normal status rc를 반환한다.

..
   .. note:: Support for adding or dropping `no` prefix is a new feature in
	     Robot Framework 2.9. In earlier versions options accepting no
	     values could be disabled by using the exact same option again.

.. note:: `no` 접두사를 넣거나 빼는 기능은 Robot Framework 2.9 의
            새로운 기능이다. 이전 버전에서는 no 값을 받는 옵션은 같은
            옵션을 다시 사용함으로써 비활성화 할 수 있다.

Simple patterns
~~~~~~~~~~~~~~~

..
   Many command line options take arguments as *simple patterns*. These
   `glob-like patterns`__ are matched according to the following rules:

많은 명령행 옵션들은 *간단한 패턴(simple patterns)* 의 인자를 갖는다.
이러한 `유사 글로브 패턴(glob-like patterns)`__ 은 아래 규칙에 따라
일치된다.

..
   - `*` is a wildcard matching any string, even an empty string.
   - `?` is a wildcard matching any single character.
   - Unless noted otherwise, pattern matching is case, space, and underscore insensitive.

- `*` 은 임의의 문자열(심지어 공백포함)에도 일치하는 와일드카드이다.
- `?` 은 임의의 단일 문자에 매칭되는 와일드카드이다.
- 특별히 언급하지 않아도, 패턴 매칭은 대소문자, 공백, 언더스코어를
  구분하지 않는다.

예제::

   --test Example*     # Matches tests with name starting 'Example', case insensitively.
   --include f??       # Matches tests with a tag that starts with 'f' or 'F' and is three characters long.

__ http://en.wikipedia.org/wiki/Glob_(programming)

Tag patterns
~~~~~~~~~~~~

..
   Most tag related options accept arguments as *tag patterns*. They have all the
   same characteristics as `simple patterns`_, but they also support `AND`,
   `OR` and `NOT` operators explained below. These operators can be
   used for combining two or more individual tags or patterns together.

옵션과 관련된 대부분의 태그는 *태그 패턴(tag patterns)* 을 인자로
받는다. 이 옵션들은 `간단한 패턴 <simple patterns_>`__ 과 같은 특징을
갖는다. 하지만 아래 설명처럼 `AND`, `OR`, `NOT` 연산자를 지원한다.
이러한 연산자는 두개 이상의 태그과 패턴을 조합하여 사용할 수 있다.

..
   `AND` or `&`
      The whole pattern matches if all individual patterns match. `AND` and
      `&` are equivalent.

`AND` or `&`
   개별 패턴이 모두 일치하면 전체 패턴이 일치한다. `AND` 과 `&` 은 같다.
   ::

      --include fooANDbar     # Matches tests containing tags 'foo' and 'bar'.
      --exclude xx&yy&zz      # Matches tests containing tags 'xx', 'yy', and 'zz'.

..
   `OR`
      The whole pattern matches if any individual pattern matches.

`OR`
   개별 패턴 중 하나라도 일치하면 전체 패턴이 일치한다.

   ::

      --include fooORbar      # Matches tests containing either tag 'foo' or tag 'bar'.
      --exclude xxORyyORzz    # Matches tests containing any of tags 'xx', 'yy', or 'zz'.

..
   `NOT`
      The whole pattern matches if the pattern on the left side matches but
      the one on the right side does not. If used multiple times, none of
      the patterns after the first `NOT` must not match.

`NOT`

   NOT의 왼쪽은 일치 하고, 오른쪽은 일치가 안되면 전체 패턴이
   일치한다. 여러번 사용되면, 첫번째 `NOT` 후에 패턴 중 어떤것도
   매치되지 않아야 한다.

   ::

      --include fooNOTbar     # Matches tests containing tag 'foo' but not tag 'bar'.
      --exclude xxNOTyyNOTzz  # Matches tests containing tag 'xx' but not tag 'yy' or tag 'zz'.

   ..
      Starting from Robot Framework 2.9 the pattern can also start with `NOT`
      in which case the pattern matches if the pattern after `NOT` does not match

   Robot Framework 2.9 부터 `NOT` 으로 시작하는 패턴 매칭을 사용할수
   있다. 이 경우, `NOT` 이후에 일치가 안되면 전체 패턴이 일치한다.

   ::

      --include NOTfoo        # Matches tests not containing tag 'foo'
      --include NOTfooANDbar  # Matches tests not containing tags 'foo' and 'bar'

..
   The above operators can also be used together. The operator precedence,
   from highest to lowest, is `AND`, `OR` and `NOT`

위의 연산자들은 함께 쓰일수 있다. 연산자 우선순위는 `AND`, `OR` ,
`NOT` 순서로 낮아진다.

::

    --include xANDyORz      # Matches tests containing either tags 'x' and 'y', or tag 'z'.
    --include xORyNOTz      # Matches tests containing either tag 'x' or 'y', but not tag 'z'.
    --include xNOTyANDz     # Matches tests containing tag 'x', but not tags 'y' and 'z'.

..
   Although tag matching itself is case-insensitive, all operators are
   case-sensitive and must be written with upper case letters. If tags themselves
   happen to contain upper case `AND`, `OR` or `NOT`, they need to specified
   using lower case letters to avoid accidental operator usage

태그 매칭이 대소문자를 구분하지 않지만, 모든 연산자는 대소문자를
구분하며 대문자로 쓰여져야한다. 만약 태그 자체에서 대문자 `AND`, `OR`
, `NOT` 를 포함하는 경우가 생기면, 실수로 연산자로 사용되는 것을 막기
위해서 태그는 소문자를 써야한다.

::

    --include port          # Matches tests containing tag 'port', case-insensitively
    --include PORT          # Matches tests containing tag 'P' or 'T', case-insensitively
    --exclude handoverORportNOTnotification

..
   .. note:: `OR` operator is new in Robot Framework 2.8.4.

.. note:: `OR` 연사자는 Robot Framework 2.8.4의 신규기능이다.

``ROBOT_OPTIONS`` and ``REBOT_OPTIONS`` environment variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Environment variables ``ROBOT_OPTIONS`` and ``REBOT_OPTIONS`` can be
   used to specify default options for `test execution`_ and `result
   post-processing`__, respectively. The options and their values must be
   defined as a space separated list and they are placed in front of any
   explicit options on the command line. The main use case for these
   environment variables is setting global default values for certain options to
   avoid the need to repeat them every time tests are run or ``rebot`` used.

환경변수 ``ROBOT_OPTIONS`` 과 ``REBOT_OPTIONS`` 은 `테스트 실행 <test
execution_>`__ 과 `결과 후처리`__ 에 대한 기본 옵션을 지정하는데
사용될 수 있다. 각각의 옵션과 값은 공백으로 구분된 목록으로 정의되어야
하고 명령행에서는 명시적으로 옵션앞에 배치되어야 한다. 이런 환경변수에
대한 주요 사용 사례는 특정 옵션에 대한 전역 기본값을 설정하는 것이다.
이는 테스트가 수행되거나 ``rebot`` 사용될 때 설정이 반복되는 것을
피한다.

.. sourcecode:: bash

   export ROBOT_OPTIONS="--critical regression --tagdoc 'mytag:Example doc with spaces'"
   robot tests.txt
   export REBOT_OPTIONS="--reportbackground green:yellow:red"
   rebot --name example output.xml

..
   .. note:: Support for ``ROBOT_OPTIONS`` and ``REBOT_OPTIONS`` environment
	     variables was added in Robot Framework 2.8.2.
          Possibility to have spaces in values by surrounding them in quotes
          is new in Robot Framework 2.9.2.

.. note:: ``ROBOT_OPTIONS`` 과 ``REBOT_OPTIONS`` 환경변수를 지원하는
          기능은 Robot Framework 2.8.2.에 추가되었다.
          따옴표 사이의 값안에 공백을 갖는 기능은 Robot Framework
          2.9.2 의 신규기능이다.

__ `Post-processing outputs`_

Test results
------------

Command line output
~~~~~~~~~~~~~~~~~~~

..
   The most visible output from test execution is the output displayed in
   the command line. All executed test suites and test cases, as well as
   their statuses, are shown there in real time. The example below shows the
   output from executing a simple test suite with only two test cases

시험 수행후에 가장 눈에 띄는 출력은 명령행에 표시되는 출력이다. 모든
수행된 test suites와 test cases 뿐만 아니라 상태도 실시간으로
출력된다. 아래 예는 두개의 test cases를 가지는 간단한 test suite의
실행에 대한 출력을 보여준다.

::

   ==============================================================================
   Example test suite
   ==============================================================================
   First test :: Possible test documentation                             | PASS |
   ------------------------------------------------------------------------------
   Second test                                                           | FAIL |
   Error message is displayed here
   ==============================================================================
   Example test suite                                                    | FAIL |
   2 critical tests, 1 passed, 1 failed
   2 tests total, 1 passed, 1 failed
   ==============================================================================
   Output:  /path/to/output.xml
   Report:  /path/to/report.html
   Log:     /path/to/log.html

..
   Starting from Robot Framework 2.7, there is also a notification on the console
   whenever a top-level keyword in a test case ends. A green dot is used if
   a keyword passes and a red F if it fails. These markers are written to the end
   of line and they are overwritten by the test status when the test itself ends.
   Writing the markers is disabled if console output is redirected to a file.

Robot Framework 2.7 부터, test case의 상위 레벨 키워드가 끝날 때마다
콘솔창에 알림이 뜬다. 키워드가 성공하면 녹색 점(.)이 사용되고,
실패하면 빨간색 F가 사용된다. 이 표식은 줄의 끝에 기록되며 시험이
끝날때까지 시험상태에 따라 덮어써진다. 콘솔 출력이 파일로 지정된
경우엔, 이 표식을 쓸수 없다.


Generated output files
~~~~~~~~~~~~~~~~~~~~~~

..
   The command line output is very limited, and separate output files are
   normally needed for investigating the test results. As the example
   above shows, three output files are generated by default. The first
   one is in XML format and contains all the information about test
   execution. The second is a higher-level report and the third is a more
   detailed log file. These files and other possible output files are
   discussed in more detail in the section `Different output files`_.

명령행 결과는 제한적이며, 보통 분리된 결과 파일이 시험 결과를
분석하는데 필요하다. 위에서 본 예처럼, 세가지 결과 파일이 기본적으로
생성된다. 첫번째는 XML 형식이고 시험 수행에 관한 모든 정보를 포함한다.
두번째는 고수준의 레포트이고, 세번째는 좀더 세세한 로그 파일이다.
`다른 출력 파일들 <Different output files_>`__ 섹션에서 이런 파일과
다른 사용가능한 결과 파일들에 대해 좀더 자세하게 설명한다.

Return codes
~~~~~~~~~~~~

..
   Runner scripts communicate the overall test execution status to the
   system running them using return codes. When the execution starts
   successfully and no `critical test`_ fail, the return code is zero.
   All possible return codes are explained in the table below.

실행 스크립트는 반환 코드를 사용하여 실행하는 시스템에 전반적인 테스트
실행 상태를 알린다. 실행이 성공적으로 시작하고, `critical test`_ 가
실패하지 않으면 0을 반환한다. 사용되는 모든 반환 코드는 아래
테이블에서 설명된다.

..
   .. table:: Possible return codes
      :class: tabular

      ========  ==========================================
	 RC                    Explanation
      ========  ==========================================
      0         All critical tests passed.
      1-249     Returned number of critical tests failed.
      250       250 or more critical failures.
      251       Help or version information printed.
      252       Invalid test data or command line options.
      253       Test execution stopped by user.
      255       Unexpected internal error.
      ========  ==========================================

.. table:: Possible return codes
   :class: tabular

   ========  ==========================================
      RC                    Explanation
   ========  ==========================================
   0         모든 critical tests가 PASS함.
   1-249     Critical tests가 FAIL한 갯수를 반환.
   250       250개 이상 critical 실패.
   251       Help나 버전 정보 출력.
   252       유효하지 않은 테스트 데이타나 명령행 옵션.
   253       사용자에 의한 테스트 실행 정지.
   255       기대하지 않은 내부 에러.
   ========  ==========================================
   
..
   Return codes should always be easily available after the execution,
   which makes it easy to automatically determine the overall execution
   status. For example, in bash shell the return code is in special
   variable `$?`, and in Windows it is in `%ERRORLEVEL%`
   variable. If you use some external tool for running tests, consult its
   documentation for how to get the return code.

반환코드는 수행 후에 쉽게 사용할 수 있어야 한다. 이를 통해 자동적으로
전체 실행 상태가 쉽게 결정된다. 예를 들어, bash shell 에서 반환 코드는
특별한 변수 `$?` 이고, 윈도우에서는 `%ERRORLEVEL%` 변수이다. 만약
시험을 수행하는데, 어떤 외부툴을 쓴다면, 반환코드를 어떻게 받는지에
대해서 툴에 대한 문서를 참고하라.


..
   The return code can be set to 0 even if there are critical failures using
   the :option:`--NoStatusRC` command line option. This might be useful, for
   example, in continuous integration servers where post-processing of results
   is needed before the overall status of test execution can be determined.

테스트 결과 중 심각한 실패가 있더라도 명령행 옵션
:option:`--NoStatusRC` 을 사용하면 반환 코드는 0으로 설정 될 수 있다.
예를 들어, 시험 실행의 모든 상태가 결정되기 전에, 결과의 후처리가
이루어지는 지속적인 통합 서버의 경우 유용하다.

..
   .. note:: Same return codes are also used with rebot_.

.. note:: 동일한 반환 코드가 rebot_ 에서도 쓰인다.

Errors and warnings during execution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   During the test execution there can be unexpected problems like
   failing to import a library or a resource file or a keyword being
   deprecated__. Depending on the severity such problems are categorized
   as errors or warnings and they are written into the console (using the
   standard error stream), shown on a separate *Test Execution Errors*
   section in log files, and also written into Robot Framework's own
   `system log`_. Normally these errors and warnings are generated by Robot
   Framework itself, but libraries can also log `errors and warnings`_.
   Example below illustrates how errors and warnings look like in the log file.

테스트를 수행되는 동안, library나 resource 파일 임포트를 실패하거나
키워드가 deprecated__ 인 경우 예상치 못한 문제가 있을 수 있다.
심각성에 따라 문제는 에러 또는 경고로 분류된다. 그리고 그것은 콘솔에
출력(표준 에러 스트림에 따라)되고, 로그파일에서는 별도의 *Test
Execution Errors* 섹션에 출력되고, Robot Framework의 `system log`_
에도 출력된다. 보통 이런 에러와 경고는 Robot Framework 자체에서
생성된다. 하지만 라이브러리들도 `에러와 경고 <errors and warnings_>`__
를 로그할수 있다. 예를 들어 어떻게 에러와 경고이 로그파일에서
보여지는지, 아래에서 설명한다.

.. raw:: html

   <table class="messages">
     <tr>
       <td class="time">20090322&nbsp;19:58:42.528</td>
       <td class="error level">ERROR</td>
       <td class="msg">Error in file '/home/robot/tests.html' in table 'Setting' in element on row 2: Resource file 'resource.html' does not exist</td>
     </tr>
     <tr>
       <td class="time">20090322&nbsp;19:58:43.931</td>
       <td class="warn level">WARN</td>
       <td class="msg">Keyword 'SomeLibrary.Example Keyword' is deprecated. Use keyword `Other Keyword` instead.</td>
     </tr>
   </table>

__ `Deprecating keywords`_


Escaping complicated characters
-------------------------------

..
   Because spaces are used for separating options from each other, it is
   problematic to use them in option values.  Some options, such as
   :option:`--name`, automatically convert underscores to spaces, but
   with others spaces must be escaped. Additionally, many special
   characters are complicated to use on the command line.
   Because escaping complicated characters with a backslash or quoting
   the values does not always work too well, Robot Framework has its own
   generic escaping mechanism. Another possibility is using `argument
   files`_ where options can be specified in the plain text format. Both of
   these mechanisms work when executing tests and when
   post-processing outputs, and also some of the external supporting
   tools have the same or similar capabilities.

각 옵션을 구분할 때 공백을 쓰기 때문에, 옵션 값으로 공백을 쓰는 것은
문제가 있다. :option:`--name` 같은 옵션들은, 자동적으로 언더스코어를
공백으로 변환하지만 다른 옵션들은 공백을 이스케이프 해야한다.
추가적으로, 많은 특수 문자를 명령행에서 사용하는 것이 복잡하다.
백슬래쉬나, 따옴표 처리된 값 같은 이스케이프가 복잡한 문자들은 항상 잘
동작하는 것은 아니기 때문에, Robot Framework은 고유의 이스케이프
메카니즘을 가진다. 다른 방법은 `전달인자 파일 <argument files_>`__ 을
쓰는 것이다. 이 파일에 옵션은 일반 텍스트 형식으로 기술할 수 있따.
이러한 메카니즘 둘다 테스트를 수행할때나, 결과 후처리를 할때 동작한다.
그리고 외부 지원툴을 쓰면 같거나 비슷한 기능을 가진다.

..
   In Robot Framework's command line escaping mechanism,
   problematic characters are escaped with freely selected text.
   The command line option to use is :option:`--escape (-E)`,
   which takes an argument in the format `what:with`,
   where `what` is the name of the character to escape and
   `with` is the string to escape it with. Characters that can
   be escaped are listed in the table below:


Robot Framework의 명령행 이스케이핑 메카니즘에서 문제되는 문자는
자유롭게 선택된 텍스트로 이스케이프 된다. 명령행 옵션은
:option:`--escape (-E)` 을 쓴다. 이것은 `what:with` 형식으로 인자를
받는다. `what` 은 이스케이프하기 위한 문자의 이름이고, `with` 는 앞의
문자를 이스케이프하기 위하여 사용하는 문자열(string)이다. 아래 표에
이스케이프 될 수 있는 문자들의 목록을 나열한다:


.. table:: Available escapes
   :class: tabular

   =========  =============  =========  =============
   Character   Name to use   Character   Name to use
   =========  =============  =========  =============
   &          amp            (          paren1
   '          apos           )          paren2
   @          at             %          percent
   \\         bslash         \|         pipe
   :          colon          ?          quest
   ,          comma          "          quot
   {          curly1         ;          semic
   }          curly2         /          slash
   $          dollar         \          space
   !          exclam         [          square1
   >          gt             ]          square2
   #          hash           \*         star
   <          lt             \          \
   =========  =============  =========  =============

..
   The following examples make the syntax more clear. In the
   first example, the metadata `X` gets the value `Value with
   spaces`, and in the second example variable `${VAR}` is assigned to
   `"Hello, world!"`

아래 예를 보면 문법을 좀 더 분명히 알 수있다. 첫번째 예로, metatada
`X` 는 `Value with spaces` 라는 값을 가져온다. 두번째 예는 `${VAR}` 에
`"Hello, world!"` 를 할당한다::

    --escape space:_ --metadata X:Value_with_spaces
    -E space:SP -E quot:QU -E comma:CO -E exclam:EX -v VAR:QUHelloCOSPworldEXQU

..
   Note that all the given command line arguments, including paths to test
   data, are escaped. Escape character sequences thus need to be
   selected carefully.

Test data에 대한 경로를 포함한 모든 주어진 명령행 인자들은, 이스케이프
된다는 것에 유의하라. 그래서 이스케이프 문자 시퀀스는 주의깊게 선택될
필요가 있다.

Argument files
--------------

..
   Argument files allow placing all or some command line options and arguments
   into an external file where they will be read. This avoids the problems with
   characters that are problematic on the command line. If lot of options or
   arguments are needed, argument files also prevent the command that is used on
   the command line growing too long.

   Argument files are taken into use with :option:`--argumentfile (-A)` option
   along with possible other command line options.


인자파일은 모든 혹은 일부 명령행 옵션과 인자들이 읽어질 수 있는
외부파일에 들어있는 것을 허용한다. 이 것은 명령행에서 문제가 될 수있는
문자들이 가지고 오는 문제들을 방지한다. 또한 만약 많은 옵션과 인자들이
필요하다면, 인자파일은 명령행에서 명령이 매우 길어지는 것을 막을 수
있다.

인자 파일은 :option:`--argumentfile (-A)` 옵션을 써서 다른 명령행
옵션을 같이 쓸 수 잇다.

Argument file syntax
~~~~~~~~~~~~~~~~~~~~

..
   Argument files can contain both command line options and paths to the test data,
   one option or data source per line. Both short and long options are supported,
   but the latter are recommended because they are easier to understand.
   Argument files can contain any characters without escaping, but spaces in
   the beginning and end of lines are ignored. Additionally, empty lines and
   lines starting with a hash mark (#) are ignored

인자 파일은 명령행 옵션과 테스트 데이타에 대한 경로, 라인당 하나의
옵션 또는 데이터 소스를 포함할 수 있다. 짧은 옵션, 긴 옵션들은 모두
지원하지만, 후자의 경우 이해하기 쉽기 때문에 추천한다. 인자 파일은
이스케이핑 없이 어떤 문자들도 포함할수 있지만 줄의 시작과 끝의 공백은
무시다. 추가적으로 빈 라인과 해시 마크 (#)로 시작하는 줄도
무시된다::

   --doc This is an example (where "special characters" are ok!)
   --metadata X:Value with spaces
   --variable VAR:Hello, world!
   # This is a comment
   path/to/my/tests

..
   In the above example the separator between options and their values is a single
   space. In Robot Framework 2.7.6 and newer it is possible to use either an equal
   sign (=) or any number of spaces. As an example, the following three lines are
   identical


위의 예제에서, 옵션과 값 사이의 구분자는 한 개의 공백이다. Robot
Framework 2.7.6 이후부터 등호(=)와 임의 갯수의 공백을 사용 할 수 있다.
예로서 아래 세개의 줄은 동일하다.::


    --name An Example
    --name=An Example
    --name       An Example

..
   If argument files contain non-ASCII characters, they must be saved using
   UTF-8 encoding.

만약 인자파일들이 non-ASCII 문자들은 포함한다면, UTF-8 인코딩을
사용하여 저장하여야한다.

Using argument files
~~~~~~~~~~~~~~~~~~~~

..
   Argument files can be used either alone so that they contain all the options
   and paths to the test data, or along with other options and paths. When
   an argument file is used with other arguments, its contents are placed into
   the original list of arguments to the same place where the argument file
   option was. This means that options in argument files can override options
   before it, and its options can be overridden by options after it. It is possible
   to use :option:`--argumentfile` option multiple times or even recursively

인자 파일은 모든 옵션과 테스트 데이터에 대한 모든 경로를 포함하여 혼자
쓰이거나, 다른 옵션과 경로와 함께 사용할 수 있다. 인자 파일이 다른
인자들과 함께 쓰이면, 그 내용은 인자 파일 옵션이 있었던 곳과 같은
장소에 있는 원래 인자 목록에 재치된다. 이것은 인자파일안의 옵션이 이
옵션 전의 옵션을 무시할 수 있다는 것을 의미한다. 그리고 이 옵션은
이후의 옵션에 의해서 무시될 수 있다. 이것은 :option:`--argumentfile`
을 쓰면 옵션을 여러번 쓰거나 재귀적으로 쓸 수 있다::

   robot --argumentfile all_arguments.txt
   robot --name Example --argumentfile other_options_and_paths.txt
   robot --argumentfile default_options.txt --name Example my_tests.html
   robot -A first.txt -A second.txt -A third.txt tests.txt

Reading argument files from standard input
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Special argument file name `STDIN` can be used to read arguments from the
   standard input stream instead of a file. This can be useful when generating
   arguments with a script.

특별한 인자파일 이름인 `STDIN` 은 파일 대신에 표준 입력 스트림으로부터
인수를 읽는 데 사용될 수 있다. 이것은 스크립트로 인자를 생성할 때
유용하다::

   generate_arguments.sh | robot --argumentfile STDIN
   generate_arguments.sh | robot --name Example --argumentfile STDIN tests.txt

Getting help and version information
------------------------------------

..
   Both when executing test cases and when post-processing outputs, it is possible
   to get command line help with the option :option:`--help (-h)`.
   These help texts have a short general overview and
   briefly explain the available command line options.

Test cases를 수행하거나 결과를 후처리 할 때, :option:`--help (-h)`
옵션으로 명령행 도움말을 가져올 수 있다. 이 도움말은 일반적인 짧은
개요와 사용가능한 명령행 옵션에 대한 간단한 설명을 가진다.

..
   All runner scripts also support getting the version information with
   the option :option:`--version`. This information also contains Python
   or Jython version and the platform type

모든 실행 스크립트들은 :option:`--version` 옵션으로 버전 정보를
가져오는 것을 지원한다. 이 정보는 Python 또는 Jython 버전과 플랫폼
타입을 포함한다::

   $ robot --version
   Robot Framework 3.0 (Jython 2.7.0 on java1.7.0_45)

   C:\>rebot --version
   Rebot 3.0 (Python 2.7.10 on win32)

.. _start-up script:
.. _start-up scripts:


Creating start-up scripts
-------------------------

..
   Test cases are often executed automatically by a continuous
   integration system or some other mechanism. In such cases, there is a
   need to have a script for starting the test execution, and possibly
   also for post-processing outputs somehow. Similar scripts are also
   useful when running tests manually, especially if a large number of
   command line options are needed or setting up the test environment is
   complicated.

Test cases는 지속 가능한 통합 시스템이나 다른 장비에 의해 자동적으로
수행될 수 있다. 이러한 경우에, 테스트 수행 시작과 결과 후처리를 위한
스크립트가 필요하다. 이러한 종류의 스크립트는 수동으로 테스트를 수행
할 때 유용하다. 특히 많은 명령행 옵션이 필요하거나, 시험 환경 설정이
복잡하다면 유용하다.

..
   In UNIX-like environments, shell scripts provide a simple but powerful
   mechanism for creating custom start-up scripts. Windows batch files
   can also be used, but they are more limited and often also more
   complicated. A platform-independent alternative is using Python or
   some other high-level programming language. Regardless of the
   language, it is recommended that long option names are used, because
   they are easier to understand than the short names.

UNIX 계열의 환경에서 쉘 스크립트는 사용자 정의 시작 스크립트를 만들기
위하여 간단하지만 강력한 메커니즘을 제공한다. 윈도우 배치 파일도
사용할 수 있지만, 보다 제한적이고 종종 더 복잡하다. 플랫폼에 독립적인
대안은 파이썬 또는 다른 높은 수준의 프로그래밍 언어를 사용하는 것이다.
언어에 상관없이, 긴 옵션 이름은 짧은 이름보다 이해하기 쉽기 때문에
사용이 권장된다.

..
   In the first examples, the same web tests are executed with different
   browsers and the results combined afterwards. This is easy with shell
   scripts, as practically you just list the needed commands one after
   another:

첫번째 예로, 동일한 웹 테스트가 다른 브라우저에서 수행되고, 결과는
이후에 결합된다. 이것은 쉘 스크립트를 사용하는 편이 쉽다. 실제로
필요한 명령을 차례로 나열만 하면된다:

.. sourcecode:: bash

   #!/bin/bash
   robot --variable BROWSER:Firefox --name Firefox --log none --report none --output out/fx.xml login
   robot --variable BROWSER:IE --name IE --log none --report none --output out/ie.xml login
   rebot --name Login --outputdir out --output login.xml out/fx.xml out/ie.xml

..
   Implementing the above example with Windows batch files is not very
   complicated, either. The most important thing to remember is that
   because ``robot`` and ``rebot`` are implemented as batch
   files, ``call`` must be used when running them from another batch
   file. Otherwise execution would end when the first batch file is
   finished.

윈도우즈 배치 파일을 사용하여 위의 예를 구현하는 것 또한 매우 복잡하지
않다. 기억해야 할 가장 중요한 것은 ``robot`` 와 ``rebot`` 이 배치
파일로 구현되어 있기 때문에 다른 배치 파일에서 실행하면, ``call`` 이
사용되어야한다는 것이다. 그렇지 않으면 실행은 첫번째 배치 파일이 종료
될 때 종료된다.

.. sourcecode:: bat

   @echo off
   call robot --variable BROWSER:Firefox --name Firefox --log none --report none --output out\fx.xml login
   call robot --variable BROWSER:IE --name IE --log none --report none --output out\ie.xml login
   call rebot --name Login --outputdir out --output login.xml out\fx.xml out\ie.xml

..
   In the next examples, jar files under the :file:`lib` directory are
   put into ``CLASSPATH`` before starting the test execution. In these
   examples, start-up scripts require that paths to the executed test
   data are given as arguments. It is also possible to use command line
   options freely, even though some options have already been set in the
   script. All this is relatively straight-forward using bash:

다음 예제의 경우 :file:`lib` 디렉토리 아래의 jar 파일은 테스트 실행을
시작하기 전에 ``CLASSPATH`` 에 놓여 있어야 한다. 이 예제에서, 시작
스크립트는 실행 된 테스트 데이터에 대한 경로가 인수로 주어져야 한다.
이는 또 몇 가지 옵션이 이미 스크립트에 설정된 경우에도, 자유롭게
명령행 옵션을 사용할 수도 있다. 이런 모든 것을 bash를 쓰는게
상대적으로 수월하다:

.. sourcecode:: bash

   #!/bin/bash

   cp=.
   for jar in lib/*.jar; do
       cp=$cp:$jar
   done
   export CLASSPATH=$cp

   robot --ouputdir /tmp/logs --suitestatlevel 2 $*

..
   Implementing this using Windows batch files is slightly more complicated. The
   difficult part is setting the variable containing the needed JARs inside a For
   loop, because, for some reason, that is not possible without a helper
   function.

윈도우즈 배치 파일을 사용하여 구현하는 것은 약간 더 복잡하다. 어려운
부분은 for 루프 내에서 필요한 JAR 파일을 포함하는 변수를 설정하는
것이다. 몇가지 이유 때문에, 헬퍼 함수 없이는 할 수 없다.

.. sourcecode:: bat

   @echo off

   set CP=.
   for %%jar in (lib\*.jar) do (
       call :set_cp %%jar
   )
   set CLASSPATH=%CP%

   robot --ouputdir c:\temp\logs --suitestatlevel 2 %*

   goto :eof

   :: Helper for setting variables inside a for loop
   :set_cp
       set CP=%CP%;%1
   goto :eof

Modifying Java startup parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   Sometimes when using Jython there is need to alter the Java startup parameters.
   The most common use case is increasing the JVM maximum memory size as the
   default value may not be enough for creating reports and logs when
   outputs are very big. There are several ways to configure JVM options:

Jython 사용시 때때로 자바 시작 변수를 변경할 필요가 있다. 가장
일반적인 사용 사례는 JVM 최대 메모리 크기를 증가시키는 것이다. 출력이
매우 큰 경우 기본값은 리포트 및 로그를 생성하기 위해 충분하지 않을 수
있다. JVM 옵션을 구성하는 방법은 몇가지가 있다:

..
   1. Modify Jython start-up script (``jython`` shell script or
      ``jython.bat`` batch file) directly. This is a permanent configuration.

1. Jython 시작 스크립트를 직접 수정한다. (``jython`` 쉘 스크립트 또는
   ``jython.bat`` 배치 파일). 이것은 고정적인 설정이다.

..
   2. Set ``JYTHON_OPTS`` environment variable. This can be done permanently
      in operating system level or per execution in a custom start-up script.

2. 환경 변수 ``JYTHON_OPTS`` 를 설정한다. 이것은 운영 체제 레벨이나,
   사용자 정의 시작 스크립트에서 시험마다 고정적으로 수행 될 수 있다.

..
   3. Pass the needed Java parameters wit :option:`-J` option to Jython start-up
      script that will pass them forward to Java. This is especially easy when
      using `direct entry points`_

3. :option:`-J` 옵션을 통해 Jython 시작 스크립트는 필요한 자바
   변수들을 자바로 전달합니다. `직접 진입점 <direct entry points_>`__
   을 쓰면 더욱 쉬워진다::

      jython -J-Xmx1024m -m robot.run some_tests.txt


Debugging problems
------------------

..
   A test case can fail because the system under test does not work
   correctly, in which case the test has found a bug, or because the test
   itself is buggy. The error message explaining the failure is shown on
   the `command line output`_ and in the `report file`_, and sometimes
   the error message alone is enough to pinpoint the problem. More often
   that not, however, `log files`_ are needed because they have also
   other log messages and they show which keyword actually failed.

Test case는 테스트 중인 시스템이 제대로 동작하지 않아서 실패할 수
있다. 이런 경우에, 테스트는 버그를 발견하거나, 테스트 자체가 버그가
많기 때문이다. 이런 실패에 대한 에러 메세지 대해 `명령행 옵션 출력 <command
line output_>`__ 과 `리포트 파일 <report file_>`__ 에 설명되어 있다.
그리고 종종 에러 메시지 하나로 문제를 정확하게 파악하는 데 충분하다.
하지만 그렇지 않을때가 더 많다. 하지만 `로그 파일 <log files_>`__ 은
다른 로그 메시지를 가지고 있고 실제로 실패하는 키워드를 보여주기
때문에 필요하다.

..
   When a failure is caused by the tested application, the error message
   and log messages ought to be enough to understand what caused it. If
   that is not the case, the test library does not provide `enough
   information`__ and needs to be enhanced. In this situation running the
   same test manually, if possible, may also reveal more information
   about the issue.

테스트 실패가 테스트되는 응용 프로그램에 의해 발생하는 경우, 로그
메시지와 오류 메시지가 원인을 이해하기에 충분해야한다. 그 경우가
아니라면, 시험 라이브러리가 `충분한 정보`__ 를 제공하고 있지 않으면,
이것은 보완되어야 한다. 이런 상황에서 가능하다면 같은 시험을 수동으로
진행하는 경우에 문제에 대한 더 자세한 정보를 밝힐 수 있다.

..
   Failures caused by test cases themselves or by keywords they use can
   sometimes be hard to debug. If the error message, for example, tells
   that a keyword is used with wrong number of arguments fixing the
   problem is obviously easy, but if a keyword is missing or fails in
   unexpected way finding the root cause can be harder. The first place
   to look for more information is the `execution errors`_ section in
   the log file. For example, an error about a failed test library import
   may well explain why a test has failed due to a missing keyword.

Test cases에 의한 것이거나, 사용하는 키워드에 의한 실패는 때때로
디버그하기가 어렵다. 예를 들어, 에러 메세지가 잘못된 인자수를 사용하는
키워드에 의한 것이다라고 말해주면 문제를 수정하는 것은 쉽다. 하지만
키워드가 없거나 근본원인을 원인을 찾기가 어려운 실패의 경우는 어렵다.
자세한 내용은 로그 파일에 있는 `실행 에러 <execution errors_>`__
섹션을 확인하라. 예를 들어, 시험 라이브러리를 임포트하는 것이 실패하여
발생한 에러는 누락된 키워드 때문에 테스트가 실패되었다고 설명할
것이다.

..
   If the log file does not provide enough information by default, it is
   possible to execute tests with a lower `log level`_. For example
   tracebacks showing where in the code the failure occurred are logged
   using the `DEBUG` level, and this information is invaluable when
   the problem is in an individual library keyword.

로그 파일이 기본적으로 충분한 정보를 제공하지 않는 경우, 더 낮은 `로그
레벨 <log level_>`__ 로 시험을 수행할 수 있다. 예를 들어, 에러가
발생한 코드를 보여주는 역추적은 `DEBUG` 레벨 일 때 기록된다. 그리고
문제가 각 라이브러리 키워드에 있을때, 이 정보는 매우 유용하다.

..
   Logged tracebacks do not contain information about methods inside Robot
   Framework itself. If you suspect an error is caused by a bug in the framework,
   you can enable showing internal traces by setting environment variable
   ``ROBOT_INTERNAL_TRACES`` to any non-empty value. This functionality is
   new in Robot Framework 2.9.2.

로그 역추적은 Robot Framework 자체적으로는 method에 대한 정보를
포함하고 있지 않다. 오류가 프레임워크의 문제로 인해 발생된다고
의심되는 경우, 당신은 어떤 비어 있지 않은 값으로 환경 변수
``ROBOT_INTERNAL_TRACES`` 을 설정하여 내부 추적을 표시하도록 설정할 수
있습니다. 이 기능은 Robot Framework 2.9.2.의 신규 기능이다.

..
   If the log file still does not have enough information, it is a good
   idea to enable the syslog_ and see what information it provides. It is
   also possible to add some keywords to the test cases to see what is
   going on. Especially BuiltIn_ keywords :name:`Log` and :name:`Log
   Variables` are useful. If nothing else works, it is always possible to
   search help from `mailing lists`_ or elsewhere.

로그 파일이 아직도 충분한 정보가 없는 경우, 그것은 syslog_ 를
활성화하여 제공하는 정보를 참조하는 것이 좋다. 진행사항을 확인하기
위해 테스트 케이스에 몇 가지 키워드를 추가 할 수있다. 특히 BuiltIn_
키워드인 :name:`Log` 과 :name:`LogVariables` 가 유용하다. 아무것도
작동하지 않는 경우, 그것은 `mailing lists`_ 나 그외의 곳에 도움을
검색할 수있다.

__ `Communicating with Robot Framework`_

Using the Python debugger (pdb)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..
   It is also possible to use the pdb__ module from the Python standard
   library to set a break point and interactively debug a running test.
   The typical way of invoking pdb by inserting

중단점을 설정하고 실행중인 테스트를 쌍방향으로 디버그 하기 위해 Python
표준 라이브러리에서 pdb__ 모듈을 사용할 수있다. 

.. sourcecode:: python

   import pdb; pdb.set_trace()

..
   at the location you want to break into debugger will not work correctly
   with Robot Framework, though, as the standard output stream is
   redirected during keyword execution. Instead, you can use the following:

위 예제 처럼 디버거로 중지하고 들어가고 싶은 지점에 끼워넣어 pdb를
호출하는 일반적인 방법은 비록 표준 출력 스트림이 키워드 실행 동안
재지정 됨에도 불구하고 Robot Framewokr와 정확하게 동작하지 않을 수
있다. 그럴 경우 대신 아래 처럼 쓸 수 있다.:

.. sourcecode:: python

   import sys, pdb; pdb.Pdb(stdout=sys.__stdout__).set_trace()


__ http://docs.python.org/2/library/pdb.html
